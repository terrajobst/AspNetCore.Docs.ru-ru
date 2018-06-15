---
title: Применять HTTPS в ASP.NET Core
author: rick-anderson
description: Показано, как требуется HTTPS/TLS в ASP.NET Core веб-приложения.
manager: wpickett
ms.author: riande
ms.date: 2/9/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/enforcing-ssl
ms.openlocfilehash: 48a25b7ba7affe84cfa6fe16096409239c510221
ms.sourcegitcommit: 40b102ecf88e53d9d872603ce6f3f7044bca95ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/15/2018
ms.locfileid: "35652192"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="b0494-103">Применять HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b0494-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="b0494-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="b0494-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b0494-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="b0494-105">This document shows how to:</span></span>

* <span data-ttu-id="b0494-106">Требуйте использования протокола HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="b0494-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="b0494-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="b0494-108">Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="b0494-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="b0494-109">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="b0494-110">Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="b0494-111">Такие клиенты могут отправлять сведения по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0494-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="b0494-112">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="b0494-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="b0494-113">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="b0494-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="b0494-114">Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="b0494-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="b0494-115">Требовать использования протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="b0494-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="b0494-116">Рекомендуется, чтобы все веб-приложения ASP.NET Core вызова HTTPS перенаправление по промежуточного слоя ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="b0494-117">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="b0494-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="b0494-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="b0494-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="b0494-119">Следующий код вызывает [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) для настройки параметры по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="b0494-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="b0494-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="b0494-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="b0494-121">Предыдущий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="b0494-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="b0494-122">Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) для `Status307TemporaryRedirect`, который является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="b0494-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) to `Status307TemporaryRedirect`, which is the default value.</span></span> <span data-ttu-id="b0494-123">Приложения в рабочей среде следует вызывать [UseHsts](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b0494-123">Production apps should call [UseHsts](#hsts).</span></span>
* <span data-ttu-id="b0494-124">Задает 5001 HTTPS-порт.</span><span class="sxs-lookup"><span data-stu-id="b0494-124">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="b0494-125">Значение по умолчанию — 443.</span><span class="sxs-lookup"><span data-stu-id="b0494-125">The default value is 443.</span></span>

<span data-ttu-id="b0494-126">Следующие механизмы автоматически задать порт:</span><span class="sxs-lookup"><span data-stu-id="b0494-126">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="b0494-127">По промежуточного слоя может обнаруживать портов через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) при наличии следующих условий:</span><span class="sxs-lookup"><span data-stu-id="b0494-127">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="b0494-128">Непосредственно с конечные точки HTTPS используется kestrel или HTTP.sys (также применяется для запуска приложения с помощью отладчика Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="b0494-128">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="b0494-129">Только **один порт HTTPS** используется приложением.</span><span class="sxs-lookup"><span data-stu-id="b0494-129">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="b0494-130">Visual Studio используется:</span><span class="sxs-lookup"><span data-stu-id="b0494-130">Visual Studio is used:</span></span>
  - <span data-ttu-id="b0494-131">IIS Express включает взаимодействие по HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-131">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="b0494-132">*launchSettings.json* задает `sslPort` для IIS Express.</span><span class="sxs-lookup"><span data-stu-id="b0494-132">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="b0494-133">При запуске приложения за обратного прокси-сервера (например, службы IIS, IIS Express) `IServerAddressesFeature` недоступна.</span><span class="sxs-lookup"><span data-stu-id="b0494-133">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="b0494-134">Необходимо вручную настроить порт.</span><span class="sxs-lookup"><span data-stu-id="b0494-134">The port must be manually configured.</span></span> <span data-ttu-id="b0494-135">Если порт не настроен, запросы на перенаправление не выполнено.</span><span class="sxs-lookup"><span data-stu-id="b0494-135">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="b0494-136">Порт может быть установлен:</span><span class="sxs-lookup"><span data-stu-id="b0494-136">The port can be configured by setting the:</span></span>

* <span data-ttu-id="b0494-137">Переменная среды `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="b0494-137">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="b0494-138">`http_port` ключ конфигурации узла (например, через *hostsettings.json* или аргумент командной строки).</span><span class="sxs-lookup"><span data-stu-id="b0494-138">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="b0494-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="b0494-139">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="b0494-140">См. предыдущий пример, демонстрирующий значение 5001 порта.</span><span class="sxs-lookup"><span data-stu-id="b0494-140">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="b0494-141">Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="b0494-141">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="b0494-142">Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно обнаруживает порт HTTPS через `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="b0494-142">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="b0494-143">Если порт не задан:</span><span class="sxs-lookup"><span data-stu-id="b0494-143">If no port is set:</span></span>

* <span data-ttu-id="b0494-144">Запросы на перенаправление не выполнено.</span><span class="sxs-lookup"><span data-stu-id="b0494-144">Requests aren't redirected.</span></span>
* <span data-ttu-id="b0494-145">По промежуточного слоя заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="b0494-145">The middleware logs a warning.</span></span>

> [!NOTE]
> <span data-ttu-id="b0494-146">Вместо использования по промежуточного слоя перенаправления HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя перезаписи URL-адрес (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="b0494-146">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="b0494-147">`AddRedirectToHttps` Можно также задать код состояния и порта при выполнении перенаправления.</span><span class="sxs-lookup"><span data-stu-id="b0494-147">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="b0494-148">Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b0494-148">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>
>
> <span data-ttu-id="b0494-149">При перенаправлении HTTPS не требует дополнительных перенаправления правила, рекомендуется использовать HTTPS перенаправление по промежуточного слоя (`UseHttpsRedirection`) описанные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b0494-149">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="b0494-150">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-150">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="b0494-151">`[RequireHttpsAttribute]` можно дополнить контроллеров или методы или могут быть применены глобально.</span><span class="sxs-lookup"><span data-stu-id="b0494-151">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="b0494-152">Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="b0494-152">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="b0494-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="b0494-153">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="b0494-154">Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="b0494-154">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="b0494-155">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b0494-155">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="b0494-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="b0494-156">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="b0494-157">Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="b0494-157">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="b0494-158">По промежуточного слоя также позволяет приложению задать код состояния или код состояния и порт, когда выполняется перенаправление.</span><span class="sxs-lookup"><span data-stu-id="b0494-158">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="b0494-159">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="b0494-159">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="b0494-160">Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="b0494-160">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="b0494-161">Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="b0494-161">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="b0494-162">Безопасность Strict транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="b0494-162">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="b0494-163">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [строгой безопасности транспорта (HSTS) HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) — расширение согласиться на использование безопасности, определяемый веб-приложения с помощью специальных заголовка.</span><span class="sxs-lookup"><span data-stu-id="b0494-163">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="b0494-164">Когда этот заголовок получает поддерживаемого браузера браузера позволит обмен данными с передачей по протоколу HTTP к указанному домену и вместо этого будет отправлять все связи по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-164">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="b0494-165">Он также препятствует щелкните HTTPS через запросы в браузерах.</span><span class="sxs-lookup"><span data-stu-id="b0494-165">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="b0494-166">ASP.NET Core 2.1 или более поздней реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="b0494-166">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="b0494-167">Следующий код вызывает `UseHsts` при это приложение не в [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="b0494-167">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="b0494-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="b0494-168">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="b0494-169">`UseHsts` не рекомендуется при разработке решений, так как заголовок HSTS высокой может быть кэширован в браузерах.</span><span class="sxs-lookup"><span data-stu-id="b0494-169">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="b0494-170">По умолчанию UseHsts исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="b0494-170">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="b0494-171">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="b0494-171">The following code:</span></span>

<span data-ttu-id="b0494-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="b0494-172">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="b0494-173">Задает параметр предварительной загрузки заголовка безопасности Strict-транспорта.</span><span class="sxs-lookup"><span data-stu-id="b0494-173">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="b0494-174">Предварительная загрузка не частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается для веб-браузера для предварительной загрузки HSTS узлов на установку.</span><span class="sxs-lookup"><span data-stu-id="b0494-174">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="b0494-175">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="b0494-175">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="b0494-176">Включает [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS дочерних узлов.</span><span class="sxs-lookup"><span data-stu-id="b0494-176">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="b0494-177">Явно задает для параметра max-age заголовка безопасности для транспорта Strict до 60 дней.</span><span class="sxs-lookup"><span data-stu-id="b0494-177">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="b0494-178">Если свойство не задано, по умолчанию используется значение 30 дней.</span><span class="sxs-lookup"><span data-stu-id="b0494-178">If not set, defaults to 30 days.</span></span> <span data-ttu-id="b0494-179">В разделе [директива max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="b0494-179">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="b0494-180">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="b0494-180">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="b0494-181">`UseHsts` исключает замыкания на себя следующие узлы:</span><span class="sxs-lookup"><span data-stu-id="b0494-181">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="b0494-182">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="b0494-182">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b0494-183">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="b0494-183">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="b0494-184">`[::1]` : Петлевой адрес IPv6.</span><span class="sxs-lookup"><span data-stu-id="b0494-184">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="b0494-185">Предыдущий пример демонстрирует добавление дополнительных узлов.</span><span class="sxs-lookup"><span data-stu-id="b0494-185">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="b0494-186">Отказаться от HTTPS на создания проекта</span><span class="sxs-lookup"><span data-stu-id="b0494-186">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="b0494-187">Включить шаблоны 2.1 или более поздней версии веб-приложений ASP.NET Core (из Visual Studio или в командной строке dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="b0494-187">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="b0494-188">Для развертываний, не требующие HTTPS вы можете отказаться от HTTPS.</span><span class="sxs-lookup"><span data-stu-id="b0494-188">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="b0494-189">Например некоторые серверных служб, где HTTPS обрабатывается извне на границе с помощью протокола HTTPS на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="b0494-189">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="b0494-190">Чтобы отказаться от HTTPS:</span><span class="sxs-lookup"><span data-stu-id="b0494-190">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="b0494-191">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="b0494-191">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="b0494-192">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="b0494-192">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="b0494-194">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="b0494-194">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="b0494-195">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="b0494-195">Use the `--no-https` option.</span></span> <span data-ttu-id="b0494-196">Пример</span><span class="sxs-lookup"><span data-stu-id="b0494-196">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="b0494-198">Как можно настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="b0494-198">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="b0494-199">В разделе [этой проблемы GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="b0494-199">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
