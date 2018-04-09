---
title: Настройка ASP.NET Core для работы с прокси-серверы и подсистемы балансировки нагрузки.
author: guardrex
description: Дополнительные сведения о конфигурации для приложений, размещенных за прокси-серверы и подсистем балансировки нагрузки, которые часто скрывать запрос важные сведения.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 03/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/proxy-load-balancer
ms.openlocfilehash: b153a7406ae1b31a2aa453135c6bd0e5ce0b2997
ms.sourcegitcommit: d45d766504c2c5aad2453f01f089bc6b696b5576
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/30/2018
---
# <a name="configure-aspnet-core-to-work-with-proxy-servers-and-load-balancers"></a><span data-ttu-id="b9bc0-103">Настройка ASP.NET Core для работы с прокси-серверы и подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-103">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>

<span data-ttu-id="b9bc0-104">По [Latham Люк](https://github.com/guardrex) и [Ross Крис](https://github.com/Tratcher)</span><span class="sxs-lookup"><span data-stu-id="b9bc0-104">By [Luke Latham](https://github.com/guardrex) and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="b9bc0-105">В рекомендуемой конфигурации для ASP.NET Core приложение размещается с использованием модуля Core IIS/ASP.NET, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-105">In the recommended configuration for ASP.NET Core, the app is hosted using IIS/ASP.NET Core Module, Nginx, or Apache.</span></span> <span data-ttu-id="b9bc0-106">Прокси-серверы, подсистемы балансировки нагрузки и другие сетевые устройства часто скрывать сведения о запросе, прежде чему будет достигнут приложения:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-106">Proxy servers, load balancers, and other network appliances often obscure information about the request before it reaches the app:</span></span>

* <span data-ttu-id="b9bc0-107">Когда HTTPS-запросы по протоколу HTTP через прокси, первоначальную схему (HTTPS) теряется и следует перенаправить в заголовке.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-107">When HTTPS requests are proxied over HTTP, the original scheme (HTTPS) is lost and must be forwarded in a header.</span></span>
* <span data-ttu-id="b9bc0-108">Так как приложение получает запрос от прокси-сервера и не true источником в Интернете или интрасети, исходный IP-адрес клиента, также следует перенаправить в заголовке.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-108">Because an app receives a request from the proxy and not its true source on the Internet or corporate network, the originating client IP address must also be forwarded in a header.</span></span>

<span data-ttu-id="b9bc0-109">Эти сведения могут быть важны в обработке запросов, например перенаправления, проверки подлинности, компоновки, оценки политики и geoloation клиента.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-109">This information may be important in request processing, for example in redirects, authentication, link generation, policy evaluation, and client geoloation.</span></span>

## <a name="forwarded-headers"></a><span data-ttu-id="b9bc0-110">Перенаправленных заголовки</span><span class="sxs-lookup"><span data-stu-id="b9bc0-110">Forwarded headers</span></span>

<span data-ttu-id="b9bc0-111">По соглашению прокси-серверы пересылки данные в заголовках HTTP.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-111">By convention, proxies forward information in HTTP headers.</span></span>

| <span data-ttu-id="b9bc0-112">Header</span><span class="sxs-lookup"><span data-stu-id="b9bc0-112">Header</span></span> | <span data-ttu-id="b9bc0-113">Описание</span><span class="sxs-lookup"><span data-stu-id="b9bc0-113">Description</span></span> |
| ------ | ----------- |
| <span data-ttu-id="b9bc0-114">X-перенаправленных для</span><span class="sxs-lookup"><span data-stu-id="b9bc0-114">X-Forwarded-For</span></span> | <span data-ttu-id="b9bc0-115">Содержит сведения о клиенте, который инициировал запрос и последующие прокси в цепочке учетных записей-посредников.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-115">Holds information about the client that initiated the request and subsequent proxies in a chain of proxies.</span></span> <span data-ttu-id="b9bc0-116">Этот параметр может содержать IP адресов (и, при необходимости, номера портов).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-116">This parameter may contain IP addresses (and, optionally, port numbers).</span></span> <span data-ttu-id="b9bc0-117">В цепочке прокси-серверов первый параметр указывает клиента, где сначала был сделан запрос.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-117">In a chain of proxy servers, the first parameter indicates the client where the request was first made.</span></span> <span data-ttu-id="b9bc0-118">Выполните идентификаторы последующих прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-118">Subsequent proxy identifiers follow.</span></span> <span data-ttu-id="b9bc0-119">Последний прокси-сервера в цепочке отсутствует в списке параметров.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-119">The last proxy in the chain isn't in the list of parameters.</span></span> <span data-ttu-id="b9bc0-120">IP-адрес последнего прокси и при необходимости номер порта, доступны как удаленный IP-адрес на транспортном уровне.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-120">The last proxy's IP address, and optionally a port number, are available as the remote IP address at the transport layer.</span></span> |
| <span data-ttu-id="b9bc0-121">X-Forwarded-Proto</span><span class="sxs-lookup"><span data-stu-id="b9bc0-121">X-Forwarded-Proto</span></span> | <span data-ttu-id="b9bc0-122">Значение исходную схему (HTTP/HTTPS).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-122">The value of the originating scheme (HTTP/HTTPS).</span></span> <span data-ttu-id="b9bc0-123">Значение также может быть список схем, если запрос обход нескольких учетных записей-посредников.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-123">The value may also be a list of schemes if the request has traversed multiple proxies.</span></span> |
| <span data-ttu-id="b9bc0-124">Пересылаемые узлов X</span><span class="sxs-lookup"><span data-stu-id="b9bc0-124">X-Forwarded-Host</span></span> | <span data-ttu-id="b9bc0-125">Исходное значение поля заголовка узла.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-125">The original value of the Host header field.</span></span> <span data-ttu-id="b9bc0-126">Как правило учетные записи-посредники не следует изменять заголовок узла.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-126">Usually, proxies don't modify the Host header.</span></span> <span data-ttu-id="b9bc0-127">В разделе [Microsoft безопасности рекомендация CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) сведения о уязвимости повышение привилегий, влияет на системах, где не проверить учетную запись-посредник или заголовки узлов restict для известных значений хорошо.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-127">See [Microsoft Security Advisory CVE-2018-0787](https://github.com/aspnet/Announcements/issues/295) for information on an elevation-of-privileges vulnerability that affects systems where the proxy doesn't validate or restict Host headers to known good values.</span></span> |

<span data-ttu-id="b9bc0-128">Пересылаемые заголовки по промежуточного слоя, из [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) упаковки, считывает эти заголовки и заполнит связанные поля на [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-128">The Forwarded Headers Middleware, from the [Microsoft.AspNetCore.HttpOverrides](https://www.nuget.org/packages/Microsoft.AspNetCore.HttpOverrides/) package, reads these headers and fills in the associated fields on [HttpContext](/dotnet/api/microsoft.aspnetcore.http.httpcontext).</span></span> 

<span data-ttu-id="b9bc0-129">Обновления по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-129">The middleware updates:</span></span>

* <span data-ttu-id="b9bc0-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; задать с помощью `X-Forwarded-For` значение заголовка.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-130">[HttpContext.Connection.RemoteIpAddress](/dotnet/api/microsoft.aspnetcore.http.connectioninfo.remoteipaddress) &ndash; Set using the `X-Forwarded-For` header value.</span></span> <span data-ttu-id="b9bc0-131">Дополнительные параметры влияют как по промежуточного слоя задает `RemoteIpAddress`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-131">Additional settings influence how the middleware sets `RemoteIpAddress`.</span></span> <span data-ttu-id="b9bc0-132">Дополнительные сведения см. в разделе [параметры пересылаются по промежуточного слоя заголовки](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-132">For details, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>
* <span data-ttu-id="b9bc0-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; задать с помощью `X-Forwarded-Proto` значение заголовка.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-133">[HttpContext.Request.Scheme](/dotnet/api/microsoft.aspnetcore.http.httprequest.scheme) &ndash; Set using the `X-Forwarded-Proto` header value.</span></span>
* <span data-ttu-id="b9bc0-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; задать с помощью `X-Forwarded-Host` значение заголовка.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-134">[HttpContext.Request.Host](/dotnet/api/microsoft.aspnetcore.http.httprequest.host) &ndash; Set using the `X-Forwarded-Host` header value.</span></span>

<span data-ttu-id="b9bc0-135">Обратите внимание, что не все сетевые устройства добавить `X-Forwarded-For` и `X-Forwarded-Proto` заголовки без дополнительной настройки.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-135">Note that not all network appliances add the `X-Forwarded-For` and `X-Forwarded-Proto` headers without additional configuration.</span></span> <span data-ttu-id="b9bc0-136">Если запросы через прокси не содержит эти заголовки, когда они достигнут приложения, обратитесь к инструкции изготовителем устройства.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-136">Consult your appliance manufacturer's guidance if the proxied requests don't contain these headers when they reach the app.</span></span>

<span data-ttu-id="b9bc0-137">Пересылаются по промежуточного слоя заголовки [параметры по умолчанию](#forwarded-headers-middleware-options) можно настроить.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-137">Forwarded Headers Middleware [default settings](#forwarded-headers-middleware-options) can be configured.</span></span> <span data-ttu-id="b9bc0-138">Значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-138">The default settings are:</span></span>

* <span data-ttu-id="b9bc0-139">Может быть только *прокси* между приложением и источником запросов.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-139">There is only *one proxy* between the app and the source of the requests.</span></span>
* <span data-ttu-id="b9bc0-140">Только адреса замыкания на себя настраиваются для известных учетных записей-посредников и известные сетей.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-140">Only loopback addresses are configured for known proxies and known networks.</span></span>

## <a name="iisiis-express-and-aspnet-core-module"></a><span data-ttu-id="b9bc0-141">IIS и IIS Express и модулей ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b9bc0-141">IIS/IIS Express and ASP.NET Core Module</span></span>

<span data-ttu-id="b9bc0-142">По промежуточного слоя перенаправленных заголовки включена по умолчанию по промежуточного слоя интеграции IIS, при запуске приложения под IIS и модуль ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-142">Forwarded Headers Middleware is enabled by default by IIS Integration Middleware when the app is run behind IIS and the ASP.NET Core Module.</span></span> <span data-ttu-id="b9bc0-143">Для запуска первого конвейера по промежуточного слоя с ограниченным доступом определенных конфигурацией в модуль ASP.NET Core из-за доверия опасения перенаправленных заголовки активируется перенаправленных заголовки по промежуточного слоя (например, [подмена IP-](https://www.iplocation.net/ip-spoofing)).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-143">Forwarded Headers Middleware is activated to run first in the middleware pipeline with a restricted configuration specific to the ASP.NET Core Module due to trust concerns with forwarded headers (for example, [IP spoofing](https://www.iplocation.net/ip-spoofing)).</span></span> <span data-ttu-id="b9bc0-144">По промежуточного слоя настроен на пересылку `X-Forwarded-For` и `X-Forwarded-Proto` заголовки и предназначен только для localhost один прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-144">The middleware is configured to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers and is restricted to a single localhost proxy.</span></span> <span data-ttu-id="b9bc0-145">Если требуется дополнительная настройка, см. раздел [параметры пересылаются по промежуточного слоя заголовки](#forwarded-headers-middleware-options).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-145">If additional configuration is required, see the [Forwarded Headers Middleware options](#forwarded-headers-middleware-options).</span></span>

## <a name="other-proxy-server-and-load-balancer-scenarios"></a><span data-ttu-id="b9bc0-146">Другие прокси-сервера и сценарии подсистемы балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="b9bc0-146">Other proxy server and load balancer scenarios</span></span>

<span data-ttu-id="b9bc0-147">Помимо использования по промежуточного слоя интеграции IIS, пересылаемые по промежуточного слоя заголовки не включен по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-147">Outside of using IIS Integration Middleware, Forwarded Headers Middleware isn't enabled by default.</span></span> <span data-ttu-id="b9bc0-148">Перенаправленных заголовки по промежуточного слоя должна быть включена для приложения, чтобы процесс перенаправленных заголовков с [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-148">Forwarded Headers Middleware must be enabled for an app to process forwarded headers with [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders).</span></span> <span data-ttu-id="b9bc0-149">После включения по промежуточного слоя, если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) задаются по промежуточного слоя, значение по умолчанию [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) , [ForwardedHeaders.None ](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-149">After enabling the middleware if no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified to the middleware, the default [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span>

<span data-ttu-id="b9bc0-150">Настройки по промежуточного слоя с [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) переслать `X-Forwarded-For` и `X-Forwarded-Proto` заголовки в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-150">Configure the middleware with [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) to forward the `X-Forwarded-For` and `X-Forwarded-Proto` headers in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="b9bc0-151">Вызвать [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) метод в `Startup.Configure` перед вызовом другого по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-151">Invoke the [UseForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders) method in `Startup.Configure` before calling other middleware:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();
    
    services.Configure<ForwardedHeadersOptions>(options =>
    {
        options.ForwardedHeaders = 
            ForwardedHeaders.XForwardedFor | ForwardedHeaders.XForwardedProto;
    });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseForwardedHeaders();

    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseStaticFiles();
    // In ASP.NET Core 1.x, replace the following line with: app.UseIdentity();
    app.UseAuthentication();
    app.UseMvc();
}
```

> [!NOTE]
> <span data-ttu-id="b9bc0-152">Если не [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) указываются в `Startup.ConfigureServices` или напрямую в метод расширения с [UseForwardedHeaders (IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), значение по умолчанию заголовки для пересылки [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-152">If no [ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) are specified in `Startup.ConfigureServices` or directly to the extension method with [UseForwardedHeaders(IApplicationBuilder, ForwardedHeadersOptions)](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersextensions.useforwardedheaders?view=aspnetcore-2.0#Microsoft_AspNetCore_Builder_ForwardedHeadersExtensions_UseForwardedHeaders_Microsoft_AspNetCore_Builder_IApplicationBuilder_Microsoft_AspNetCore_Builder_ForwardedHeadersOptions_), the default headers to forward are [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> <span data-ttu-id="b9bc0-153">[ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) свойства должны быть настроены заголовки для пересылки.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-153">The [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) property must be configured with the headers to forward.</span></span>

## <a name="forwarded-headers-middleware-options"></a><span data-ttu-id="b9bc0-154">Перенаправленных параметры по промежуточного слоя заголовки</span><span class="sxs-lookup"><span data-stu-id="b9bc0-154">Forwarded Headers Middleware options</span></span>

<span data-ttu-id="b9bc0-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) управляют поведением по промежуточного слоя перенаправленных заголовки:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-155">[ForwardedHeadersOptions](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions) control the behavior of the Forwarded Headers Middleware:</span></span>

```csharp
services.Configure<ForwardedHeadersOptions>(options =>
{
    options.ForwardLimit = 2;
    options.KnownProxies.Add(IPAddress.Parse("127.0.10.1"));
    options.ForwardedForHeaderName = "X-Forwarded-For-Custom-Header-Name";
});
```

| <span data-ttu-id="b9bc0-156">Параметр</span><span class="sxs-lookup"><span data-stu-id="b9bc0-156">Option</span></span> | <span data-ttu-id="b9bc0-157">Описание</span><span class="sxs-lookup"><span data-stu-id="b9bc0-157">Description</span></span> |
| ------ | ----------- |
| [<span data-ttu-id="b9bc0-158">ForwardedForHeaderName</span><span class="sxs-lookup"><span data-stu-id="b9bc0-158">ForwardedForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedforheadername) | <span data-ttu-id="b9bc0-159">Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-159">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedforheadername).</span></span><br><br><span data-ttu-id="b9bc0-160">Значение по умолчанию — `X-Forwarded-For`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-160">The default is `X-Forwarded-For`.</span></span> |
| [<span data-ttu-id="b9bc0-161">ForwardedHeaders</span><span class="sxs-lookup"><span data-stu-id="b9bc0-161">ForwardedHeaders</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) | <span data-ttu-id="b9bc0-162">Определяет, какие серверы пересылки будут обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-162">Identifies which forwarders should be processed.</span></span> <span data-ttu-id="b9bc0-163">В разделе [ForwardedHeaders перечисления](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) список полей, которые относятся.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-163">See the [ForwardedHeaders Enum](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders) for the list of fields that apply.</span></span> <span data-ttu-id="b9bc0-164">Допустимые значения этого свойства: <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-164">Typical values assigned to this property are <code>ForwardedHeaders.XForwardedFor &#124; ForwardedHeaders.XForwardedProto</code>.</span></span><br><br><span data-ttu-id="b9bc0-165">Значение по умолчанию — [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-165">The default value is [ForwardedHeaders.None](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheaders).</span></span> |
| [<span data-ttu-id="b9bc0-166">ForwardedHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="b9bc0-166">ForwardedHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedhostheadername) | <span data-ttu-id="b9bc0-167">Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-167">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedhostheadername).</span></span><br><br><span data-ttu-id="b9bc0-168">Значение по умолчанию — `X-Forwarded-Host`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-168">The default is `X-Forwarded-Host`.</span></span> |
| [<span data-ttu-id="b9bc0-169">ForwardedProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="b9bc0-169">ForwardedProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedprotoheadername) | <span data-ttu-id="b9bc0-170">Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-170">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XForwardedProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xforwardedprotoheadername).</span></span><br><br><span data-ttu-id="b9bc0-171">Значение по умолчанию — `X-Forwarded-Proto`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-171">The default is `X-Forwarded-Proto`.</span></span> |
| [<span data-ttu-id="b9bc0-172">ForwardLimit</span><span class="sxs-lookup"><span data-stu-id="b9bc0-172">ForwardLimit</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardlimit) | <span data-ttu-id="b9bc0-173">Ограничивает количество записей в заголовках, которые обрабатываются.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-173">Limits the number of entries in the headers that are processed.</span></span> <span data-ttu-id="b9bc0-174">Значение `null` отключить ограничение, но это следует делать только если `KnownProxies` или `KnownNetworks` настроены.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-174">Set to `null` to disable the limit, but this should only be done if `KnownProxies` or `KnownNetworks` are configured.</span></span><br><br><span data-ttu-id="b9bc0-175">Значение по умолчанию — 1.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-175">The default is 1.</span></span> |
| [<span data-ttu-id="b9bc0-176">KnownNetworks</span><span class="sxs-lookup"><span data-stu-id="b9bc0-176">KnownNetworks</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownnetworks) | <span data-ttu-id="b9bc0-177">Диапазоны известных учетных записей-посредников для приема перенаправленных заголовки из адресов.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-177">Address ranges of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="b9bc0-178">Укажите диапазоны IP-адресов, используя нотацию бесклассовой междоменного маршрутизации (CIDR).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-178">Provide IP ranges using Classless Interdomain Routing (CIDR) notation.</span></span><br><br><span data-ttu-id="b9bc0-179">Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-сети](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> содержащий одну запись для `IPAddress.Loopback`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-179">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPNetwork](/dotnet/api/microsoft.aspnetcore.httpoverrides.ipnetwork)> containing a single entry for `IPAddress.Loopback`.</span></span> |
| [<span data-ttu-id="b9bc0-180">KnownProxies</span><span class="sxs-lookup"><span data-stu-id="b9bc0-180">KnownProxies</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.knownproxies) | <span data-ttu-id="b9bc0-181">Адреса для принятия перенаправленных заголовки из известных прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-181">Addresses of known proxies to accept forwarded headers from.</span></span> <span data-ttu-id="b9bc0-182">Используйте `KnownProxies` для указания точного IP-адрес соответствует.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-182">Use `KnownProxies` to specify exact IP address matches.</span></span><br><br><span data-ttu-id="b9bc0-183">Значение по умолчанию — [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IP-адрес](/dotnet/api/system.net.ipaddress)> содержащий одну запись для `IPAddress.IPv6Loopback`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-183">The default is an [IList](/dotnet/api/system.collections.generic.ilist-1)\<[IPAddress](/dotnet/api/system.net.ipaddress)> containing a single entry for `IPAddress.IPv6Loopback`.</span></span> |
| [<span data-ttu-id="b9bc0-184">OriginalForHeaderName</span><span class="sxs-lookup"><span data-stu-id="b9bc0-184">OriginalForHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalforheadername) | <span data-ttu-id="b9bc0-185">Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-185">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalForHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalforheadername).</span></span><br><br><span data-ttu-id="b9bc0-186">Значение по умолчанию — `X-Original-For`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-186">The default is `X-Original-For`.</span></span> |
| [<span data-ttu-id="b9bc0-187">OriginalHostHeaderName</span><span class="sxs-lookup"><span data-stu-id="b9bc0-187">OriginalHostHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalhostheadername) | <span data-ttu-id="b9bc0-188">Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-188">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalHostHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalhostheadername).</span></span><br><br><span data-ttu-id="b9bc0-189">Значение по умолчанию — `X-Original-Host`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-189">The default is `X-Original-Host`.</span></span> |
| [<span data-ttu-id="b9bc0-190">OriginalProtoHeaderName</span><span class="sxs-lookup"><span data-stu-id="b9bc0-190">OriginalProtoHeaderName</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.originalprotoheadername) | <span data-ttu-id="b9bc0-191">Использовать заголовок, заданный этим свойством, а не, указанного параметром [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-191">Use the header specified by this property instead of the one specified by [ForwardedHeadersDefaults.XOriginalProtoHeaderName](/dotnet/api/microsoft.aspnetcore.httpoverrides.forwardedheadersdefaults.xoriginalprotoheadername).</span></span><br><br><span data-ttu-id="b9bc0-192">Значение по умолчанию — `X-Original-Proto`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-192">The default is `X-Original-Proto`.</span></span> |
| [<span data-ttu-id="b9bc0-193">RequireHeaderSymmetry</span><span class="sxs-lookup"><span data-stu-id="b9bc0-193">RequireHeaderSymmetry</span></span>](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.requireheadersymmetry) | <span data-ttu-id="b9bc0-194">Требуется номер значений заголовка, должны быть синхронизированы между [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-194">Require the number of header values to be in sync between the [ForwardedHeadersOptions.ForwardedHeaders](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.forwardedheaders) being processed.</span></span><br><br><span data-ttu-id="b9bc0-195">Значение по умолчанию в ASP.NET Core 1.x — `true`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-195">The default in ASP.NET Core 1.x is `true`.</span></span> <span data-ttu-id="b9bc0-196">Значение по умолчанию в ASP.NET Core 2.0 или более поздней версии — `false`.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-196">The default in ASP.NET Core 2.0 or later is `false`.</span></span> |

## <a name="scenarios-and-use-cases"></a><span data-ttu-id="b9bc0-197">Сценарии и примеры</span><span class="sxs-lookup"><span data-stu-id="b9bc0-197">Scenarios and use cases</span></span>

### <a name="when-it-isnt-possible-to-add-forwarded-headers-and-all-requests-are-secure"></a><span data-ttu-id="b9bc0-198">При нельзя добавить перенаправленных заголовки и все запросы являются безопасными</span><span class="sxs-lookup"><span data-stu-id="b9bc0-198">When it isn't possible to add forwarded headers and all requests are secure</span></span>

<span data-ttu-id="b9bc0-199">В некоторых случаях может быть невозможно добавить перенаправленных заголовки для запросов через прокси, чтобы приложение.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-199">In some cases, it might not be possible to add forwarded headers to the requests proxied to the app.</span></span> <span data-ttu-id="b9bc0-200">Если прокси-сервер установлен режим принудительного применения, все открытые внешние запросы HTTPS, схему можно вручную задать в `Startup.Configure` перед использованием любой тип по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-200">If the proxy is enforcing that all public external requests are HTTPS, the scheme can be manually set in `Startup.Configure` before using any type of middleware:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.Scheme = "https";
    return next();
});
```

<span data-ttu-id="b9bc0-201">Этот код можно отключить с помощью переменной среды или другого параметра конфигурации при разработке или в тестовой среде.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-201">This code can be disabled with an environment variable or other configuration setting in a development or staging environment.</span></span>

### <a name="deal-with-path-base-and-proxies-that-change-the-request-path"></a><span data-ttu-id="b9bc0-202">Работать с база пути и учетных записей-посредников, которые изменяют путь запроса</span><span class="sxs-lookup"><span data-stu-id="b9bc0-202">Deal with path base and proxies that change the request path</span></span>

<span data-ttu-id="b9bc0-203">Некоторые учетные записи-посредники передать путь без изменений, но с приложением, базовый путь, который должен быть удален, чтобы маршрутизации работает правильно.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-203">Some proxies pass the path intact but with an app base path that should be removed so that routing works properly.</span></span> <span data-ttu-id="b9bc0-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) по промежуточного слоя разделяет путь в [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) и базовой папке приложения в [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-204">[UsePathBaseExtensions.UsePathBase](/dotnet/api/microsoft.aspnetcore.builder.usepathbaseextensions.usepathbase) middleware splits the path into [HttpRequest.Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) and the app base path into [HttpRequest.PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase).</span></span>

<span data-ttu-id="b9bc0-205">Если `/foo` является базовой папке приложения, для прокси-сервера путь передается в качестве `/foo/api/1`, по промежуточного слоя наборы `Request.PathBase` для `/foo` и `Request.Path` для `/api/1` с помощью следующей команды:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-205">If `/foo` is the app base path for a proxy path passed as `/foo/api/1`, the middleware sets `Request.PathBase` to `/foo` and `Request.Path` to `/api/1` with the following command:</span></span>

```csharp
app.UsePathBase("/foo");
```

<span data-ttu-id="b9bc0-206">Исходный путь и базовый путь применяются, когда по промежуточного слоя вызывается повторно в обратном порядке.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-206">The original path and path base are reapplied when the middleware is called again in reverse.</span></span> <span data-ttu-id="b9bc0-207">Дополнительные сведения об обработке заказов по промежуточного слоя см. в разделе [по промежуточного слоя](xref:fundamentals/middleware/index).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-207">For more information on middleware order processing, see [Middleware](xref:fundamentals/middleware/index).</span></span>

<span data-ttu-id="b9bc0-208">Если прокси-сервер усекает путь (например, пересылка `/foo/api/1` для `/api/1`), исправление перенаправляет и ссылки, задав запроса [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) свойства:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-208">If the proxy trims the path (for example, forwarding `/foo/api/1` to `/api/1`), fix redirects and links by setting the request's [PathBase](/dotnet/api/microsoft.aspnetcore.http.httprequest.pathbase) property:</span></span>

```csharp
app.Use((context, next) =>
{
    context.Request.PathBase = new PathString("/foo");
    return next();
});
```

<span data-ttu-id="b9bc0-209">Если прокси-сервер добавляет данные пути, извлеките часть пути устранения ошибки перенаправления и ссылки [StartsWithSegments (PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) и присвоение [путь](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) свойство:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-209">If the proxy is adding path data, discard part of the path to fix redirects and links by using [StartsWithSegments(PathString, PathString)](/dotnet/api/microsoft.aspnetcore.http.pathstring.startswithsegments#Microsoft_AspNetCore_Http_PathString_StartsWithSegments_Microsoft_AspNetCore_Http_PathString_Microsoft_AspNetCore_Http_PathString__) and assigning to the [Path](/dotnet/api/microsoft.aspnetcore.http.httprequest.path) property:</span></span>

```csharp
app.Use((context, next) =>
{
    if (context.Request.Path.StartsWithSegments("/foo", out var remainder))
    {
        context.Request.Path = remainder;
    }

    return next();
});
```

## <a name="troubleshoot"></a><span data-ttu-id="b9bc0-210">Устранение неполадок</span><span class="sxs-lookup"><span data-stu-id="b9bc0-210">Troubleshoot</span></span>

<span data-ttu-id="b9bc0-211">Включить заголовки не перенаправления должным образом, [ведение журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="b9bc0-211">When headers aren't forwarded as expected, enable [logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="b9bc0-212">Если журналы не предоставляет достаточно сведений для устранения неполадок, указаны заголовки запросов, полученных сервером.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-212">If the logs don't provide sufficient information to troubleshoot the problem, enumerate the request headers received by the server.</span></span> <span data-ttu-id="b9bc0-213">Заголовки могут записываться для ответа на приложения, с помощью встроенного ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b9bc0-213">The headers can be written to an app response using inline middleware:</span></span>

```csharp
public void Configure(IApplicationBuilder app, ILoggerFactory loggerfactory)
{
    app.Run(async (context) =>
    {
        context.Response.ContentType = "text/plain";

        // Request method, scheme, and path
        await context.Response.WriteAsync(
            $"Request Method: {context.Request.Method}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Scheme: {context.Request.Scheme}{Environment.NewLine}");
        await context.Response.WriteAsync(
            $"Request Path: {context.Request.Path}{Environment.NewLine}");

        // Headers
        await context.Response.WriteAsync($"Request Headers:{Environment.NewLine}");

        foreach (var header in context.Request.Headers)
        {
            await context.Response.WriteAsync($"{header.Key}: " +
                $"{header.Value}{Environment.NewLine}");
        }

        await context.Response.WriteAsync(Environment.NewLine);

        // Connection: RemoteIp
        await context.Response.WriteAsync(
            $"Request RemoteIp: {context.Connection.RemoteIpAddress}");
    }
}
```

<span data-ttu-id="b9bc0-214">Убедитесь, что X - перенаправленных-\* заголовки, полученных от сервера с ожидаемыми значениями.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-214">Ensure that the X-Forwarded-\* headers are received by the server with the expected values.</span></span> <span data-ttu-id="b9bc0-215">Если существует несколько значений в данный заголовок, обратите внимание, пересылаемые по промежуточного слоя заголовки процессы заголовки в обратном порядке справа налево.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-215">If there are multiple values in a given header, note Forwarded Headers Middleware processes headers in reverse order from right to left.</span></span>

<span data-ttu-id="b9bc0-216">Исходный IP удаленного запроса должно соответствовать запись в `KnownProxies` или `KnownNetworks` перечислены до X-перенаправленных-для обработки.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-216">The request's original remote IP must match an entry in the `KnownProxies` or `KnownNetworks` lists before X-Forwarded-For is processed.</span></span> <span data-ttu-id="b9bc0-217">Это ограничивает заголовок спуфинг, не принимая пересылки из ненадежных учетных записей-посредников.</span><span class="sxs-lookup"><span data-stu-id="b9bc0-217">This limits header spoofing by not accepting forwarders from untrusted proxies.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b9bc0-218">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b9bc0-218">Additional resources</span></span>

* [<span data-ttu-id="b9bc0-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core повышения уровня прав доступа</span><span class="sxs-lookup"><span data-stu-id="b9bc0-219">Microsoft Security Advisory CVE-2018-0787: ASP.NET Core Elevation Of Privilege Vulnerability</span></span>](https://github.com/aspnet/Announcements/issues/295)
