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
ms.openlocfilehash: 69ce182855878e4d05bff95139fefb9e1312f3d5
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252078"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="4e91a-103">Применять HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4e91a-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="4e91a-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="4e91a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4e91a-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="4e91a-105">This document shows how to:</span></span>

* <span data-ttu-id="4e91a-106">Требуйте использования протокола HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="4e91a-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="4e91a-107">Перенаправление всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-107">Redirect all HTTP requests to HTTPS.</span></span>

> [!WARNING]
> <span data-ttu-id="4e91a-108">Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="4e91a-108">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="4e91a-109">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления обозревателей с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-109">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="4e91a-110">Клиенты API не может понять или подчиняются перенаправлений с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-110">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="4e91a-111">Такие клиенты могут отправлять сведения по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="4e91a-111">Such clients may send information over HTTP.</span></span> <span data-ttu-id="4e91a-112">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="4e91a-112">Web APIs should either:</span></span>
>
> * <span data-ttu-id="4e91a-113">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="4e91a-113">Not listen on HTTP.</span></span>
> * <span data-ttu-id="4e91a-114">Закрывает соединение с кодом состояния 400 (неправильный запрос) и обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="4e91a-114">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

<a name="require"></a>
## <a name="require-https"></a><span data-ttu-id="4e91a-115">Требовать использования протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="4e91a-115">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="4e91a-116">Рекомендуется, чтобы все веб-приложения ASP.NET Core вызова HTTPS перенаправление по промежуточного слоя ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) для перенаправления всех запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-116">We recommend all ASP.NET Core web apps call HTTPS Redirection Middleware ([UseHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpspolicybuilderextensions.usehttpsredirection)) to redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="4e91a-117">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="4e91a-117">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

<span data-ttu-id="4e91a-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span><span class="sxs-lookup"><span data-stu-id="4e91a-118">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]</span></span>

<span data-ttu-id="4e91a-119">Следующий код вызывает [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) для настройки параметры по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="4e91a-119">The following code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

<span data-ttu-id="4e91a-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span><span class="sxs-lookup"><span data-stu-id="4e91a-120">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]</span></span>

<span data-ttu-id="4e91a-121">Предыдущий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="4e91a-121">The preceding highlighted code:</span></span>

