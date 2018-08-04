---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требовать HTTPS/TLS, в ASP.NET Core веб-приложения.
ms.author: riande
ms.date: 2/9/2018
uid: security/enforcing-ssl
ms.openlocfilehash: d8bf11d7d2df8d8b197f001570a8fab1f3262814
ms.sourcegitcommit: 4e34ce61e1e7f1317102b16012ce0742abf2cca6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/04/2018
ms.locfileid: "39514808"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="e8232-103">Принудительное использование HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8232-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="e8232-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e8232-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e8232-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="e8232-105">This document shows how to:</span></span>

* <span data-ttu-id="e8232-106">Требование HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="e8232-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="e8232-107">Перенаправлять все запросы HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="e8232-108">Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получения конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="e8232-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="e8232-109">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления браузеров с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="e8232-110">Клиенты API не может понять и подчиняются перенаправление с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="e8232-111">Такие клиенты могут отправлять данные по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="e8232-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="e8232-112">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="e8232-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="e8232-113">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="e8232-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="e8232-114">Закрыть соединение с кодом состояния 400 (неправильный запрос) и не обработать запрос.</span><span class="sxs-lookup"><span data-stu-id="e8232-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="e8232-115">Требование HTTPS</span><span class="sxs-lookup"><span data-stu-id="e8232-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="e8232-116">Рекомендуется, чтобы все веб-приложения ASP.NET Core вызвать по промежуточного слоя переадресации HTTPS ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="e8232-117">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="e8232-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="e8232-118">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="e8232-118">The preceding highlighted code:</span></span>

