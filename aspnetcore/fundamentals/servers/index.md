---
title: Реализации веб-сервера в ASP.NET Core
author: guardrex
description: Откройте возможности веб-серверов Kestrel и HTTP.sys для ASP.NET Core. Рекомендации по выбору сервера и сведения о сценариях использования обратного прокси-сервера.
ms.author: tdykstra
ms.custom: mvc
ms.date: 12/01/2018
uid: fundamentals/servers/index
ms.openlocfilehash: 965d69dd071ec71d283284d58e6e1a6e78604f90
ms.sourcegitcommit: 9bb58d7c8dad4bbd03419bcc183d027667fefa20
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/04/2018
ms.locfileid: "52861360"
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="a64ae-104">Реализации веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a64ae-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="a64ae-105">Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="a64ae-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="a64ae-106">Приложение ASP.NET Core выполняется вместе с внутрипроцессной реализацией HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="a64ae-106">An ASP.NET Core app runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="a64ae-107">Реализация сервера прослушивает HTTP-запросы и передает их в приложение как наборы [функций запросов](xref:fundamentals/request-features), объединенных в <xref:Microsoft.AspNetCore.Http.HttpContext>.</span><span class="sxs-lookup"><span data-stu-id="a64ae-107">The server implementation listens for HTTP requests and surfaces them to the app as sets of [request features](xref:fundamentals/request-features) composed into an <xref:Microsoft.AspNetCore.Http.HttpContext>.</span></span>

::: moniker range=">= aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="a64ae-108">Windows</span><span class="sxs-lookup"><span data-stu-id="a64ae-108">Windows</span></span>](#tab/windows)

<span data-ttu-id="a64ae-109">В состав ASP.NET Core входит следующее:</span><span class="sxs-lookup"><span data-stu-id="a64ae-109">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="a64ae-110">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — кроссплатформенный HTTP-сервер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a64ae-110">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="a64ae-111">HTTP-сервер IIS (`IISHttpServer`) является [внутрипроцессной реализацией IIS-сервера](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model), которая используется с [модулем ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="a64ae-111">IIS HTTP Server (`IISHttpServer`) is an [IIS in-process server](xref:fundamentals/servers/aspnet-core-module#in-process-hosting-model) implementation used with the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span>
* <span data-ttu-id="a64ae-112">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="a64ae-112">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a64ae-113">В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="a64ae-113">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="a64ae-114">macOS</span><span class="sxs-lookup"><span data-stu-id="a64ae-114">macOS</span></span>](#tab/macos)

