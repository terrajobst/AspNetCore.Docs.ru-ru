---
title: Принудительное применение HTTPS в ASP.NET Core
author: rick-anderson
description: Узнайте, как требовать использования HTTPS/TLS в ASP.NET Core веб-приложении.
ms.author: riande
ms.custom: mvc
ms.date: 09/14/2019
uid: security/enforcing-ssl
ms.openlocfilehash: aa42b1c7199e951714be809de9c9c5f857473485
ms.sourcegitcommit: 994da92edb0abf856b1655c18880028b15a28897
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71278756"
---
# <a name="enforce-https-in-aspnet-core"></a><span data-ttu-id="0e6f8-103">Принудительное применение HTTPS в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0e6f8-103">Enforce HTTPS in ASP.NET Core</span></span>

<span data-ttu-id="0e6f8-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="0e6f8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="0e6f8-105">В этом документе показано, как:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-105">This document shows how to:</span></span>

* <span data-ttu-id="0e6f8-106">Требовать протокол HTTPS для всех запросов.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-106">Require HTTPS for all requests.</span></span>
* <span data-ttu-id="0e6f8-107">Перенаправляет все запросы HTTP на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-107">Redirect all HTTP requests to HTTPS.</span></span>

<span data-ttu-id="0e6f8-108">Ни один API не может предотвратить отправку конфиденциальных данных клиентом при первом запросе.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-108">No API can prevent a client from sending sensitive data on the first request.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="0e6f8-109">Проекты API</span><span class="sxs-lookup"><span data-stu-id="0e6f8-109">API projects</span></span>
>
> <span data-ttu-id="0e6f8-110">**Не** используйте [Рекуирехттпсаттрибуте](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) в веб-API, которые получают конфиденциальную информацию.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-110">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0e6f8-111">`RequireHttpsAttribute`использует коды состояния HTTP для перенаправления браузеров из HTTP в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-111">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0e6f8-112">Клиенты API могут не распознать или соблюдать перенаправления от HTTP к HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-112">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0e6f8-113">Такие клиенты могут передавать данные по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-113">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0e6f8-114">Веб-API должны быть:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-114">Web APIs should either:</span></span>
>
> * <span data-ttu-id="0e6f8-115">Не прослушивать по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-115">Not listen on HTTP.</span></span>
> * <span data-ttu-id="0e6f8-116">Закройте подключение с кодом состояния 400 (недопустимый запрос) и не обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-116">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>
>
> ## <a name="hsts-and-api-projects"></a><span data-ttu-id="0e6f8-117">Проекты HSTS и API</span><span class="sxs-lookup"><span data-stu-id="0e6f8-117">HSTS and API projects</span></span>
>
> <span data-ttu-id="0e6f8-118">Проекты API по умолчанию не включают [HSTS](#hsts) , так как HSTS обычно является инструкцией только браузера.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-118">The default API projects don't include [HSTS](#hsts) because HSTS is generally a browser only instruction.</span></span> <span data-ttu-id="0e6f8-119">Другие вызывающие объекты, такие как приложения для телефонов или настольных систем, **не** подчиняются инструкции.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-119">Other callers, such as phone or desktop apps, do **not** obey the instruction.</span></span> <span data-ttu-id="0e6f8-120">Даже в обозревателях, один вызов API через HTTP, прошедший проверку подлинности, имеет риски в незащищенных сетях.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-120">Even within browsers, a single authenticated call to an API over HTTP has risks on insecure networks.</span></span> <span data-ttu-id="0e6f8-121">Безопасный подход заключается в настройке проектов API для прослушивания и реагирования по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-121">The secure approach is to configure API projects to only listen to and respond over HTTPS.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

> [!WARNING]
> ## <a name="api-projects"></a><span data-ttu-id="0e6f8-122">Проекты API</span><span class="sxs-lookup"><span data-stu-id="0e6f8-122">API projects</span></span>
>
> <span data-ttu-id="0e6f8-123">**Не** используйте [Рекуирехттпсаттрибуте](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) в веб-API, которые получают конфиденциальную информацию.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-123">Do **not** use [RequireHttpsAttribute](/dotnet/api/microsoft.aspnetcore.mvc.requirehttpsattribute) on Web APIs that receive sensitive information.</span></span> <span data-ttu-id="0e6f8-124">`RequireHttpsAttribute`использует коды состояния HTTP для перенаправления браузеров из HTTP в HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-124">`RequireHttpsAttribute` uses HTTP status codes to redirect browsers from HTTP to HTTPS.</span></span> <span data-ttu-id="0e6f8-125">Клиенты API могут не распознать или соблюдать перенаправления от HTTP к HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-125">API clients may not understand or obey redirects from HTTP to HTTPS.</span></span> <span data-ttu-id="0e6f8-126">Такие клиенты могут передавать данные по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-126">Such clients may send information over HTTP.</span></span> <span data-ttu-id="0e6f8-127">Веб-API должны быть:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-127">Web APIs should either:</span></span>
>
> * <span data-ttu-id="0e6f8-128">Не прослушивать по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-128">Not listen on HTTP.</span></span>
> * <span data-ttu-id="0e6f8-129">Закройте подключение с кодом состояния 400 (недопустимый запрос) и не обслуживает запрос.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-129">Close the connection with status code 400 (Bad Request) and not serve the request.</span></span>

::: moniker-end

## <a name="require-https"></a><span data-ttu-id="0e6f8-130">Требование к использованию протокола HTTPS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-130">Require HTTPS</span></span>

<span data-ttu-id="0e6f8-131">В рабочей ASP.NET Core веб-приложений рекомендуется использовать:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-131">We recommend that production ASP.NET Core web apps use:</span></span>

* <span data-ttu-id="0e6f8-132">По промежуточного слоя перенаправления<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>HTTPS () для перенаправления HTTP-запросов к HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-132">HTTPS Redirection Middleware (<xref:Microsoft.AspNetCore.Builder.HttpsPolicyBuilderExtensions.UseHttpsRedirection*>) to redirect HTTP requests to HTTPS.</span></span>
* <span data-ttu-id="0e6f8-133">HSTS по промежуточного слоя ([усехстс](#http-strict-transport-security-protocol-hsts)) для отправки клиентам заголовков протокола безопасности транспорта HTTP (HSTS).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-133">HSTS Middleware ([UseHsts](#http-strict-transport-security-protocol-hsts)) to send HTTP Strict Transport Security Protocol (HSTS) headers to clients.</span></span>

> [!NOTE]
> <span data-ttu-id="0e6f8-134">Приложения, развернутые в конфигурации обратных прокси-серверов, позволяют прокси-серверу управлять безопасностью подключения (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-134">Apps deployed in a reverse proxy configuration allow the proxy to handle connection security (HTTPS).</span></span> <span data-ttu-id="0e6f8-135">Если прокси-сервер также обрабатывает перенаправление HTTPS, нет необходимости использовать по промежуточного слоя перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-135">If the proxy also handles HTTPS redirection, there's no need to use HTTPS Redirection Middleware.</span></span> <span data-ttu-id="0e6f8-136">Если прокси-сервер также обрабатывает запись заголовков HSTS (например, [поддержка собственного HSTS в IIS 10,0 (1709) или более поздней версии](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), по промежуточного слоя HSTS не требуется для приложения.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-136">If the proxy server also handles writing HSTS headers (for example, [native HSTS support in IIS 10.0 (1709) or later](/iis/get-started/whats-new-in-iis-10-version-1709/iis-10-version-1709-hsts#iis-100-version-1709-native-hsts-support)), HSTS Middleware isn't required by the app.</span></span> <span data-ttu-id="0e6f8-137">Дополнительные сведения см. [в разделе отказ от HTTPS/HSTS при создании проекта](#opt-out-of-httpshsts-on-project-creation).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-137">For more information, see [Opt-out of HTTPS/HSTS on project creation](#opt-out-of-httpshsts-on-project-creation).</span></span>

### <a name="usehttpsredirection"></a><span data-ttu-id="0e6f8-138">усехттпсредиректион</span><span class="sxs-lookup"><span data-stu-id="0e6f8-138">UseHttpsRedirection</span></span>

<span data-ttu-id="0e6f8-139">Следующий код вызывает `UseHttpsRedirection` `Startup` в классе:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-139">The following code calls `UseHttpsRedirection` in the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=14)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=13)]

::: moniker-end

<span data-ttu-id="0e6f8-140">Предыдущий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-140">The preceding highlighted code:</span></span>

* <span data-ttu-id="0e6f8-141">Использует значение по умолчанию [хттпсредиректионоптионс. редиректстатускоде](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-141">Uses the default [HttpsRedirectionOptions.RedirectStatusCode](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.redirectstatuscode) ([Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect)).</span></span>
* <span data-ttu-id="0e6f8-142">Использует значение по умолчанию [хттпсредиректионоптионс. HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null), если оно `ASPNETCORE_HTTPS_PORT` не переопределено переменной среды или [исервераддрессесфеатуре](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-142">Uses the default [HttpsRedirectionOptions.HttpsPort](/dotnet/api/microsoft.aspnetcore.httpspolicy.httpsredirectionoptions.httpsport) (null) unless overridden by the `ASPNETCORE_HTTPS_PORT` environment variable or [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature).</span></span>

<span data-ttu-id="0e6f8-143">Рекомендуется использовать временные перенаправления, а не постоянные перенаправления.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-143">We recommend using temporary redirects rather than permanent redirects.</span></span> <span data-ttu-id="0e6f8-144">Кэширование ссылок может привести к нестабильной работе в средах разработки.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-144">Link caching can cause unstable behavior in development environments.</span></span> <span data-ttu-id="0e6f8-145">Если вы предпочитаете отправить код состояния с постоянным перенаправлением, когда приложение находится в среде, не являющейся средой разработки, см. раздел [Настройка постоянных перенаправлений в производстве](#configure-permanent-redirects-in-production) .</span><span class="sxs-lookup"><span data-stu-id="0e6f8-145">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, see the [Configure permanent redirects in production](#configure-permanent-redirects-in-production) section.</span></span> <span data-ttu-id="0e6f8-146">Мы рекомендуем использовать [HSTS](#http-strict-transport-security-protocol-hsts) для передачи клиентам, что только защищенные запросы ресурсов должны отправляться в приложение (только в рабочей среде).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-146">We recommend using [HSTS](#http-strict-transport-security-protocol-hsts) to signal to clients that only secure resource requests should be sent to the app (only in production).</span></span>

### <a name="port-configuration"></a><span data-ttu-id="0e6f8-147">Настройка порта</span><span class="sxs-lookup"><span data-stu-id="0e6f8-147">Port configuration</span></span>

<span data-ttu-id="0e6f8-148">Порт должен быть доступен для промежуточного слоя, чтобы перенаправить незащищенный запрос на HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-148">A port must be available for the middleware to redirect an insecure request to HTTPS.</span></span> <span data-ttu-id="0e6f8-149">Если порт недоступен:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-149">If no port is available:</span></span>

* <span data-ttu-id="0e6f8-150">Перенаправление на HTTPS не выполняется.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-150">Redirection to HTTPS doesn't occur.</span></span>
* <span data-ttu-id="0e6f8-151">По промежуточного слоя регистрируется предупреждение "не удалось определить HTTPS порт для перенаправления".</span><span class="sxs-lookup"><span data-stu-id="0e6f8-151">The middleware logs the warning "Failed to determine the https port for redirect."</span></span>

<span data-ttu-id="0e6f8-152">Укажите HTTPS порт, используя любой из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-152">Specify the HTTPS port using any of the following approaches:</span></span>

* <span data-ttu-id="0e6f8-153">Задайте [хттпсредиректионоптионс. HttpsPort](#options).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-153">Set [HttpsRedirectionOptions.HttpsPort](#options).</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="0e6f8-154">Задайте параметр [узла:](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port) `https_port`</span><span class="sxs-lookup"><span data-stu-id="0e6f8-154">Set the `https_port` [host setting](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#https_port):</span></span>

  * <span data-ttu-id="0e6f8-155">В конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-155">In host configuration.</span></span>
  * <span data-ttu-id="0e6f8-156">Путем задания `ASPNETCORE_HTTPS_PORT` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-156">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="0e6f8-157">Путем добавления записи верхнего уровня в *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-157">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/3.x/appsettings.json?highlight=2)]

* <span data-ttu-id="0e6f8-158">Укажите порт с защитой схемы с помощью [переменной среды ASPNETCORE_URLS](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-158">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](/aspnet/core/fundamentals/host/generic-host?view=aspnetcore-3.0#urls).</span></span> <span data-ttu-id="0e6f8-159">Переменная среды настраивает сервер.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-159">The environment variable configures the server.</span></span> <span data-ttu-id="0e6f8-160">По промежуточного слоя косвенно обнаруживает HTTPS порт с <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>помощью.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-160">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="0e6f8-161">Этот подход не работает в обратных развертываниях прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-161">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

* <span data-ttu-id="0e6f8-162">Задайте параметр [узла:](xref:fundamentals/host/web-host#https-port) `https_port`</span><span class="sxs-lookup"><span data-stu-id="0e6f8-162">Set the `https_port` [host setting](xref:fundamentals/host/web-host#https-port):</span></span>

  * <span data-ttu-id="0e6f8-163">В конфигурации узла.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-163">In host configuration.</span></span>
  * <span data-ttu-id="0e6f8-164">Путем задания `ASPNETCORE_HTTPS_PORT` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-164">By setting the `ASPNETCORE_HTTPS_PORT` environment variable.</span></span>
  * <span data-ttu-id="0e6f8-165">Путем добавления записи верхнего уровня в *appSettings. JSON*:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-165">By adding a top-level entry in *appsettings.json*:</span></span>

    [!code-json[](enforcing-ssl/sample-snapshot/2.x/appsettings.json?highlight=2)]

* <span data-ttu-id="0e6f8-166">Укажите порт с защитой схемы с помощью [переменной среды ASPNETCORE_URLS](xref:fundamentals/host/web-host#server-urls).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-166">Indicate a port with the secure scheme using the [ASPNETCORE_URLS environment variable](xref:fundamentals/host/web-host#server-urls).</span></span> <span data-ttu-id="0e6f8-167">Переменная среды настраивает сервер.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-167">The environment variable configures the server.</span></span> <span data-ttu-id="0e6f8-168">По промежуточного слоя косвенно обнаруживает HTTPS порт с <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>помощью.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-168">The middleware indirectly discovers the HTTPS port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span> <span data-ttu-id="0e6f8-169">Этот подход не работает в обратных развертываниях прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-169">This approach doesn't work in reverse proxy deployments.</span></span>

::: moniker-end

* <span data-ttu-id="0e6f8-170">В среде разработки задайте URL-адрес HTTPS в *launchsettings. JSON*.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-170">In development, set an HTTPS URL in *launchsettings.json*.</span></span> <span data-ttu-id="0e6f8-171">При использовании IIS Express включите протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-171">Enable HTTPS when IIS Express is used.</span></span>

* <span data-ttu-id="0e6f8-172">Настройте конечную точку HTTPS URL-адреса для общедоступного пограничной развертывания сервера [Kestrel](xref:fundamentals/servers/kestrel) или [http. sys](xref:fundamentals/servers/httpsys) .</span><span class="sxs-lookup"><span data-stu-id="0e6f8-172">Configure an HTTPS URL endpoint for a public-facing edge deployment of [Kestrel](xref:fundamentals/servers/kestrel) server or [HTTP.sys](xref:fundamentals/servers/httpsys) server.</span></span> <span data-ttu-id="0e6f8-173">Приложение использует только **один HTTPS-порт** .</span><span class="sxs-lookup"><span data-stu-id="0e6f8-173">Only **one HTTPS port** is used by the app.</span></span> <span data-ttu-id="0e6f8-174">По промежуточного слоя обнаруживает порт через <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-174">The middleware discovers the port via <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature>.</span></span>

> [!NOTE]
> <span data-ttu-id="0e6f8-175">Если приложение запускается в конфигурации обратного прокси-сервера, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> оно недоступно.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-175">When an app is run in a reverse proxy configuration, <xref:Microsoft.AspNetCore.Hosting.Server.Features.IServerAddressesFeature> isn't available.</span></span> <span data-ttu-id="0e6f8-176">Задайте порт с помощью одного из других подходов, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-176">Set the port using one of the other approaches described in this section.</span></span>

### <a name="edge-deployments"></a><span data-ttu-id="0e6f8-177">Развертывания пограничных устройств</span><span class="sxs-lookup"><span data-stu-id="0e6f8-177">Edge deployments</span></span> 

<span data-ttu-id="0e6f8-178">Если Kestrel или HTTP. sys используется в качестве общедоступного пограничной сервера, то Kestrel или HTTP. sys должны быть настроены на прослушивание обоих типов:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-178">When Kestrel or HTTP.sys is used as a public-facing edge server, Kestrel or HTTP.sys must be configured to listen on both:</span></span>

* <span data-ttu-id="0e6f8-179">Безопасный порт, на который перенаправляется клиент (как правило, 443 в рабочей среде и 5001 в разработке).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-179">The secure port where the client is redirected (typically, 443 in production and 5001 in development).</span></span>
* <span data-ttu-id="0e6f8-180">Небезопасный порт (как правило, 80 в рабочей среде и 5000 в разработке).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-180">The insecure port (typically, 80 in production and 5000 in development).</span></span>

<span data-ttu-id="0e6f8-181">Клиент должен получить доступ к незащищенному порту, чтобы приложение получило незащищенный запрос и перенаправлять клиент на безопасный порт.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-181">The insecure port must be accessible by the client in order for the app to receive an insecure request and redirect the client to the secure port.</span></span>

<span data-ttu-id="0e6f8-182">Дополнительные сведения см. в разделе [Kestrel Endpoint Configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) или <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-182">For more information, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) or <xref:fundamentals/servers/httpsys>.</span></span>

### <a name="deployment-scenarios"></a><span data-ttu-id="0e6f8-183">Сценарии развертывания</span><span class="sxs-lookup"><span data-stu-id="0e6f8-183">Deployment scenarios</span></span>

<span data-ttu-id="0e6f8-184">Все брандмауэры между клиентом и сервером также должны иметь открытые порты связи для трафика.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-184">Any firewall between the client and server must also have communication ports open for traffic.</span></span>

<span data-ttu-id="0e6f8-185">Если запросы перенаправляются в конфигурации обратных прокси-серверов, используйте [промежуточный слой перенаправленных заголовков](xref:host-and-deploy/proxy-load-balancer) перед вызовом по промежуточного слоя перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-185">If requests are forwarded in a reverse proxy configuration, use [Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) before calling HTTPS Redirection Middleware.</span></span> <span data-ttu-id="0e6f8-186">По промежуточного слоя перенаправленных заголовков обновляет `Request.Scheme`, `X-Forwarded-Proto` используя заголовок.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-186">Forwarded Headers Middleware updates the `Request.Scheme`, using the `X-Forwarded-Proto` header.</span></span> <span data-ttu-id="0e6f8-187">По промежуточного слоя позволяет правильно работать с URI перенаправления и другими политиками безопасности.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-187">The middleware permits redirect URIs and other security policies to work correctly.</span></span> <span data-ttu-id="0e6f8-188">Если по промежуточного слоя перенаправленных заголовков не используется, серверное приложение может не получить правильную схему и завершить цикл перенаправления.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-188">When Forwarded Headers Middleware isn't used, the backend app might not receive the correct scheme and end up in a redirect loop.</span></span> <span data-ttu-id="0e6f8-189">Обычное сообщение об ошибке конечного пользователя состоит в том, что произошло слишком много перенаправлений.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-189">A common end user error message is that too many redirects have occurred.</span></span>

<span data-ttu-id="0e6f8-190">При развертывании в службе приложений Azure следуйте указаниям в [учебнике: Привязывание существующего настраиваемого SSL-сертификата к веб-приложениям Azure](/azure/app-service/app-service-web-tutorial-custom-ssl).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-190">When deploying to Azure App Service, follow the guidance in [Tutorial: Bind an existing custom SSL certificate to Azure Web Apps](/azure/app-service/app-service-web-tutorial-custom-ssl).</span></span>

### <a name="options"></a><span data-ttu-id="0e6f8-191">Параметры</span><span class="sxs-lookup"><span data-stu-id="0e6f8-191">Options</span></span>

<span data-ttu-id="0e6f8-192">Следующий выделенный код вызывает [аддхттпсредиректион](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) для настройки параметров промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-192">The following highlighted code calls [AddHttpsRedirection](/dotnet/api/microsoft.aspnetcore.builder.httpsredirectionservicesextensions.addhttpsredirection) to configure middleware options:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=14-18)]

::: moniker-end


<span data-ttu-id="0e6f8-193">Вызов `AddHttpsRedirection` необходим только для изменения `HttpsPort` значений или `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-193">Calling `AddHttpsRedirection` is only necessary to change the values of `HttpsPort` or `RedirectStatusCode`.</span></span>

<span data-ttu-id="0e6f8-194">Предыдущий выделенный код:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-194">The preceding highlighted code:</span></span>

* <span data-ttu-id="0e6f8-195">Устанавливает [хттпсредиректионоптионс. редиректстатускоде](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) в <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>значение, которое является значением по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-195">Sets [HttpsRedirectionOptions.RedirectStatusCode](xref:Microsoft.AspNetCore.HttpsPolicy.HttpsRedirectionOptions.RedirectStatusCode*) to <xref:Microsoft.AspNetCore.Http.StatusCodes.Status307TemporaryRedirect>, which is the default value.</span></span> <span data-ttu-id="0e6f8-196">Используйте поля <xref:Microsoft.AspNetCore.Http.StatusCodes> класса для назначений в `RedirectStatusCode`.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-196">Use the fields of the <xref:Microsoft.AspNetCore.Http.StatusCodes> class for assignments to `RedirectStatusCode`.</span></span>
* <span data-ttu-id="0e6f8-197">Устанавливает HTTPS порт 5001.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-197">Sets the HTTPS port to 5001.</span></span> <span data-ttu-id="0e6f8-198">Значение по умолчанию — 443.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-198">The default value is 443.</span></span>

#### <a name="configure-permanent-redirects-in-production"></a><span data-ttu-id="0e6f8-199">Настройка постоянных перенаправлений в рабочей среде</span><span class="sxs-lookup"><span data-stu-id="0e6f8-199">Configure permanent redirects in production</span></span>

<span data-ttu-id="0e6f8-200">По промежуточного слоя по умолчанию отправляется [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) со всеми перенаправлениями.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-200">The middleware defaults to sending a [Status307TemporaryRedirect](/dotnet/api/microsoft.aspnetcore.http.statuscodes.status307temporaryredirect) with all redirects.</span></span> <span data-ttu-id="0e6f8-201">Если вы предпочитаете отправить код состояния с постоянным перенаправлением, когда приложение находится в среде, не являющейся средой разработки, заключите конфигурацию параметров по промежуточного слоя в условную проверку для среды без разработки.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-201">If you prefer to send a permanent redirect status code when the app is in a non-Development environment, wrap the middleware options configuration in a conditional check for a non-Development environment.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="0e6f8-202">При настройке служб в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-202">When configuring services in *Startup.cs*:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // IWebHostEnvironment (stored in _env) is injected into the Startup class.
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

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="0e6f8-203">При настройке служб в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-203">When configuring services in *Startup.cs*:</span></span>

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

::: moniker-end


## <a name="https-redirection-middleware-alternative-approach"></a><span data-ttu-id="0e6f8-204">Альтернативный подход по промежуточного слоя перенаправления HTTPS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-204">HTTPS Redirection Middleware alternative approach</span></span>

<span data-ttu-id="0e6f8-205">Альтернативой по промежуточного слоя перенаправления HTTPS (`UseHttpsRedirection`) является использование по промежуточного слоя переписывания`AddRedirectToHttps`URL-адресов ().</span><span class="sxs-lookup"><span data-stu-id="0e6f8-205">An alternative to using HTTPS Redirection Middleware (`UseHttpsRedirection`) is to use URL Rewriting Middleware (`AddRedirectToHttps`).</span></span> <span data-ttu-id="0e6f8-206">`AddRedirectToHttps`также может задавать код состояния и порт при выполнении перенаправления.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-206">`AddRedirectToHttps` can also set the status code and port when the redirect is executed.</span></span> <span data-ttu-id="0e6f8-207">Дополнительные сведения см. в разделе по [промежуточного слоя перезаписи URL-адресов](xref:fundamentals/url-rewriting).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-207">For more information, see [URL Rewriting Middleware](xref:fundamentals/url-rewriting).</span></span>

<span data-ttu-id="0e6f8-208">При перенаправлении по протоколу HTTPS без требования дополнительных правил перенаправления рекомендуется использовать по промежуточного слоя перенаправления HTTPS (`UseHttpsRedirection`), описанные в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-208">When redirecting to HTTPS without the requirement for additional redirect rules, we recommend using HTTPS Redirection Middleware (`UseHttpsRedirection`) described in this topic.</span></span>

<a name="hsts"></a>

## <a name="http-strict-transport-security-protocol-hsts"></a><span data-ttu-id="0e6f8-209">Протокол HTTPS протокола безопасности транспорта HTTP (HSTS)</span><span class="sxs-lookup"><span data-stu-id="0e6f8-209">HTTP Strict Transport Security Protocol (HSTS)</span></span>

<span data-ttu-id="0e6f8-210">В [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [обеспечение безопасности транспорта по протоколу HTTP (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) является расширением безопасности, которое определяется веб-приложением с помощью заголовка ответа.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-210">Per [OWASP](https://www.owasp.org/index.php/About_The_Open_Web_Application_Security_Project), [HTTP Strict Transport Security (HSTS)](https://cheatsheetseries.owasp.org/cheatsheets/HTTP_Strict_Transport_Security_Cheat_Sheet.html) is an opt-in security enhancement that's specified by a web app through the use of a response header.</span></span> <span data-ttu-id="0e6f8-211">Когда [браузер, поддерживающий HSTS,](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) получает этот заголовок:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-211">When a [browser that supports HSTS](https://cheatsheetseries.owasp.org/cheatsheets/Transport_Layer_Protection_Cheat_Sheet.html#browser-support) receives this header:</span></span>

* <span data-ttu-id="0e6f8-212">В браузере хранится конфигурация домена, которая предотвращает отправку обмена данными по протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-212">The browser stores configuration for the domain that prevents sending any communication over HTTP.</span></span> <span data-ttu-id="0e6f8-213">Браузер принудительно выполняет все связи по протоколу HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-213">The browser forces all communication over HTTPS.</span></span>
* <span data-ttu-id="0e6f8-214">Браузер не разрешает пользователю использовать недоверенные или недопустимые сертификаты.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-214">The browser prevents the user from using untrusted or invalid certificates.</span></span> <span data-ttu-id="0e6f8-215">Браузер отключает запросы, позволяющие пользователю временно доверять такому сертификату.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-215">The browser disables prompts that allow a user to temporarily trust such a certificate.</span></span>

<span data-ttu-id="0e6f8-216">Так как HSTS обеспечивается клиентом, он имеет некоторые ограничения:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-216">Because HSTS is enforced by the client it has some limitations:</span></span>

* <span data-ttu-id="0e6f8-217">Клиент должен поддерживать HSTS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-217">The client must support HSTS.</span></span>
* <span data-ttu-id="0e6f8-218">HSTS требует по крайней мере одного успешного запроса HTTPS для установки политики HSTS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-218">HSTS requires at least one successful HTTPS request to establish the HSTS policy.</span></span>
* <span data-ttu-id="0e6f8-219">Приложение должно проверять каждый HTTP-запрос и перенаправлять или отклонять HTTP-запрос.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-219">The application must check every HTTP request and redirect or reject the HTTP request.</span></span>

<span data-ttu-id="0e6f8-220">ASP.NET Core 2,1 и более поздних версий реализует `UseHsts` HSTS с помощью метода расширения.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-220">ASP.NET Core 2.1 and later implements HSTS with the `UseHsts` extension method.</span></span> <span data-ttu-id="0e6f8-221">Следующий код вызывает `UseHsts` , когда приложение не находится в [режиме разработки](xref:fundamentals/environments):</span><span class="sxs-lookup"><span data-stu-id="0e6f8-221">The following code calls `UseHsts` when the app isn't in [development mode](xref:fundamentals/environments):</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet1&highlight=11)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet1&highlight=10)]

::: moniker-end

<span data-ttu-id="0e6f8-222">`UseHsts`не рекомендуется в разработке, так как параметры HSTS очень кэшируются браузерами.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-222">`UseHsts` isn't recommended in development because the HSTS settings are highly cacheable by browsers.</span></span> <span data-ttu-id="0e6f8-223">По умолчанию `UseHsts` исключает локальный петлевой адрес.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-223">By default, `UseHsts` excludes the local loopback address.</span></span>

<span data-ttu-id="0e6f8-224">Для рабочих сред, которые реализуют протокол HTTPS в первый раз, задайте для свойства Initial [хстсоптионс. MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) небольшое значение с помощью одного из <xref:System.TimeSpan> методов.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-224">For production environments that are implementing HTTPS for the first time, set the initial [HstsOptions.MaxAge](xref:Microsoft.AspNetCore.HttpsPolicy.HstsOptions.MaxAge*) to a small value using one of the <xref:System.TimeSpan> methods.</span></span> <span data-ttu-id="0e6f8-225">Задайте в качестве значения в часах не более одного дня на случай, если необходимо вернуть инфраструктуру HTTPS к протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-225">Set the value from hours to no more than a single day in case you need to revert the HTTPS infrastructure to HTTP.</span></span> <span data-ttu-id="0e6f8-226">Убедившись в устойчивости конфигурации HTTPS, увеличьте значение HSTS max-age. часто используемое значение — один год.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-226">After you're confident in the sustainability of the HTTPS configuration, increase the HSTS max-age value; a commonly used value is one year.</span></span>

<span data-ttu-id="0e6f8-227">В приведенном ниже коде</span><span class="sxs-lookup"><span data-stu-id="0e6f8-227">The following code:</span></span>


::: moniker range=">= aspnetcore-3.0"

[!code-csharp[](enforcing-ssl/sample-snapshot/3.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[](enforcing-ssl/sample-snapshot/2.x/Startup.cs?name=snippet2&highlight=5-12)]

::: moniker-end


* <span data-ttu-id="0e6f8-228">Задает параметр предварительной загрузки для заголовка с уровнем безопасности "Долгосрочный транспорт — Безопасность".</span><span class="sxs-lookup"><span data-stu-id="0e6f8-228">Sets the preload parameter of the Strict-Transport-Security header.</span></span> <span data-ttu-id="0e6f8-229">Предварительная загрузка не является частью [спецификации RFC HSTS](https://tools.ietf.org/html/rfc6797), но поддерживается веб-браузерами для предварительной загрузки HSTS сайтов при новой установке.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-229">Preload isn't part of the [RFC HSTS specification](https://tools.ietf.org/html/rfc6797), but is supported by web browsers to preload HSTS sites on fresh install.</span></span> <span data-ttu-id="0e6f8-230">Дополнительные сведения см. в разделе [https://hstspreload.org/](https://hstspreload.org/).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-230">See [https://hstspreload.org/](https://hstspreload.org/) for more information.</span></span>
* <span data-ttu-id="0e6f8-231">Включает [инклудесубдомаин](https://tools.ietf.org/html/rfc6797#section-6.1.2), который применяет политику HSTS для размещения поддоменов.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-231">Enables [includeSubDomain](https://tools.ietf.org/html/rfc6797#section-6.1.2), which applies the HSTS policy to Host subdomains.</span></span>
* <span data-ttu-id="0e6f8-232">Явно задает параметр max-age для заголовка с ограничением транспорта-безопасности до 60 дней.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-232">Explicitly sets the max-age parameter of the Strict-Transport-Security header to 60 days.</span></span> <span data-ttu-id="0e6f8-233">Если значение не задано, по умолчанию используется значение 30 дней.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-233">If not set, defaults to 30 days.</span></span> <span data-ttu-id="0e6f8-234">Дополнительные сведения см. в [директиве max-age](https://tools.ietf.org/html/rfc6797#section-6.1.1) .</span><span class="sxs-lookup"><span data-stu-id="0e6f8-234">See the [max-age directive](https://tools.ietf.org/html/rfc6797#section-6.1.1) for more information.</span></span>
* <span data-ttu-id="0e6f8-235">Добавляет `example.com` в список узлов для исключения.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-235">Adds `example.com` to the list of hosts to exclude.</span></span>

<span data-ttu-id="0e6f8-236">`UseHsts`исключает следующие узлы замыкания на себя:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-236">`UseHsts` excludes the following loopback hosts:</span></span>

* <span data-ttu-id="0e6f8-237">`localhost`: Адрес замыкания на себя IPv4.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-237">`localhost` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0e6f8-238">`127.0.0.1`: Адрес замыкания на себя IPv4.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-238">`127.0.0.1` : The IPv4 loopback address.</span></span>
* <span data-ttu-id="0e6f8-239">`[::1]`: IPv6-адрес замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-239">`[::1]` : The IPv6 loopback address.</span></span>

## <a name="opt-out-of-httpshsts-on-project-creation"></a><span data-ttu-id="0e6f8-240">Отказ от HTTPS/HSTS при создании проекта</span><span class="sxs-lookup"><span data-stu-id="0e6f8-240">Opt-out of HTTPS/HSTS on project creation</span></span>

<span data-ttu-id="0e6f8-241">В некоторых сценариях серверной службы, где безопасность подключения обрабатывается на общедоступной границе сети, Настройка безопасности подключения на каждом узле не требуется.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-241">In some backend service scenarios where connection security is handled at the public-facing edge of the network, configuring connection security at each node isn't required.</span></span> <span data-ttu-id="0e6f8-242">Веб-приложения, созданные на основе шаблонов в Visual Studio или команды [DotNet New](/dotnet/core/tools/dotnet-new) , включают [Перенаправление HTTPS](#require-https) и [HSTS](#http-strict-transport-security-protocol-hsts).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-242">Web apps that are generated from the templates in Visual Studio or from the [dotnet new](/dotnet/core/tools/dotnet-new) command enable [HTTPS redirection](#require-https) and [HSTS](#http-strict-transport-security-protocol-hsts).</span></span> <span data-ttu-id="0e6f8-243">Для развертываний, которые не нуждаются в этих сценариях, можно отказаться от HTTPS/HSTS при создании приложения из шаблона.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-243">For deployments that don't require these scenarios, you can opt-out of HTTPS/HSTS when the app is created from the template.</span></span>

<span data-ttu-id="0e6f8-244">Чтобы отказаться от HTTPS/HSTS:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-244">To opt-out of HTTPS/HSTS:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="0e6f8-245">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="0e6f8-245">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="0e6f8-246">Снимите флажок **настроить для HTTPS** .</span><span class="sxs-lookup"><span data-stu-id="0e6f8-246">Uncheck the **Configure for HTTPS** check box.</span></span>

::: moniker range=">= aspnetcore-3.0"

![Диалоговое окно нового ASP.NET Core веб-приложения с снятым флажком настроить для HTTPS.](enforcing-ssl/_static/out-vs2019.png)

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

![Диалоговое окно нового ASP.NET Core веб-приложения с снятым флажком настроить для HTTPS.](enforcing-ssl/_static/out.png)

::: moniker-end


# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="0e6f8-249">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="0e6f8-249">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="0e6f8-250">Использовать параметр `--no-https`.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-250">Use the `--no-https` option.</span></span> <span data-ttu-id="0e6f8-251">Пример</span><span class="sxs-lookup"><span data-stu-id="0e6f8-251">For example</span></span>

```dotnetcli
dotnet new webapp --no-https
```

---

<a name="trust"></a>

## <a name="trust-the-aspnet-core-https-development-certificate-on-windows-and-macos"></a><span data-ttu-id="0e6f8-252">Доверять сертификату разработки ASP.NET Core HTTPS в Windows и macOS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-252">Trust the ASP.NET Core HTTPS development certificate on Windows and macOS</span></span>

<span data-ttu-id="0e6f8-253">Пакет SDK для .NET Core включает сертификат разработки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-253">The .NET Core SDK includes an HTTPS development certificate.</span></span> <span data-ttu-id="0e6f8-254">Сертификат устанавливается в рамках первого запуска.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-254">The certificate is installed as part of the first-run experience.</span></span> <span data-ttu-id="0e6f8-255">Например, выводит результат, `dotnet --info` аналогичный приведенному ниже:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-255">For example, `dotnet --info` produces output similar to the following:</span></span>

```text
ASP.NET Core
------------
Successfully installed the ASP.NET Core HTTPS Development Certificate.
To trust the certificate run 'dotnet dev-certs https --trust' (Windows and macOS only).
For establishing trust on other platforms refer to the platform specific documentation.
For more information on configuring HTTPS see https://go.microsoft.com/fwlink/?linkid=848054.
```

<span data-ttu-id="0e6f8-256">При установке пакета SDK для .NET Core в локальное хранилище сертификатов пользователя устанавливается сертификат разработки HTTPS ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-256">Installing the .NET Core SDK installs the ASP.NET Core HTTPS development certificate to the local user certificate store.</span></span> <span data-ttu-id="0e6f8-257">Сертификат установлен, но не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-257">The certificate has been installed, but it's not trusted.</span></span> <span data-ttu-id="0e6f8-258">Чтобы сделать сертификат доверенным, выполните однократный шаг для запуска средства DotNet `dev-certs` :</span><span class="sxs-lookup"><span data-stu-id="0e6f8-258">To trust the certificate perform the one-time step to run the dotnet `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --trust
```

<span data-ttu-id="0e6f8-259">Следующая команда вызывает справку по средству `dev-certs`.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-259">The following command provides help on the `dev-certs` tool:</span></span>

```dotnetcli
dotnet dev-certs https --help
```

## <a name="how-to-set-up-a-developer-certificate-for-docker"></a><span data-ttu-id="0e6f8-260">Как настроить сертификат разработчика для DOCKER</span><span class="sxs-lookup"><span data-stu-id="0e6f8-260">How to set up a developer certificate for Docker</span></span>

<span data-ttu-id="0e6f8-261">Также см. [эту проблему в GitHub](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span><span class="sxs-lookup"><span data-stu-id="0e6f8-261">See [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/6199).</span></span>

<a name="wsl"></a>

## <a name="trust-https-certificate-from-windows-subsystem-for-linux"></a><span data-ttu-id="0e6f8-262">Доверенный HTTPS сертификат из подсистемы Windows для Linux</span><span class="sxs-lookup"><span data-stu-id="0e6f8-262">Trust HTTPS certificate from Windows Subsystem for Linux</span></span>

<span data-ttu-id="0e6f8-263">Подсистема Windows для Linux (WSL) создает самозаверяющий сертификат HTTPS. Чтобы настроить хранилище сертификатов Windows для доверия к сертификату WSL, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-263">The Windows Subsystem for Linux (WSL) generates a HTTPS self-signed cert. To configure the Windows certificate store to trust the WSL certificate:</span></span>

* <span data-ttu-id="0e6f8-264">Выполните следующую команду, чтобы экспортировать созданный сертификат WSL:`dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span><span class="sxs-lookup"><span data-stu-id="0e6f8-264">Run the following command to export the WSL generated certificate: `dotnet dev-certs https -ep %USERPROFILE%\.aspnet\https\aspnetapp.pfx -p <cryptic-password>`</span></span>
* <span data-ttu-id="0e6f8-265">В окне WSL выполните следующую команду:`ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span><span class="sxs-lookup"><span data-stu-id="0e6f8-265">In a WSL window, run the following command: `ASPNETCORE_Kestrel__Certificates__Default__Password="<cryptic-password>" ASPNETCORE_Kestrel__Certificates__Default__Path=/mnt/c/Users/user-name/.aspnet/https/aspnetapp.pfx dotnet watch run`</span></span>

  <span data-ttu-id="0e6f8-266">Приведенная выше команда задает переменные среды, чтобы в Linux использовался доверенный сертификат Windows.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-266">The preceding command sets the environment variables so Linux uses the Windows trusted certificate.</span></span>

## <a name="troubleshoot-certificate-problems"></a><span data-ttu-id="0e6f8-267">Устранение неполадок с сертификатами</span><span class="sxs-lookup"><span data-stu-id="0e6f8-267">Troubleshoot certificate problems</span></span>

<span data-ttu-id="0e6f8-268">В этом разделе содержатся сведения о том, когда сертификат разработки ASP.NET Core HTTPS [установлен и является доверенным](#trust), но по-прежнему имеются предупреждения браузера о том, что сертификат не является доверенным.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-268">This section provides help when the ASP.NET Core HTTPS development certificate has been [installed and trusted](#trust), but you still have browser warnings that the certificate is not trusted.</span></span>

### <a name="all-platforms---certificate-not-trusted"></a><span data-ttu-id="0e6f8-269">Все платформы — сертификат не является доверенным</span><span class="sxs-lookup"><span data-stu-id="0e6f8-269">All platforms - certificate not trusted</span></span>

<span data-ttu-id="0e6f8-270">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-270">Run the following commands:</span></span>

```dotnetcli
dotnet devcerts https --clean
dotnet devcerts https --trust
```

<span data-ttu-id="0e6f8-271">Закройте все открытые экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-271">Close any browser instances open.</span></span> <span data-ttu-id="0e6f8-272">Откройте новое окно браузера для приложения.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-272">Open a new browser window to app.</span></span> <span data-ttu-id="0e6f8-273">Доверие сертификатов кэшируется браузерами.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-273">Certificate trust is cached by browsers.</span></span>

<span data-ttu-id="0e6f8-274">Предыдущие команды решают большинство проблем с доверием к браузеру.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-274">The preceding commands solve most browser trust issues.</span></span> <span data-ttu-id="0e6f8-275">Если браузер все еще не доверяет сертификату, следуйте приведенным ниже рекомендациям для платформы.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-275">If the browser is still not trusting the certificate, follow the platform specific suggestions that follow.</span></span>

### <a name="docker---certificate-not-trusted"></a><span data-ttu-id="0e6f8-276">DOCKER — сертификат не является доверенным</span><span class="sxs-lookup"><span data-stu-id="0e6f8-276">Docker - certificate not trusted</span></span>

* <span data-ttu-id="0e6f8-277">Удалите папку *\аппдата\роаминг\асп.нет\хттпс\{C:\Users User}* .</span><span class="sxs-lookup"><span data-stu-id="0e6f8-277">Delete the *C:\Users\{USER}\AppData\Roaming\ASP.NET\Https* folder.</span></span>
* <span data-ttu-id="0e6f8-278">Очистите решение.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-278">Clean the solution.</span></span> <span data-ttu-id="0e6f8-279">Удалите папки *bin* и *obj*.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-279">Delete the *bin* and *obj* folders.</span></span>
* <span data-ttu-id="0e6f8-280">Перезапустите средство разработки.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-280">Restart the development tool.</span></span> <span data-ttu-id="0e6f8-281">Например, Visual Studio, Visual Studio Code или Visual Studio для Mac.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-281">For example, Visual Studio, Visual Studio Code, or Visual Studio for Mac.</span></span>

### <a name="windows---certificate-not-trusted"></a><span data-ttu-id="0e6f8-282">Windows — сертификат не является доверенным</span><span class="sxs-lookup"><span data-stu-id="0e6f8-282">Windows - certificate not trusted</span></span>

* <span data-ttu-id="0e6f8-283">Проверьте сертификаты в хранилище сертификатов.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-283">Check the certificates in the certificate store.</span></span> <span data-ttu-id="0e6f8-284">Должен быть `localhost` сертификат `ASP.NET Core HTTPS development certificate` с понятным именем в `Current User > Personal > Certificates` и`Current User > Trusted root certification authorities > Certificates`</span><span class="sxs-lookup"><span data-stu-id="0e6f8-284">There should be a `localhost` certificate with the `ASP.NET Core HTTPS development certificate` friendly name both under `Current User > Personal > Certificates` and `Current User > Trusted root certification authorities > Certificates`</span></span>
* <span data-ttu-id="0e6f8-285">Удалите все найденные сертификаты из личных и доверенных корневых центров сертификации.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-285">Remove all the found certificates from both Personal and Trusted root certification authorities.</span></span> <span data-ttu-id="0e6f8-286">**Не** удаляйте сертификат IIS Express localhost.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-286">Do **not** remove the IIS Express localhost certificate.</span></span>
* <span data-ttu-id="0e6f8-287">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-287">Run the following commands:</span></span>

```dotnetcli
dotnet devcerts https --clean
dotnet devcerts https --trust
```

<span data-ttu-id="0e6f8-288">Закройте все открытые экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-288">Close any browser instances open.</span></span> <span data-ttu-id="0e6f8-289">Откройте новое окно браузера для приложения.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-289">Open a new browser window to app.</span></span>

### <a name="os-x---certificate-not-trusted"></a><span data-ttu-id="0e6f8-290">OS X — сертификат не является доверенным</span><span class="sxs-lookup"><span data-stu-id="0e6f8-290">OS X - certificate not trusted</span></span>

* <span data-ttu-id="0e6f8-291">Откройте доступ к цепочке ключей.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-291">Open KeyChain Access.</span></span>
* <span data-ttu-id="0e6f8-292">Выберите цепочку ключей системы.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-292">Select the System keychain.</span></span>
* <span data-ttu-id="0e6f8-293">Проверьте наличие сертификата localhost.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-293">Check for the presence of a localhost certificate.</span></span>
* <span data-ttu-id="0e6f8-294">Убедитесь, что он содержит `+` символ на значке, чтобы указать, что он является доверенным для всех пользователей.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-294">Check that it contains a `+` symbol on the icon to indicate its trusted for all users.</span></span>
* <span data-ttu-id="0e6f8-295">Удалите сертификат из цепочки ключей системы.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-295">Remove the certificate from the system keychain.</span></span>
* <span data-ttu-id="0e6f8-296">Выполните следующие команды:</span><span class="sxs-lookup"><span data-stu-id="0e6f8-296">Run the following commands:</span></span>

```dotnetcli
dotnet devcerts https --clean
dotnet devcerts https --trust
```

<span data-ttu-id="0e6f8-297">Закройте все открытые экземпляры браузера.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-297">Close any browser instances open.</span></span> <span data-ttu-id="0e6f8-298">Откройте новое окно браузера для приложения.</span><span class="sxs-lookup"><span data-stu-id="0e6f8-298">Open a new browser window to app.</span></span>

## <a name="additional-information"></a><span data-ttu-id="0e6f8-299">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="0e6f8-299">Additional information</span></span>

* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="0e6f8-300">ASP.NET Core узла в Linux с помощью Apache: Конфигурация HTTPS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-300">Host ASP.NET Core on Linux with Apache: HTTPS configuration</span></span>](xref:host-and-deploy/linux-apache#https-configuration)
* [<span data-ttu-id="0e6f8-301">ASP.NET Core узла в Linux с помощью nginx: Конфигурация HTTPS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-301">Host ASP.NET Core on Linux with Nginx: HTTPS configuration</span></span>](xref:host-and-deploy/linux-nginx#https-configuration)
* [<span data-ttu-id="0e6f8-302">Настройка SSL в службах IIS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-302">How to Set Up SSL on IIS</span></span>](/iis/manage/configuring-security/how-to-set-up-ssl-on-iis)
* [<span data-ttu-id="0e6f8-303">Поддержка браузера OWASP HSTS</span><span class="sxs-lookup"><span data-stu-id="0e6f8-303">OWASP HSTS browser support</span></span>](https://www.owasp.org/index.php/HTTP_Strict_Transport_Security_Cheat_Sheet#Browser_Support)