* <span data-ttu-id="e8232-119">Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span><span class="sxs-lookup"><span data-stu-id="e8232-119">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) (`Status307TemporaryRedirect`).</span></span> <span data-ttu-id="e8232-120">Рабочие приложения должны вызывать [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e8232-120">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="e8232-121">Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span><span class="sxs-lookup"><span data-stu-id="e8232-121">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (443).</span></span>

<span data-ttu-id="e8232-122">Следующий код вызывает [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="e8232-122">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="e8232-123">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="e8232-123">The preceding highlighted code:</span></span>

* <span data-ttu-id="e8232-124">Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) для `Status307TemporaryRedirect`, который является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e8232-124">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="e8232-125">Рабочие приложения должны вызывать [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e8232-125">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="e8232-126">Задать HTTPS-порт на 5001.</span><span class="sxs-lookup"><span data-stu-id="e8232-126">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="e8232-127">Значение по умолчанию — 443.</span><span class="sxs-lookup"><span data-stu-id="e8232-127">The default value is 443.</span></span>

<span data-ttu-id="e8232-128">Следующие механизмы автоматически задать порт:</span><span class="sxs-lookup"><span data-stu-id="e8232-128">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="e8232-129">По промежуточного слоя можно обнаружить порты, через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) когда применяются следующие условия:</span><span class="sxs-lookup"><span data-stu-id="e8232-129">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="e8232-130">Kestrel и HTTP.sys используется непосредственно с помощью конечных точек HTTPS (также относится к выполняющемуся приложению с помощью отладчика Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="e8232-130">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="e8232-131">Только **один порт HTTPS** используется приложением.</span><span class="sxs-lookup"><span data-stu-id="e8232-131">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="e8232-132">Visual Studio используется:</span><span class="sxs-lookup"><span data-stu-id="e8232-132">Visual Studio is used:</span></span>
  - <span data-ttu-id="e8232-133">IIS Express поддерживает протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-133">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="e8232-134">*launchSettings.json* задает `sslPort` для IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e8232-134">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="e8232-135">При запуске приложения позади обратного прокси-сервера (например, IIS, IIS Express), `IServerAddressesFeature` недоступна.</span><span class="sxs-lookup"><span data-stu-id="e8232-135">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="e8232-136">Необходимо вручную настроить порт.</span><span class="sxs-lookup"><span data-stu-id="e8232-136">The port must be manually configured.</span></span> <span data-ttu-id="e8232-137">Если порт не задан, не перенаправляет запросы.</span><span class="sxs-lookup"><span data-stu-id="e8232-137">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="e8232-138">Порт можно настроить, задав [параметр конфигурации веб-узел https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="e8232-138">The port can be configured by setting the [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

<span data-ttu-id="e8232-139">**Ключ**: https_port **тип**: *строка*
**по умолчанию**: не задано значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e8232-139">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="e8232-140">**Задать с помощью**: `UseSetting` 
 **переменной среды**: `<PREFIX_>HTTPS_PORT` (используется префикс `ASPNETCORE_` при использовании веб-узла.)</span><span class="sxs-lookup"><span data-stu-id="e8232-140">**Set using**: `UseSetting`
**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the Web Host.)</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

> [!NOTE]
> <span data-ttu-id="e8232-141">Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e8232-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="e8232-142">Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно за HTTPS-порт, через `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="e8232-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="e8232-143">Если порт не задан:</span><span class="sxs-lookup"><span data-stu-id="e8232-143">If no port is set:</span></span>

* <span data-ttu-id="e8232-144">Не перенаправлять запросы.</span><span class="sxs-lookup"><span data-stu-id="e8232-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="e8232-145">По промежуточного слоя заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="e8232-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="e8232-146">Альтернативой использованию по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя для переопределения URL-адрес (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="e8232-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="e8232-147">`AddRedirectToHttps` Можно также задать код состояния и порт при выполнении перенаправления.</span><span class="sxs-lookup"><span data-stu-id="e8232-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="e8232-148">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e8232-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="e8232-149">При перенаправлении HTTPS без необходимости для перенаправления дополнительных правил, мы рекомендуем использовать по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) описано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="e8232-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="e8232-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="e8232-151">`[RequireHttpsAttribute]` можно снабдить контроллеров или методы, или могут применяться глобально.</span><span class="sxs-lookup"><span data-stu-id="e8232-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="e8232-152">Чтобы применить атрибут глобально, добавьте следующий код, чтобы `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e8232-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="e8232-153">Выделенный выше код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="e8232-153">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="e8232-154">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e8232-154">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="e8232-155">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="e8232-155">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="e8232-156">По промежуточного слоя также позволяет приложению задать код состояния или код состояния и номер порта, если выполняется перенаправление.</span><span class="sxs-lookup"><span data-stu-id="e8232-156">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="e8232-157">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) — это рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="e8232-157">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="e8232-158">Применение `[RequireHttps]` атрибута ко всем страницам контроллеров/Razor не считается так безопасен, как требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="e8232-158">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="e8232-159">Не может гарантировать `[RequireHttps]` атрибут применяется, когда добавляются новые контроллеры и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="e8232-159">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="e8232-160">Безопасность строгой транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="e8232-160">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="e8232-161">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP строгой безопасности транспорта (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) представляет собой улучшение центра безопасности, который задается параметром веб-приложения при помощи специального заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="e8232-161">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a special response header.</span></span> <span data-ttu-id="e8232-162">Когда браузер, поддерживающий HSTS получает этот заголовок, он сохраняет конфигурацию для домена, который предотвращает отправку любые данные, передаваемые по протоколу HTTP и вместо этого принудительно весь обмен данными по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-162">When a browser that supports HSTS receives this header, it stores configuration for the domain that prevents sending any communication over HTTP and instead forces all communication over HTTPS.</span></span> <span data-ttu-id="e8232-163">Он также не позволяет пользователю с помощью недействителен или сертификаты, отключение браузера подсказок, которые позволяют пользователю временно доверять такой сертификат.</span><span class="sxs-lookup"><span data-stu-id="e8232-163">It also prevents the user from using untrusted or invalid certificates, disabling the browser prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="e8232-164">ASP.NET Core 2.1 или более поздних версий реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="e8232-164">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="e8232-165">Следующий код вызывает `UseHsts` когда приложение не [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="e8232-165">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="e8232-166">`UseHsts` не следует использовать в разработки так, как заголовок HSTS высокой кэширован в браузерах.</span><span class="sxs-lookup"><span data-stu-id="e8232-166">`UseHsts` isn't recommended in development because the HSTS header is highly cacheable by browsers.</span></span> <span data-ttu-id="e8232-167">По умолчанию `UseHsts` исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="e8232-167">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="e8232-168">Для рабочих сред, реализация HTTPS в первый раз задайте начальное значение HSTS небольшое значение.</span><span class="sxs-lookup"><span data-stu-id="e8232-168">For production environments implementing HTTPS for the first time, set the initial HSTS value to a small value.</span></span> <span data-ttu-id="e8232-169">Задайте значение от часов не больше, чем в один день в случае необходимости использования инфраструктуры HTTPS-HTTP.</span><span class="sxs-lookup"><span data-stu-id="e8232-169">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="e8232-170">После вы уверены в устойчивости конфигурации HTTPS, увеличьте значение max-age HSTS; часто используемые значение — один год.</span><span class="sxs-lookup"><span data-stu-id="e8232-170">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span> 

<span data-ttu-id="e8232-171">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="e8232-171">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="e8232-172">Задает параметр предварительной загрузки заголовок Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="e8232-172">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="e8232-173">Предварительная загрузка не является частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления.</span><span class="sxs-lookup"><span data-stu-id="e8232-173">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="e8232-174">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="e8232-174">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="e8232-175">Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов.</span><span class="sxs-lookup"><span data-stu-id="e8232-175">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="e8232-176">Явно задает параметр max-age заголовок Strict-Transport-Security, чтобы до 60 дней.</span><span class="sxs-lookup"><span data-stu-id="e8232-176">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="e8232-177">Если не установлено значение по умолчанию — 30 дней.</span><span class="sxs-lookup"><span data-stu-id="e8232-177">If not set, defaults to 30 days.</span></span> <span data-ttu-id="e8232-178">См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="e8232-178">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="e8232-179">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="e8232-179">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="e8232-180">`UseHsts` исключает следующими узлами замыкания на себя:</span><span class="sxs-lookup"><span data-stu-id="e8232-180">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="e8232-181">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="e8232-181">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e8232-182">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="e8232-182">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="e8232-183">`[::1]` : IPv6-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="e8232-183">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="e8232-184">В предыдущем примере показано, как добавить дополнительные узлы.</span><span class="sxs-lookup"><span data-stu-id="e8232-184">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="e8232-185">Отказаться от HTTPS при создании проекта</span><span class="sxs-lookup"><span data-stu-id="e8232-185">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="e8232-186">Включить шаблоны ASP.NET Core 2.1 или более поздней версии веб-приложения (из Visual Studio или командной строки dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="e8232-186">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="e8232-187">Для развертываний, которые не требуют HTTPS вы можете отказаться от протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="e8232-187">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="e8232-188">Например некоторые службы серверной части, где HTTPS обрабатывается извне на границе, с помощью протокола HTTPS на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="e8232-188">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="e8232-189">Чтобы отказаться от HTTPS:</span><span class="sxs-lookup"><span data-stu-id="e8232-189">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e8232-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e8232-190">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="e8232-191">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="e8232-191">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="e8232-193">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="e8232-193">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="e8232-194">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="e8232-194">Use the `--no-https` option.</span></span> <span data-ttu-id="e8232-195">Пример</span><span class="sxs-lookup"><span data-stu-id="e8232-195">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="e8232-196">Как настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="e8232-196">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="e8232-197">См. в разделе [проблема GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="e8232-197">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
