---
title: "Реализация kestrel веб-сервера в ASP.NET Core"
author: tdykstra
description: "Вводит Kestrel кросс платформенных веб-сервер для ASP.NET Core в соответствии с libuv."
ms.author: tdykstra
manager: wpickett
ms.date: 08/02/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/servers/kestrel
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3695a6a127f77bd90538d72af6112ccf507f3482
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="df6cb-103">Общие сведения о Kestrel реализация веб-сервера в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="df6cb-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="df6cb-104">По [Tom Dykstra](https://github.com/tdykstra), [Ross Крис](https://github.com/Tratcher), и [Стивен Хальтер](https://twitter.com/halter73)</span><span class="sxs-lookup"><span data-stu-id="df6cb-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="df6cb-105">Kestrel является кросс платформенных [веб-сервер для ASP.NET Core](index.md) на основе [libuv](https://github.com/libuv/libuv), кросс платформенной библиотекой асинхронных операций ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="df6cb-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="df6cb-106">Kestrel является веб-сервером, который включен по умолчанию в шаблонах проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df6cb-106">Kestrel is the web server that is included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="df6cb-107">Kestrel поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="df6cb-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="df6cb-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="df6cb-108">HTTPS</span></span>
  * <span data-ttu-id="df6cb-109">Непрозрачный обновления используется для включения [WebSockets](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="df6cb-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="df6cb-110">Сокеты UNIX для повышения производительности за Nginx</span><span class="sxs-lookup"><span data-stu-id="df6cb-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="df6cb-111">Kestrel поддерживается на всех платформах и версии, поддерживаемые .NET Core.</span><span class="sxs-lookup"><span data-stu-id="df6cb-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df6cb-113">[Просмотреть или загрузить образец кода для 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="df6cb-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="df6cb-115">[Просмотреть или загрузить образец кода для 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([загрузке](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="df6cb-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="df6cb-116">Когда следует использовать Kestrel с обратного прокси-сервера</span><span class="sxs-lookup"><span data-stu-id="df6cb-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df6cb-118">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="df6cb-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="df6cb-119">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="df6cb-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="df6cb-122">Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="df6cb-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="df6cb-124">Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.</span><span class="sxs-lookup"><span data-stu-id="df6cb-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="df6cb-126">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*.</span><span class="sxs-lookup"><span data-stu-id="df6cb-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="df6cb-127">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="df6cb-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="df6cb-129">Обратный прокси-сервер необходим для развертываний edge (открытый для трафика из Интернета), по соображениям безопасности.</span><span class="sxs-lookup"><span data-stu-id="df6cb-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="df6cb-130">Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="df6cb-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="df6cb-131">Это включает в себя, но не ограничиваются соответствующие значения времени ожидания, ограничения на размер и ограничение числа одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="df6cb-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="df6cb-132">Сценария, требующего обратного прокси-сервера при наличии нескольких приложений, которые совместно используют того же IP-адрес и порт, работающих на одном сервере.</span><span class="sxs-lookup"><span data-stu-id="df6cb-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="df6cb-133">Это не поможет с Kestrel непосредственно Kestrel поддерживает совместное использование того же IP-адрес и порт между несколькими процессами.</span><span class="sxs-lookup"><span data-stu-id="df6cb-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="df6cb-134">При настройке Kestrel для прослушивания порта, он обрабатывает весь трафик для этого порта, независимо от того, заголовок узла.</span><span class="sxs-lookup"><span data-stu-id="df6cb-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="df6cb-135">Обратный прокси-сервер, который может совместно использоваться порты необходимо направить Kestrel на уникальный IP-адрес и порт.</span><span class="sxs-lookup"><span data-stu-id="df6cb-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="df6cb-136">Даже если обратного прокси-сервера не требуется, с помощью одного может быть хорошим выбором по другим причинам.</span><span class="sxs-lookup"><span data-stu-id="df6cb-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="df6cb-137">Он может ограничивать предоставляемых область.</span><span class="sxs-lookup"><span data-stu-id="df6cb-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="df6cb-138">Он предоставляет необязательный дополнительный уровень защиты и настройка.</span><span class="sxs-lookup"><span data-stu-id="df6cb-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="df6cb-139">Он может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="df6cb-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="df6cb-140">Оно упрощает балансировку нагрузки и настройке протокола SSL.</span><span class="sxs-lookup"><span data-stu-id="df6cb-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="df6cb-141">Только обратного прокси-сервера требуется сертификат SSL, и этот сервер может обмениваться данными с серверами приложений во внутренней сети, с помощью обычного протокола HTTP.</span><span class="sxs-lookup"><span data-stu-id="df6cb-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="df6cb-142">Как использовать в приложениях ASP.NET Core Kestrel</span><span class="sxs-lookup"><span data-stu-id="df6cb-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df6cb-144">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) пакет включен в [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="df6cb-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="df6cb-145">Шаблоны проектов ASP.NET Core Kestrel используется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="df6cb-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="df6cb-146">В *Program.cs*, этот код вызывает шаблон `CreateDefaultBuilder`, который вызывает [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) незаметно.</span><span class="sxs-lookup"><span data-stu-id="df6cb-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="df6cb-147">Если необходимо настроить параметры Kestrel, вызовите `UseKestrel` в *Program.cs* как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="df6cb-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="df6cb-149">Установка [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="df6cb-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="df6cb-150">Вызовите [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) метод расширения в `WebHostBuilder` в ваш `Main` метод, указывая любой [Kestrel параметры](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) , требуется, как показано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="df6cb-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="df6cb-151">Параметры kestrel</span><span class="sxs-lookup"><span data-stu-id="df6cb-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df6cb-153">Веб-сервер Kestrel имеет ограничение параметры конфигурации, которые особенно полезны в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="df6cb-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="df6cb-154">Ниже приведены некоторые ограничения, которые можно задать.</span><span class="sxs-lookup"><span data-stu-id="df6cb-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="df6cb-155">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="df6cb-155">Maximum client connections</span></span>
- <span data-ttu-id="df6cb-156">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="df6cb-156">Maximum request body size</span></span>
- <span data-ttu-id="df6cb-157">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="df6cb-157">Minimum request body data rate</span></span>

<span data-ttu-id="df6cb-158">Задайте эти ограничения, а также других на `Limits` свойство [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) класса.</span><span class="sxs-lookup"><span data-stu-id="df6cb-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="df6cb-159">`Limits` Свойство содержит экземпляр [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) класса.</span><span class="sxs-lookup"><span data-stu-id="df6cb-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="df6cb-160">**Максимальное клиентских подключений**</span><span class="sxs-lookup"><span data-stu-id="df6cb-160">**Maximum client connections**</span></span>

<span data-ttu-id="df6cb-161">Максимальное число одновременных открытых TCP-подключений можно задать для всего приложения с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="df6cb-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="df6cb-162">Отдельный предел для подключений, которые были обновлены с HTTP или HTTPS для другой протокол (например, в запросе WebSockets) отсутствует.</span><span class="sxs-lookup"><span data-stu-id="df6cb-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="df6cb-163">После обновления подключение, он не считается относительно `MaxConcurrentConnections` ограничение.</span><span class="sxs-lookup"><span data-stu-id="df6cb-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="df6cb-164">Максимальное число подключений — неограниченное (null) по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="df6cb-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="df6cb-165">**Размер максимального запроса**</span><span class="sxs-lookup"><span data-stu-id="df6cb-165">**Maximum request body size**</span></span>

<span data-ttu-id="df6cb-166">Размер максимального запроса текст по умолчанию — 30,000,000 байт которого составляет приблизительно 28.6 МБ.</span><span class="sxs-lookup"><span data-stu-id="df6cb-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="df6cb-167">Чтобы переопределить ограничение в приложении ASP.NET Core MVC рекомендуется использовать [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) атрибута в методе действия:</span><span class="sxs-lookup"><span data-stu-id="df6cb-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="df6cb-168">Ниже приведен пример, в котором показано, как настроить ограничения для всего приложения, каждый запрос:</span><span class="sxs-lookup"><span data-stu-id="df6cb-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="df6cb-169">Можно переопределить параметр на конкретный запрос в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="df6cb-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="df6cb-170">Исключение при попытке настроить ограничение на запрос после начала чтение запроса приложением.</span><span class="sxs-lookup"><span data-stu-id="df6cb-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="df6cb-171">Отсутствует `IsReadOnly` свойство, определяющее, если `MaxRequestBodySize` свойства находится в состоянии только для чтения, то есть ограничение уже слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="df6cb-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="df6cb-172">**Скорость передачи данных запроса минимальное текста**</span><span class="sxs-lookup"><span data-stu-id="df6cb-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="df6cb-173">Если данные поступают со скоростью, указанной в байтах в секунду, kestrel проверяет каждую секунду.</span><span class="sxs-lookup"><span data-stu-id="df6cb-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="df6cb-174">Если скорость падает ниже минимума, соединения истекло время ожидания. Льготный период — количество времени, что Kestrel дает клиента, чтобы увеличить его скорость отправки до минимально; скорость в течение этого времени не проверяется.</span><span class="sxs-lookup"><span data-stu-id="df6cb-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate is not checked during that time.</span></span> <span data-ttu-id="df6cb-175">Льготный период помогает избежать потери подключения, которые первоначально отправляют данные малый размер из-за медленного пуска TCP.</span><span class="sxs-lookup"><span data-stu-id="df6cb-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="df6cb-176">Минимальная скорость по умолчанию — 240 байт/сек, с 5-секундный льготный период.</span><span class="sxs-lookup"><span data-stu-id="df6cb-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="df6cb-177">Минимальная скорость также применяется в ответ.</span><span class="sxs-lookup"><span data-stu-id="df6cb-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="df6cb-178">Код, чтобы задать ограничение запроса и ответа ограничение зависит от того, за исключением того `RequestBody` или `Response` в именах свойств и интерфейс.</span><span class="sxs-lookup"><span data-stu-id="df6cb-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="df6cb-179">Ниже приведен пример, в котором показано, как настроить курсы минимальный набор данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="df6cb-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="df6cb-180">Тарифы на запрос можно настроить в по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="df6cb-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="df6cb-181">Сведения о других параметрах Kestrel см. следующие классы:</span><span class="sxs-lookup"><span data-stu-id="df6cb-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="df6cb-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="df6cb-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="df6cb-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="df6cb-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="df6cb-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="df6cb-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="df6cb-186">Сведения о параметрах Kestrel см. в разделе [KestrelServerOptions класса](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="df6cb-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="df6cb-187">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="df6cb-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df6cb-189">По умолчанию ASP.NET Core привязывается к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="df6cb-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="df6cb-190">Настройте префиксы URL-адреса и порты для Kestrel на прослушивание путем вызова `Listen` или `ListenUnixSocket` методы `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="df6cb-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="df6cb-191">(`UseUrls`, `urls` аргумент командной строки, а также работают переменная среды ASPNETCORE_URLS, но имеют ограничения, которые указаны [далее в этой статье](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="df6cb-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="df6cb-192">**Привязка к TCP-сокет**</span><span class="sxs-lookup"><span data-stu-id="df6cb-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="df6cb-193">`Listen` Метод привязан к TCP-сокет и параметров лямбда-выражения можно настроить SSL-сертификата:</span><span class="sxs-lookup"><span data-stu-id="df6cb-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="df6cb-194">Обратите внимание на то, как в этом примере настраивается SSL для конкретной конечной точки с помощью [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="df6cb-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="df6cb-195">Такой же API можно использовать для настройки других параметров Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="df6cb-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="df6cb-196">**Привязка к сокету Unix**</span><span class="sxs-lookup"><span data-stu-id="df6cb-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="df6cb-197">Могут прослушивать сокета Unix для улучшения производительности Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="df6cb-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="df6cb-198">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="df6cb-198">**Port 0**</span></span>

<span data-ttu-id="df6cb-199">Если указать номер порта 0 Kestrel динамически связывает доступный порт.</span><span class="sxs-lookup"><span data-stu-id="df6cb-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="df6cb-200">В следующем примере показано, как определить, какой порт Kestrel фактически привязан к во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="df6cb-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="df6cb-201">**Ограничения UseUrls**</span><span class="sxs-lookup"><span data-stu-id="df6cb-201">**UseUrls limitations**</span></span>

<span data-ttu-id="df6cb-202">Можно настроить конечные точки, вызвав `UseUrls` метода или с помощью `urls` аргумент командной строки или переменной среды ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="df6cb-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="df6cb-203">Эти методы полезны, если требуется, чтобы код работал с серверы, отличные от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="df6cb-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="df6cb-204">Однако следует учитывать следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="df6cb-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="df6cb-205">SSL нельзя использовать с помощью этих методов.</span><span class="sxs-lookup"><span data-stu-id="df6cb-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="df6cb-206">Если используются и `Listen` метод и `UseUrls`, `Listen` переопределить конечные точки `UseUrls` конечных точек.</span><span class="sxs-lookup"><span data-stu-id="df6cb-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="df6cb-207">**Конфигурация конечной точки для служб IIS**</span><span class="sxs-lookup"><span data-stu-id="df6cb-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="df6cb-208">При использовании IIS привязки URL-адрес для IIS переопределить все привязки, которые задаются с помощью вызова `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="df6cb-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="df6cb-209">Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="df6cb-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="df6cb-211">По умолчанию ASP.NET Core привязывается к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="df6cb-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="df6cb-212">Можно настроить префиксы URL-адреса и порты для Kestrel на прослушивание с помощью `UseUrls` метод расширения `urls` аргумент командной строки или система конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="df6cb-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="df6cb-213">Дополнительные сведения об этих методах см. в разделе [размещения](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="df6cb-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="df6cb-214">Сведения о работе привязку URL-адресов при использовании служб IIS в качестве обратного прокси-сервера см. в разделе [модуль ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="df6cb-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="df6cb-215">Префиксы URL-адрес</span><span class="sxs-lookup"><span data-stu-id="df6cb-215">URL prefixes</span></span>

<span data-ttu-id="df6cb-216">При вызове метода `UseUrls` или использовать `urls` аргумент командной строки или переменной среды ASPNETCORE_URLS, префиксы URL-адрес может быть в любом из следующих форматов.</span><span class="sxs-lookup"><span data-stu-id="df6cb-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="df6cb-218">Допустимы только префиксы URL-адрес HTTP; Kestrel не поддерживает SSL, при настройке URL-адрес привязки с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="df6cb-218">Only HTTP URL prefixes are valid; Kestrel does not support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="df6cb-219">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="df6cb-220">0.0.0.0 является особым случаем, который привязывает всех адресов IPv4.</span><span class="sxs-lookup"><span data-stu-id="df6cb-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="df6cb-221">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="df6cb-222">[::] эквивалентно IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="df6cb-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="df6cb-223">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="df6cb-224">Имена, узлов, \*, и +, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="df6cb-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="df6cb-225">Все компоненты, не является распознаваемым IP-адрес или «localhost» будет привязано ко всем IPv4 и IPv6 IP-адресов.</span><span class="sxs-lookup"><span data-stu-id="df6cb-225">Anything that is not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="df6cb-226">Если необходимо привязать другие имена узлов для разных приложений ASP.NET Core, к тому же порту, используйте [HTTP.sys](httpsys.md) или обратного прокси-сервера, таких как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="df6cb-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="df6cb-227">Имя «Localhost» с IP-номер или замыкания на себя порт с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="df6cb-228">Когда `localhost` указан, Kestrel пытается связаться с интерфейсы замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="df6cb-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="df6cb-229">Если запрошенный порт уже используется другой службой или интерфейс замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="df6cb-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="df6cb-230">Если один из этих интерфейсов замыкания на себя недоступна по другой причине (большинство часто, поскольку IPv6 не поддерживается), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="df6cb-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="df6cb-232">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="df6cb-233">0.0.0.0 является особым случаем, который привязывает всех адресов IPv4.</span><span class="sxs-lookup"><span data-stu-id="df6cb-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="df6cb-234">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="df6cb-235">[::] эквивалентно IPv6 IPv4 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="df6cb-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="df6cb-236">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="df6cb-237">Имена, узлов \*, а + не специальные.</span><span class="sxs-lookup"><span data-stu-id="df6cb-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="df6cb-238">Все, что не является распознаваемым IP-адрес или «localhost» привязывается ко всем IPv4 и IPv6 IP-адресов.</span><span class="sxs-lookup"><span data-stu-id="df6cb-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="df6cb-239">Если необходимо привязать другие имена узлов для разных приложений ASP.NET Core, к тому же порту, используйте [WebListener](weblistener.md) или обратного прокси-сервера, таких как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="df6cb-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="df6cb-240">Имя «Localhost» с IP-номер или замыкания на себя порт с номером порта</span><span class="sxs-lookup"><span data-stu-id="df6cb-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="df6cb-241">Когда `localhost` указан, Kestrel пытается связаться с интерфейсы замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="df6cb-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="df6cb-242">Если запрошенный порт уже используется другой службой или интерфейс замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="df6cb-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="df6cb-243">Если один из этих интерфейсов замыкания на себя недоступна по другой причине (большинство часто, поскольку IPv6 не поддерживается), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="df6cb-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 is not supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="df6cb-244">Сокет UNIX</span><span class="sxs-lookup"><span data-stu-id="df6cb-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="df6cb-245">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="df6cb-245">**Port 0**</span></span>

<span data-ttu-id="df6cb-246">Если указать номер порта 0 Kestrel динамически связывает доступный порт.</span><span class="sxs-lookup"><span data-stu-id="df6cb-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="df6cb-247">Привязка к порту 0 допускается для любого имени узла или IP-адрес за исключением `localhost` имя.</span><span class="sxs-lookup"><span data-stu-id="df6cb-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="df6cb-248">В следующем примере показано, как определить, какой порт Kestrel фактически привязан к во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="df6cb-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="df6cb-249">**Префиксы URL-адреса для SSL**</span><span class="sxs-lookup"><span data-stu-id="df6cb-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="df6cb-250">Убедитесь, что префиксы URL-адрес с `https:` при вызове метода `UseHttps` метод расширения, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="df6cb-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="df6cb-251">HTTPS и HTTP не может размещаться на тот же порт.</span><span class="sxs-lookup"><span data-stu-id="df6cb-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="df6cb-252">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="df6cb-252">Next steps</span></span>

<span data-ttu-id="df6cb-253">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="df6cb-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="df6cb-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="df6cb-255">Образец приложения для 2.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="df6cb-256">Kestrel исходного кода</span><span class="sxs-lookup"><span data-stu-id="df6cb-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="df6cb-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="df6cb-258">Образец приложения для 1.x</span><span class="sxs-lookup"><span data-stu-id="df6cb-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="df6cb-259">Kestrel исходного кода</span><span class="sxs-lookup"><span data-stu-id="df6cb-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
