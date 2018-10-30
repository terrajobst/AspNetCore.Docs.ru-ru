---
title: Принудительное использование HTTPS в ASP.NET Core
author: rick-anderson
description: Узнайте, как требовать HTTPS/TLS в веб-приложения ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: security/enforcing-ssl
ms.openlocfilehash: afa40db4c84820db91878bb98dae082b3dd9a2e2
ms.sourcegitcommit: c43a6f1fe72d7c2db4b5815fd532f2b45d964e07
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2018
ms.locfileid: "50244892"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="7ccc2-103">Принудительное использование HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7ccc2-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="7ccc2-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="7ccc2-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="7ccc2-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-105">This document shows how to:</span></span>

* <span data-ttu-id="7ccc2-106">Требование HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="7ccc2-107">Перенаправлять все запросы HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="7ccc2-108">Интерфейс API может препятствовать передачи конфиденциальных данных при первом запросе клиента.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

> [!WARNING]
> <span data-ttu-id="7ccc2-109">Сделать **не** использовать [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) на веб-API, получения конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-109">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="7ccc2-110">`RequireHttpsAttribute` использует коды состояния HTTP для перенаправления браузеров с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-110">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="7ccc2-111">Клиенты API не может понять и подчиняются перенаправление с HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-111">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="7ccc2-112">Такие клиенты могут отправлять данные по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-112">Such clients may send information over HTTP.</span></span> <span data-ttu-id="7ccc2-113">Веб-API должен:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-113">Web APIs should either:</span></span>
>
> * <span data-ttu-id="7ccc2-114">Прослушивает HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-114">Not listen on HTTP.</span></span>
> * <span data-ttu-id="7ccc2-115">Закрыть соединение с кодом состояния 400 (неправильный запрос) и не обработать запрос.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-115">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

## <a name="require-https"></a><span data-ttu-id="7ccc2-116">Требование HTTPS</span><span class="sxs-lookup"><span data-stu-id="7ccc2-116">Require HTTPS</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="7ccc2-117">Мы рекомендуем производственные ASP.NET Core, веб-приложений вызова:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-117">We recommend that production ASP.NET Core web apps call:</span></span>

