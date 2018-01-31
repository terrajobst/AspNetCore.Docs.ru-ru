---
title: "Реализации веб-сервера в ASP.NET Core"
author: tdykstra
description: "Статья представляет веб-серверы Kestrel и WebListener для ASP.NET Core. Рекомендации по выбору одного из веб-серверов и информация о том, когда следует использовать веб-сервер с обратным прокси-сервером."
manager: wpickett
ms.author: tdykstra
ms.date: 08/03/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/index
ms.openlocfilehash: 9e2bea396e50615bd02affad93f0ee55255d299f
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="web-server-implementations-in-aspnet-core"></a><span data-ttu-id="4d82e-104">Реализации веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d82e-104">Web server implementations in ASP.NET Core</span></span>

<span data-ttu-id="4d82e-105">Авторы: [Том Дисктра](https://github.com/tdykstra) (Tom Dykstra), [Стив Смит](https://ardalis.com/) (Steve Smith), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Крис Росс](https://github.com/Tratcher) (Chris Ross)</span><span class="sxs-lookup"><span data-stu-id="4d82e-105">By [Tom Dykstra](https://github.com/tdykstra), [Steve Smith](https://ardalis.com/), [Stephen Halter](https://twitter.com/halter73), and [Chris Ross](https://github.com/Tratcher)</span></span>

<span data-ttu-id="4d82e-106">Приложение ASP.NET Core выполняется вместе с реализацией HTTP-сервера.</span><span class="sxs-lookup"><span data-stu-id="4d82e-106">An ASP.NET Core application runs with an in-process HTTP server implementation.</span></span> <span data-ttu-id="4d82e-107">Реализация сервера прослушивает HTTP-запросы и передает их в приложение как наборы [функций запросов](https://docs.microsoft.com/aspnet/core/fundamentals/request-features), объединенных в `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="4d82e-107">The server implementation listens for HTTP requests and surfaces them to the application as sets of [request features](https://docs.microsoft.com/aspnet/core/fundamentals/request-features) composed into an `HttpContext`.</span></span>

<span data-ttu-id="4d82e-108">ASP.NET Core предоставляет две реализации серверов.</span><span class="sxs-lookup"><span data-stu-id="4d82e-108">ASP.NET Core ships two server implementations:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d82e-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* <span data-ttu-id="4d82e-110">[Kestrel](kestrel.md) — это межплатформенный HTTP-сервер на основе [libuv](https://github.com/libuv/libuv), межплатформенной библиотеки асинхронных операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="4d82e-110">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="4d82e-111">[HTTP.sys](httpsys.md) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d82e-111">[HTTP.sys](httpsys.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d82e-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="4d82e-113">[Kestrel](kestrel.md) — это межплатформенный HTTP-сервер на основе [libuv](https://github.com/libuv/libuv), межплатформенной библиотеки асинхронных операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="4d82e-113">[Kestrel](kestrel.md) is a cross-platform HTTP server based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span>

* <span data-ttu-id="4d82e-114">[WebListener](weblistener.md) — это HTTP-сервер, предназначенный только для Windows и основанный на [драйвере ядра Http.Sys](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d82e-114">[WebListener](weblistener.md) is a Windows-only HTTP server based on the [Http.Sys kernel driver](https://msdn.microsoft.com/library/windows/desktop/aa364510.aspx).</span></span>

---

## <a name="kestrel"></a><span data-ttu-id="4d82e-115">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4d82e-115">Kestrel</span></span>

<span data-ttu-id="4d82e-116">Kestrel — это веб-сервер, который по умолчанию включается в шаблоны новых проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4d82e-116">Kestrel is the web server that's included by default in ASP.NET Core new-project templates.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d82e-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4d82e-118">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="4d82e-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="4d82e-119">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="4d82e-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4d82e-122">Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="4d82e-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

<span data-ttu-id="4d82e-123">Сведения об использовании Kestrel с обратным прокси-сервером см. в статье [Введение в Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="4d82e-123">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d82e-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4d82e-125">Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.</span><span class="sxs-lookup"><span data-stu-id="4d82e-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="4d82e-127">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*.</span><span class="sxs-lookup"><span data-stu-id="4d82e-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="4d82e-128">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки, как показано на следующей схеме.</span><span class="sxs-lookup"><span data-stu-id="4d82e-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling, as shown in the following diagram.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="4d82e-130">Основная причина использования обратного прокси-сервера для развертывания пограничного сервера (открытого для Интернет-трафика) — это безопасность.</span><span class="sxs-lookup"><span data-stu-id="4d82e-130">The most important reason for using a reverse proxy for edge deployments (exposed to traffic from the Internet) is security.</span></span> <span data-ttu-id="4d82e-131">Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="4d82e-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="4d82e-132">Сюда входят, например, соответствующее время ожидания, предельные размеры и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="4d82e-132">This includes, but isn't limited to, appropriate timeouts, size limits, and concurrent connection limits.</span></span>

<span data-ttu-id="4d82e-133">Сведения об использовании Kestrel с обратным прокси-сервером см. в статье [Введение в Kestrel](kestrel.md).</span><span class="sxs-lookup"><span data-stu-id="4d82e-133">For information about when to use Kestrel with a reverse proxy, see [Introduction to Kestrel](kestrel.md).</span></span>

---

<span data-ttu-id="4d82e-134">Использовать IIS, Nginx или Apache без Kestrel или какой-либо [реализации пользовательского сервера](#custom-servers) нельзя.</span><span class="sxs-lookup"><span data-stu-id="4d82e-134">You can't use IIS, Nginx, or Apache without Kestrel or a [custom server implementation](#custom-servers).</span></span> <span data-ttu-id="4d82e-135">ASP.NET Core рассчитан на выполнение в качестве отдельного процесса, что позволяет ему на разных платформах вести себя одинаково.</span><span class="sxs-lookup"><span data-stu-id="4d82e-135">ASP.NET Core was designed to run in its own process so that it can behave consistently across platforms.</span></span> <span data-ttu-id="4d82e-136">IIS, Nginx и Apache определяют собственный процесс запуска и среду; чтобы использовать их напрямую, ASP.NET Core необходимо адаптировать к требованиям каждого из них.</span><span class="sxs-lookup"><span data-stu-id="4d82e-136">IIS, Nginx, and Apache dictate their own startup process and environment; to use them directly, ASP.NET Core would have to adapt to the needs of each one.</span></span> <span data-ttu-id="4d82e-137">Применение веб-сервера, такого как Kestrel, позволяет ASP.NET Core контролировать процесс запуска и среду.</span><span class="sxs-lookup"><span data-stu-id="4d82e-137">Using a web server implementation such as Kestrel gives ASP.NET Core control over the startup process and environment.</span></span> <span data-ttu-id="4d82e-138">Вот почему вместо того, чтобы адаптировать ASP.NET Core для IIS, Nginx или Apache, можно настроить их таким образом, чтобы они передавали запросы в Kestrel через прокси.</span><span class="sxs-lookup"><span data-stu-id="4d82e-138">So rather than trying to adapt ASP.NET Core to IIS, Nginx, or Apache, you just set up those web servers to proxy requests to Kestrel.</span></span> <span data-ttu-id="4d82e-139">Эта схема позволяет использовать классы `Program.Main` и `Startup` практически без изменений независимо от места развертывания.</span><span class="sxs-lookup"><span data-stu-id="4d82e-139">This arrangement allows your `Program.Main` and `Startup` classes to be essentially the same no matter where you deploy.</span></span>

### <a name="iis-with-kestrel"></a><span data-ttu-id="4d82e-140">IIS с Kestrel</span><span class="sxs-lookup"><span data-stu-id="4d82e-140">IIS with Kestrel</span></span>

<span data-ttu-id="4d82e-141">При использовании IIS или IIS Express в качестве обратного прокси-сервера для ASP.NET Core приложение ASP.NET Core выполняется отдельно от рабочего процесса IIS.</span><span class="sxs-lookup"><span data-stu-id="4d82e-141">When you use IIS or IIS Express as a reverse proxy for ASP.NET Core, the ASP.NET Core application runs in a process separate from the IIS worker process.</span></span> <span data-ttu-id="4d82e-142">В процессе IIS для координации связи с обратным прокси-сервером выполняется специальный модуль IIS.</span><span class="sxs-lookup"><span data-stu-id="4d82e-142">In the IIS process, a special IIS module runs to coordinate the reverse proxy relationship.</span></span>  <span data-ttu-id="4d82e-143">Это *модуль ASP.NET Core*.</span><span class="sxs-lookup"><span data-stu-id="4d82e-143">This is the *ASP.NET Core Module*.</span></span> <span data-ttu-id="4d82e-144">Основные функции модуля ASP.NET Core — это запуск приложения ASP.NET Core, его перезапуск в случае сбоя и перенаправление HTTP-трафика в это приложение.</span><span class="sxs-lookup"><span data-stu-id="4d82e-144">The primary functions of the ASP.NET Core Module are to start the ASP.NET Core application, restart it when it crashes, and forward HTTP traffic to it.</span></span> <span data-ttu-id="4d82e-145">Дополнительные сведения см. в статье [Модуль ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="4d82e-145">For more information, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

### <a name="nginx-with-kestrel"></a><span data-ttu-id="4d82e-146">Nginx с Kestrel</span><span class="sxs-lookup"><span data-stu-id="4d82e-146">Nginx with Kestrel</span></span>

<span data-ttu-id="4d82e-147">Сведения о том, как использовать Nginx в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Nginx](xref:host-and-deploy/linux-nginx).</span><span class="sxs-lookup"><span data-stu-id="4d82e-147">For information about how to use Nginx on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Nginx](xref:host-and-deploy/linux-nginx).</span></span>

### <a name="apache-with-kestrel"></a><span data-ttu-id="4d82e-148">Apache с Kestrel</span><span class="sxs-lookup"><span data-stu-id="4d82e-148">Apache with Kestrel</span></span>

<span data-ttu-id="4d82e-149">Сведения о том, как использовать Apache в Linux в качестве обратного прокси-сервера для Kestrel, см. в статье [Размещение в Linux с использованием Apache](xref:host-and-deploy/linux-apache).</span><span class="sxs-lookup"><span data-stu-id="4d82e-149">For information about how to use Apache on Linux as a reverse proxy server for Kestrel, see [Host on Linux with Apache](xref:host-and-deploy/linux-apache).</span></span>

## <a name="httpsys"></a><span data-ttu-id="4d82e-150">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d82e-150">HTTP.sys</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d82e-151">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-151">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="4d82e-152">Если приложение ASP.NET Core запускается в Windows, вместо Kestrel можно использовать HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="4d82e-152">If you run your ASP.NET Core app on Windows, HTTP.sys is an alternative to Kestrel.</span></span> <span data-ttu-id="4d82e-153">HTTP.sys подойдет для сценариев, когда приложение имеет доступ к Интернету и вам требуются функции HTTP.sys, которые Kestrel не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="4d82e-153">You can use HTTP.sys for scenarios where you expose your app to the Internet and you need HTTP.sys features that Kestrel doesn't support.</span></span> 

![HTTP.sys взаимодействует с Интернетом напрямую.](httpsys/_static/httpsys-to-internet.png)

<span data-ttu-id="4d82e-155">HTTP.sys можно также использовать для приложений, которые имеют доступ только к внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="4d82e-155">HTTP.sys can also be used for applications that are exposed only to an internal network.</span></span> 

![HTTP.sys взаимодействует с вашей внутренней сетью напрямую.](httpsys/_static/httpsys-to-internal.png)

<span data-ttu-id="4d82e-157">Для оптимальной производительности в сценариях с внутренней сетью обычно рекомендуется использовать Kestrel, но в некоторых случаях может потребоваться функция, которую предоставляет только HTTP.sys.</span><span class="sxs-lookup"><span data-stu-id="4d82e-157">For internal network scenarios, Kestrel is generally recommended for best performance; but in some scenarios, you might want to use a feature that only HTTP.sys offers.</span></span> <span data-ttu-id="4d82e-158">Сведения о функциях HTTP.sys см. в разделе [HTTP.sys](httpsys.md).</span><span class="sxs-lookup"><span data-stu-id="4d82e-158">For information about HTTP.sys features, see [HTTP.sys](httpsys.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d82e-159">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-159">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="4d82e-160">В ASP.NET Core 1.x HTTP.sys называется WebListener.</span><span class="sxs-lookup"><span data-stu-id="4d82e-160">HTTP.sys is named WebListener in ASP.NET Core 1.x.</span></span> <span data-ttu-id="4d82e-161">Если приложение ASP.NET Core запускается в Windows, в случаях, когда приложение должно иметь доступ в Интернет, но использовать IIS нельзя, пригодится WebListener.</span><span class="sxs-lookup"><span data-stu-id="4d82e-161">If you run your ASP.NET Core app on Windows, WebListener is an alternative that you can use for scenarios where you want to expose your app to the Internet but you can't use IIS.</span></span>

![Weblistener взаимодействует с Интернетом напрямую.](weblistener/_static/weblistener-to-internet.png)

<span data-ttu-id="4d82e-163">WebListener можно также использовать вместо Kestrel для приложений, которые имеют доступ только к внутренней сети, если вам требуются функции Weblistener, которые Kestrel не поддерживает.</span><span class="sxs-lookup"><span data-stu-id="4d82e-163">WebListener can also be used in place of Kestrel for applications that are exposed only to an internal network, if you need WebListener features that Kestrel doesn't support.</span></span> 

![Weblistener взаимодействует с вашей внутренней сетью напрямую.](weblistener/_static/weblistener-to-internal.png)

<span data-ttu-id="4d82e-165">Для оптимальной производительности в сценариях с внутренней сетью обычно рекомендуется использовать Kestrel, но в некоторых случаях может потребоваться функция, которую предоставляет только Weblistener.</span><span class="sxs-lookup"><span data-stu-id="4d82e-165">For internal network scenarios, Kestrel is generally recommended for best performance, but in some scenarios you might want to use a feature that only WebListener offers.</span></span> <span data-ttu-id="4d82e-166">Сведения о функциях WebListener см. в разделе [WebListener](weblistener.md).</span><span class="sxs-lookup"><span data-stu-id="4d82e-166">For information about WebListener features, see [WebListener](weblistener.md).</span></span>

---

## <a name="notes-about-aspnet-core-server-infrastructure"></a><span data-ttu-id="4d82e-167">Примечания об инфраструктуре сервера ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4d82e-167">Notes about ASP.NET Core server infrastructure</span></span>

<span data-ttu-id="4d82e-168">[`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder), доступный к методе `Configure` класса `Startup`, предоставляет свойство `ServerFeatures` типа [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span><span class="sxs-lookup"><span data-stu-id="4d82e-168">The [`IApplicationBuilder`](/aspnet/core/api/microsoft.aspnetcore.builder.iapplicationbuilder) available in the `Startup` class `Configure` method exposes the `ServerFeatures` property of type [`IFeatureCollection`](/aspnet/core/api/microsoft.aspnetcore.http.features.ifeaturecollection).</span></span> <span data-ttu-id="4d82e-169">И Kestrel, и WebListener предоставляют только один компонент, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), но различные реализации серверов могут предоставлять дополнительные функциональные возможности.</span><span class="sxs-lookup"><span data-stu-id="4d82e-169">Kestrel and WebListener both expose only a single feature, [`IServerAddressesFeature`](/aspnet/core/api/microsoft.aspnetcore.hosting.server.features.iserveraddressesfeature), but different server implementations may expose additional functionality.</span></span>

<span data-ttu-id="4d82e-170">`IServerAddressesFeature` можно использовать для того, чтобы узнать, какой порт в реализации сервера привязан к среде выполнения.</span><span class="sxs-lookup"><span data-stu-id="4d82e-170">`IServerAddressesFeature` can be used to find out which port the server implementation has bound to at runtime.</span></span>

## <a name="custom-servers"></a><span data-ttu-id="4d82e-171">Пользовательские серверы</span><span class="sxs-lookup"><span data-stu-id="4d82e-171">Custom servers</span></span>

<span data-ttu-id="4d82e-172">Если встроенные серверы не отвечают вашим требованиям, можно создать реализацию пользовательского сервера.</span><span class="sxs-lookup"><span data-stu-id="4d82e-172">If the built-in servers don't meet your needs, you can create a custom server implementation.</span></span> <span data-ttu-id="4d82e-173">В [руководстве по открытому веб-интерфейсу .NET (OWIN)](../owin.md) демонстрируется запись реализации [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) на основе [Nowin](https://github.com/Bobris/Nowin).</span><span class="sxs-lookup"><span data-stu-id="4d82e-173">The [Open Web Interface for .NET (OWIN) guide](../owin.md) demonstrates how to write a [Nowin](https://github.com/Bobris/Nowin)-based [IServer](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.server.iserver) implementation.</span></span> <span data-ttu-id="4d82e-174">Вы можете реализовать интерфейсы только тех компонентов, которые требуются вашему приложению, но как минимум требуется поддержка [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) и [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span><span class="sxs-lookup"><span data-stu-id="4d82e-174">You're free to implement just the feature interfaces your application needs, though at a minimum you must support [IHttpRequestFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttprequestfeature) and [IHttpResponseFeature](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.http.features.ihttpresponsefeature).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4d82e-175">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="4d82e-175">Next steps</span></span>

<span data-ttu-id="4d82e-176">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="4d82e-176">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4d82e-177">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-177">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

- [<span data-ttu-id="4d82e-178">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4d82e-178">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="4d82e-179">Kestrel с IIS</span><span class="sxs-lookup"><span data-stu-id="4d82e-179">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="4d82e-180">Размещение в Linux с использованием Nginx</span><span class="sxs-lookup"><span data-stu-id="4d82e-180">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="4d82e-181">Размещение в Linux с использованием Apache</span><span class="sxs-lookup"><span data-stu-id="4d82e-181">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="4d82e-182">HTTP.sys</span><span class="sxs-lookup"><span data-stu-id="4d82e-182">HTTP.sys</span></span>](httpsys.md)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4d82e-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4d82e-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

- [<span data-ttu-id="4d82e-184">Kestrel</span><span class="sxs-lookup"><span data-stu-id="4d82e-184">Kestrel</span></span>](kestrel.md)
- [<span data-ttu-id="4d82e-185">Kestrel с IIS</span><span class="sxs-lookup"><span data-stu-id="4d82e-185">Kestrel with IIS</span></span>](aspnet-core-module.md)
- [<span data-ttu-id="4d82e-186">Размещение в Linux с использованием Nginx</span><span class="sxs-lookup"><span data-stu-id="4d82e-186">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
- [<span data-ttu-id="4d82e-187">Размещение в Linux с использованием Apache</span><span class="sxs-lookup"><span data-stu-id="4d82e-187">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
- [<span data-ttu-id="4d82e-188">WebListener</span><span class="sxs-lookup"><span data-stu-id="4d82e-188">WebListener</span></span>](weblistener.md)

---
