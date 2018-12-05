---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Узнайте, как требовать HTTPS/TLS в веб-приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 12/01/2018
uid: security/enforcing-ssl
ms.openlocfilehash: b15c6b5ac77f047c40704c9e164165c55b6ae93b
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861528"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="425f1-103">Принудительное использование HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="425f1-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="425f1-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="425f1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="425f1-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="425f1-105">This document shows how to:</span></span>

* <span data-ttu-id="425f1-106">Требование HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="425f1-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="425f1-107">Перенаправлять все запросы HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="425f1-108">Интерфейс API может препятствовать передачи конфиденциальных данных при первом запросе клиента.</span><span class="sxs-lookup"><span data-stu-id="425f1-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="425f1-109">Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получения конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="425f1-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="425f1-110">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления браузеров с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="425f1-111">Клиенты API не может понять и подчиняются перенаправление с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="425f1-112">Такие клиенты могут отправлять данные по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="425f1-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="425f1-113">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="425f1-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="425f1-114">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="425f1-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="425f1-115">Закрыть соединение с кодом состояния 400 (неправильный запрос) и не обработать запрос.</span><span class="sxs-lookup"><span data-stu-id="425f1-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="425f1-116">Требование HTTPS</span><span class="sxs-lookup"><span data-stu-id="425f1-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="425f1-117">Мы рекомендуем производственные ASP.NET Core, веб-приложений вызова:</span><span class="sxs-lookup"><span data-stu-id="425f1-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="425f1-118">По промежуточного слоя переадресации HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) для перенаправления запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="425f1-119">По промежуточного слоя HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) для отправки заголовков HTTP Strict транспорта безопасности протокола (HSTS) клиентам.</span><span class="sxs-lookup"><span data-stu-id="425f1-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="425f1-120">Приложения, развернутые в конфигурации обратного прокси-сервера разрешить прокси-сервер для обработки безопасность подключения (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="425f1-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="425f1-121">Если прокси-сервер также обрабатывает перенаправления HTTPS, нет необходимости использовать по промежуточного слоя переадресации HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="425f1-122">Если прокси-сервера также обрабатывает операции записи HSTS заголовков (например, [собственного HSTS поддержки в IIS 10.0 (1709) или более поздней версии](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), по промежуточного слоя HSTS не требуется приложением.</span><span class="sxs-lookup"><span data-stu-id="425f1-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="425f1-123">Дополнительные сведения см. в разделе [выхода из HTTPS/HSTS при создании проекта](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="425f1-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="425f1-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="425f1-124">UseHttpsRedirection</span></span>

<span data-ttu-id="425f1-125">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="425f1-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="425f1-126">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="425f1-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="425f1-127">Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="425f1-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="425f1-128">Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если не переопределено `ASPNETCORE_HTTPS_PORT` переменной среды или [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="425f1-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="425f1-129">Мы рекомендуем использовать временный перенаправления, а не постоянной переадресации.</span><span class="sxs-lookup"><span data-stu-id="425f1-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="425f1-130">Кэширование ссылок может привести к нестабильной работе поведению в средах разработки.</span><span class="sxs-lookup"><span data-stu-id="425f1-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="425f1-131">Если вы хотите отправить код состояния постоянное перенаправление, во время работы приложения в среде не разработки, см. в разделе [Настройка постоянной переадресации в рабочей среде](#configure-permanent-redirects-in-production) раздел.</span><span class="sxs-lookup"><span data-stu-id="425f1-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="425f1-132">Мы рекомендуем использовать [HSTS](#http-strict-transport-security-protocol-hsts) сигнал для клиентов, которые только защитить ресурс запросы следует отправлять в приложение (только в рабочей среде).</span><span class="sxs-lookup"><span data-stu-id="425f1-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="425f1-133">Конфигурация порта</span><span class="sxs-lookup"><span data-stu-id="425f1-133">Port configuration</span></span>

<span data-ttu-id="425f1-134">Порт должен быть доступен для по промежуточного слоя для перенаправления небезопасный запрос HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="425f1-135">Если порт не доступен:</span><span class="sxs-lookup"><span data-stu-id="425f1-135">If no port is available:</span></span>

* <span data-ttu-id="425f1-136">Не произойдет перенаправление на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="425f1-137">По промежуточного слоя записывает предупреждение «Не удалось определить https-порт для перенаправления.»</span><span class="sxs-lookup"><span data-stu-id="425f1-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="425f1-138">Укажите порт HTTPS, с помощью любого из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="425f1-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="425f1-139">Задайте [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="425f1-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="425f1-140">Задайте `ASPNETCORE_HTTPS_PORT` переменной среды или [параметр конфигурации веб-узел https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="425f1-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="425f1-141">**Ключ**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="425f1-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="425f1-142">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="425f1-142">**Type**: *string*</span></span>  
  <span data-ttu-id="425f1-143">**По умолчанию**: не задано значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="425f1-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="425f1-144">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="425f1-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="425f1-145">**Переменная среды**: `<PREFIX_>HTTPS_PORT` (используется префикс `ASPNETCORE_` при использовании [веб-узел](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="425f1-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="425f1-146">При настройке <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> в `Program`:</span><span class="sxs-lookup"><span data-stu-id="425f1-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="425f1-147">Указать порт с безопасной схемы с помощью `ASPNETCORE_URLS` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="425f1-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="425f1-148">Переменная среды настраивает сервер.</span><span class="sxs-lookup"><span data-stu-id="425f1-148">The environment variable configures the server.</span></span> <span data-ttu-id="425f1-149">По промежуточного слоя косвенно обнаруживает HTTPS-порт, через <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="425f1-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="425f1-150">Этот подход не работает в развертываниях обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="425f1-150">This approach doesn't work in reverse proxy deployments.</span></span>
* <span data-ttu-id="425f1-151">В разработке, задать URL-адрес HTTPS в *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="425f1-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="425f1-152">Включите протокол HTTPS, если используется IIS Express.</span><span class="sxs-lookup"><span data-stu-id="425f1-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="425f1-153">Настроить конечную точку HTTPS URL-адрес для развертывания общедоступных edge [Kestrel](xref:fundamentals/servers/kestrel) сервера или [HTTP.sys](xref:fundamentals/servers/httpsys) сервера.</span><span class="sxs-lookup"><span data-stu-id="425f1-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="425f1-154">Только **один порт HTTPS** используется приложением.</span><span class="sxs-lookup"><span data-stu-id="425f1-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="425f1-155">По промежуточного слоя обнаруживает порт с помощью <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="425f1-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="425f1-156">Когда приложение выполняется в конфигурации обратного прокси-сервера, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> недоступна.</span><span class="sxs-lookup"><span data-stu-id="425f1-156">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="425f1-157">Задайте номер порта, с помощью одного из других подходов, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="425f1-157">Set the port using one of the other approaches described in this section.</span></span>

<span data-ttu-id="425f1-158">Если Kestrel и HTTP.sys используется в качестве общедоступных пограничного сервера, Kestrel и HTTP.sys должны быть настроены на прослушивание оба:</span><span class="sxs-lookup"><span data-stu-id="425f1-158">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="425f1-159">Безопасного порта, где перенаправляется клиент (как правило, 443 в рабочей среде и 5001 в разработке).</span><span class="sxs-lookup"><span data-stu-id="425f1-159">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="425f1-160">Небезопасный порт (как правило, 80 в рабочей среде) до 5000 при разработке.</span><span class="sxs-lookup"><span data-stu-id="425f1-160">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="425f1-161">Небезопасный порт должен быть доступны клиенту в порядке для приложения для получения небезопасный запрос и перенаправления клиента для безопасного порта.</span><span class="sxs-lookup"><span data-stu-id="425f1-161">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="425f1-162">Дополнительные сведения см. в разделе [конфигурации конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) или <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="425f1-162">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="425f1-163">Сценарии развертывания</span><span class="sxs-lookup"><span data-stu-id="425f1-163">Deployment scenarios</span></span>

<span data-ttu-id="425f1-164">Любой брандмауэра между клиентом и сервером также должны быть открыты для трафика порты связи.</span><span class="sxs-lookup"><span data-stu-id="425f1-164">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="425f1-165">Если запросы перенаправляются в конфигурации обратного прокси-сервера, используйте [пересылаемые по промежуточного слоя заголовки](xref:host-and-deploy/proxy-load-balancer) перед вызовом по промежуточного слоя переадресации HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-165">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="425f1-166">Пересылаемые обновлений по промежуточного слоя заголовки `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовка.</span><span class="sxs-lookup"><span data-stu-id="425f1-166">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="425f1-167">По промежуточного слоя позволяет перенаправлять URI и других политик безопасности, для правильной работы.</span><span class="sxs-lookup"><span data-stu-id="425f1-167">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="425f1-168">При пересылаемые по промежуточного слоя заголовки не используется, во внутреннее приложение может не получать правильные схему и цикл перенаправления.</span><span class="sxs-lookup"><span data-stu-id="425f1-168">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="425f1-169">Сообщение об ошибке распространенных конечного пользователя — в том, что произошло слишком много перенаправлений.</span><span class="sxs-lookup"><span data-stu-id="425f1-169">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="425f1-170">При развертывании в службе приложений Azure, следуйте указаниям в [учебник: привязка существующего пользовательского SSL-сертификата к веб-приложениях Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="425f1-170">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="425f1-171">Параметры</span><span class="sxs-lookup"><span data-stu-id="425f1-171">Options</span></span>

<span data-ttu-id="425f1-172">Следующий выделенный код вызывает метод [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="425f1-172">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="425f1-173">Вызов `AddHttpsRedirection` необходимо изменить значения `HttpsPort` или `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="425f1-173">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="425f1-174">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="425f1-174">The preceding highlighted code:</span></span>

* <span data-ttu-id="425f1-175">Наборы [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) для <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, который является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="425f1-175">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="425f1-176">Используйте поля <xref:Microsoft.AspNetCore.Http.StatusCodes> класс для назначений `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="425f1-176">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="425f1-177">Задать HTTPS-порт на 5001.</span><span class="sxs-lookup"><span data-stu-id="425f1-177">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="425f1-178">Значение по умолчанию — 443.</span><span class="sxs-lookup"><span data-stu-id="425f1-178">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="425f1-179">Настройка постоянной переадресации в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="425f1-179">Configure permanent redirects in production</span></span>

<span data-ttu-id="425f1-180">По умолчанию по промежуточного слоя на отправку [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) с все операции перенаправления.</span><span class="sxs-lookup"><span data-stu-id="425f1-180">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="425f1-181">Если вы хотите отправить код состояния постоянное перенаправление, во время работы приложения в среде не разработки, поместите параметры конфигурации по промежуточного слоя в условную проверку на среду не разработки.</span><span class="sxs-lookup"><span data-stu-id="425f1-181">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="425f1-182">При настройке `IWebHostBuilder` в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="425f1-182">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IHostingEnvironment (stored in _env) is injected into the Startup class.
    if (!_env.IsDevelopment())
    {
        services.AddHttpsRedirection(options =>
        {
            options.RedirectStatusCode = StatusCodes.Status308PermanentRedirect;
            options.HttpsPort = 443;
        });
    }
}
```

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="425f1-183">Альтернативный подход по промежуточного слоя переадресации HTTPS</span><span class="sxs-lookup"><span data-stu-id="425f1-183">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="425f1-184">Альтернативой использованию по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя для переопределения URL-адрес (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="425f1-184">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="425f1-185">`AddRedirectToHttps` Можно также задать код состояния и порт при выполнении перенаправления.</span><span class="sxs-lookup"><span data-stu-id="425f1-185">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="425f1-186">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="425f1-186">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="425f1-187">При перенаправлении HTTPS без необходимости для перенаправления дополнительных правил, мы рекомендуем использовать по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) описано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="425f1-187">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="425f1-188">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-188">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="425f1-189">`[RequireHttpsAttribute]` можно снабдить контроллеров или методы, или могут применяться глобально.</span><span class="sxs-lookup"><span data-stu-id="425f1-189">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="425f1-190">Чтобы применить атрибут глобально, добавьте следующий код, чтобы `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="425f1-190">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="425f1-191">Выделенный выше код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="425f1-191">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="425f1-192">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="425f1-192">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="425f1-193">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="425f1-193">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="425f1-194">По промежуточного слоя также позволяет приложению задать код состояния или код состояния и номер порта, если выполняется перенаправление.</span><span class="sxs-lookup"><span data-stu-id="425f1-194">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="425f1-195">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) — это рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="425f1-195">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="425f1-196">Применение `[RequireHttps]` атрибута ко всем страницам контроллеров/Razor не считается так безопасен, как требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="425f1-196">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="425f1-197">Не может гарантировать `[RequireHttps]` атрибут применяется, когда добавляются новые контроллеры и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="425f1-197">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="425f1-198">Безопасность строгой транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="425f1-198">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="425f1-199">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP строгой безопасности транспорта (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) представляет собой улучшение центра безопасности, который задается параметром веб-приложения при помощи заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="425f1-199">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="425f1-200">Когда [браузера, поддерживающего HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) получения этого заголовка:</span><span class="sxs-lookup"><span data-stu-id="425f1-200">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="425f1-201">Браузер хранит конфигурацию для домена, который предотвращает отправку любые данные, передаваемые по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="425f1-201">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="425f1-202">Браузер заставляет весь обмен данными по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="425f1-202">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="425f1-203">Браузер запрещает пользователю с помощью недействителен или сертификаты.</span><span class="sxs-lookup"><span data-stu-id="425f1-203">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="425f1-204">Браузер отключает подсказок, которые позволяют пользователю временно доверять такой сертификат.</span><span class="sxs-lookup"><span data-stu-id="425f1-204">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="425f1-205">Поскольку HSTS обеспечивается с помощью клиента он имеет некоторые ограничения:</span><span class="sxs-lookup"><span data-stu-id="425f1-205">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="425f1-206">Клиент должен поддерживать HSTS.</span><span class="sxs-lookup"><span data-stu-id="425f1-206">The client must support HSTS.</span></span>
* <span data-ttu-id="425f1-207">HSTS требуется по крайней мере один успешный запрос HTTPS для задания политики HSTS.</span><span class="sxs-lookup"><span data-stu-id="425f1-207">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="425f1-208">Приложение должно проверить каждый HTTP-запрос и перенаправления или отклонить запрос HTTP.</span><span class="sxs-lookup"><span data-stu-id="425f1-208">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="425f1-209">ASP.NET Core 2.1 или более поздних версий реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="425f1-209">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="425f1-210">Следующий код вызывает `UseHsts` когда приложение не [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="425f1-210">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="425f1-211">`UseHsts` не рекомендуется в разработке, так как параметры HSTS высокой кэшируемых обозревателями.</span><span class="sxs-lookup"><span data-stu-id="425f1-211">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="425f1-212">По умолчанию `UseHsts` исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="425f1-212">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="425f1-213">Для рабочих сред реализации HTTPS в первый раз установить начального [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) на малое значение, с помощью одного из <xref:System.TimeSpan> методы.</span><span class="sxs-lookup"><span data-stu-id="425f1-213">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="425f1-214">Задайте значение от часов не больше, чем в один день в случае необходимости использования инфраструктуры HTTPS-HTTP.</span><span class="sxs-lookup"><span data-stu-id="425f1-214">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="425f1-215">После вы уверены в устойчивости конфигурации HTTPS, увеличьте значение max-age HSTS; часто используемые значение — один год.</span><span class="sxs-lookup"><span data-stu-id="425f1-215">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="425f1-216">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="425f1-216">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="425f1-217">Задает параметр предварительной загрузки заголовок Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="425f1-217">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="425f1-218">Предварительной загрузки не входит в состав [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления.</span><span class="sxs-lookup"><span data-stu-id="425f1-218">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="425f1-219">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="425f1-219">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="425f1-220">Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов.</span><span class="sxs-lookup"><span data-stu-id="425f1-220">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="425f1-221">Явно задает параметр max-age заголовок Strict-Transport-Security 60 дней.</span><span class="sxs-lookup"><span data-stu-id="425f1-221">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="425f1-222">Если не установлено значение по умолчанию — 30 дней.</span><span class="sxs-lookup"><span data-stu-id="425f1-222">If not set, defaults to 30 days.</span></span> <span data-ttu-id="425f1-223">См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="425f1-223">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="425f1-224">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="425f1-224">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="425f1-225">`UseHsts` исключает следующими узлами замыкания на себя:</span><span class="sxs-lookup"><span data-stu-id="425f1-225">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="425f1-226">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="425f1-226">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="425f1-227">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="425f1-227">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="425f1-228">`[::1]` : IPv6-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="425f1-228">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="425f1-229">Отказаться от HTTPS/HSTS при создании проекта</span><span class="sxs-lookup"><span data-stu-id="425f1-229">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="425f1-230">В некоторых сценариях службы серверной части, где безопасность подключения обрабатывается на границе общедоступные сети Настройка безопасности подключения на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="425f1-230">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="425f1-231">Веб-приложений, созданных из шаблонов в Visual Studio или из [команды dotnet new](/dotnet/core/tools/dotnet-new) команду enable [перенаправления HTTPS](#require-https) и [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="425f1-231">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="425f1-232">Для развертываний, которые не требуют этих сценариев вы можете отказаться от HTTPS/HSTS создания приложения на основе шаблона.</span><span class="sxs-lookup"><span data-stu-id="425f1-232">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="425f1-233">Чтобы отказаться от HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="425f1-233">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="425f1-234">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="425f1-234">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="425f1-235">Снимите флажок **настроить для использования протокола HTTPS** "флажок".</span><span class="sxs-lookup"><span data-stu-id="425f1-235">Uncheck the **Configure for HTTPS** check box.</span></span>

![Новое диалоговое окно веб-приложение ASP.NET Core, показывающий настройки для HTTPS флажок снят.](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="425f1-237">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="425f1-237">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="425f1-238">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="425f1-238">Use the `--no-https` option.</span></span> <span data-ttu-id="425f1-239">Пример</span><span class="sxs-lookup"><span data-stu-id="425f1-239">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="425f1-240">Доверять сертификату разработки ASP.NET Core HTTPS в Windows и macOS</span><span class="sxs-lookup"><span data-stu-id="425f1-240">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="425f1-241">Пакет SDK для .NET core включает в себя HTTPS-сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="425f1-241">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="425f1-242">Сертификат установлен как часть при первом запуске.</span><span class="sxs-lookup"><span data-stu-id="425f1-242">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="425f1-243">Например `dotnet --info` выходные данные выглядят примерно следующим:</span><span class="sxs-lookup"><span data-stu-id="425f1-243">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="425f1-244">При установке пакета SDK для .NET Core устанавливается сертификат разработки ASP.NET Core HTTPS в хранилище сертификатов локального пользователя.</span><span class="sxs-lookup"><span data-stu-id="425f1-244">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="425f1-245">Сертификат был установлен, но он не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="425f1-245">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="425f1-246">Доверие сертификата выполнить однократное действие для выполнения dotnet `dev-certs` средство:</span><span class="sxs-lookup"><span data-stu-id="425f1-246">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="425f1-247">Следующая команда предоставляет справку для `dev-certs` средство:</span><span class="sxs-lookup"><span data-stu-id="425f1-247">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="425f1-248">Как настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="425f1-248">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="425f1-249">См. в разделе [проблема GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="425f1-249">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="425f1-250">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="425f1-250">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="425f1-251">Размещение ASP.NET Core в Linux с использованием Apache: конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="425f1-251">Host ASP.NET Core on Linux with Apache: SSL configuration</span></span>](xref:host-and-deploy/linux-apache#ssl-configuration)
* [<span data-ttu-id="425f1-252">Размещение ASP.NET Core в Linux с Nginx: конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="425f1-252">Host ASP.NET Core on Linux with Nginx: SSL configuration</span></span>](xref:host-and-deploy/linux-nginx#configure-ssl)
* [<span data-ttu-id="425f1-253">Настройка SSL в IIS</span><span class="sxs-lookup"><span data-stu-id="425f1-253">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="425f1-254">Поддержка браузеров OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="425f1-254">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
