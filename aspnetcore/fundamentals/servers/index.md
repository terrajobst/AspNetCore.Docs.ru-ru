---
title: "Реализации веб-сервера в ASP.NET Core"
author: tdykstra
description: "Статья представляет веб-серверы Kestrel и WebListener для ASP.NET Core. Рекомендации по выбору одного из веб-серверов и информация о том, когда следует использовать веб-сервер с обратным прокси-сервером."
keywords: "ASP.NET Core, IServer, веб-сервер, Kestrel, WebListener, обратный прокси-сервер"
ms.author: tdykstra
manager: wpickett
ms.date: 08/03/2017
ms.topic: article
ms.assetid: dba74f39-58cd-4dee-a061-6d15f7346959
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/index
ms.openlocfilehash: 17124f1ef181a4f1572d9375ae8cd27ce8845016
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="247d9-105">Реализации веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="247d9-105">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="247d9-106">Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="247d9-106">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="247d9-107">Приложение ASP.NET Core выполняется вместе с реализацией HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="247d9-107">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="247d9-108">Реализация сервера прослушивает HTTP-запросы и передает их в приложение как наборы [функций запросов](https://docs.microsoft.com/aspnet/core/fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="247d9-108">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="247d9-109">ASP.NET Core предоставляет две реализации серверов.</span><span class="sxs-lookup"><span data-stu-id="247d9-109">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="247d9-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="247d9-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="247d9-111">[Kestrel](kestrel.md) — это межплатформенный HTTP-сервер на основе [libuv](https://github.com/libuv/libuv), межплатформенной библиотеки асинхронных операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="247d9-111">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="247d9-112">[HTTP.sys](httpsys.md) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="247d9-112">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="247d9-113">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="247d9-113">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="247d9-114">[Kestrel](kestrel.md) — это межплатформенный HTTP-сервер на основе [libuv](https://github.com/libuv/libuv), межплатформенной библиотеки асинхронных операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="247d9-114">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="247d9-115">[WebListener](weblistener.md) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="247d9-115">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="247d9-116">Kestrel</span><span class="sxs-lookup"><span data-stu-id="247d9-116">Kestrel</span></span>

<span data-ttu-id="247d9-117">Kestrel — это веб-сервер, который по умолчанию включается в шаблоны новых проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="247d9-117">Kestrel is the web server that is included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="247d9-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="247d9-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="247d9-119">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="247d9-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="247d9-120">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="247d9-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="247d9-123">Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="247d9-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="247d9-124">Сведения об использовании Kestrel с обратным прокси-сервером см. в статье [Введение в Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-124">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="247d9-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="247d9-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="247d9-126">Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.</span><span class="sxs-lookup"><span data-stu-id="247d9-126">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="247d9-128">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*.</span><span class="sxs-lookup"><span data-stu-id="247d9-128">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="247d9-129">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки, как показано на следующей схеме.</span><span class="sxs-lookup"><span data-stu-id="247d9-129">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="247d9-131">Основная причина использования обратного прокси-сервера для развертывания пограничного сервера (открытого для Интернет-трафика) — это безопасность.</span><span class="sxs-lookup"><span data-stu-id="247d9-131">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="247d9-132">Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="247d9-132">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="247d9-133">Сюда входят, например, соответствующее время ожидания, предельные размеры и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="247d9-133">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="247d9-134">Сведения об использовании Kestrel с обратным прокси-сервером см. в статье [Введение в Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-134">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="247d9-135">Использовать IIS, Nginx или Apache без Kestrel или какой-либо [реализации пользовательского сервера](#custom-servers) нельзя.</span><span class="sxs-lookup"><span data-stu-id="247d9-135">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="247d9-136">ASP.NET Core рассчитан на выполнение в качестве отдельного процесса, что позволяет ему на разных платформах вести себя одинаково.</span><span class="sxs-lookup"><span data-stu-id="247d9-136">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="247d9-137">IIS, Nginx и Apache определяют собственный процесс запуска и среду; чтобы использовать их напрямую, ASP.NET Core необходимо адаптировать к требованиям каждого из них.</span><span class="sxs-lookup"><span data-stu-id="247d9-137">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="247d9-138">Применение веб-сервера, такого как Kestrel, позволяет ASP.NET Core контролировать процесс запуска и среду.</span><span class="sxs-lookup"><span data-stu-id="247d9-138">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="247d9-139">Вот почему вместо того, чтобы адаптировать ASP.NET Core для IIS, Nginx или Apache, можно настроить их таким образом, чтобы они передавали запросы в Kestrel через прокси.</span><span class="sxs-lookup"><span data-stu-id="247d9-139">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="247d9-140">Эта схема позволяет использовать классы `Program.Main` и `Startup` практически без изменений независимо от места развертывания.</span><span class="sxs-lookup"><span data-stu-id="247d9-140">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="247d9-141">IIS с Kestrel</span><span class="sxs-lookup"><span data-stu-id="247d9-141">IIS with Kestrel</span></span>

<span data-ttu-id="247d9-142">При использовании IIS или IIS Express в качестве обратного прокси-сервера для ASP.NET Core приложение ASP.NET Core выполняется отдельно от рабочего процесса IIS.</span><span class="sxs-lookup"><span data-stu-id="247d9-142">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="247d9-143">В процессе IIS для координации связи с обратным прокси-сервером выполняется специальный модуль IIS.</span><span class="sxs-lookup"><span data-stu-id="247d9-143">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="247d9-144">Это *модуль ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="247d9-144">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="247d9-145">Основные функции модуля ASP.NET Core — это запуск приложения ASP.NET Core, его перезапуск в случае сбоя и перенаправление HTTP-трафика в это приложение.</span><span class="sxs-lookup"><span data-stu-id="247d9-145">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="247d9-146">Дополнительные сведения см. в статье [Модуль ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-146">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="247d9-147">Nginx с Kestrel</span><span class="sxs-lookup"><span data-stu-id="247d9-147">Nginx with Kestrel</span></span>

<span data-ttu-id="247d9-148">Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Публикация в рабочей среде Linux](../../publishing/linuxproduction.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-148">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Publish to a Linux Production Environment](../../publishing/linuxproduction.md).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="247d9-149">Apache с Kestrel</span><span class="sxs-lookup"><span data-stu-id="247d9-149">Apache with Kestrel</span></span>

<span data-ttu-id="247d9-150">Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Использование веб-сервера Apache в качестве обратного прокси-сервера](../../publishing/apache-proxy.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-150">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Using Apache Web Server as a reverse proxy](../../publishing/apache-proxy.md).</span></span>

## <a name="httpsys"></a><span data-ttu-id="247d9-151">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="247d9-151">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="247d9-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="247d9-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="247d9-153">Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="247d9-153">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="247d9-154">HTTP.sys подойдет для сценариев, когда приложение имеет доступ к Интернету и вам требуются функции HTTP.sys, которые Kestrel не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="247d9-154">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="247d9-156">HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="247d9-156">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="247d9-158">Для оптимальной производительности в сценариях с внутренней сетью обычно рекомендуется использовать Kestrel, но в некоторых случаях может потребоваться функция, которую предоставляет только HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="247d9-158">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="247d9-159">Сведения о функциях HTTP.sys см. в разделе [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-159">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="247d9-160">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="247d9-160">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="247d9-161">В ASP.NET Core 1.x HTTP.sys называется WebListener.</span><span class="sxs-lookup"><span data-stu-id="247d9-161">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="247d9-162">Если приложение ASP.NET Core запускается в Windows, в случаях, когда приложение должно иметь доступ в Интернет, но использовать IIS нельзя, пригодится WebListener.</span><span class="sxs-lookup"><span data-stu-id="247d9-162">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="247d9-164">WebListener можно также использовать вместо Kestrel для приложений, которые имеют доступ только к внутренней сети, если вам требуются функции Weblistener, которые Kestrel не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="247d9-164">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="247d9-166">Для оптимальной производительности в сценариях с внутренней сетью обычно рекомендуется использовать Kestrel, но в некоторых случаях может потребоваться функция, которую предоставляет только Weblistener.</span><span class="sxs-lookup"><span data-stu-id="247d9-166">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="247d9-167">Сведения о функциях WebListener см. в разделе [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="247d9-167">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="247d9-168">Примечания об инфраструктуре сервера ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="247d9-168">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="247d9-169">[`IApplicationBuilder`](https://docs.microsoft.com/aspnet/core/api), доступный к методе `Configure` класса `Startup`, предоставляет свойство `ServerFeatures` типа [`IFeatureCollection`](https://docs.microsoft.com/aspnet/core/api).</span><span class="sxs-lookup"><span data-stu-id="247d9-169">The [`IApplicationBuilder`](https://docs.microsoft.com/aspnet/core/api) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](https://docs.microsoft.com/aspnet/core/api).</span></span> <span data-ttu-id="247d9-170">И Kestrel, и WebListener предоставляют только один компонент, [`IServerAddressesFeature`](https://docs.microsoft.com/aspnet/core/api), но различные реализации серверов могут предоставлять дополнительные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="247d9-170">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](https://docs.microsoft.com/aspnet/core/api), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="247d9-171">`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="247d9-171">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="247d9-172">Пользовательские серверы</span><span class="sxs-lookup"><span data-stu-id="247d9-172">Custom servers</span></span>

<span data-ttu-id="247d9-173">Если встроенные серверы не отвечают вашим требованиям, можно создать реализацию пользовательского сервера.</span><span class="sxs-lookup"><span data-stu-id="247d9-173">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="247d9-174">В [руководстве по открытому веб-интерфейсу .NET (OWIN)](../owin.md) демонстрируется запись реализации [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) на основе [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="247d9-174">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="247d9-175">Вы можете реализовать интерфейсы только тех компонентов, которые требуются вашему приложению, но как минимум требуется поддержка [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) и [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="247d9-175">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="247d9-176">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="247d9-176">Next steps</span></span>

<span data-ttu-id="247d9-177">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="247d9-177">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="247d9-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="247d9-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="247d9-179">Kestrel</span><span class="sxs-lookup"><span data-stu-id="247d9-179">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="247d9-180">Kestrel с IIS</span><span class="sxs-lookup"><span data-stu-id="247d9-180">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="247d9-181">Kestrel с Nginx</span><span class="sxs-lookup"><span data-stu-id="247d9-181">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="247d9-182">Kestrel с Apache</span><span class="sxs-lookup"><span data-stu-id="247d9-182">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="247d9-183">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="247d9-183">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="247d9-184">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="247d9-184">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="247d9-185">Kestrel</span><span class="sxs-lookup"><span data-stu-id="247d9-185">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="247d9-186">Kestrel с IIS</span><span class="sxs-lookup"><span data-stu-id="247d9-186">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="247d9-187">Kestrel с Nginx</span><span class="sxs-lookup"><span data-stu-id="247d9-187">Kestrel with Nginx</span></span>](../../publishing/linuxproduction.md)
- [<span data-ttu-id="247d9-188">Kestrel с Apache</span><span class="sxs-lookup"><span data-stu-id="247d9-188">Kestrel with Apache</span></span>](../../publishing/apache-proxy.md)
- [<span data-ttu-id="247d9-189">WebListener</span><span class="sxs-lookup"><span data-stu-id="247d9-189">WebListener</span></span>](weblistener.md)

---