* <span data-ttu-id="4e91a-122">Наборы [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span><span class="sxs-lookup"><span data-stu-id="4e91a-122">Sets [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode).</span></span>
* <span data-ttu-id="4e91a-123">Задает 5001 HTTPS-порт.</span><span class="sxs-lookup"><span data-stu-id="4e91a-123">Sets the HTTPS port to 5001.</span></span>

<span data-ttu-id="4e91a-124">Следующие механизмы автоматически задать порт:</span><span class="sxs-lookup"><span data-stu-id="4e91a-124">The following mechanisms set the port automatically:</span></span>

* <span data-ttu-id="4e91a-125">По промежуточного слоя может обнаруживать портов через [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) при наличии следующих условий:</span><span class="sxs-lookup"><span data-stu-id="4e91a-125">The middleware can discover the ports via [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature) when the following conditions apply:</span></span>
  - <span data-ttu-id="4e91a-126">Непосредственно с конечные точки HTTPS используется kestrel или HTTP.sys (также применяется для запуска приложения с помощью отладчика Visual Studio Code).</span><span class="sxs-lookup"><span data-stu-id="4e91a-126">Kestrel or HTTP.sys is used directly with HTTPS endpoints (also applies to running the app with Visual Studio Code's debugger).</span></span>
  - <span data-ttu-id="4e91a-127">Только **один порт HTTPS** используется приложением.</span><span class="sxs-lookup"><span data-stu-id="4e91a-127">Only **one HTTPS port** is used by the app.</span></span>
* <span data-ttu-id="4e91a-128">Visual Studio используется:</span><span class="sxs-lookup"><span data-stu-id="4e91a-128">Visual Studio is used:</span></span>
  - <span data-ttu-id="4e91a-129">IIS Express включает взаимодействие по HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-129">IIS Express has HTTPS enabled.</span></span>
  - <span data-ttu-id="4e91a-130">*launchSettings.json* задает `sslPort` для IIS Express.</span><span class="sxs-lookup"><span data-stu-id="4e91a-130">*launchSettings.json* sets the `sslPort` for IIS Express.</span></span>

> [!NOTE]
> <span data-ttu-id="4e91a-131">При запуске приложения за обратного прокси-сервера (например, службы IIS, IIS Express) `IServerAddressesFeature` недоступна.</span><span class="sxs-lookup"><span data-stu-id="4e91a-131">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="4e91a-132">Необходимо вручную настроить порт.</span><span class="sxs-lookup"><span data-stu-id="4e91a-132">The port must be manually configured.</span></span> <span data-ttu-id="4e91a-133">Если порт не настроен, запросы на перенаправление не выполнено.</span><span class="sxs-lookup"><span data-stu-id="4e91a-133">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="4e91a-134">Порт может быть установлен:</span><span class="sxs-lookup"><span data-stu-id="4e91a-134">The port can be configured by setting the:</span></span>

* <span data-ttu-id="4e91a-135">Переменная среды `ASPNETCORE_HTTPS_PORT`.</span><span class="sxs-lookup"><span data-stu-id="4e91a-135">`ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
* <span data-ttu-id="4e91a-136">`http_port` ключ конфигурации узла (например, через *hostsettings.json* или аргумент командной строки).</span><span class="sxs-lookup"><span data-stu-id="4e91a-136">`http_port` host configuration key (for example, via *hostsettings.json* or a command line argument).</span></span>
* <span data-ttu-id="4e91a-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span><span class="sxs-lookup"><span data-stu-id="4e91a-137">[HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport).</span></span> <span data-ttu-id="4e91a-138">См. предыдущий пример, демонстрирующий значение 5001 порта.</span><span class="sxs-lookup"><span data-stu-id="4e91a-138">See the preceding example that shows how to set the port to 5001.</span></span>

> [!NOTE]
> <span data-ttu-id="4e91a-139">Порт можно настроить, задав URL-адрес с косвенно `ASPNETCORE_URLS` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="4e91a-139">The port can be configured indirectly by setting the URL with the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="4e91a-140">Настраивает сервер в переменной среды, а затем по промежуточного слоя косвенно обнаруживает порт HTTPS через `IServerAddressesFeature`.</span><span class="sxs-lookup"><span data-stu-id="4e91a-140">The environment variable configures the server, and then the middleware indirectly discovers the HTTPS port via `IServerAddressesFeature`.</span></span>

<span data-ttu-id="4e91a-141">Если порт не задан:</span><span class="sxs-lookup"><span data-stu-id="4e91a-141">If no port is set:</span></span>

* <span data-ttu-id="4e91a-142">Запросы на перенаправление не выполнено.</span><span class="sxs-lookup"><span data-stu-id="4e91a-142">Requests aren't redirected.</span></span>
* <span data-ttu-id="4e91a-143">По промежуточного слоя заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="4e91a-143">The middleware logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="4e91a-144">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-144">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="4e91a-145">`[RequireHttpsAttribute]` можно дополнить контроллеров или методы или могут быть применены глобально.</span><span class="sxs-lookup"><span data-stu-id="4e91a-145">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="4e91a-146">Чтобы применить атрибут глобально, добавьте следующий код в `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="4e91a-146">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

<span data-ttu-id="4e91a-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span><span class="sxs-lookup"><span data-stu-id="4e91a-147">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]</span></span>

<span data-ttu-id="4e91a-148">Предыдущий выделенный код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="4e91a-148">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="4e91a-149">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4e91a-149">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

<span data-ttu-id="4e91a-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span><span class="sxs-lookup"><span data-stu-id="4e91a-150">[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]</span></span>

<span data-ttu-id="4e91a-151">Дополнительные сведения см. в разделе [по промежуточного слоя перезаписи URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="4e91a-151">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="4e91a-152">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) является рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="4e91a-152">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="4e91a-153">Применение `[RequireHttps]` атрибут ко всем страницам контроллеры и Razor не считается защищено настолько, насколько требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="4e91a-153">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="4e91a-154">Не может гарантировать `[RequireHttps]` атрибут при добавлении новых контроллеров и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="4e91a-154">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="hsts"></a>
## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="4e91a-155">Безопасность Strict транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="4e91a-155">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="4e91a-156">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [строгой безопасности транспорта (HSTS) HTTP](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) — расширение согласиться на использование безопасности, определяемый веб-приложения с помощью специальных заголовка.</span><span class="sxs-lookup"><span data-stu-id="4e91a-156">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that is specified by a web application through the use of a special response header.</span></span> <span data-ttu-id="4e91a-157">Когда этот заголовок получает поддерживаемого браузера браузера позволит обмен данными с передачей по протоколу HTTP к указанному домену и вместо этого будет отправлять все связи по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-157">Once a supported browser receives this header that browser will prevent any communications from being sent over HTTP to the specified domain and will instead send all communications over HTTPS.</span></span> <span data-ttu-id="4e91a-158">Он также препятствует щелкните HTTPS через запросы в браузерах.</span><span class="sxs-lookup"><span data-stu-id="4e91a-158">It also prevents HTTPS click through prompts on browsers.</span></span>

<span data-ttu-id="4e91a-159">ASP.NET Core 2.1 или более поздней реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="4e91a-159">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="4e91a-160">Следующий код вызывает `UseHsts` при это приложение не в [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="4e91a-160">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

<span data-ttu-id="4e91a-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span><span class="sxs-lookup"><span data-stu-id="4e91a-161">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]</span></span>

<span data-ttu-id="4e91a-162">`UseHsts` не рекомендуется при разработке решений, так как заголовок HSTS высокой может быть кэширован в браузерах.</span><span class="sxs-lookup"><span data-stu-id="4e91a-162">`UseHsts` is not recommend in development because the HSTS header is highly cachable by browsers.</span></span> <span data-ttu-id="4e91a-163">По умолчанию UseHsts исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="4e91a-163">By default, UseHsts excludes the local loopback address.</span></span>

<span data-ttu-id="4e91a-164">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="4e91a-164">The following code:</span></span>

<span data-ttu-id="4e91a-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span><span class="sxs-lookup"><span data-stu-id="4e91a-165">[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]</span></span>

* <span data-ttu-id="4e91a-166">Задает параметр предварительной загрузки заголовка безопасности Strict-транспорта.</span><span class="sxs-lookup"><span data-stu-id="4e91a-166">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="4e91a-167">Предварительная загрузка не частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается для веб-браузера для предварительной загрузки HSTS узлов на установку.</span><span class="sxs-lookup"><span data-stu-id="4e91a-167">Preload is not part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="4e91a-168">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="4e91a-168">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="4e91a-169">Включает [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS дочерних узлов.</span><span class="sxs-lookup"><span data-stu-id="4e91a-169">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span> 
* <span data-ttu-id="4e91a-170">Явно задает для параметра max-age заголовка безопасности для транспорта Strict до 60 дней.</span><span class="sxs-lookup"><span data-stu-id="4e91a-170">Explicitly sets the max-age parameter of the Strict-Transport-Security header to to 60 days.</span></span> <span data-ttu-id="4e91a-171">Если свойство не задано, по умолчанию используется значение 30 дней.</span><span class="sxs-lookup"><span data-stu-id="4e91a-171">If not set, defaults to 30 days.</span></span> <span data-ttu-id="4e91a-172">В разделе [директива max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="4e91a-172">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="4e91a-173">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="4e91a-173">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="4e91a-174">`UseHsts` исключает замыкания на себя следующие узлы:</span><span class="sxs-lookup"><span data-stu-id="4e91a-174">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="4e91a-175">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="4e91a-175">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="4e91a-176">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="4e91a-176">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="4e91a-177">`[::1]` : Петлевой адрес IPv6.</span><span class="sxs-lookup"><span data-stu-id="4e91a-177">`[::1]` : The IPv6 loopback address.</span></span>

<span data-ttu-id="4e91a-178">Предыдущий пример демонстрирует добавление дополнительных узлов.</span><span class="sxs-lookup"><span data-stu-id="4e91a-178">The preceding example shows how to add additional hosts.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<a name="https"></a>
## <a name="opt-out-of-https-on-project-creation"></a><span data-ttu-id="4e91a-179">Отказаться от HTTPS на создания проекта</span><span class="sxs-lookup"><span data-stu-id="4e91a-179">Opt-out of HTTPS on project creation</span></span>

<span data-ttu-id="4e91a-180">Включить шаблоны 2.1 или более поздней версии веб-приложений ASP.NET Core (из Visual Studio или в командной строке dotnet) [перенаправления HTTPS](#require) и [HSTS](#hsts).</span><span class="sxs-lookup"><span data-stu-id="4e91a-180">The ASP.NET Core 2.1 or later web application templates (from Visual Studio or the dotnet command line) enable [HTTPS redirection](#require) and [HSTS](#hsts).</span></span> <span data-ttu-id="4e91a-181">Для развертываний, не требующие HTTPS вы можете отказаться от HTTPS.</span><span class="sxs-lookup"><span data-stu-id="4e91a-181">For deployments that don't require HTTPS, you can opt-out of HTTPS.</span></span> <span data-ttu-id="4e91a-182">Например некоторые серверных служб, где HTTPS обрабатывается извне на границе с помощью протокола HTTPS на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="4e91a-182">For example, some backend services where HTTPS is being handled externally at the edge, using HTTPS at each node is not needed.</span></span>

<span data-ttu-id="4e91a-183">Чтобы отказаться от HTTPS:</span><span class="sxs-lookup"><span data-stu-id="4e91a-183">To opt-out of HTTPS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4e91a-184">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4e91a-184">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="4e91a-185">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="4e91a-185">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Схема сущностей](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="4e91a-187">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="4e91a-187">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="4e91a-188">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="4e91a-188">Use the `--no-https` option.</span></span> <span data-ttu-id="4e91a-189">Пример</span><span class="sxs-lookup"><span data-stu-id="4e91a-189">For example</span></span>

```console
dotnet new webapp --no-https
```

[!INCLUDE[](~/includes/webapp-alias-notice.md)]

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="how-to-setup-a-developer-certificate-for-docker"></a><span data-ttu-id="4e91a-191">Как можно настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="4e91a-191">How to setup a developer certificate for Docker</span></span>

<span data-ttu-id="4e91a-192">В разделе [этой проблемы GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="4e91a-192">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end