<span data-ttu-id="a64ae-115">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a64ae-115">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a64ae-116">Linux</span><span class="sxs-lookup"><span data-stu-id="a64ae-116">Linux</span></span>](#tab/linux)

<span data-ttu-id="a64ae-117">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a64ae-117">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="windowstabwindows"></a>[<span data-ttu-id="a64ae-118">Windows</span><span class="sxs-lookup"><span data-stu-id="a64ae-118">Windows</span></span>](#tab/windows)

<span data-ttu-id="a64ae-119">В состав ASP.NET Core входит следующее:</span><span class="sxs-lookup"><span data-stu-id="a64ae-119">ASP.NET Core ships with the following:</span></span>

* <span data-ttu-id="a64ae-120">Сервер [Kestrel](xref:fundamentals/servers/kestrel) — кроссплатформенный HTTP-сервер по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a64ae-120">[Kestrel server](xref:fundamentals/servers/kestrel) is the default, cross-platform HTTP server.</span></span>
* <span data-ttu-id="a64ae-121">[Сервер HTTP.sys](xref:fundamentals/servers/httpsys) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра HTTP.sys и API HTTP-сервера](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="a64ae-121">[HTTP.sys server](xref:fundamentals/servers/httpsys) is a Windows-only HTTP server based on the [HTTP.sys kernel driver and HTTP Server API](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span> <span data-ttu-id="a64ae-122">В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="a64ae-122">HTTP.sys is called [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span>

# <a name="macostabmacos"></a>[<span data-ttu-id="a64ae-123">macOS</span><span class="sxs-lookup"><span data-stu-id="a64ae-123">macOS</span></span>](#tab/macos)

<span data-ttu-id="a64ae-124">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a64ae-124">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

# <a name="linuxtablinux"></a>[<span data-ttu-id="a64ae-125">Linux</span><span class="sxs-lookup"><span data-stu-id="a64ae-125">Linux</span></span>](#tab/linux)

<span data-ttu-id="a64ae-126">ASP.NET Core поставляется с [сервером Kestrel](xref:fundamentals/servers/kestrel), который является кроссплатформенным HTTP-сервером по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a64ae-126">ASP.NET Core ships with [Kestrel server](xref:fundamentals/servers/kestrel), which is the default, cross-platform HTTP server.</span></span>

---

::: moniker-end

## <a name="kestrel"></a><span data-ttu-id="a64ae-127">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a64ae-127">Kestrel</span></span>

<span data-ttu-id="a64ae-128">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a64ae-128">Kestrel is the default web server included in ASP.NET Core project templates.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a64ae-129">Kestrel можно использовать:</span><span class="sxs-lookup"><span data-stu-id="a64ae-129">Kestrel can be used:</span></span>

* <span data-ttu-id="a64ae-130">Сам по себе как пограничный сервер обработки запросов непосредственно из сети, включая Интернет.</span><span class="sxs-lookup"><span data-stu-id="a64ae-130">By itself as an edge server processing requests directly from a network, including the Internet.</span></span>
* <span data-ttu-id="a64ae-131">С *обратным прокси-сервером*, таким как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a64ae-131">With a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a64ae-132">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a64ae-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a64ae-135">Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше.</span><span class="sxs-lookup"><span data-stu-id="a64ae-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span> <span data-ttu-id="a64ae-136">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="a64ae-136">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a64ae-137">Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel сам по себе.</span><span class="sxs-lookup"><span data-stu-id="a64ae-137">If the app only accepts requests from an internal network, Kestrel can be used by itself.</span></span>

![Kestrel взаимодействует с внутренней сетью напрямую](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="a64ae-139">Если приложение имеет доступ к Интернету, Kestrel должен использовать *обратный прокси-сервер*, такой как [службы IIS](https://www.iis.net/), [Nginx](http://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="a64ae-139">If the app is exposed to the Internet, Kestrel must use a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](http://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="a64ae-140">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a64ae-140">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="a64ae-142">Основная причина использования обратного прокси-сервера для развертываний пограничных серверов, открытых для общего доступа, — это безопасность.</span><span class="sxs-lookup"><span data-stu-id="a64ae-142">The most important reason for using a reverse proxy for public-facing edge server deployments that are exposed directly the Internet is security.</span></span> <span data-ttu-id="a64ae-143">Версии Kestrel 1.x не обладают важными функциями безопасности для защиты от атак из Интернета.</span><span class="sxs-lookup"><span data-stu-id="a64ae-143">The 1.x versions of Kestrel don't include important security features to defend against attacks from the Internet.</span></span> <span data-ttu-id="a64ae-144">Сюда входят, например, соответствующее время ожидания, предельные размеры запроса и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="a64ae-144">This includes, but isn't limited to, appropriate timeouts, request size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="a64ae-145">Дополнительные сведения см. в статье [Использование Kestrel с обратным прокси-сервером](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span><span class="sxs-lookup"><span data-stu-id="a64ae-145">For more information, see [When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel#when-to-use-kestrel-with-a-reverse-proxy).</span></span>

::: moniker-end

### <a name="iis-with-kestrel"></a><span data-ttu-id="a64ae-146">IIS с Kestrel</span><span class="sxs-lookup"><span data-stu-id="a64ae-146">IIS with Kestrel</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="a64ae-147">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) ASP.NET Core запускает приложение в том же процессе, что и рабочий процесс IIS (модель *внутрипроцессного* размещения) или отдельном процессе из рабочего процесса IIS (модель *внепроцессного* размещения).</span><span class="sxs-lookup"><span data-stu-id="a64ae-147">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview), the ASP.NET Core app either runs in the same process as the IIS worker process (the *in-process* hosting model) or in a process separate from the IIS worker process (the *out-of-process* hosting model).</span></span>

<span data-ttu-id="a64ae-148">[Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) — это собственный модуль IIS, который обрабатывает собственные запросы IIS между HTTP-сервером IIS (внутрипроцессно) или сервером Kestrel (внепроцессно).</span><span class="sxs-lookup"><span data-stu-id="a64ae-148">The [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) is a native IIS module that handles native IIS requests between either the in-process IIS HTTP Server or the out-of-process Kestrel server.</span></span> <span data-ttu-id="a64ae-149">Дополнительные сведения см. в разделе <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a64ae-149">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="a64ae-150">При использовании [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) или [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) в качестве обратного прокси-сервера для ASP.NET Core приложение ASP.NET Core выполняется отдельно от рабочего процесса IIS.</span><span class="sxs-lookup"><span data-stu-id="a64ae-150">When using [IIS](/iis/get-started/introduction-to-iis/introduction-to-iis-architecture) or [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview) as a reverse proxy for ASP.NET Core, the ASP.NET Core app runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="a64ae-151">В процессе IIS [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) координирует связь с обратным прокси-сервером.</span><span class="sxs-lookup"><span data-stu-id="a64ae-151">In the IIS process, the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) coordinates the reverse proxy relationship.</span></span> <span data-ttu-id="a64ae-152">Основные функции модуля ASP.NET Core — это запуск приложения, его перезапуск в случае сбоя и перенаправление HTTP-трафика в это приложение.</span><span class="sxs-lookup"><span data-stu-id="a64ae-152">The primary functions of the ASP.NET Core Module are to start the app, restart the app when it crashes, and forward HTTP traffic to the app.</span></span> <span data-ttu-id="a64ae-153">Дополнительные сведения см. в разделе <xref:fundamentals/servers/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="a64ae-153">For more information, see <xref:fundamentals/servers/aspnet-core-module>.</span></span>

::: moniker-end

### <a name="nginx-with-kestrel"></a><span data-ttu-id="a64ae-154">Nginx с Kestrel</span><span class="sxs-lookup"><span data-stu-id="a64ae-154">Nginx with Kestrel</span></span>

<span data-ttu-id="a64ae-155">Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-nginx>.</span><span class="sxs-lookup"><span data-stu-id="a64ae-155">For information on how to use Nginx on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-nginx>.</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="a64ae-156">Apache с Kestrel</span><span class="sxs-lookup"><span data-stu-id="a64ae-156">Apache with Kestrel</span></span>

<span data-ttu-id="a64ae-157">Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в <xref:host-and-deploy/linux-apache>.</span><span class="sxs-lookup"><span data-stu-id="a64ae-157">For information on how to use Apache on Linux as a reverse proxy server for Kestrel, see <xref:host-and-deploy/linux-apache>.</span></span>

## <a name="httpsys"></a><span data-ttu-id="a64ae-158">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a64ae-158">HTTP.sys</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="a64ae-159">Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a64ae-159">If ASP.NET Core apps are run on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="a64ae-160">Как правило, для оптимальной производительности рекомендуется Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a64ae-160">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="a64ae-161">HTTP.sys можно использовать в сценариях, где приложение имеет доступ к Интернету и необходимые возможности поддерживаются HTTP.sys, но не Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a64ae-161">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span> <span data-ttu-id="a64ae-162">Дополнительные сведения см. в разделе <xref:fundamentals/servers/httpsys>.</span><span class="sxs-lookup"><span data-stu-id="a64ae-162">For more information, see <xref:fundamentals/servers/httpsys>.</span></span>

![HTTP.sys взаимодействует с Интернетом напрямую](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="a64ae-164">HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="a64ae-164">HTTP.sys can also be used for apps that are only exposed to an internal network.</span></span>

![HTTP.sys взаимодействует с внутренней сетью напрямую](httpsys/_static/httpsys-to-internal.png)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="a64ae-166">В ASP.NET Core 1.x HTTP.sys называется [WebListener](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="a64ae-166">HTTP.sys is named [WebListener](xref:fundamentals/servers/weblistener) in ASP.NET Core 1.x.</span></span> <span data-ttu-id="a64ae-167">Если приложения ASP.NET Core запускаются в Windows, WebListener можно использовать в случаях, когда служба IIS недоступна для размещения приложений.</span><span class="sxs-lookup"><span data-stu-id="a64ae-167">If ASP.NET Core apps are run on Windows, WebListener is an alternative for scenarios where IIS isn't available to host apps.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="a64ae-169">WebListener можно также использовать вместо Kestrel для приложений, которые имеют доступ только к внутренней сети, если вам требуются возможности, поддерживаемые WebListener, но не Kestrel.</span><span class="sxs-lookup"><span data-stu-id="a64ae-169">WebListener can also be used in place of Kestrel for apps that are only exposed to an internal network, if required capabilities are supported by WebListener but not Kestrel.</span></span> <span data-ttu-id="a64ae-170">Сведения о WebListener см. [здесь](xref:fundamentals/servers/weblistener).</span><span class="sxs-lookup"><span data-stu-id="a64ae-170">For information on WebListener, see [WebListener](xref:fundamentals/servers/weblistener).</span></span>

![Weblistener взаимодействует с внутренней сетью напрямую](weblistener/_static/weblistener-to-internal.png)

::: moniker-end

## <a name="aspnet-core-server-infrastructure"></a><span data-ttu-id="a64ae-172">Инфраструктура сервера ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a64ae-172">ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="a64ae-173">[IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) в методе `Startup.Configure` предоставляет свойство [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) типа [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="a64ae-173">The [IApplicationBuilder](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup.Configure` method exposes the [ServerFeatures](/dotnet/api/microsoft.aspnetcore.builder.iapplicationbuilder.serverfeatures) property of type [IFeatureCollection](/dotnet/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="a64ae-174">Kestrel и HTTP.sys (WebListener в ASP.NET Core 1.x) предоставляют только один компонент, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), но разные реализации сервера могут предоставлять дополнительные возможности.</span><span class="sxs-lookup"><span data-stu-id="a64ae-174">Kestrel and HTTP.sys (WebListener in ASP.NET Core 1.x) only expose a single feature each, [IServerAddressesFeature](/dotnet/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="a64ae-175">`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="a64ae-175">`IServerAddressesFeature` can be used to find out which port the server implementation has bound at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="a64ae-176">Пользовательские серверы</span><span class="sxs-lookup"><span data-stu-id="a64ae-176">Custom servers</span></span>

<span data-ttu-id="a64ae-177">Если встроенные серверы не отвечают требованиям приложения, можно создать реализацию пользовательского сервера.</span><span class="sxs-lookup"><span data-stu-id="a64ae-177">If the built-in servers don't meet the app's requirements, a custom server implementation can be created.</span></span> <span data-ttu-id="a64ae-178">В [руководстве по открытому веб-интерфейсу .NET (OWIN)](xref:fundamentals/owin) демонстрируется запись реализации [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) на основе [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="a64ae-178">The [Open Web Interface for .NET (OWIN) guide](xref:fundamentals/owin) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](/dotnet/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="a64ae-179">Требуют реализации только интерфейсы компонентов, используемых приложением, но как минимум должны поддерживаться [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) и [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="a64ae-179">Only the feature interfaces that the app uses require implementation, though at a minimum [IHttpRequestFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](/dotnet/api/microsoft.aspnetcore.http.features.ihttpresponsefeature) must be supported.</span></span>

## <a name="server-startup"></a><span data-ttu-id="a64ae-180">Запуск сервера</span><span class="sxs-lookup"><span data-stu-id="a64ae-180">Server startup</span></span>

<span data-ttu-id="a64ae-181">При использовании [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio для Mac](https://www.visualstudio.com/vs/mac/), или [Visual Studio Code](https://code.visualstudio.com/) сервер запускается при запуске приложения интегрированной средой разработки (IDE).</span><span class="sxs-lookup"><span data-stu-id="a64ae-181">When using [Visual Studio](https://www.visualstudio.com/vs/), [Visual Studio for Mac](https://www.visualstudio.com/vs/mac/), or [Visual Studio Code](https://code.visualstudio.com/), the server is launched when the app is started by the Integrated Development Environment (IDE).</span></span> <span data-ttu-id="a64ae-182">В Visual Studio в Windows профили запуска можно использовать для запуска приложения и сервера с помощью [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) или консоли.</span><span class="sxs-lookup"><span data-stu-id="a64ae-182">In Visual Studio on Windows, launch profiles can be used to start the app and server with either [IIS Express](/iis/extensions/introduction-to-iis-express/iis-express-overview)/[ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) or the console.</span></span> <span data-ttu-id="a64ae-183">В Visual Studio Code приложение и сервер запускаются [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), который активирует отладчик CoreCLR.</span><span class="sxs-lookup"><span data-stu-id="a64ae-183">In Visual Studio Code, the app and server are started by [Omnisharp](https://github.com/OmniSharp/omnisharp-vscode), which activates the CoreCLR debugger.</span></span> <span data-ttu-id="a64ae-184">При использовании Visual Studio для Mac приложение и сервер запускаются отладчиком [Mono Soft Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span><span class="sxs-lookup"><span data-stu-id="a64ae-184">Using Visual Studio for Mac, the app and server are started by the [Mono Soft-Mode Debugger](http://www.mono-project.com/docs/advanced/runtime/docs/soft-debugger/).</span></span>

<span data-ttu-id="a64ae-185">При запуске приложения из командной строки в папке проекта [dotnet run](/dotnet/core/tools/dotnet-run) запускает приложение и сервер (только Kestrel и HTTP.sys).</span><span class="sxs-lookup"><span data-stu-id="a64ae-185">When launching an app from a command prompt in the project's folder, [dotnet run](/dotnet/core/tools/dotnet-run) launches the app and server (Kestrel and HTTP.sys only).</span></span> <span data-ttu-id="a64ae-186">Конфигурация определяется параметром `-c|--configuration`, который может принимать значение `Debug` (по умолчанию) или `Release`.</span><span class="sxs-lookup"><span data-stu-id="a64ae-186">The configuration is specified by the `-c|--configuration` option, which is set to either `Debug` (default) or `Release`.</span></span> <span data-ttu-id="a64ae-187">Если профили запуска указаны в файле *launchSettings.json*, используйте параметр `--launch-profile <NAME>`, чтобы настроить профиль запуска (например, `Development` или `Production`).</span><span class="sxs-lookup"><span data-stu-id="a64ae-187">If launch profiles are present in a *launchSettings.json* file, use the `--launch-profile <NAME>` option to set the launch profile (for example, `Development` or `Production`).</span></span> <span data-ttu-id="a64ae-188">Дополнительные сведения см. в разделах [dotnet run](/dotnet/core/tools/dotnet-run) и [Упаковка дистрибутивов .NET Core](/dotnet/core/build/distribution-packaging).</span><span class="sxs-lookup"><span data-stu-id="a64ae-188">For more information, see the [dotnet run](/dotnet/core/tools/dotnet-run) and [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging) topics.</span></span>

## <a name="http2-support"></a><span data-ttu-id="a64ae-189">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="a64ae-189">HTTP/2 support</span></span>

<span data-ttu-id="a64ae-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) поддерживается в ASP.NET Core для следующих сценариев развертывания:</span><span class="sxs-lookup"><span data-stu-id="a64ae-190">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is supported with ASP.NET Core in the following deployment scenarios:</span></span>

::: moniker range=">= aspnetcore-2.2"

* [<span data-ttu-id="a64ae-191">Kestrel</span><span class="sxs-lookup"><span data-stu-id="a64ae-191">Kestrel</span></span>](xref:fundamentals/servers/kestrel#http2-support)
  * <span data-ttu-id="a64ae-192">Операционная система</span><span class="sxs-lookup"><span data-stu-id="a64ae-192">Operating system</span></span>
    * <span data-ttu-id="a64ae-193">Windows Server 2016 / Windows 10 или более поздних версий&dagger;</span><span class="sxs-lookup"><span data-stu-id="a64ae-193">Windows Server 2016/Windows 10 or later&dagger;</span></span>
    * <span data-ttu-id="a64ae-194">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="a64ae-194">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
    * <span data-ttu-id="a64ae-195">HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="a64ae-195">HTTP/2 will be supported on macOS in a future release.</span></span>
  * <span data-ttu-id="a64ae-196">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="a64ae-196">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="a64ae-197">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a64ae-197">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="a64ae-198">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="a64ae-198">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="a64ae-199">Требуемая версия .NET Framework: не применимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a64ae-199">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="a64ae-200">IIS (внутрипроцессный)</span><span class="sxs-lookup"><span data-stu-id="a64ae-200">IIS (in-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a64ae-201">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a64ae-201">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a64ae-202">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="a64ae-202">Target framework: .NET Core 2.2 or later</span></span>
* [<span data-ttu-id="a64ae-203">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="a64ae-203">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a64ae-204">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a64ae-204">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a64ae-205">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="a64ae-205">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a64ae-206">Требуемая версия .NET Framework: не применимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="a64ae-206">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

<span data-ttu-id="a64ae-207">&dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="a64ae-207">&dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="a64ae-208">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="a64ae-208">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="a64ae-209">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="a64ae-209">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* [<span data-ttu-id="a64ae-210">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="a64ae-210">HTTP.sys</span></span>](xref:fundamentals/servers/httpsys#http2-support)
  * <span data-ttu-id="a64ae-211">Windows Server 2016 / Windows 10 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="a64ae-211">Windows Server 2016/Windows 10 or later</span></span>
  * <span data-ttu-id="a64ae-212">Требуемая версия .NET Framework: не применимо к развертываниям HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="a64ae-212">Target framework: Not applicable to HTTP.sys deployments.</span></span>
* [<span data-ttu-id="a64ae-213">IIS (внепроцессный)</span><span class="sxs-lookup"><span data-stu-id="a64ae-213">IIS (out-of-process)</span></span>](xref:host-and-deploy/iis/index#http2-support)
  * <span data-ttu-id="a64ae-214">Windows Server 2016 / Windows 10 или более поздних версий; IIS 10 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="a64ae-214">Windows Server 2016/Windows 10 or later; IIS 10 or later</span></span>
  * <span data-ttu-id="a64ae-215">Подключения к пограничным серверам, открытых для общего доступа, выполняются по протоколу HTTP/2, а подключения к серверу Kestrel через обратный прокси-сервер — по протоколу HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="a64ae-215">Public-facing edge server connections use HTTP/2, but the reverse proxy connection to Kestrel uses HTTP/1.1.</span></span>
  * <span data-ttu-id="a64ae-216">Требуемая версия .NET Framework: не применимо к внепроцессным развертываниям IIS.</span><span class="sxs-lookup"><span data-stu-id="a64ae-216">Target framework: Not applicable to IIS out-of-process deployments.</span></span>

::: moniker-end

<span data-ttu-id="a64ae-217">Подключение HTTP/2 должно использовать [согласование протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) и TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="a64ae-217">An HTTP/2 connection must use [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) and TLS 1.2 or later.</span></span> <span data-ttu-id="a64ae-218">Дополнительные сведения см. в разделах, о конкретных сценариях развертывания сервера.</span><span class="sxs-lookup"><span data-stu-id="a64ae-218">For more information, see the topics that pertain to your server deployment scenarios.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a64ae-219">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a64ae-219">Additional resources</span></span>

* <xref:fundamentals/servers/kestrel>
* <xref:fundamentals/servers/aspnet-core-module>
* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <span data-ttu-id="a64ae-220"><xref:fundamentals/servers/httpsys> (или <xref:fundamentals/servers/weblistener> для ASP.NET Core 1.x)</span><span class="sxs-lookup"><span data-stu-id="a64ae-220"><xref:fundamentals/servers/httpsys> (for ASP.NET Core 1.x, see <xref:fundamentals/servers/weblistener>)</span></span>
