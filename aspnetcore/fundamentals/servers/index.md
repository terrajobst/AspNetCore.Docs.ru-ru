---
title: Реализации веб-сервера в ASP.NET Core
author: rick-anderson
description: Откройте возможности веб-серверов Kestrel и HTTP.sys для ASP.NET Core. Рекомендации по выбору сервера и сведения о сценариях использования обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 0f1460af5bc1cd879ff11e43775ac16ca36b150e
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/18/2018
ms.locfileid: "46011759"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="619c0-104">Реализации веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="619c0-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="619c0-105">Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="619c0-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="619c0-106">Приложение ASP.NET Core выполняется вместе с внутрипроцессной реализацией HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="619c0-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="619c0-107">Реализация сервера прослушивает HTTP-запросы и передает их в приложение как наборы [функций запросов](xref:fundamentals/request-features), объединенных в [HttpContext](/dotnet/api/system.web.httpcontext).</span><span class="sxs-lookup"><span data-stu-id="619c0-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an [HttpContext](/dotnet/api/system.web.httpcontext).</span></span>

<span data-ttu-id="619c0-108">ASP.NET Core предоставляет две реализации серверов.</span><span class="sxs-lookup"><span data-stu-id="619c0-108">ASP.NET Core ships two server implementations:</span></span>

