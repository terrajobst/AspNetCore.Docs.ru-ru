---
title: "Реализация kestrel веб-сервера в ASP.NET Core"
author: tdykstra
description: "Вводит Kestrel кросс платформенных веб-сервер для ASP.NET Core в соответствии с libuv."
keywords: "ASP.NET Core, Kestrel, libuv, префиксы URL-адрес"
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.assetid: 560bd66f-7dd0-4e68-b5fb-f31477e4b785
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 451c36fc9095b6e076e5287c992b6163205c523b
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="e37e8-104">Общие сведения о Kestrel реализация веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e37e8-104">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="e37e8-105">По [Tom Dykstra](https://github.com/tdykstra), [Ross Крис](https://github.com/Tratcher), и [Стивен Хальтер](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="e37e8-105">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="e37e8-106">Kestrel является кросс платформенных [веб-сервер для ASP.NET Core](index.md) на основе [libuv](https://github.com/libuv/libuv), кросс платформенной библиотекой асинхронных операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="e37e8-106">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="e37e8-107">Kestrel является веб-сервером, который включен по умолчанию в шаблонах проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e37e8-107">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="e37e8-108">Kestrel поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="e37e8-108">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="e37e8-109">HTTPS</span><span class="sxs-lookup"><span data-stu-id="e37e8-109">HTTPS</span></span>
  * <span data-ttu-id="e37e8-110">Непрозрачный обновления используется для включения [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="e37e8-110">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="e37e8-111">Сокеты UNIX для повышения производительности за Nginx</span><span class="sxs-lookup"><span data-stu-id="e37e8-111">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="e37e8-112">Kestrel поддерживается на всех платформах и версии, поддерживаемые .NET Core.</span><span class="sxs-lookup"><span data-stu-id="e37e8-112">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-113">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-113">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e37e8-114">[Просмотреть или загрузить образец кода для 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e37e8-114">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e37e8-116">[Просмотреть или загрузить образец кода для 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e37e8-116">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="e37e8-117">Когда следует использовать Kestrel с обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="e37e8-117">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-118">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-118">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e37e8-119">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="e37e8-119">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="e37e8-120">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="e37e8-120">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="e37e8-123">Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="e37e8-123">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-124">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-124">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e37e8-125">Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.</span><span class="sxs-lookup"><span data-stu-id="e37e8-125">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="e37e8-127">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*.</span><span class="sxs-lookup"><span data-stu-id="e37e8-127">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="e37e8-128">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="e37e8-128">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="e37e8-130">Обратный прокси-сервер необходим для развертываний edge (открытый для трафика из Интернета), по соображениям безопасности.</span><span class="sxs-lookup"><span data-stu-id="e37e8-130">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="e37e8-131">Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="e37e8-131">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="e37e8-132">Это включает в себя, но не ограничиваются соответствующие значения времени ожидания, ограничения на размер и ограничение числа одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="e37e8-132">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="e37e8-133">Сценария, требующего обратного прокси-сервера при наличии нескольких приложений, которые совместно используют того же IP-адрес и порт, работающих на одном сервере.</span><span class="sxs-lookup"><span data-stu-id="e37e8-133">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="e37e8-134">Это не поможет с Kestrel непосредственно Kestrel поддерживает совместное использование того же IP-адрес и порт между несколькими процессами.</span><span class="sxs-lookup"><span data-stu-id="e37e8-134">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="e37e8-135">При настройке Kestrel для прослушивания порта, он обрабатывает весь трафик для этого порта, независимо от того, заголовок узла.</span><span class="sxs-lookup"><span data-stu-id="e37e8-135">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="e37e8-136">Обратный прокси-сервер, который может совместно использоваться порты необходимо направить Kestrel на уникальный IP-адрес и порт.</span><span class="sxs-lookup"><span data-stu-id="e37e8-136">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="e37e8-137">Даже если обратного прокси-сервера не требуется, с помощью одного может быть хорошим выбором по другим причинам.</span><span class="sxs-lookup"><span data-stu-id="e37e8-137">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="e37e8-138">Он может ограничивать предоставляемых область.</span><span class="sxs-lookup"><span data-stu-id="e37e8-138">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="e37e8-139">Он предоставляет необязательный дополнительный уровень защиты и настройка.</span><span class="sxs-lookup"><span data-stu-id="e37e8-139">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="e37e8-140">Он может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="e37e8-140">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="e37e8-141">Оно упрощает балансировку нагрузки и настройке протокола SSL.</span><span class="sxs-lookup"><span data-stu-id="e37e8-141">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="e37e8-142">Только обратного прокси-сервера требуется сертификат SSL, и этот сервер может обмениваться данными с серверами приложений во внутренней сети, с помощью обычного протокола HTTP.</span><span class="sxs-lookup"><span data-stu-id="e37e8-142">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="e37e8-143">Как использовать в приложениях ASP.NET Core Kestrel</span><span class="sxs-lookup"><span data-stu-id="e37e8-143">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-144">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-144">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e37e8-145">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) пакет включен в [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="e37e8-145">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="e37e8-146">Шаблоны проектов ASP.NET Core Kestrel используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e37e8-146">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="e37e8-147">В *Program.cs*, этот код вызывает шаблон `CreateDefaultBuilder`, который вызывает [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) незаметно.</span><span class="sxs-lookup"><span data-stu-id="e37e8-147">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="e37e8-148">Если необходимо настроить параметры Kestrel, вызовите `UseKestrel` в *Program.cs* как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e37e8-148">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-149">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-149">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e37e8-150">Установка [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="e37e8-150">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="e37e8-151">Вызовите [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) метод расширения в `WebHostBuilder` в ваш `Main` метод, указывая любой [Kestrel параметры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , требуется, как показано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="e37e8-151">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="e37e8-152">Параметры kestrel</span><span class="sxs-lookup"><span data-stu-id="e37e8-152">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e37e8-154">Веб-сервер Kestrel имеет ограничение параметры конфигурации, которые особенно полезны в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="e37e8-154">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="e37e8-155">Ниже приведены некоторые ограничения, которые можно задать.</span><span class="sxs-lookup"><span data-stu-id="e37e8-155">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="e37e8-156">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="e37e8-156">Maximum client connections</span></span>
- <span data-ttu-id="e37e8-157">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="e37e8-157">Maximum request body size</span></span>
- <span data-ttu-id="e37e8-158">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="e37e8-158">Minimum request body data rate</span></span>

<span data-ttu-id="e37e8-159">Задайте эти ограничения, а также других на `Limits` свойство [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) класса.</span><span class="sxs-lookup"><span data-stu-id="e37e8-159">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="e37e8-160">`Limits` Свойство содержит экземпляр [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) класса.</span><span class="sxs-lookup"><span data-stu-id="e37e8-160">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="e37e8-161">**Максимальное клиентских подключений**</span><span class="sxs-lookup"><span data-stu-id="e37e8-161">**Maximum client connections**</span></span>

<span data-ttu-id="e37e8-162">Максимальное число одновременных открытых TCP-подключений можно задать для всего приложения с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="e37e8-162">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="e37e8-163">Отдельный предел для подключений, которые были обновлены с HTTP или HTTPS для другой протокол (например, в запросе WebSockets) отсутствует.</span><span class="sxs-lookup"><span data-stu-id="e37e8-163">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="e37e8-164">После обновления подключение, он не считается относительно `MaxConcurrentConnections` ограничение.</span><span class="sxs-lookup"><span data-stu-id="e37e8-164">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="e37e8-165">Максимальное число подключений — неограниченное (null) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e37e8-165">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="e37e8-166">**Размер максимального запроса**</span><span class="sxs-lookup"><span data-stu-id="e37e8-166">**Maximum request body size**</span></span>

<span data-ttu-id="e37e8-167">Размер максимального запроса текст по умолчанию — 30,000,000 байт которого составляет приблизительно 28.6 МБ.</span><span class="sxs-lookup"><span data-stu-id="e37e8-167">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="e37e8-168">Чтобы переопределить ограничение в приложении ASP.NET Core MVC рекомендуется использовать [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) атрибута в методе действия:</span><span class="sxs-lookup"><span data-stu-id="e37e8-168">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="e37e8-169">Ниже приведен пример, в котором показано, как настроить ограничения для всего приложения, каждый запрос:</span><span class="sxs-lookup"><span data-stu-id="e37e8-169">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="e37e8-170">Можно переопределить параметр на конкретный запрос в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e37e8-170">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="e37e8-171">Исключение при попытке настроить ограничение на запрос после начала чтение запроса приложением.</span><span class="sxs-lookup"><span data-stu-id="e37e8-171">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="e37e8-172">Отсутствует `IsReadOnly` свойство, определяющее, если `MaxRequestBodySize` свойства находится в состоянии только для чтения, то есть ограничение уже слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="e37e8-172">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="e37e8-173">**Скорость передачи данных запроса минимальное текста**</span><span class="sxs-lookup"><span data-stu-id="e37e8-173">**Minimum request body data rate**</span></span>

<span data-ttu-id="e37e8-174">Если данные поступают со скоростью, указанной в байтах в секунду, kestrel проверяет каждую секунду.</span><span class="sxs-lookup"><span data-stu-id="e37e8-174">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="e37e8-175">Если скорость падает ниже минимума, соединения истекло время ожидания. Льготный период — количество времени, что Kestrel дает клиента, чтобы увеличить его скорость отправки до минимально; скорость в течение этого времени не проверяется.</span><span class="sxs-lookup"><span data-stu-id="e37e8-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="e37e8-176">Льготный период помогает избежать потери подключения, которые первоначально отправляют данные малый размер из-за медленного пуска TCP.</span><span class="sxs-lookup"><span data-stu-id="e37e8-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="e37e8-177">Минимальная скорость по умолчанию — 240 байт/сек, с 5-секундный льготный период.</span><span class="sxs-lookup"><span data-stu-id="e37e8-177">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="e37e8-178">Минимальная скорость также применяется в ответ.</span><span class="sxs-lookup"><span data-stu-id="e37e8-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="e37e8-179">Код, чтобы задать ограничение запроса и ответа ограничение зависит от того, за исключением того `RequestBody` или `Response` в именах свойств и интерфейс.</span><span class="sxs-lookup"><span data-stu-id="e37e8-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="e37e8-180">Ниже приведен пример, в котором показано, как настроить курсы минимальный набор данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="e37e8-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="e37e8-181">Тарифы на запрос можно настроить в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e37e8-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="e37e8-182">Сведения о других параметрах Kestrel см. следующие классы:</span><span class="sxs-lookup"><span data-stu-id="e37e8-182">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="e37e8-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="e37e8-183">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="e37e8-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="e37e8-184">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="e37e8-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="e37e8-185">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e37e8-187">Сведения о параметрах Kestrel см. в разделе [KestrelServerOptions класса](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="e37e8-187">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="e37e8-188">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="e37e8-188">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e37e8-190">По умолчанию ASP.NET Core привязывается к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e37e8-190">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="e37e8-191">Настройте префиксы URL-адреса и порты для Kestrel на прослушивание путем вызова `Listen` или `ListenUnixSocket` методы `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="e37e8-191">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="e37e8-192">(`UseUrls`, `urls` аргумент командной строки, а также работают переменная среды ASPNETCORE_URLS, но имеют ограничения, которые указаны [далее в этой статье](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="e37e8-192">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="e37e8-193">**Привязка к TCP-сокет**</span><span class="sxs-lookup"><span data-stu-id="e37e8-193">**Bind to a TCP socket**</span></span>

<span data-ttu-id="e37e8-194">`Listen` Метод привязан к TCP-сокет и параметров лямбда-выражения можно настроить SSL-сертификата:</span><span class="sxs-lookup"><span data-stu-id="e37e8-194">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="e37e8-195">Обратите внимание на то, как в этом примере настраивается SSL для конкретной конечной точки с помощью [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="e37e8-195">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="e37e8-196">Такой же API можно использовать для настройки других параметров Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="e37e8-196">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="e37e8-197">**Привязка к сокету Unix**</span><span class="sxs-lookup"><span data-stu-id="e37e8-197">**Bind to a Unix socket**</span></span>

<span data-ttu-id="e37e8-198">Могут прослушивать сокета Unix для улучшения производительности Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="e37e8-198">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="e37e8-199">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="e37e8-199">**Port 0**</span></span>

<span data-ttu-id="e37e8-200">Если указать номер порта 0 Kestrel динамически связывает доступный порт.</span><span class="sxs-lookup"><span data-stu-id="e37e8-200">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="e37e8-201">В следующем примере показано, как определить, какой порт Kestrel фактически привязан к во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="e37e8-201">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="e37e8-202">**Ограничения UseUrls**</span><span class="sxs-lookup"><span data-stu-id="e37e8-202">**UseUrls limitations**</span></span>

<span data-ttu-id="e37e8-203">Можно настроить конечные точки, вызвав `UseUrls` метода или с помощью `urls` аргумент командной строки или переменной среды ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="e37e8-203">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="e37e8-204">Эти методы полезны, если требуется, чтобы код работал с серверы, отличные от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="e37e8-204">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="e37e8-205">Однако следует учитывать следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="e37e8-205">However, be aware of these limitations:</span></span>

* <span data-ttu-id="e37e8-206">SSL нельзя использовать с помощью этих методов.</span><span class="sxs-lookup"><span data-stu-id="e37e8-206">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="e37e8-207">Если используются и `Listen` метод и `UseUrls`, `Listen` переопределить конечные точки `UseUrls` конечных точек.</span><span class="sxs-lookup"><span data-stu-id="e37e8-207">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="e37e8-208">**Конфигурация конечной точки для служб IIS**</span><span class="sxs-lookup"><span data-stu-id="e37e8-208">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="e37e8-209">При использовании IIS привязки URL-адрес для IIS переопределить все привязки, которые задаются с помощью вызова `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="e37e8-209">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="e37e8-210">Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="e37e8-210">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-211">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-211">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e37e8-212">По умолчанию ASP.NET Core привязывается к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="e37e8-212">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="e37e8-213">Можно настроить префиксы URL-адреса и порты для Kestrel на прослушивание с помощью `UseUrls` метод расширения `urls` аргумент командной строки или система конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e37e8-213">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="e37e8-214">Дополнительные сведения об этих методах см. в разделе [размещения](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="e37e8-214">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="e37e8-215">Сведения о работе привязку URL-адресов при использовании служб IIS в качестве обратного прокси-сервера см. в разделе [модуль ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="e37e8-215">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="e37e8-216">Префиксы URL-адрес</span><span class="sxs-lookup"><span data-stu-id="e37e8-216">URL prefixes</span></span>

<span data-ttu-id="e37e8-217">При вызове метода `UseUrls` или использовать `urls` аргумент командной строки или переменной среды ASPNETCORE_URLS, префиксы URL-адрес может быть в любом из следующих форматов.</span><span class="sxs-lookup"><span data-stu-id="e37e8-217">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e37e8-219">Допустимы только префиксы URL-адрес HTTP; Kestrel не поддерживает SSL, при настройке URL-адрес привязки с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="e37e8-219">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="e37e8-220">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-220">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="e37e8-221">0.0.0.0 является особым случаем, который привязывает всех адресов IPv4.</span><span class="sxs-lookup"><span data-stu-id="e37e8-221">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="e37e8-222">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-222">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="e37e8-223">[::] эквивалентно IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="e37e8-223">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="e37e8-224">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-224">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="e37e8-225">Имена, узлов, *, и +, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="e37e8-225">Host names, *, and +, are not special.</span></span> <span data-ttu-id="e37e8-226">Все компоненты, не является распознаваемым IP-адрес или «localhost» будет привязано ко всем IPv4 и IPv6 IP-адресов.</span><span class="sxs-lookup"><span data-stu-id="e37e8-226">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="e37e8-227">Если необходимо привязать другие имена узлов для разных приложений ASP.NET Core, к тому же порту, используйте [HTTP.sys](httpsys.md) или обратного прокси-сервера, таких как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="e37e8-227">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="e37e8-228">Имя «Localhost» с IP-номер или замыкания на себя порт с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-228">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="e37e8-229">Когда `localhost` указан, Kestrel пытается связаться с интерфейсы замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="e37e8-229">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="e37e8-230">Если запрошенный порт уже используется другой службой или интерфейс замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="e37e8-230">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="e37e8-231">Если один из этих интерфейсов замыкания на себя недоступна по другой причине (большинство часто, поскольку IPv6 не поддерживается), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="e37e8-231">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="e37e8-233">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-233">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="e37e8-234">0.0.0.0 является особым случаем, который привязывает всех адресов IPv4.</span><span class="sxs-lookup"><span data-stu-id="e37e8-234">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="e37e8-235">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-235">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="e37e8-236">[::] эквивалентно IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="e37e8-236">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="e37e8-237">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-237">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="e37e8-238">Имена, узлов \*, а + не специальные.</span><span class="sxs-lookup"><span data-stu-id="e37e8-238">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="e37e8-239">Все, что не является распознаваемым IP-адрес или «localhost» привязывается ко всем IPv4 и IPv6 IP-адресов.</span><span class="sxs-lookup"><span data-stu-id="e37e8-239">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="e37e8-240">Если необходимо привязать другие имена узлов для разных приложений ASP.NET Core, к тому же порту, используйте [WebListener](weblistener.md) или обратного прокси-сервера, таких как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="e37e8-240">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="e37e8-241">Имя «Localhost» с IP-номер или замыкания на себя порт с номером порта</span><span class="sxs-lookup"><span data-stu-id="e37e8-241">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="e37e8-242">Когда `localhost` указан, Kestrel пытается связаться с интерфейсы замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="e37e8-242">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="e37e8-243">Если запрошенный порт уже используется другой службой или интерфейс замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="e37e8-243">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="e37e8-244">Если один из этих интерфейсов замыкания на себя недоступна по другой причине (большинство часто, поскольку IPv6 не поддерживается), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="e37e8-244">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="e37e8-245">Сокет UNIX</span><span class="sxs-lookup"><span data-stu-id="e37e8-245">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="e37e8-246">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="e37e8-246">**Port 0**</span></span>

<span data-ttu-id="e37e8-247">Если указать номер порта 0 Kestrel динамически связывает доступный порт.</span><span class="sxs-lookup"><span data-stu-id="e37e8-247">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="e37e8-248">Привязка к порту 0 допускается для любого имени узла или IP-адрес за исключением `localhost` имя.</span><span class="sxs-lookup"><span data-stu-id="e37e8-248">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="e37e8-249">В следующем примере показано, как определить, какой порт Kestrel фактически привязан к во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="e37e8-249">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="e37e8-250">**Префиксы URL-адреса для SSL**</span><span class="sxs-lookup"><span data-stu-id="e37e8-250">**URL prefixes for SSL**</span></span>

<span data-ttu-id="e37e8-251">Убедитесь, что префиксы URL-адрес с `https:` при вызове метода `UseHttps` метод расширения, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="e37e8-251">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

```csharp
var host = new WebHostBuilder() 
    .UseKestrel(options => 
    { 
        options.UseHttps("testCert.pfx", "testPassword"); 
    }) 
   .UseUrls("http://localhost:5000", "https://localhost:5001") 
   .UseContentRoot(Directory.GetCurrentDirectory()) 
   .UseStartup<Startup>() 
   .Build(); 
```

> [!NOTE]
> <span data-ttu-id="e37e8-252">HTTPS и HTTP не может размещаться на тот же порт.</span><span class="sxs-lookup"><span data-stu-id="e37e8-252">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="e37e8-253">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e37e8-253">Next steps</span></span>

<span data-ttu-id="e37e8-254">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="e37e8-254">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e37e8-255">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-255">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="e37e8-256">Образец приложения для 2.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-256">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="e37e8-257">Kestrel исходного кода</span><span class="sxs-lookup"><span data-stu-id="e37e8-257">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e37e8-258">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-258">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="e37e8-259">Образец приложения для 1.x</span><span class="sxs-lookup"><span data-stu-id="e37e8-259">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="e37e8-260">Kestrel исходного кода</span><span class="sxs-lookup"><span data-stu-id="e37e8-260">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
