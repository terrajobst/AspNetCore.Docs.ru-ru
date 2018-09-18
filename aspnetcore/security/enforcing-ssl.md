---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требовать HTTPS/TLS, в ASP.NET Core веб-приложения.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: 06ff89d30fb3586c69274cc7cb3e6c75065b098a
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011330"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="e47b1-103">Принудительное использование HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e47b1-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="e47b1-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e47b1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e47b1-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="e47b1-105">This document shows how to:</span></span>

* <span data-ttu-id="e47b1-106">Требование HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="e47b1-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="e47b1-107">Перенаправлять все запросы HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="e47b1-108">Интерфейс API может препятствовать передачи конфиденциальных данных при первом запросе клиента.</span><span class="sxs-lookup"><span data-stu-id="e47b1-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="e47b1-109">Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получения конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="e47b1-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="e47b1-110">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления браузеров с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="e47b1-111">Клиенты API не может понять и подчиняются перенаправление с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="e47b1-112">Такие клиенты могут отправлять данные по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="e47b1-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="e47b1-113">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="e47b1-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="e47b1-114">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="e47b1-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="e47b1-115">Закрыть соединение с кодом состояния 400 (неправильный запрос) и не обработать запрос.</span><span class="sxs-lookup"><span data-stu-id="e47b1-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="e47b1-116">Требование HTTPS</span><span class="sxs-lookup"><span data-stu-id="e47b1-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e47b1-117">Мы рекомендуем все производство ASP.NET Core, веб-приложений вызова:</span><span class="sxs-lookup"><span data-stu-id="e47b1-117">We recommend all production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="e47b1-118">По промежуточного слоя переадресации HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-118">The HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="e47b1-119">[UseHsts](#hsts), безопасность строгой транспортный протокол HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="e47b1-119">[UseHsts](#hsts), HTTP Strict Transport Security Protocol (HSTS).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="e47b1-120">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="e47b1-120">UseHttpsRedirection</span></span>

<span data-ttu-id="e47b1-121">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="e47b1-121">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="e47b1-122">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="e47b1-122">The preceding highlighted code:</span></span>

* <span data-ttu-id="e47b1-123">Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="e47b1-123">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span>
* <span data-ttu-id="e47b1-124">Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если не переопределено `ASPNETCORE_HTTPS_PORT` переменной среды или [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="e47b1-124">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

> [!WARNING] 
><span data-ttu-id="e47b1-125">Порт должен быть доступен для по промежуточного слоя для перенаправления на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-125">A port must be available for the middleware to redirect to HTTPS.</span></span> <span data-ttu-id="e47b1-126">Если порт не доступен, выполняется перенаправление на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-126">If no port is available, redirection to HTTPS does not occur.</span></span> <span data-ttu-id="e47b1-127">HTTPS-порт можно указать с любой из следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="e47b1-127">The HTTPS port can be specified by any of the following setting:</span></span>
> 
>* `HttpsRedirectionOptions.HttpsPort` 
>* <span data-ttu-id="e47b1-128">`ASPNETCORE_HTTPS_PORT` Переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e47b1-128">The `ASPNETCORE_HTTPS_PORT` environment variable.</span></span> 
>* <span data-ttu-id="e47b1-129">В разработке, URL-адрес HTTPS в *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="e47b1-129">In development, an HTTPS url in *launchsettings.json*.</span></span> 
>* <span data-ttu-id="e47b1-130">URL-адрес HTTPS настроены непосредственно в Kestrel или HttpSys.</span><span class="sxs-lookup"><span data-stu-id="e47b1-130">An HTTPS url configured directly on Kestrel or HttpSys.</span></span> 

<span data-ttu-id="e47b1-131">Следующий выделенный код вызывает метод [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="e47b1-131">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="e47b1-132">Вызов `AddHttpsRedirection` необходимо изменить значения ` HttpsPort` или ` RedirectStatusCode`;</span><span class="sxs-lookup"><span data-stu-id="e47b1-132">Calling `AddHttpsRedirection` is only necessary to change the values of ` HttpsPort` or ` RedirectStatusCode`;</span></span>

<span data-ttu-id="e47b1-133">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="e47b1-133">The preceding highlighted code:</span></span>

* <span data-ttu-id="e47b1-134">Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) для `Status307TemporaryRedirect`, который является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e47b1-134">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span>
* <span data-ttu-id="e47b1-135">Задать HTTPS-порт на 5001.</span><span class="sxs-lookup"><span data-stu-id="e47b1-135">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="e47b1-136">Значение по умолчанию — 443.</span><span class="sxs-lookup"><span data-stu-id="e47b1-136">The default value is 443.</span></span>

<span data-ttu-id="e47b1-137">Следующие механизмы автоматически задать порт:</span><span class="sxs-lookup"><span data-stu-id="e47b1-137">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="e47b1-138">По промежуточного слоя можно обнаружить порты, через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) когда применяются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="e47b1-138">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>

   * <span data-ttu-id="e47b1-139">Kestrel и HTTP.sys используется непосредственно с помощью конечных точек HTTPS (также относится к выполняющемуся приложению с помощью отладчика Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="e47b1-139">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
   * <span data-ttu-id="e47b1-140">Только **один порт HTTPS** используется приложением.</span><span class="sxs-lookup"><span data-stu-id="e47b1-140">Only **one HTTPS port** is used by the app.</span></span>

* <span data-ttu-id="e47b1-141">Visual Studio используется:</span><span class="sxs-lookup"><span data-stu-id="e47b1-141">Visual Studio is used:</span></span>
   * <span data-ttu-id="e47b1-142">IIS Express поддерживает протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-142">IIS Express has HTTPS enabled.</span></span>
   * <span data-ttu-id="e47b1-143">*launchSettings.json* задает `sslPort` для IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e47b1-143">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="e47b1-144">При запуске приложения позади обратного прокси-сервера (например, IIS, IIS Express), `IServerAddressesFeature` недоступна.</span><span class="sxs-lookup"><span data-stu-id="e47b1-144">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="e47b1-145">Необходимо вручную настроить порт.</span><span class="sxs-lookup"><span data-stu-id="e47b1-145">The port must be manually configured.</span></span> <span data-ttu-id="e47b1-146">Если порт не задан, не перенаправляет запросы.</span><span class="sxs-lookup"><span data-stu-id="e47b1-146">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="e47b1-147">Порт можно настроить, задав [параметр конфигурации веб-узел https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="e47b1-147">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="e47b1-148">**Ключ**: https_port</span><span class="sxs-lookup"><span data-stu-id="e47b1-148">**Key**: https_port</span></span>  
<span data-ttu-id="e47b1-149">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="e47b1-149">**Type**: *string*</span></span>  
<span data-ttu-id="e47b1-150">**По умолчанию**: не задано значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e47b1-150">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="e47b1-151">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="e47b1-151">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="e47b1-152">**Переменная среды**: `<PREFIX_>HTTPS_PORT` (используется префикс `ASPNETCORE_` при использовании веб-узла.)</span><span class="sxs-lookup"><span data-stu-id="e47b1-152">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="e47b1-153">Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e47b1-153">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="e47b1-154">Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно за HTTPS-порт, через `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="e47b1-154">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="e47b1-155">Если порт не задан:</span><span class="sxs-lookup"><span data-stu-id="e47b1-155">If no port is set:</span></span>

* <span data-ttu-id="e47b1-156">Не перенаправлять запросы.</span><span class="sxs-lookup"><span data-stu-id="e47b1-156">Requests aren't redirected.</span></span>
* <span data-ttu-id="e47b1-157">По промежуточного слоя записывает предупреждение «Не удалось определить https-порт для перенаправления.»</span><span class="sxs-lookup"><span data-stu-id="e47b1-157">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

> [!NOTE]
> <span data-ttu-id="e47b1-158">Альтернативой использованию по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя для переопределения URL-адрес (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="e47b1-158">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="e47b1-159">`AddRedirectToHttps` Можно также задать код состояния и порт при выполнении перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e47b1-159">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="e47b1-160">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e47b1-160">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="e47b1-161">При перенаправлении HTTPS без необходимости для перенаправления дополнительных правил, мы рекомендуем использовать по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) описано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="e47b1-161">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="e47b1-162">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-162">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="e47b1-163">`[RequireHttpsAttribute]` можно снабдить контроллеров или методы, или могут применяться глобально.</span><span class="sxs-lookup"><span data-stu-id="e47b1-163">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="e47b1-164">Чтобы применить атрибут глобально, добавьте следующий код, чтобы `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e47b1-164">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="e47b1-165">Выделенный выше код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="e47b1-165">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="e47b1-166">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e47b1-166">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="e47b1-167">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e47b1-167">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="e47b1-168">По промежуточного слоя также позволяет приложению задать код состояния или код состояния и номер порта, если выполняется перенаправление.</span><span class="sxs-lookup"><span data-stu-id="e47b1-168">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="e47b1-169">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) — это рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="e47b1-169">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="e47b1-170">Применение `[RequireHttps]` атрибута ко всем страницам контроллеров/Razor не считается так безопасен, как требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="e47b1-170">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="e47b1-171">Не может гарантировать `[RequireHttps]` атрибут применяется, когда добавляются новые контроллеры и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e47b1-171">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="e47b1-172">Безопасность строгой транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="e47b1-172">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="e47b1-173">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP строгой безопасности транспорта (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) представляет собой улучшение центра безопасности, который задается параметром веб-приложения при помощи заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="e47b1-173">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="e47b1-174">Когда [браузера, поддерживающего HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) получения этого заголовка:</span><span class="sxs-lookup"><span data-stu-id="e47b1-174">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="e47b1-175">Браузер хранит конфигурацию для домена, который предотвращает отправку любые данные, передаваемые по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="e47b1-175">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="e47b1-176">Браузер заставляет весь обмен данными по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-176">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="e47b1-177">Браузер запрещает пользователю с помощью недействителен или сертификаты.</span><span class="sxs-lookup"><span data-stu-id="e47b1-177">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="e47b1-178">Браузер отключает подсказок, которые позволяют пользователю временно доверять такой сертификат.</span><span class="sxs-lookup"><span data-stu-id="e47b1-178">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="e47b1-179">Поскольку HSTS обеспечивается с помощью клиента он имеет некоторые ограничения:</span><span class="sxs-lookup"><span data-stu-id="e47b1-179">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="e47b1-180">Клиент должен поддерживать HSTS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-180">The client must support HSTS.</span></span>
* <span data-ttu-id="e47b1-181">HSTS требуется по крайней мере один успешный запрос HTTPS для задания политики HSTS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-181">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="e47b1-182">Приложение должно проверить каждый HTTP-запрос и перенаправления или отклонить запрос HTTP.</span><span class="sxs-lookup"><span data-stu-id="e47b1-182">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="e47b1-183">ASP.NET Core 2.1 или более поздних версий реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="e47b1-183">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="e47b1-184">Следующий код вызывает `UseHsts` когда приложение не [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="e47b1-184">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="e47b1-185">`UseHsts` не рекомендуется в разработке, так как параметры HSTS высокой кэшируемых обозревателями.</span><span class="sxs-lookup"><span data-stu-id="e47b1-185">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="e47b1-186">По умолчанию `UseHsts` исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="e47b1-186">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="e47b1-187">Для рабочих сред, реализация HTTPS в первый раз задайте начальное значение HSTS небольшое значение.</span><span class="sxs-lookup"><span data-stu-id="e47b1-187">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="e47b1-188">Задайте значение от часов не больше, чем в один день в случае необходимости использования инфраструктуры HTTPS-HTTP.</span><span class="sxs-lookup"><span data-stu-id="e47b1-188">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="e47b1-189">После вы уверены в устойчивости конфигурации HTTPS, увеличьте значение max-age HSTS; часто используемые значение — один год.</span><span class="sxs-lookup"><span data-stu-id="e47b1-189">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="e47b1-190">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="e47b1-190">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="e47b1-191">Задает параметр предварительной загрузки заголовок Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="e47b1-191">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="e47b1-192">Предварительной загрузки не входит в состав [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления.</span><span class="sxs-lookup"><span data-stu-id="e47b1-192">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="e47b1-193">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="e47b1-193">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="e47b1-194">Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов.</span><span class="sxs-lookup"><span data-stu-id="e47b1-194">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="e47b1-195">Явно задает параметр max-age заголовок Strict-Transport-Security 60 дней.</span><span class="sxs-lookup"><span data-stu-id="e47b1-195">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="e47b1-196">Если не установлено значение по умолчанию — 30 дней.</span><span class="sxs-lookup"><span data-stu-id="e47b1-196">If not set, defaults to 30 days.</span></span> <span data-ttu-id="e47b1-197">См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="e47b1-197">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="e47b1-198">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="e47b1-198">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="e47b1-199">`UseHsts` исключает следующими узлами замыкания на себя:</span><span class="sxs-lookup"><span data-stu-id="e47b1-199">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="e47b1-200">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="e47b1-200">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e47b1-201">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="e47b1-201">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e47b1-202">`[::1]` : IPv6-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="e47b1-202">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="e47b1-203">В предыдущем примере показано, как добавить дополнительные узлы.</span><span class="sxs-lookup"><span data-stu-id="e47b1-203">The preceding example shows how to add additional hosts.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="e47b1-204">Отказаться от HTTPS при создании проекта</span><span class="sxs-lookup"><span data-stu-id="e47b1-204">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="e47b1-205">Включить шаблоны ASP.NET Core 2.1 или более поздней версии веб-приложения (из Visual Studio или командной строки dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e47b1-205">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="e47b1-206">Для развертываний, которые не требуют HTTPS вы можете отказаться от протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e47b1-206">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="e47b1-207">Например некоторые службы серверной части, где HTTPS обрабатывается извне на границе, с помощью протокола HTTPS на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="e47b1-207">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node isn't needed.</span></span>

<span data-ttu-id="e47b1-208">Чтобы отказаться от HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e47b1-208">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e47b1-209">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e47b1-209">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e47b1-210">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="e47b1-210">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e47b1-212">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e47b1-212">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e47b1-213">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="e47b1-213">Use the `--no-https` option.</span></span> <span data-ttu-id="e47b1-214">Пример</span><span class="sxs-lookup"><span data-stu-id="e47b1-214">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="e47b1-215">Как настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="e47b1-215">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="e47b1-216">См. в разделе [проблема GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="e47b1-216">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="e47b1-217">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="e47b1-217">Additional information</span></span>

* [<span data-ttu-id="e47b1-218">Поддержка браузеров OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="e47b1-218">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
