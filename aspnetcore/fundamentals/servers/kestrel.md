---
title: "Реализации веб-сервера Kestrel в ASP.NET Core"
author: tdykstra
description: "Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core, основанном на libuv."
manager: wpickett
ms.author: tdykstra
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: bfe7644891296c7c3485c9a870d90951ba87e9e8
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="bd83e-103">Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bd83e-103">Introduction to Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="bd83e-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter)</span><span class="sxs-lookup"><span data-stu-id="bd83e-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="bd83e-105">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](index.md) на основе [libuv](https://github.com/libuv/libuv), кроссплатформенный асинхронной библиотеки ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="bd83e-105">Kestrel is a cross-platform [web server for ASP.NET Core](index.md) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="bd83e-106">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd83e-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span> 

<span data-ttu-id="bd83e-107">Kestrel поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="bd83e-107">Kestrel supports the following features:</span></span>

  * <span data-ttu-id="bd83e-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="bd83e-108">HTTPS</span></span>
  * <span data-ttu-id="bd83e-109">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="bd83e-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
  * <span data-ttu-id="bd83e-110">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="bd83e-110">Unix sockets for high performance behind Nginx</span></span> 

<span data-ttu-id="bd83e-111">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd83e-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-112">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-112">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bd83e-113">[Просмотреть или скачать пример кода для версии 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bd83e-113">[View or download sample code for 2.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-114">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-114">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bd83e-115">[Просмотреть или скачать пример кода для версии 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="bd83e-115">[View or download sample code for 1.x](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

---

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="bd83e-116">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="bd83e-116">When to use Kestrel with a reverse proxy</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bd83e-118">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="bd83e-118">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="bd83e-119">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="bd83e-119">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="bd83e-122">Любая конфигурация &mdash; с обратным прокси-сервером или без &mdash; также может использоваться, если Kestrel предоставляется только для внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="bd83e-122">Either configuration &mdash; with or without a reverse proxy server &mdash; can also be used if Kestrel is exposed only to an internal network.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bd83e-124">Если приложение принимает запросы только из внутренней сети, можно использовать Kestrel отдельно.</span><span class="sxs-lookup"><span data-stu-id="bd83e-124">If your application accepts requests only from an internal network, you can use Kestrel by itself.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="bd83e-126">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache как *обратный прокси-сервер*.</span><span class="sxs-lookup"><span data-stu-id="bd83e-126">If you expose your application to the Internet, you must use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="bd83e-127">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="bd83e-127">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="bd83e-129">По соображениям безопасности для развертываний пограничного сервера (открытого для интернет-трафика) нужен обратный прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="bd83e-129">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="bd83e-130">Версии Kestrel 1.x не оснащены всем комплектом функций для защиты от атак.</span><span class="sxs-lookup"><span data-stu-id="bd83e-130">The 1.x versions of Kestrel don't have a full complement of defenses against attacks.</span></span> <span data-ttu-id="bd83e-131">Сюда входят, например, соответствующее время ожидания, предельные размеры и максимальное количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="bd83e-131">This includes but isn't limited to appropriate timeouts, size limits, and concurrent connection limits.</span></span>

---

<span data-ttu-id="bd83e-132">Сценарием, в котором требуется обратный прокси-сервер, является выполнение нескольких приложений, которые совместно используют один IP-адрес и порт, на одном сервере.</span><span class="sxs-lookup"><span data-stu-id="bd83e-132">A scenario that requires a reverse proxy is when you have multiple applications that share the same IP and port running on a single server.</span></span> <span data-ttu-id="bd83e-133">В Kestrel это невозможно реализовать напрямую, так как Kestrel не поддерживает совместное использование одного IP-адреса и порта несколькими процессами.</span><span class="sxs-lookup"><span data-stu-id="bd83e-133">That doesn't work with Kestrel directly because Kestrel doesn't support sharing the same IP and port between multiple processes.</span></span> <span data-ttu-id="bd83e-134">Если Kestrel настроен для прослушивания порта, он обрабатывает весь трафик через этот порт, независимо от заголовка узла.</span><span class="sxs-lookup"><span data-stu-id="bd83e-134">When you configure Kestrel to listen on a port, it handles all traffic for that port regardless of host header.</span></span> <span data-ttu-id="bd83e-135">Поэтому обратный прокси-сервер, который может совместно использовать порты, должен пересылать данные в Kestrel на уникальный IP-адрес и порт.</span><span class="sxs-lookup"><span data-stu-id="bd83e-135">A reverse proxy that can share ports must then forward to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="bd83e-136">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным по другим причинам:</span><span class="sxs-lookup"><span data-stu-id="bd83e-136">Even if a reverse proxy server isn't required, using one might be a good choice for other reasons:</span></span>

* <span data-ttu-id="bd83e-137">Он позволяет ограничить открытую контактную зону.</span><span class="sxs-lookup"><span data-stu-id="bd83e-137">It can limit your exposed surface area.</span></span>
* <span data-ttu-id="bd83e-138">Он предоставляет необязательный дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="bd83e-138">It provides an optional additional layer of configuration and defense.</span></span>
* <span data-ttu-id="bd83e-139">Он может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="bd83e-139">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="bd83e-140">Он упрощает балансировку нагрузки и настройку SSL.</span><span class="sxs-lookup"><span data-stu-id="bd83e-140">It simplifies load balancing and SSL set-up.</span></span> <span data-ttu-id="bd83e-141">SSL-сертификат требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложений во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="bd83e-141">Only your reverse proxy server requires an SSL certificate, and that server can communicate with your application servers on the internal network using plain HTTP.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="bd83e-142">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bd83e-142">How to use Kestrel in ASP.NET Core apps</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-143">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-143">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bd83e-144">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="bd83e-144">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="bd83e-145">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bd83e-145">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="bd83e-146">В файле *Program.cs* код шаблона вызывает `CreateDefaultBuilder`, который в фоновом режиме вызывает [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_).</span><span class="sxs-lookup"><span data-stu-id="bd83e-146">In *Program.cs*, the template code calls `CreateDefaultBuilder`, which calls [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) behind the scenes.</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="bd83e-147">Если нужно настроить параметры Kestrel, вызовите `UseKestrel` в *Program.cs*, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="bd83e-147">If you need to configure Kestrel options, call `UseKestrel` in *Program.cs* as shown in the following example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-148">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-148">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bd83e-149">Установите пакет NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="bd83e-149">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="bd83e-150">Вызовите метод расширения [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) для `WebHostBuilder` в вашем методе `Main`, указав все нужные [параметры Kestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions), как показано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="bd83e-150">Call the [UseKestrel](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions#Microsoft_AspNetCore_Hosting_WebHostBuilderKestrelExtensions_UseKestrel_Microsoft_AspNetCore_Hosting_IWebHostBuilder_) extension method on `WebHostBuilder` in your `Main` method, specifying any [Kestrel options](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions) that you need, as shown in the next section.</span></span>

[!code-csharp[](kestrel/sample1/Program.cs?name=snippet_Main&highlight=13-19)]

---

### <a name="kestrel-options"></a><span data-ttu-id="bd83e-151">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="bd83e-151">Kestrel options</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-152">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-152">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bd83e-153">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="bd83e-153">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="bd83e-154">Ниже приведены некоторые ограничения, которые можно задать:</span><span class="sxs-lookup"><span data-stu-id="bd83e-154">Here are some of the limits you can set:</span></span>

- <span data-ttu-id="bd83e-155">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="bd83e-155">Maximum client connections</span></span>
- <span data-ttu-id="bd83e-156">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="bd83e-156">Maximum request body size</span></span>
- <span data-ttu-id="bd83e-157">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="bd83e-157">Minimum request body data rate</span></span>

<span data-ttu-id="bd83e-158">Для задания этих и других ограничений используется свойство `Limits` класса [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="bd83e-158">You set these constraints and others in the `Limits` property of the [KestrelServerOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs) class.</span></span> <span data-ttu-id="bd83e-159">Свойство `Limits` содержит экземпляр класса [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs).</span><span class="sxs-lookup"><span data-stu-id="bd83e-159">The `Limits` property holds an instance of the [KestrelServerLimits](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs) class.</span></span> 

<span data-ttu-id="bd83e-160">**Максимальное число клиентских подключений**</span><span class="sxs-lookup"><span data-stu-id="bd83e-160">**Maximum client connections**</span></span>

<span data-ttu-id="bd83e-161">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="bd83e-161">The maximum number of concurrent open TCP connections can be set for the entire application with the following code:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="bd83e-162">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="bd83e-162">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="bd83e-163">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-163">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span> 

<span data-ttu-id="bd83e-164">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="bd83e-164">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="bd83e-165">**Maximum request body size** (Максимальный размер текста запроса)</span><span class="sxs-lookup"><span data-stu-id="bd83e-165">**Maximum request body size**</span></span>

<span data-ttu-id="bd83e-166">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="bd83e-166">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6MB.</span></span> 

<span data-ttu-id="bd83e-167">Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) в методе действия:</span><span class="sxs-lookup"><span data-stu-id="bd83e-167">The recommended way to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](https://github.com/aspnet/Mvc/blob/rel/2.0.0/src/Microsoft.AspNetCore.Mvc.Core/RequestSizeLimitAttribute.cs) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="bd83e-168">Приведенный ниже пример показывает, как настроить ограничение для всего приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="bd83e-168">Here's an example that shows how to configure the constraint for the entire application, every request:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="bd83e-169">Можно переопределить параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="bd83e-169">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=3-4)]
 
<span data-ttu-id="bd83e-170">При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="bd83e-170">An exception is thrown if you try to configure the limit on a request after the application has started reading the request.</span></span> <span data-ttu-id="bd83e-171">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="bd83e-171">There's an `IsReadOnly` property that tells you if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="bd83e-172">**Minimum request body data rate** (Минимальная скорость передачи данных в тексте запроса)</span><span class="sxs-lookup"><span data-stu-id="bd83e-172">**Minimum request body data rate**</span></span>

<span data-ttu-id="bd83e-173">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="bd83e-173">Kestrel checks every second if data is coming in at the specified rate in bytes/second.</span></span> <span data-ttu-id="bd83e-174">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="bd83e-174">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="bd83e-175">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="bd83e-175">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="bd83e-176">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="bd83e-176">The default minimum rate is 240 bytes/second, with a 5 second grace period.</span></span>

<span data-ttu-id="bd83e-177">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="bd83e-177">A minimum rate also applies to the response.</span></span> <span data-ttu-id="bd83e-178">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="bd83e-178">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span> 

<span data-ttu-id="bd83e-179">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="bd83e-179">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="bd83e-180">Вы можете настроить скорости для отдельного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="bd83e-180">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="bd83e-181">Сведения о других параметрах Kestrel см. в описании следующих классов:</span><span class="sxs-lookup"><span data-stu-id="bd83e-181">For information about other Kestrel options, see the following classes:</span></span>

* [<span data-ttu-id="bd83e-182">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="bd83e-182">KestrelServerOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerOptions.cs)
* [<span data-ttu-id="bd83e-183">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="bd83e-183">KestrelServerLimits</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/KestrelServerLimits.cs)
* [<span data-ttu-id="bd83e-184">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="bd83e-184">ListenOptions</span></span>](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-185">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-185">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bd83e-186">Сведения о параметрах Kestrel см. в разделе [Класс KestrelServerOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="bd83e-186">For information about Kestrel options, see [KestrelServerOptions class](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions).</span></span>

---

### <a name="endpoint-configuration"></a><span data-ttu-id="bd83e-187">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="bd83e-187">Endpoint configuration</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-188">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-188">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bd83e-189">По умолчанию ASP.NET Core привязан к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-189">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="bd83e-190">Вы можете настроить префиксы URL-адресов и порты, которые должен прослушивать Kestrel, вызывая методы `Listen` или `ListenUnixSocket` для `KestrelServerOptions`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-190">You configure URL prefixes and ports for Kestrel to listen on by calling `Listen` or `ListenUnixSocket` methods on `KestrelServerOptions`.</span></span> <span data-ttu-id="bd83e-191">(`UseUrls`, аргумент командной строки `urls` и переменная среды ASPNETCORE_URLS тоже работают, однако на них распространяются ограничения, указанные [далее в этой статье](#useurls-limitations).)</span><span class="sxs-lookup"><span data-stu-id="bd83e-191">(`UseUrls`, the `urls` command-line argument, and the ASPNETCORE_URLS environment variable also work but have the limitations noted [later in this article](#useurls-limitations).)</span></span>

<span data-ttu-id="bd83e-192">**Привязка к TCP-сокету**</span><span class="sxs-lookup"><span data-stu-id="bd83e-192">**Bind to a TCP socket**</span></span>

<span data-ttu-id="bd83e-193">Метод `Listen` привязан к TCP-сокету, а лямбда-выражение параметров позволяет настроить SSL-сертификат:</span><span class="sxs-lookup"><span data-stu-id="bd83e-193">The `Listen` method binds to a TCP socket, and an options lambda lets you configure an SSL certificate:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="bd83e-194">Обратите внимание, как в этом примере настраивается SSL для конечной точки с помощью [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span><span class="sxs-lookup"><span data-stu-id="bd83e-194">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](https://github.com/aspnet/KestrelHttpServer/blob/rel/2.0.0/src/Microsoft.AspNetCore.Server.Kestrel.Core/ListenOptions.cs).</span></span> <span data-ttu-id="bd83e-195">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="bd83e-195">You can use the same API to configure other Kestrel settings for particular endpoints.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

<span data-ttu-id="bd83e-196">**Привязка к сокету UNIX**</span><span class="sxs-lookup"><span data-stu-id="bd83e-196">**Bind to a Unix socket**</span></span>

<span data-ttu-id="bd83e-197">Вы можете прослушивать сокет UNIX для улучшения производительности Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="bd83e-197">You can listen on a Unix socket for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/sample2/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="bd83e-198">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="bd83e-198">**Port 0**</span></span>

<span data-ttu-id="bd83e-199">Если указать номер порта 0, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="bd83e-199">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="bd83e-200">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="bd83e-200">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample2/Startup.cs?name=snippet_Configure&highlight=3,13,16-17)]

<a id="useurls-limitations"></a>

<span data-ttu-id="bd83e-201">**Ограничения UseUrls**</span><span class="sxs-lookup"><span data-stu-id="bd83e-201">**UseUrls limitations**</span></span>

<span data-ttu-id="bd83e-202">Вы можете настроить конечные точки, вызвав метод `UseUrls` либо используя аргумент командной строки `urls` или переменную среды ASPNETCORE_URLS.</span><span class="sxs-lookup"><span data-stu-id="bd83e-202">You can configure endpoints by calling the `UseUrls` method or using the `urls` command-line argument or the ASPNETCORE_URLS environment variable.</span></span> <span data-ttu-id="bd83e-203">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="bd83e-203">These methods are useful if you want your code to work with servers other than Kestrel.</span></span> <span data-ttu-id="bd83e-204">Однако следует учитывать следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="bd83e-204">However, be aware of these limitations:</span></span>

* <span data-ttu-id="bd83e-205">С этими методами невозможно использовать SSL.</span><span class="sxs-lookup"><span data-stu-id="bd83e-205">You can't use SSL with these methods.</span></span>
* <span data-ttu-id="bd83e-206">Если вы используете как метод `Listen`, так и `UseUrls`, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-206">If you use both the `Listen` method and `UseUrls`, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="bd83e-207">**Конфигурация конечной точки для IIS**</span><span class="sxs-lookup"><span data-stu-id="bd83e-207">**Endpoint configuration for IIS**</span></span>

<span data-ttu-id="bd83e-208">Если вы используете IIS, привязки URL-адресов для IIS переопределяют любые привязки, заданные с помощью вызова `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-208">If you use IIS, the URL bindings for IIS override any bindings that you set by calling either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="bd83e-209">Дополнительные сведения см. в статье [Введение в модуль ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="bd83e-209">For more information, see [Introduction to ASP.NET Core Module](aspnet-core-module.md).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-210">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-210">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="bd83e-211">По умолчанию ASP.NET Core привязан к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-211">By default ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="bd83e-212">Настроить префиксы URL-адресов и порты, прослушиваемые Kestrel, можно с помощью метода расширения `UseUrls`, аргумента командной строки `urls` или системы конфигурации ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="bd83e-212">You can configure URL prefixes and ports for Kestrel to listen on by using the `UseUrls` extension method, the `urls` command-line argument, or the ASP.NET Core configuration system.</span></span> <span data-ttu-id="bd83e-213">Дополнительные сведения об этих методах см. в разделе [Размещение](../../fundamentals/hosting.md).</span><span class="sxs-lookup"><span data-stu-id="bd83e-213">For more information about these methods, see [Hosting](../../fundamentals/hosting.md).</span></span> <span data-ttu-id="bd83e-214">Сведения о работе привязки URL-адресов при использовании IIS в качестве обратного прокси-сервера см. в разделе [Модуль ASP.NET Core](aspnet-core-module.md).</span><span class="sxs-lookup"><span data-stu-id="bd83e-214">For information about how URL binding works when you use IIS as a reverse proxy, see [ASP.NET Core Module](aspnet-core-module.md).</span></span> 

---

### <a name="url-prefixes"></a><span data-ttu-id="bd83e-215">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="bd83e-215">URL prefixes</span></span>

<span data-ttu-id="bd83e-216">Когда вы вызываете метод `UseUrls` либо используете аргумент командной строки `urls` или переменную среды ASPNETCORE_URLS, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="bd83e-216">If you call `UseUrls` or use the `urls` command-line argument or ASPNETCORE_URLS environment variable, the URL prefixes can be in any of the following formats.</span></span> 

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="bd83e-218">Допустимы только префиксы URL-адресов HTTP; Kestrel не поддерживает SSL при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-218">Only HTTP URL prefixes are valid; Kestrel doesn't support SSL when you configure URL bindings by using `UseUrls`.</span></span>

* <span data-ttu-id="bd83e-219">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-219">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="bd83e-220">0.0.0.0 является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="bd83e-220">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="bd83e-221">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-221">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  ```

  <span data-ttu-id="bd83e-222">[::] является IPv6-аналогом IPv4-адреса 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="bd83e-222">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="bd83e-223">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-223">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="bd83e-224">Имена узлов, \* и + не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="bd83e-224">Host names, \*, and +, are not special.</span></span> <span data-ttu-id="bd83e-225">Все, что не является распознаваемым IP-адресом или "localhost", привязывается ко всем IPv4- и IPv6-адресам.</span><span class="sxs-lookup"><span data-stu-id="bd83e-225">Anything that's not a recognized IP address or "localhost" will bind to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="bd83e-226">Если нужно привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](httpsys.md) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="bd83e-226">If you need to bind different host names to different ASP.NET Core applications on the same port, use [HTTP.sys](httpsys.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="bd83e-227">Имя "Localhost" с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-227">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="bd83e-228">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="bd83e-228">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="bd83e-229">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="bd83e-229">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="bd83e-230">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="bd83e-230">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-231">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-231">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* <span data-ttu-id="bd83e-232">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-232">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="bd83e-233">0.0.0.0 является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="bd83e-233">0.0.0.0 is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="bd83e-234">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-234">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/ 
  https://[0:0:0:0:0:ffff:4137:270a]:443/ 
  ```

  <span data-ttu-id="bd83e-235">[::] является IPv6-аналогом IPv4-адреса 0.0.0.0.</span><span class="sxs-lookup"><span data-stu-id="bd83e-235">[::] is the IPv6 equivalent of IPv4 0.0.0.0.</span></span>


* <span data-ttu-id="bd83e-236">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-236">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="bd83e-237">Имена узлов, \* и + не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="bd83e-237">Host names, \*, and + aren't special.</span></span> <span data-ttu-id="bd83e-238">Все, что не является распознаваемым IP-адресом или "localhost", привязывается ко всем IPv4- и IPv6-адресам.</span><span class="sxs-lookup"><span data-stu-id="bd83e-238">Anything that isn't a recognized IP address or "localhost" binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="bd83e-239">Если нужно привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [WebListener](weblistener.md) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="bd83e-239">If you need to bind different host names to different ASP.NET Core applications on the same port, use [WebListener](weblistener.md) or a reverse proxy server such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="bd83e-240">Имя "Localhost" с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="bd83e-240">"Localhost" name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="bd83e-241">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="bd83e-241">When `localhost` is specified, Kestrel tries to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="bd83e-242">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="bd83e-242">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="bd83e-243">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="bd83e-243">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span> 

* <span data-ttu-id="bd83e-244">Сокет UNIX</span><span class="sxs-lookup"><span data-stu-id="bd83e-244">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock  
  ```

<span data-ttu-id="bd83e-245">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="bd83e-245">**Port 0**</span></span>

<span data-ttu-id="bd83e-246">Если указать номер порта 0, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="bd83e-246">If you specify port number 0, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="bd83e-247">Привязка к порту 0 разрешена для любого имени узла или IP-адреса, кроме имени `localhost`.</span><span class="sxs-lookup"><span data-stu-id="bd83e-247">Binding to port 0 is allowed for any host name or IP except for `localhost` name.</span></span>

<span data-ttu-id="bd83e-248">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="bd83e-248">The following example shows how to determine which port Kestrel actually bound to at runtime:</span></span>

[!code-csharp[](kestrel/sample1/Startup.cs?name=snippet_Configure)]

<span data-ttu-id="bd83e-249">**Префиксы URL-адресов для SSL**</span><span class="sxs-lookup"><span data-stu-id="bd83e-249">**URL prefixes for SSL**</span></span>

<span data-ttu-id="bd83e-250">Обязательно включите префиксы URL-адресов с `https:`, если вызываете метод расширения `UseHttps`, как показано ниже.</span><span class="sxs-lookup"><span data-stu-id="bd83e-250">Be sure to include URL prefixes with `https:` if you call the `UseHttps` extension method, as shown below.</span></span>

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
> <span data-ttu-id="bd83e-251">HTTPS и HTTP не могут размещаться на одном порту.</span><span class="sxs-lookup"><span data-stu-id="bd83e-251">HTTPS and HTTP cannot be hosted on the same port.</span></span>

[!INCLUDE[How to make an SSL cert](../../includes/make-ssl-cert.md)]

---
## <a name="next-steps"></a><span data-ttu-id="bd83e-252">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="bd83e-252">Next steps</span></span>

<span data-ttu-id="bd83e-253">Дополнительные сведения см. в следующих ресурсах:</span><span class="sxs-lookup"><span data-stu-id="bd83e-253">For more information, see the following resources:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="bd83e-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

* [<span data-ttu-id="bd83e-255">Пример приложения для 2.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-255">Sample app for 2.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample2)
* [<span data-ttu-id="bd83e-256">Исходный код Kestrel</span><span class="sxs-lookup"><span data-stu-id="bd83e-256">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="bd83e-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

* [<span data-ttu-id="bd83e-258">Пример приложения для 1.x</span><span class="sxs-lookup"><span data-stu-id="bd83e-258">Sample app for 1.x</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/sample1)
* [<span data-ttu-id="bd83e-259">Исходный код Kestrel</span><span class="sxs-lookup"><span data-stu-id="bd83e-259">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)

---