* <span data-ttu-id="7ccc2-118">По промежуточного слоя переадресации HTTPS (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) для перенаправления запросов HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-118">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="7ccc2-119">По промежуточного слоя HSTS ([UseHsts](#http-strict-transport-security-protocol-hsts)) для отправки заголовков HTTP Strict транспорта безопасности протокола (HSTS) клиентам.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-119">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="7ccc2-120">Приложения, развернутые в конфигурации обратного прокси-сервера разрешить прокси-сервер для обработки безопасность подключения (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-120">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="7ccc2-121">Если прокси-сервер также обрабатывает перенаправления HTTPS, нет необходимости использовать по промежуточного слоя переадресации HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-121">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="7ccc2-122">Если прокси-сервера также обрабатывает операции записи HSTS заголовков (например, [собственного HSTS поддержки в IIS 10.0 (1709) или более поздней версии](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), по промежуточного слоя HSTS не требуется приложением.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-122">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="7ccc2-123">Дополнительные сведения см. в разделе [выхода из HTTPS/HSTS при создании проекта](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-123">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="7ccc2-124">UseHttpsRedirection</span><span class="sxs-lookup"><span data-stu-id="7ccc2-124">UseHttpsRedirection</span></span>

<span data-ttu-id="7ccc2-125">Следующий код вызывает `UseHttpsRedirection` в `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-125">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=13)]

<span data-ttu-id="7ccc2-126">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-126">The preceding highlighted code:</span></span>

* <span data-ttu-id="7ccc2-127">Использует значение по умолчанию [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-127">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="7ccc2-128">Использует значение по умолчанию [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если не переопределено `ASPNETCORE_HTTPS_PORT` переменной среды или [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-128">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="7ccc2-129">Мы рекомендуем использовать временный перенаправления, а не постоянной переадресации.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-129">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="7ccc2-130">Кэширование ссылок может привести к нестабильной работе поведению в средах разработки.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-130">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="7ccc2-131">Если вы хотите отправить код состояния постоянное перенаправление, во время работы приложения в среде не разработки, см. в разделе [Настройка постоянной переадресации в рабочей среде](#configure-permanent-redirects-in-production) раздел.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-131">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="7ccc2-132">Мы рекомендуем использовать [HSTS](#http-strict-transport-security-protocol-hsts) сигнал для клиентов, которые только защитить ресурс запросы следует отправлять в приложение (только в рабочей среде).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-132">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="7ccc2-133">Конфигурация порта</span><span class="sxs-lookup"><span data-stu-id="7ccc2-133">Port configuration</span></span>

<span data-ttu-id="7ccc2-134">Порт должен быть доступен для по промежуточного слоя для перенаправления небезопасный запрос HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-134">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="7ccc2-135">Если порт не доступен:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-135">If no port is available:</span></span>

* <span data-ttu-id="7ccc2-136">Не произойдет перенаправление на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-136">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="7ccc2-137">По промежуточного слоя записывает предупреждение «Не удалось определить https-порт для перенаправления.»</span><span class="sxs-lookup"><span data-stu-id="7ccc2-137">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="7ccc2-138">Укажите порт HTTPS, с помощью любого из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-138">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="7ccc2-139">Задайте [HttpsRedirectionOptions.HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-139">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>
* <span data-ttu-id="7ccc2-140">Задайте `ASPNETCORE_HTTPS_PORT` переменной среды или [параметр конфигурации веб-узел https_port](xref:fundamentals/host/web-host#https-port):</span><span class="sxs-lookup"><span data-stu-id="7ccc2-140">Set the `ASPNETCORE_HTTPS_PORT` environment variable or [https_port Web Host configuration setting](xref:fundamentals/host/web-host#https-port):</span></span>

  <span data-ttu-id="7ccc2-141">**Ключ**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="7ccc2-141">**Key**: `https_port`</span></span>  
  <span data-ttu-id="7ccc2-142">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7ccc2-142">**Type**: *string*</span></span>  
  <span data-ttu-id="7ccc2-143">**По умолчанию**: не задано значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-143">**Default**: A default value isn't set.</span></span>  
  <span data-ttu-id="7ccc2-144">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="7ccc2-144">**Set using**: `UseSetting`</span></span>  
  <span data-ttu-id="7ccc2-145">**Переменная среды**: `<PREFIX_>HTTPS_PORT` (используется префикс `ASPNETCORE_` при использовании [веб-узел](xref:fundamentals/host/web-host).)</span><span class="sxs-lookup"><span data-stu-id="7ccc2-145">**Environment variable**: `<PREFIX_>HTTPS_PORT` (The prefix is `ASPNETCORE_` when using the [Web Host](xref:fundamentals/host/web-host).)</span></span>

  <span data-ttu-id="7ccc2-146">При настройке <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> в `Program`:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-146">When configuring an <xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> in `Program`:</span></span>

  [!code-csharp[](enforcing-ssl/sample-snapshot/Program.cs?name=snippet_Program&highlight=10)]
* <span data-ttu-id="7ccc2-147">Указать порт с безопасной схемы с помощью `ASPNETCORE_URLS` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-147">Indicate a port with the secure scheme using the `ASPNETCORE_URLS` environment variable.</span></span> <span data-ttu-id="7ccc2-148">Переменная среды настраивает сервер.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-148">The environment variable configures the server.</span></span> <span data-ttu-id="7ccc2-149">По промежуточного слоя косвенно обнаруживает HTTPS-порт, через <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-149">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="7ccc2-150">(Does **не** работают в развертываниях обратного прокси-сервера.)</span><span class="sxs-lookup"><span data-stu-id="7ccc2-150">(Does **not** work in reverse proxy deployments.)</span></span>
* <span data-ttu-id="7ccc2-151">В разработке, задать URL-адрес HTTPS в *launchsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-151">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="7ccc2-152">Включите протокол HTTPS, если используется IIS Express.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-152">Enable HTTPS when IIS Express is used.</span></span>
* <span data-ttu-id="7ccc2-153">Настроить конечную точку HTTPS URL-адрес для развертывания общедоступных edge [Kestrel](xref:fundamentals/servers/kestrel) или [HTTP.sys](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-153">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) or [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span> <span data-ttu-id="7ccc2-154">Только **один порт HTTPS** используется приложением.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-154">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="7ccc2-155">По промежуточного слоя обнаруживает порт с помощью <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-155">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="7ccc2-156">При запуске приложения позади обратного прокси-сервера (например, IIS, IIS Express), `IServerAddressesFeature` недоступна.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-156">When an app is run behind a reverse proxy (for example, IIS, IIS Express), `IServerAddressesFeature` isn't available.</span></span> <span data-ttu-id="7ccc2-157">Необходимо вручную настроить порт.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-157">The port must be manually configured.</span></span> <span data-ttu-id="7ccc2-158">Если порт не задан, не перенаправляет запросы.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-158">When the port isn't set, requests aren't redirected.</span></span>

<span data-ttu-id="7ccc2-159">Если Kestrel и HTTP.sys используется в качестве общедоступных пограничного сервера, Kestrel и HTTP.sys должны быть настроены на прослушивание оба:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-159">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="7ccc2-160">Безопасного порта, где перенаправляется клиент (как правило, 443 в рабочей среде и 5001 в разработке).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-160">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="7ccc2-161">Небезопасный порт (как правило, 80 в рабочей среде) до 5000 при разработке.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-161">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="7ccc2-162">Небезопасный порт должен быть доступны клиенту в порядке для приложения для получения небезопасный запрос и перенаправления клиента для безопасного порта.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-162">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="7ccc2-163">Дополнительные сведения см. в разделе [конфигурации конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration) или <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-163">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="7ccc2-164">Сценарии развертывания</span><span class="sxs-lookup"><span data-stu-id="7ccc2-164">Deployment scenarios</span></span>

<span data-ttu-id="7ccc2-165">Любой брандмауэра между клиентом и сервером также должны быть открыты для трафика порты связи.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-165">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="7ccc2-166">Если запросы перенаправляются в конфигурации обратного прокси-сервера, используйте [пересылаемые по промежуточного слоя заголовки](xref:host-and-deploy/proxy-load-balancer) перед вызовом по промежуточного слоя переадресации HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-166">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="7ccc2-167">Пересылаемые обновлений по промежуточного слоя заголовки `Request.Scheme`, с использованием `X-Forwarded-Proto` заголовка.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-167">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="7ccc2-168">По промежуточного слоя позволяет перенаправлять URI и других политик безопасности, для правильной работы.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-168">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="7ccc2-169">При пересылаемые по промежуточного слоя заголовки не используется, во внутреннее приложение может не получать правильные схему и цикл перенаправления.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-169">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="7ccc2-170">Сообщение об ошибке распространенных конечного пользователя — в том, что произошло слишком много перенаправлений.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-170">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="7ccc2-171">При развертывании в службе приложений Azure, следуйте указаниям в [учебник: привязка существующего пользовательского SSL-сертификата к веб-приложениях Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-171">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="7ccc2-172">Параметры</span><span class="sxs-lookup"><span data-stu-id="7ccc2-172">Options</span></span>

<span data-ttu-id="7ccc2-173">Следующий выделенный код вызывает метод [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) настроить параметры по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-173">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=14-99)]

<span data-ttu-id="7ccc2-174">Вызов `AddHttpsRedirection` необходимо изменить значения `HttpsPort` или `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-174">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="7ccc2-175">Выделенный выше код:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-175">The preceding highlighted code:</span></span>

* <span data-ttu-id="7ccc2-176">Наборы [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) для <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, который является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-176">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="7ccc2-177">Используйте поля <xref:Microsoft.AspNetCore.Http.StatusCodes> класс для назначений `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-177">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="7ccc2-178">Задать HTTPS-порт на 5001.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-178">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="7ccc2-179">Значение по умолчанию — 443.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-179">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="7ccc2-180">Настройка постоянной переадресации в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="7ccc2-180">Configure permanent redirects in production</span></span>

<span data-ttu-id="7ccc2-181">По умолчанию по промежуточного слоя на отправку [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) с все операции перенаправления.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-181">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="7ccc2-182">Если вы хотите отправить код состояния постоянное перенаправление, во время работы приложения в среде не разработки, поместите параметры конфигурации по промежуточного слоя в условную проверку на среду не разработки.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-182">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

<span data-ttu-id="7ccc2-183">При настройке `IWebHostBuilder` в *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-183">When configuring an `IWebHostBuilder` in *Startup.cs*:</span></span>

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

## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="7ccc2-184">Альтернативный подход по промежуточного слоя переадресации HTTPS</span><span class="sxs-lookup"><span data-stu-id="7ccc2-184">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="7ccc2-185">Альтернативой использованию по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя для переопределения URL-адрес (`AddRedirectToHttps`).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-185">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="7ccc2-186">`AddRedirectToHttps` Можно также задать код состояния и порт при выполнении перенаправления.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-186">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="7ccc2-187">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-187">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="7ccc2-188">При перенаправлении HTTPS без необходимости для перенаправления дополнительных правил, мы рекомендуем использовать по промежуточного слоя переадресации HTTPS (`UseHttpsRedirection`) описано в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-188">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="7ccc2-189">[RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) используется для обязательного использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-189">The [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) is used to require HTTPS.</span></span> <span data-ttu-id="7ccc2-190">`[RequireHttpsAttribute]` можно снабдить контроллеров или методы, или могут применяться глобально.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-190">`[RequireHttpsAttribute]` can decorate controllers or methods, or can be applied globally.</span></span> <span data-ttu-id="7ccc2-191">Чтобы применить атрибут глобально, добавьте следующий код, чтобы `ConfigureServices` в `Startup`:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-191">To apply the attribute globally, add the following code to `ConfigureServices` in `Startup`:</span></span>

[!code-csharp[](~/security/authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet2&highlight=4-999)]

<span data-ttu-id="7ccc2-192">Выделенный выше код требует использовать все запросы `HTTPS`; таким образом, HTTP-запросы учитываются.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-192">The preceding highlighted code requires all requests use `HTTPS`; therefore, HTTP requests are ignored.</span></span> <span data-ttu-id="7ccc2-193">Следующий выделенный код перенаправляет все запросы HTTP на HTTPS:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-193">The following highlighted code redirects all HTTP requests to HTTPS:</span></span>

[!code-csharp[](authentication/accconfirm/sample/WebApp1/Startup.cs?name=snippet_AddRedirectToHttps&highlight=7-999)]

<span data-ttu-id="7ccc2-194">Дополнительные сведения см. в разделе [по промежуточного слоя для переопределения URL-адрес](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-194">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span> <span data-ttu-id="7ccc2-195">По промежуточного слоя также позволяет приложению задать код состояния или код состояния и номер порта, если выполняется перенаправление.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-195">The middleware also permits the app to set the status code or the status code and the port when the redirect is executed.</span></span>

<span data-ttu-id="7ccc2-196">Требования HTTPS глобально (`options.Filters.Add(new RequireHttpsAttribute());`) — это рекомендации по безопасности.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-196">Requiring HTTPS globally (`options.Filters.Add(new RequireHttpsAttribute());`) is a security best practice.</span></span> <span data-ttu-id="7ccc2-197">Применение `[RequireHttps]` атрибута ко всем страницам контроллеров/Razor не считается так безопасен, как требования HTTPS глобально.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-197">Applying the `[RequireHttps]` attribute to all controllers/Razor Pages isn't considered as secure as requiring HTTPS globally.</span></span> <span data-ttu-id="7ccc2-198">Не может гарантировать `[RequireHttps]` атрибут применяется, когда добавляются новые контроллеры и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-198">You can't guarantee the `[RequireHttps]` attribute is applied when new controllers and Razor Pages are added.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="7ccc2-199">Безопасность строгой транспортный протокол HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="7ccc2-199">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="7ccc2-200">На [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP строгой безопасности транспорта (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) представляет собой улучшение центра безопасности, который задается параметром веб-приложения при помощи заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-200">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="7ccc2-201">Когда [браузера, поддерживающего HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) получения этого заголовка:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-201">When a [browser that supports HSTS](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support) receives this header:</span></span>

* <span data-ttu-id="7ccc2-202">Браузер хранит конфигурацию для домена, который предотвращает отправку любые данные, передаваемые по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-202">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="7ccc2-203">Браузер заставляет весь обмен данными по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-203">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="7ccc2-204">Браузер запрещает пользователю с помощью недействителен или сертификаты.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-204">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="7ccc2-205">Браузер отключает подсказок, которые позволяют пользователю временно доверять такой сертификат.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-205">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="7ccc2-206">Поскольку HSTS обеспечивается с помощью клиента он имеет некоторые ограничения:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-206">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="7ccc2-207">Клиент должен поддерживать HSTS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-207">The client must support HSTS.</span></span>
* <span data-ttu-id="7ccc2-208">HSTS требуется по крайней мере один успешный запрос HTTPS для задания политики HSTS.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-208">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="7ccc2-209">Приложение должно проверить каждый HTTP-запрос и перенаправления или отклонить запрос HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-209">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="7ccc2-210">ASP.NET Core 2.1 или более поздних версий реализует HSTS с `UseHsts` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-210">ASP.NET Core 2.1 or later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="7ccc2-211">Следующий код вызывает `UseHsts` когда приложение не [режим разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="7ccc2-211">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet1&highlight=10)]

<span data-ttu-id="7ccc2-212">`UseHsts` не рекомендуется в разработке, так как параметры HSTS высокой кэшируемых обозревателями.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-212">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="7ccc2-213">По умолчанию `UseHsts` исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-213">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="7ccc2-214">Для рабочих сред реализации HTTPS в первый раз установить начального [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) на малое значение, с помощью одного из <xref:System.TimeSpan> методы.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-214">For production environments implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="7ccc2-215">Задайте значение от часов не больше, чем в один день в случае необходимости использования инфраструктуры HTTPS-HTTP.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-215">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="7ccc2-216">После вы уверены в устойчивости конфигурации HTTPS, увеличьте значение max-age HSTS; часто используемые значение — один год.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-216">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="7ccc2-217">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="7ccc2-217">The following code:</span></span>

[!code-csharp[](enforcing-ssl/sample/Startup.cs?name=snippet2&highlight=5-12)]

* <span data-ttu-id="7ccc2-218">Задает параметр предварительной загрузки заголовок Strict-Transport-Security.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-218">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="7ccc2-219">Предварительной загрузки не входит в состав [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузеры для предварительной загрузки HSTS узлов на установку обновления.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-219">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="7ccc2-220">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-220">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="7ccc2-221">Позволяет [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения таких поддоменов.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-221">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="7ccc2-222">Явно задает параметр max-age заголовок Strict-Transport-Security 60 дней.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-222">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="7ccc2-223">Если не установлено значение по умолчанию — 30 дней.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-223">If not set, defaults to 30 days.</span></span> <span data-ttu-id="7ccc2-224">См. в разделе [директива максимального](https://tools.ietf.org/html/rfc6797#section-6.1.1) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-224">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="7ccc2-225">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-225">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="7ccc2-226">`UseHsts` исключает следующими узлами замыкания на себя:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-226">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="7ccc2-227">`localhost` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-227">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="7ccc2-228">`127.0.0.1` : IPv4-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-228">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="7ccc2-229">`[::1]` : IPv6-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-229">`[::1]` : The IPv6 loopback address.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="7ccc2-230">Отказаться от HTTPS/HSTS при создании проекта</span><span class="sxs-lookup"><span data-stu-id="7ccc2-230">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="7ccc2-231">В некоторых сценариях службы серверной части, где безопасность подключения обрабатывается на границе общедоступные сети Настройка безопасности подключения на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-231">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="7ccc2-232">Веб-приложений, созданных из шаблонов в Visual Studio или из [команды dotnet new](/dotnet/core/tools/dotnet-new) команду enable [перенаправления HTTPS](#require-https) и [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-232">Web apps generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="7ccc2-233">Для развертываний, которые не требуют этих сценариев вы можете отказаться от HTTPS/HSTS создания приложения на основе шаблона.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-233">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="7ccc2-234">Чтобы отказаться от HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-234">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7ccc2-235">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7ccc2-235">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="7ccc2-236">Снимите флажок **настроить для использования протокола HTTPS** флажок.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-236">Uncheck the **Configure for HTTPS** checkbox.</span></span>

![Новое диалоговое окно веб-приложение ASP.NET Core, показывающий настройки для HTTPS флажок снят.](enforcing-ssl/_static/out.png)

#   <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="7ccc2-238">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="7ccc2-238">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="7ccc2-239">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-239">Use the `--no-https` option.</span></span> <span data-ttu-id="7ccc2-240">Пример</span><span class="sxs-lookup"><span data-stu-id="7ccc2-240">For example</span></span>

```console
dotnet new webapp --no-https
```

---

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="7ccc2-241">Доверять сертификату разработки ASP.NET Core HTTPS в Windows и macOS</span><span class="sxs-lookup"><span data-stu-id="7ccc2-241">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="7ccc2-242">Пакет SDK для .NET core включает в себя HTTPS-сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-242">.NET Core SDK includes a HTTPS development certificate.</span></span> <span data-ttu-id="7ccc2-243">Сертификат установлен как часть при первом запуске.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-243">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="7ccc2-244">Например `dotnet --info` выходные данные выглядят примерно следующим:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-244">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="7ccc2-245">При установке пакета SDK для .NET Core устанавливается сертификат разработки ASP.NET Core HTTPS в хранилище сертификатов локального пользователя.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-245">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="7ccc2-246">Сертификат был установлен, но он не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="7ccc2-246">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="7ccc2-247">Доверие сертификата выполнить однократное действие для выполнения dotnet `dev-certs` средство:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-247">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --trust
```

<span data-ttu-id="7ccc2-248">Следующая команда предоставляет справку для `dev-certs` средство:</span><span class="sxs-lookup"><span data-stu-id="7ccc2-248">The following command provides help on the `dev-certs` tool:</span></span>

```console
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="7ccc2-249">Как настроить сертификат разработчика для Docker</span><span class="sxs-lookup"><span data-stu-id="7ccc2-249">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="7ccc2-250">См. в разделе [проблема GitHub](https://github.com/aspnet/Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="7ccc2-250">See [this GitHub issue](https://github.com/aspnet/Docs/issues/6199).</span></span>

::: moniker-end

## <a name="additional-information"></a><span data-ttu-id="7ccc2-251">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="7ccc2-251">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="7ccc2-252">Размещение ASP.NET Core в Linux с использованием Apache: конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="7ccc2-252">Host ASP.NET Core on Linux with Apache: SSL configuration</span></span>](xref:host-and-deploy/linux-apache#ssl-configuration)
* [<span data-ttu-id="7ccc2-253">Размещение ASP.NET Core в Linux с Nginx: конфигурация SSL</span><span class="sxs-lookup"><span data-stu-id="7ccc2-253">Host ASP.NET Core on Linux with Nginx: SSL configuration</span></span>](xref:host-and-deploy/linux-nginx#configure-ssl)
* [<span data-ttu-id="7ccc2-254">Настройка SSL в IIS</span><span class="sxs-lookup"><span data-stu-id="7ccc2-254">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="7ccc2-255">Поддержка браузеров OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="7ccc2-255">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