* <span data-ttu-id="619c0-109">[Kestrel](xref:fundamentals/servers/kestrel) — кросс-платформенный HTTP-сервер по умолчанию для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="619c0-109">[Kestrel](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server for ASP.NET Core.</span></span>
* <span data-ttu-id="619c0-110">[HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="619c0-110">[HTTP.sys](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="619c0-111">(В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).)</span><span class="sxs-lookup"><span data-stu-id="619c0-111">(HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.)</span></span>

## <a name="kestrel"></a><span data-ttu-id="619c0-112">Kestrel</span><span class="sxs-lookup"><span data-stu-id="619c0-112">Kestrel</span></span>

<span data-ttu-id="619c0-113">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="619c0-113">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="619c0-114">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="619c0-114">Kestrel can be used by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="619c0-115">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="619c0-115">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="619c0-118">Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше.</span><span class="sxs-lookup"><span data-stu-id="619c0-118">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="619c0-119">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="619c0-119">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="619c0-120">Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel сам по себе.</span><span class="sxs-lookup"><span data-stu-id="619c0-120">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel взаимодействует с внутренней сетью напрямую](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="619c0-122">Если приложение имеет доступ к Интернету, Kestrel должен использовать IIS, Nginx или Apache как *обратный прокси-сервер*.</span><span class="sxs-lookup"><span data-stu-id="619c0-122">If the app is exposed to the Internet, Kestrel must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="619c0-123">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки, как показано на следующей схеме.</span><span class="sxs-lookup"><span data-stu-id="619c0-123">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="619c0-125">Основная причина использования обратного прокси-сервера для развертывания пограничного сервера (открытого для Интернет-трафика) — это безопасность.</span><span class="sxs-lookup"><span data-stu-id="619c0-125">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="619c0-126">Версии Kestrel 1.x не обладают важными функциями безопасности для защиты от атак из Интернета.</span><span class="sxs-lookup"><span data-stu-id="619c0-126">The 1.x versions of Kestrel don't have important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="619c0-127">Сюда входят, например, соответствующее время ожидания, предельные размеры запроса и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="619c0-127">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="619c0-128">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="619c0-128">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

<span data-ttu-id="619c0-129">IIS, Nginx или Apache нельзя использовать без Kestrel или какой-либо [реализации пользовательского сервера](#custom-servers).</span><span class="sxs-lookup"><span data-stu-id="619c0-129">IIS, Nginx, and Apache can't be used without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="619c0-130">ASP.NET Core рассчитан на выполнение в качестве отдельного процесса, что позволяет ему на разных платформах вести себя одинаково.</span><span class="sxs-lookup"><span data-stu-id="619c0-130">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="619c0-131">IIS, Nginx и Apache сами определяют свою процедуру запуска и среду.</span><span class="sxs-lookup"><span data-stu-id="619c0-131">IIS, Nginx, and Apache dictate their own startup procedure and environment.</span></span> <span data-ttu-id="619c0-132">Чтобы использовать эти технологии сервера напрямую, ASP.NET Core придется адаптироваться к требованиям каждого сервера.</span><span class="sxs-lookup"><span data-stu-id="619c0-132">To use these server technologies directly, ASP.NET Core would need to adapt to the requirements of each server.</span></span> <span data-ttu-id="619c0-133">Реализация веб-сервера, такого как Kestrel, позволяет ASP.NET Core контролировать процесс запуска и среду при размещении на различных технологиях сервера.</span><span class="sxs-lookup"><span data-stu-id="619c0-133">Using a web server implementation, such as Kestrel, ASP.NET Core has control over the startup process and environment when hosted on different server technologies.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="619c0-134">IIS с Kestrel</span><span class="sxs-lookup"><span data-stu-id="619c0-134">IIS with Kestrel</span></span>

<span data-ttu-id="619c0-135">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) в качестве обратного прокси-сервера для ASP.NET Core приложение ASP.NET Core выполняется отдельно от рабочего процесса IIS.</span><span class="sxs-lookup"><span data-stu-id="619c0-135">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="619c0-136">В процессе IIS [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) координирует связь с обратным прокси-сервером.</span><span class="sxs-lookup"><span data-stu-id="619c0-136">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="619c0-137">Основные функции модуля ASP.NET Core — это запуск приложения ASP.NET Core, его перезапуск в случае сбоя и перенаправление HTTP-трафика в это приложение.</span><span class="sxs-lookup"><span data-stu-id="619c0-137">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="619c0-138">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="619c0-138">For more information, see [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="619c0-139">Nginx с Kestrel</span><span class="sxs-lookup"><span data-stu-id="619c0-139">Nginx with Kestrel</span></span>

<span data-ttu-id="619c0-140">Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="619c0-140">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="619c0-141">Apache с Kestrel</span><span class="sxs-lookup"><span data-stu-id="619c0-141">Apache with Kestrel</span></span>

<span data-ttu-id="619c0-142">Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="619c0-142">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="619c0-143">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="619c0-143">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="619c0-144">Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="619c0-144">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="619c0-145">Как правило, для оптимальной производительности рекомендуется Kestrel.</span><span class="sxs-lookup"><span data-stu-id="619c0-145">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="619c0-146">HTTP.sys можно использовать в сценариях, где приложение имеет доступ к Интернету и необходимые возможности поддерживаются HTTP.sys, но не Kestrel.</span><span class="sxs-lookup"><span data-stu-id="619c0-146">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="619c0-147">Сведения о HTTP.sys см. [здесь](xref:fundamentals/servers/httpsys).</span><span class="sxs-lookup"><span data-stu-id="619c0-147">For information on HTTP.sys, see [HTTP.sys](xref:fundamentals/servers/httpsys).</span></span>

![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="619c0-149">HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="619c0-149">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="619c0-151">В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="619c0-151">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="619c0-152">Если приложения ASP.NET Core запускаются в Windows, WebListener можно использовать в случаях, когда служба IIS недоступна для размещения приложений.</span><span class="sxs-lookup"><span data-stu-id="619c0-152">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="619c0-154">WebListener можно также использовать вместо Kestrel для приложений, которые имеют доступ только к внутренней сети, если вам требуются возможности, поддерживаемые WebListener, но не Kestrel.</span><span class="sxs-lookup"><span data-stu-id="619c0-154">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="619c0-155">Сведения о WebListener см. [здесь](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="619c0-155">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener взаимодействует с внутренней сетью напрямую](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="619c0-157">Инфраструктура сервера ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="619c0-157">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="619c0-158">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) в методе `Startup.Configure` предоставляет свойство [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) типа [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="619c0-158">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="619c0-159">Kestrel и HTTP.sys (WebListener в ASP.NET Core 1.x) предоставляют только один компонент, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), но разные реализации сервера могут предоставлять дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="619c0-159">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="619c0-160">`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="619c0-160">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="619c0-161">Пользовательские серверы</span><span class="sxs-lookup"><span data-stu-id="619c0-161">Custom servers</span></span>

<span data-ttu-id="619c0-162">Если встроенные серверы не отвечают требованиям приложения, можно создать реализацию пользовательского сервера.</span><span class="sxs-lookup"><span data-stu-id="619c0-162">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="619c0-163">В [руководстве по открытому веб-интерфейсу .NET (OWIN)](xref:fundamentals/owin) демонстрируется запись реализации [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) на основе [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="619c0-163">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="619c0-164">Требуют реализации только интерфейсы компонентов, используемых приложением, но как минимум должны поддерживаться [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) и [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="619c0-164">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="619c0-165">Запуск сервера</span><span class="sxs-lookup"><span data-stu-id="619c0-165">Server startup</span></span>

<span data-ttu-id="619c0-166">При использовании [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio для Mac](https://www.visualstudio.com/vs/mac/), или [Visual Studio Code](https://code.visualstudio.com/) сервер запускается при запуске приложения интегрированной средой разработки (IDE).</span><span class="sxs-lookup"><span data-stu-id="619c0-166">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="619c0-167">В Visual Studio в Windows профили запуска можно использовать для запуска приложения и сервера с помощью [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) или консоли.</span><span class="sxs-lookup"><span data-stu-id="619c0-167">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="619c0-168">В Visual Studio Code приложение и сервер запускаются [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), который активирует отладчик CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="619c0-168">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="619c0-169">При использовании Visual Studio для Mac приложение и сервер запускаются отладчиком [Mono Soft Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="619c0-169">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="619c0-170">При запуске приложения из командной строки в папке проекта [dotnet run](/dotnet/core/tools/dotnet-run) запускает приложение и сервер (только Kestrel и HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="619c0-170">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="619c0-171">Конфигурация определяется параметром `-c|--configuration`, который может принимать значение `Debug` (по умолчанию) или `Release`.</span><span class="sxs-lookup"><span data-stu-id="619c0-171">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="619c0-172">Если профили запуска указаны в файле *launchSettings.json*, используйте параметр `--launch-profile <NAME>`, чтобы настроить профиль запуска (например, `Development` или `Production`).</span><span class="sxs-lookup"><span data-stu-id="619c0-172">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="619c0-173">Дополнительные сведения см. в разделах [dotnet run](/dotnet/core/tools/dotnet-run) и [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="619c0-173">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="619c0-174">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="619c0-174">HTTP/2 support</span></span>

<span data-ttu-id="619c0-175">[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания:</span><span class="sxs-lookup"><span data-stu-id="619c0-175">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="619c0-176">Kestrel</span><span class="sxs-lookup"><span data-stu-id="619c0-176">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="619c0-177">Операционная система</span><span class="sxs-lookup"><span data-stu-id="619c0-177">Operating system</span></span>
    * <span data-ttu-id="619c0-178">Windows Server 2012 R2/Windows 8.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="619c0-178">Windows Server 2012 R2/Windows 8.1 or later</span></span>
    * <span data-ttu-id="619c0-179">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии)</span><span class="sxs-lookup"><span data-stu-id="619c0-179">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="619c0-180">HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="619c0-180">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="619c0-181">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="619c0-181">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="619c0-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="619c0-182">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="619c0-183">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="619c0-183">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="619c0-184">Требуемая версия .NET Framework: не применимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="619c0-184">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="619c0-185">IIS (внутрипроцессный)</span><span class="sxs-lookup"><span data-stu-id="619c0-185">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="619c0-186">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="619c0-186">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="619c0-187">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="619c0-187">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="619c0-188">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="619c0-188">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="619c0-189">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="619c0-189">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="619c0-190">Подключения Edge используют протокол HTTP/2, но подключения к Kestrel через обратный прокси-сервер выполняются по HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="619c0-190">Edge connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="619c0-191">Требуемая версия .NET Framework: не применимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="619c0-191">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="619c0-192">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="619c0-192">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="619c0-193">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="619c0-193">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="619c0-194">Требуемая версия .NET Framework: не применимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="619c0-194">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="619c0-195">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="619c0-195">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="619c0-196">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="619c0-196">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="619c0-197">Подключения Edge используют протокол HTTP/2, но подключения к Kestrel через обратный прокси-сервер выполняются по HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="619c0-197">Edge connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="619c0-198">Требуемая версия .NET Framework: не применимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="619c0-198">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="619c0-199">Подключение HTTP/2 должно использовать [согласование протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) и TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="619c0-199">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="619c0-200">Дополнительные сведения см. в разделах, о конкретных сценариях развертывания сервера.</span><span class="sxs-lookup"><span data-stu-id="619c0-200">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="619c0-201">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="619c0-201">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="619c0-202"><xref:fundamentals/servers/httpsys> (или <xref:fundamentals/servers/weblistener> для ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="619c0-202"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
