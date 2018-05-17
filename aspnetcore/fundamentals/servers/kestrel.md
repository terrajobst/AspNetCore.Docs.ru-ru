---
title: Реализации веб-сервера Kestrel в ASP.NET Core
author: tdykstra
description: Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core на основе libuv.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 04/26/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 1709d26f5dfe40d178da70c286d328982f2c39a0
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="9ca27-103">Реализации веб-сервера Kestrel в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ca27-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="9ca27-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter)</span><span class="sxs-lookup"><span data-stu-id="9ca27-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="9ca27-105">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index) на основе [libuv](https://github.com/libuv/libuv), кроссплатформенный асинхронной библиотеки ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="9ca27-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index) based on [libuv](https://github.com/libuv/libuv), a cross-platform asynchronous I/O library.</span></span> <span data-ttu-id="9ca27-106">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ca27-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="9ca27-107">Kestrel поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="9ca27-107">Kestrel supports the following features:</span></span>

* <span data-ttu-id="9ca27-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="9ca27-108">HTTPS</span></span>
* <span data-ttu-id="9ca27-109">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="9ca27-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="9ca27-110">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="9ca27-110">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="9ca27-111">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="9ca27-111">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="9ca27-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9ca27-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="9ca27-113">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="9ca27-113">When to use Kestrel with a reverse proxy</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9ca27-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="9ca27-115">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="9ca27-115">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="9ca27-116">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-116">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="9ca27-119">Мы рекомендуем использовать Kestrel с обратным прокси-сервером, кроме случаев, когда Kestrel предоставляется только для внутренней сети.</span><span class="sxs-lookup"><span data-stu-id="9ca27-119">We recommend using Kestrel with a reverse proxy server unless Kestrel is only exposed to an internal network.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9ca27-120">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-120">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="9ca27-121">Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel напрямую как сервер приложения.</span><span class="sxs-lookup"><span data-stu-id="9ca27-121">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="9ca27-123">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache в качестве *обратного прокси-сервера*.</span><span class="sxs-lookup"><span data-stu-id="9ca27-123">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="9ca27-124">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-124">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="9ca27-126">По соображениям безопасности для развертываний пограничного сервера (открытого для интернет-трафика) нужен обратный прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="9ca27-126">A reverse proxy is required for edge deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="9ca27-127">В версиях Kestrel 1.x нет полного набора функций защиты от атак, например надлежащего времени ожидания, ограничений по размеру и ограничений количества одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="9ca27-127">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

* * *

<span data-ttu-id="9ca27-128">Обратный прокси-сервер требуется в ситуациях, когда несколько приложений совместно используют один IP-адрес и порт и выполняются на одном сервере.</span><span class="sxs-lookup"><span data-stu-id="9ca27-128">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="9ca27-129">Kestrel не поддерживает этот сценарий, поскольку не поддерживает совместное использование одного IP-адреса и порта несколькими процессами.</span><span class="sxs-lookup"><span data-stu-id="9ca27-129">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="9ca27-130">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка узла запросов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-130">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="9ca27-131">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="9ca27-131">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="9ca27-132">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным:</span><span class="sxs-lookup"><span data-stu-id="9ca27-132">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="9ca27-133">Он может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="9ca27-133">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="9ca27-134">Он предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="9ca27-134">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="9ca27-135">Он может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="9ca27-135">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="9ca27-136">Он упрощает балансировку нагрузки и настройку SSL.</span><span class="sxs-lookup"><span data-stu-id="9ca27-136">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="9ca27-137">SSL-сертификат требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложений во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="9ca27-137">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="9ca27-138">Если не используется обратный прокси-сервер с включенной фильтрацией узла, необходимо включить [фильтрацию узла](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="9ca27-138">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="9ca27-139">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9ca27-139">How to use Kestrel in ASP.NET Core apps</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9ca27-140">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-140">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9ca27-141">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.All](xref:fundamentals/metapackage).</span><span class="sxs-lookup"><span data-stu-id="9ca27-141">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.All metapackage](xref:fundamentals/metapackage).</span></span>

<span data-ttu-id="9ca27-142">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9ca27-142">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="9ca27-143">В файле *Program.cs* код шаблона вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), который в фоновом режиме вызывает [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel).</span><span class="sxs-lookup"><span data-stu-id="9ca27-143">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9ca27-144">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-144">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9ca27-145">Установите пакет NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="9ca27-145">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="9ca27-146">Вызовите метод расширения [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) для [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) в методе `Main`, указав все необходимые [параметры Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1), как показано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="9ca27-146">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/Program.cs?name=snippet_Main&highlight=13-19)]

* * *

### <a name="kestrel-options"></a><span data-ttu-id="9ca27-147">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="9ca27-147">Kestrel options</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9ca27-148">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-148">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9ca27-149">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="9ca27-149">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="9ca27-150">Несколько важных ограничений, которые можно настроить:</span><span class="sxs-lookup"><span data-stu-id="9ca27-150">A few important limits that can be customized:</span></span>

* <span data-ttu-id="9ca27-151">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="9ca27-151">Maximum client connections</span></span>
* <span data-ttu-id="9ca27-152">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="9ca27-152">Maximum request body size</span></span>
* <span data-ttu-id="9ca27-153">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="9ca27-153">Minimum request body data rate</span></span>

<span data-ttu-id="9ca27-154">Задайте эти и другие ограничения в свойстве [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) класса [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="9ca27-154">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="9ca27-155">Свойство `Limits` содержит экземпляр класса [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="9ca27-155">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

<span data-ttu-id="9ca27-156">**Максимальное число клиентских подключений**</span><span class="sxs-lookup"><span data-stu-id="9ca27-156">**Maximum client connections**</span></span>

[<span data-ttu-id="9ca27-157">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="9ca27-157">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="9ca27-158">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="9ca27-158">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

<span data-ttu-id="9ca27-159">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="9ca27-159">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="9ca27-160">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="9ca27-160">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="9ca27-161">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-161">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

<span data-ttu-id="9ca27-162">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="9ca27-162">The maximum number of connections is unlimited (null) by default.</span></span>

<span data-ttu-id="9ca27-163">**Maximum request body size** (Максимальный размер текста запроса)</span><span class="sxs-lookup"><span data-stu-id="9ca27-163">**Maximum request body size**</span></span>

[<span data-ttu-id="9ca27-164">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="9ca27-164">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="9ca27-165">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="9ca27-165">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="9ca27-166">Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) в методе действия:</span><span class="sxs-lookup"><span data-stu-id="9ca27-166">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="9ca27-167">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="9ca27-167">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="9ca27-168">Можно переопределить параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="9ca27-168">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="9ca27-169">При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="9ca27-169">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="9ca27-170">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="9ca27-170">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="9ca27-171">**Minimum request body data rate** (Минимальная скорость передачи данных в тексте запроса)</span><span class="sxs-lookup"><span data-stu-id="9ca27-171">**Minimum request body data rate**</span></span>

[<span data-ttu-id="9ca27-172">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="9ca27-172">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="9ca27-173">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="9ca27-173">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="9ca27-174">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="9ca27-174">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="9ca27-175">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="9ca27-175">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="9ca27-176">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="9ca27-176">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="9ca27-177">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="9ca27-177">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="9ca27-178">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="9ca27-178">A minimum rate also applies to the response.</span></span> <span data-ttu-id="9ca27-179">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-179">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="9ca27-180">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="9ca27-180">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="9ca27-181">Вы можете настроить скорости для отдельного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="9ca27-181">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Limits&highlight=5-8)]

<span data-ttu-id="9ca27-182">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="9ca27-182">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="9ca27-183">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="9ca27-183">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="9ca27-184">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="9ca27-184">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="9ca27-185">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="9ca27-185">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9ca27-186">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-186">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9ca27-187">Сведения о параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="9ca27-187">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="9ca27-188">Класс KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="9ca27-188">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="9ca27-189">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="9ca27-189">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

* * *

### <a name="endpoint-configuration"></a><span data-ttu-id="9ca27-190">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="9ca27-190">Endpoint configuration</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9ca27-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="9ca27-192">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-192">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9ca27-193">Вызовите метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) или [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) в классе [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions), чтобы настроить префиксы URL-адреса и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9ca27-193">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="9ca27-194">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="9ca27-194">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="9ca27-195">Ключ конфигурации узла `urls` должен поступать из конфигурации узла, а не конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="9ca27-195">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="9ca27-196">Добавление ключа и значения `urls` в *appsettings.json* не влияет на конфигурацию узла, так как узел полностью инициализируется к моменту считывания конфигурации из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9ca27-196">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="9ca27-197">Тем не менее ключ `urls` в *appsettings.json* можно использовать с [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в конструкторе узла для настройки узла:</span><span class="sxs-lookup"><span data-stu-id="9ca27-197">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

```csharp
var config = new ConfigurationBuilder()
    .SetBasePath(Directory.GetCurrentDirectory())
    .AddJsonFile("appSettings.json", optional: true, reloadOnChange: true)
    .Build();

var host = new WebHostBuilder()
    .UseKestrel()
    .UseConfiguration(config)
    .UseContentRoot(Directory.GetCurrentDirectory())
    .UseStartup<Startup>()
    .Build();
```
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="9ca27-199">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="9ca27-199">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="9ca27-200">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="9ca27-200">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="9ca27-201">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="9ca27-201">A development certificate is created:</span></span>

* <span data-ttu-id="9ca27-202">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="9ca27-202">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="9ca27-203">используется [средство dev-certs](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="9ca27-203">The [dev-certs tool](https://github.com/aspnet/DotNetTools/tree/dev/src/dotnet-dev-certs) is used to create a certificate.</span></span>

<span data-ttu-id="9ca27-204">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-204">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="9ca27-205">Шаблоны проектов на ASP.NET Core 2.1 и более поздних версий настраивают приложения так, чтобы они запускались на HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="9ca27-205">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="9ca27-206">Вызовите метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) или [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) в классе [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions), чтобы настроить префиксы URL-адреса и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9ca27-206">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="9ca27-207">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="9ca27-207">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="9ca27-208">Конфигурация `KestrelServerOptions` в ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="9ca27-208">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

<span data-ttu-id="9ca27-209">**ConfigureEndpointDefaults(Action<ListenOptions>)**</span><span class="sxs-lookup"><span data-stu-id="9ca27-209">**ConfigureEndpointDefaults(Action<ListenOptions>)**</span></span>  
<span data-ttu-id="9ca27-210">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-210">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="9ca27-211">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-211">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="9ca27-212">**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**</span><span class="sxs-lookup"><span data-stu-id="9ca27-212">**ConfigureHttpsDefaults(Action<HttpsConnectionAdapterOptions>)**</span></span>  
<span data-ttu-id="9ca27-213">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ca27-213">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="9ca27-214">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-214">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

<span data-ttu-id="9ca27-215">**Configure(IConfiguration)**</span><span class="sxs-lookup"><span data-stu-id="9ca27-215">**Configure(IConfiguration)**</span></span>  
<span data-ttu-id="9ca27-216">Создает загрузчик конфигурации для настройки Kestrel, который принимает [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="9ca27-216">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="9ca27-217">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9ca27-217">The configuration must be scoped to the configuration section for Kestrel.</span></span>

<span data-ttu-id="9ca27-218">**ListenOptions.UseHttps**</span><span class="sxs-lookup"><span data-stu-id="9ca27-218">**ListenOptions.UseHttps**</span></span>  
<span data-ttu-id="9ca27-219">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ca27-219">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="9ca27-220">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="9ca27-220">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="9ca27-221">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9ca27-221">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="9ca27-222">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="9ca27-222">Throws an exception if no default certificate is configured.</span></span>
* `UseHttps(string fileName)`
* `UseHttps(string fileName, string password)`
* `UseHttps(string fileName, string password, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(StoreName storeName, string subject)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location)`
* `UseHttps(StoreName storeName, string subject, bool allowInvalid, StoreLocation location, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(X509Certificate2 serverCertificate)`
* `UseHttps(X509Certificate2 serverCertificate, Action<HttpsConnectionAdapterOptions> configureOptions)`
* `UseHttps(Action<HttpsConnectionAdapterOptions> configureOptions)`

<span data-ttu-id="9ca27-223">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="9ca27-223">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="9ca27-224">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="9ca27-224">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="9ca27-225">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="9ca27-225">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="9ca27-226">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-226">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="9ca27-227">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-227">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="9ca27-228">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="9ca27-228">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="9ca27-229">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="9ca27-229">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="9ca27-230">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="9ca27-230">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="9ca27-231">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="9ca27-231">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="9ca27-232">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="9ca27-232">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="9ca27-233">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="9ca27-233">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="9ca27-234">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9ca27-234">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="9ca27-235">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="9ca27-235">Supported configurations described next:</span></span>

* <span data-ttu-id="9ca27-236">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="9ca27-236">No configuration</span></span>
* <span data-ttu-id="9ca27-237">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="9ca27-237">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="9ca27-238">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="9ca27-238">Change the defaults in code</span></span>

<span data-ttu-id="9ca27-239">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="9ca27-239">*No configuration*</span></span>

<span data-ttu-id="9ca27-240">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="9ca27-240">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="9ca27-241">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="9ca27-241">Specify URLs using the:</span></span>

* <span data-ttu-id="9ca27-242">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-242">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="9ca27-243">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-243">`--urls` command-line argument.</span></span>
* <span data-ttu-id="9ca27-244">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-244">`urls` host configuration key.</span></span>
* <span data-ttu-id="9ca27-245">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-245">`UseUrls` extension method.</span></span>

<span data-ttu-id="9ca27-246">Дополнительные сведения см. в разделах [URL-адреса серверов](xref:fundamentals/hosting#server-urls) и [Переопределение конфигурации](xref:fundamentals/hosting#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="9ca27-246">For more information, see [Server URLs](xref:fundamentals/hosting#server-urls) and [Overriding configuration](xref:fundamentals/hosting#overriding-configuration).</span></span>

<span data-ttu-id="9ca27-247">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="9ca27-247">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="9ca27-248">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="9ca27-248">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="9ca27-249">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="9ca27-249">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="9ca27-250">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` по умолчанию, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9ca27-250">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="9ca27-251">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9ca27-251">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="9ca27-252">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-252">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="9ca27-253">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="9ca27-253">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="9ca27-254">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="9ca27-254">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="9ca27-255">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию** , или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-255">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
"Kestrel": {
  "EndPoints": {
    "Http": {
      "Url": "http://localhost:5000"
    },

    "HttpsInlineCertFile": {
      "Url": "https://localhost:5001",
      "Certificate": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    },

    "HttpsInlineCertStore": {
      "Url": "https://localhost:5002",
      "Certificate": {
        "Subject": "<subject; required>",
        "Store": "<certificate store; defaults to My>",
        "Location": "<location; defaults to CurrentUser>",
        "AllowInvalid": "<true or false; defaults to false>"
      }
    },

    "HttpsDefaultCert": {
      "Url": "https://localhost:5003"
    },

    "Https": {
      "Url": "https://*:5004",
      "Certificate": {
      "Path": "<path to .pfx file>",
      "Password": "<certificate password>"
      }
    }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="9ca27-256">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-256">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="9ca27-257">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9ca27-257">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="9ca27-258">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="9ca27-258">Schema notes:</span></span>

* <span data-ttu-id="9ca27-259">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="9ca27-259">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="9ca27-260">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="9ca27-260">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="9ca27-261">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-261">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="9ca27-262">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="9ca27-262">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="9ca27-263">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="9ca27-263">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="9ca27-264">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9ca27-264">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="9ca27-265">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="9ca27-265">The `Certificate` section is optional.</span></span> <span data-ttu-id="9ca27-266">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="9ca27-266">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="9ca27-267">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="9ca27-267">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="9ca27-268">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="9ca27-268">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="9ca27-269">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-269">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="9ca27-270">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, options => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="9ca27-270">`serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="9ca27-271">Можно также напрямую использовать `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным `WebHost.CreatedDeafaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-271">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by `WebHost.CreatedDeafaultBuilder`.</span></span>

* <span data-ttu-id="9ca27-272">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="9ca27-272">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="9ca27-273">Можно загрузить несколько конфигураций, снова вызвав `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="9ca27-273">Multiple configurations may be loaded by calling `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="9ca27-274">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="9ca27-274">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="9ca27-275">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9ca27-275">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="9ca27-276">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="9ca27-276">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="9ca27-277">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="9ca27-277">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="9ca27-278">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="9ca27-278">*Change the defaults in code*</span></span>

<span data-ttu-id="9ca27-279">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="9ca27-279">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="9ca27-280">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="9ca27-280">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
options.ConfigureEndpointDefaults(opt =>
{
    opt.NoDelay = true;
});

options.ConfigureHttpsDefaults(httpsOptions =>
{
    httpsOptions.SslProtocols = SslProtocols.Tls12;
});
```

<span data-ttu-id="9ca27-281">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="9ca27-281">*Kestrel support for SNI*</span></span>

<span data-ttu-id="9ca27-282">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="9ca27-282">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="9ca27-283">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="9ca27-283">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="9ca27-284">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="9ca27-284">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="9ca27-285">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-285">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="9ca27-286">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="9ca27-286">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="9ca27-287">Для поддержки SNI приложение должно выполняться на целевой платформе `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-287">SNI support requires running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="9ca27-288">В `netcoreapp2.0` и `net461` обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-288">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="9ca27-289">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="9ca27-289">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>

```csharp
WebHost.CreateDefaultBuilder()
    .UseKestrel((context, options) =>
    {
        options.ListenAnyIP(5005, listenOptions =>
        {
            listenOptions.UseHttps(httpsOptions =>
            {
                var localhostCert = CertificateLoader.LoadFromStoreCert(
                    "localhost", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var exampleCert = CertificateLoader.LoadFromStoreCert(
                    "example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var subExampleCert = CertificateLoader.LoadFromStoreCert(
                    "sub.example.com", "My", StoreLocation.CurrentUser, 
                    allowInvalid: true);
                var certs = new Dictionary(StringComparer.OrdinalIgnoreCase);
                certs["localhost"] = localhostCert;
                certs["example.com"] = exampleCert;
                certs["sub.example.com"] = subExampleCert;

                httpsOptions.ServerCertificateSelector = (connectionContext, name) =>
                {
                    if (name != null && certs.TryGetValue(name, out var cert))
                    {
                        return cert;
                    }

                    return exampleCert;
                };
            });
        });
    });
```
::: moniker-end

<span data-ttu-id="9ca27-290">**Привязка к TCP-сокету**</span><span class="sxs-lookup"><span data-stu-id="9ca27-290">**Bind to a TCP socket**</span></span>

<span data-ttu-id="9ca27-291">Метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) привязан к TCP-сокету, а лямбда-выражение параметров позволяет настроить SSL-сертификат:</span><span class="sxs-lookup"><span data-stu-id="9ca27-291">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_DefaultBuilder&highlight=9-16)]

<span data-ttu-id="9ca27-292">Обратите внимание, как в этом примере настраивается SSL для конечной точки с помощью [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="9ca27-292">Notice how this example configures SSL for a particular endpoint by using [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="9ca27-293">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="9ca27-293">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

<span data-ttu-id="9ca27-294">**Привязка к сокету UNIX**</span><span class="sxs-lookup"><span data-stu-id="9ca27-294">**Bind to a Unix socket**</span></span>

<span data-ttu-id="9ca27-295">Вы можете прослушивать сокет UNIX с помощью [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) для улучшения производительности Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="9ca27-295">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/Program.cs?name=snippet_UnixSocket)]

<span data-ttu-id="9ca27-296">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="9ca27-296">**Port 0**</span></span>

<span data-ttu-id="9ca27-297">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="9ca27-297">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="9ca27-298">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="9ca27-298">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/Startup.cs?name=snippet_Port0&highlight=3)]

<span data-ttu-id="9ca27-299">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="9ca27-299">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="9ca27-300">**Ограничения UseUrls, аргумента командной строки --urls, ключа конфигурации узла urls и переменной среды ASPNETCORE_URLS**</span><span class="sxs-lookup"><span data-stu-id="9ca27-300">**UseUrls, --urls command-line argument, urls host configuration key, and ASPNETCORE_URLS environment variable limitations**</span></span>

<span data-ttu-id="9ca27-301">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="9ca27-301">Configure endpoints with the following approaches:</span></span>

* <span data-ttu-id="9ca27-302">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls).</span><span class="sxs-lookup"><span data-stu-id="9ca27-302">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)</span></span>
* <span data-ttu-id="9ca27-303">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-303">`--urls` command-line argument</span></span>
* <span data-ttu-id="9ca27-304">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-304">`urls` host configuration key</span></span>
* <span data-ttu-id="9ca27-305">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-305">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="9ca27-306">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="9ca27-306">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="9ca27-307">Однако следует учитывать следующие ограничения:</span><span class="sxs-lookup"><span data-stu-id="9ca27-307">However, be aware of these limitations:</span></span>

* <span data-ttu-id="9ca27-308">С этими подходами нельзя использовать SSL, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="9ca27-308">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="9ca27-309">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-309">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

<span data-ttu-id="9ca27-310">**Конфигурация конечной точки IIS**</span><span class="sxs-lookup"><span data-stu-id="9ca27-310">**IIS endpoint configuration**</span></span>

<span data-ttu-id="9ca27-311">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-311">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="9ca27-312">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9ca27-312">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9ca27-313">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-313">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="9ca27-314">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-314">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="9ca27-315">Настройте префиксы URL-адресов и порты для Kestrel, используя:</span><span class="sxs-lookup"><span data-stu-id="9ca27-315">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="9ca27-316">Метод расширения [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="9ca27-316">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="9ca27-317">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-317">`--urls` command-line argument</span></span>
* <span data-ttu-id="9ca27-318">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-318">`urls` host configuration key</span></span>
* <span data-ttu-id="9ca27-319">Система конфигурации ASP.NET Core, включая переменную среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-319">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="9ca27-320">Дополнительные сведения об этих методах см. в разделе [Размещение](xref:fundamentals/hosting).</span><span class="sxs-lookup"><span data-stu-id="9ca27-320">For more information on these methods, see [Hosting](xref:fundamentals/hosting).</span></span>

<span data-ttu-id="9ca27-321">**Конфигурация конечной точки IIS**</span><span class="sxs-lookup"><span data-stu-id="9ca27-321">**IIS endpoint configuration**</span></span>

<span data-ttu-id="9ca27-322">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-322">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="9ca27-323">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="9ca27-323">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

* * *

### <a name="url-prefixes"></a><span data-ttu-id="9ca27-324">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="9ca27-324">URL prefixes</span></span>

<span data-ttu-id="9ca27-325">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-325">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="9ca27-326">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-326">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="9ca27-327">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="9ca27-327">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="9ca27-328">Kestrel не поддерживает SSL при настройке привязки URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-328">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="9ca27-329">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-329">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="9ca27-330">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="9ca27-330">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>


* <span data-ttu-id="9ca27-331">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-331">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="9ca27-332">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-332">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>


* <span data-ttu-id="9ca27-333">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-333">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="9ca27-334">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="9ca27-334">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="9ca27-335">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="9ca27-335">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="9ca27-336">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="9ca27-336">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="9ca27-337">Если не используется обратный прокси-сервер с включенной фильтрацией узла, необходимо включить [фильтрацию узла](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="9ca27-337">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="9ca27-338">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-338">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="9ca27-339">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="9ca27-339">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="9ca27-340">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="9ca27-340">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="9ca27-341">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="9ca27-341">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="9ca27-342">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="9ca27-342">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

* <span data-ttu-id="9ca27-343">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-343">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="9ca27-344">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="9ca27-344">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="9ca27-345">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-345">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="9ca27-346">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-346">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="9ca27-347">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-347">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="9ca27-348">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="9ca27-348">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="9ca27-349">Все, что не распознается как IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="9ca27-349">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="9ca27-350">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [WebListener](xref:fundamentals/servers/weblistener) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="9ca27-350">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="9ca27-351">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="9ca27-351">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="9ca27-352">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="9ca27-352">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="9ca27-353">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="9ca27-353">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="9ca27-354">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="9ca27-354">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="9ca27-355">Сокет UNIX</span><span class="sxs-lookup"><span data-stu-id="9ca27-355">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="9ca27-356">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="9ca27-356">**Port 0**</span></span>

<span data-ttu-id="9ca27-357">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="9ca27-357">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="9ca27-358">Привязка к порту `0` разрешена для любого имени узла или IP-адреса, кроме `localhost`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-358">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="9ca27-359">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="9ca27-359">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="9ca27-360">**Префиксы URL-адресов для SSL**</span><span class="sxs-lookup"><span data-stu-id="9ca27-360">**URL prefixes for SSL**</span></span>

<span data-ttu-id="9ca27-361">При вызове метода расширения `UseHttps` укажите префиксы URL-адресов с `https:`:</span><span class="sxs-lookup"><span data-stu-id="9ca27-361">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="9ca27-362">HTTPS и HTTP не могут размещаться на одном порте.</span><span class="sxs-lookup"><span data-stu-id="9ca27-362">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

* * *

## <a name="host-filtering"></a><span data-ttu-id="9ca27-363">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="9ca27-363">Host filtering</span></span>

<span data-ttu-id="9ca27-364">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="9ca27-364">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="9ca27-365">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="9ca27-365">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="9ca27-366">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="9ca27-366">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="9ca27-367">Эта информация не используется для проверки заголовков запроса `Host`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-367">None of this information is used to validate request `Host` headers.</span></span>

<span data-ttu-id="9ca27-368">У вас есть два варианта обойти эту проблему.</span><span class="sxs-lookup"><span data-stu-id="9ca27-368">There are two workarounds:</span></span>

* <span data-ttu-id="9ca27-369">Узел за обратным прокси-сервером с фильтрацией заголовков узла.</span><span class="sxs-lookup"><span data-stu-id="9ca27-369">Host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="9ca27-370">Это был единственный поддерживаемый сценарий для Kestrel в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="9ca27-370">This was the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>
* <span data-ttu-id="9ca27-371">Используйте ПО промежуточного слоя, чтобы фильтровать запросы по заголовку `Host`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-371">Use a middleware to filter requests by the `Host` header.</span></span> <span data-ttu-id="9ca27-372">Пример ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="9ca27-372">A sample middleware follows:</span></span>

```csharp
using Microsoft.AspNetCore.Http;
using Microsoft.Extensions.Configuration;
using Microsoft.Extensions.Logging;
using Microsoft.Extensions.Primitives;
using Microsoft.Net.Http.Headers;
using System;
using System.Collections.Generic;
using System.Threading.Tasks;

// A normal middleware would provide an options type, config binding, extension methods, etc..
// This intentionally does all of the work inside of the middleware so it can be
// easily copy-pasted into docs and other projects.
public class HostFilteringMiddleware
{
    private readonly RequestDelegate _next;
    private readonly IList<string> _hosts;
    private readonly ILogger<HostFilteringMiddleware> _logger;

    public HostFilteringMiddleware(RequestDelegate next, IConfiguration config, ILogger<HostFilteringMiddleware> logger)
    {
        if (config == null)
        {
            throw new ArgumentNullException(nameof(config));
        }

        _next = next ?? throw new ArgumentNullException(nameof(next));
        _logger = logger ?? throw new ArgumentNullException(nameof(logger));

        // A semicolon separated list of host names without the port numbers.
        // IPv6 addresses must use the bounding brackets and be in their normalized form.
        _hosts = config["AllowedHosts"]?.Split(new[] { ';' }, StringSplitOptions.RemoveEmptyEntries);
        if (_hosts == null || _hosts.Count == 0)
        {
            throw new InvalidOperationException("No configuration entry found for AllowedHosts.");
        }
    }

    public Task Invoke(HttpContext context)
    {
        if (!ValidateHost(context))
        {
            context.Response.StatusCode = 400;
            _logger.LogDebug("Request rejected due to incorrect Host header.");
            return Task.CompletedTask;
        }

        return _next(context);
    }

    // This does not duplicate format validations that are expected to be performed by the host.
    private bool ValidateHost(HttpContext context)
    {
        StringSegment host = context.Request.Headers[HeaderNames.Host].ToString().Trim();

        if (StringSegment.IsNullOrEmpty(host))
        {
            // Http/1.0 does not require the Host header.
            // Http/1.1 requires the header but the value may be empty.
            return true;
        }

        // Drop the port

        var colonIndex = host.LastIndexOf(':');

        // IPv6 special case
        if (host.StartsWith("[", StringComparison.Ordinal))
        {
            var endBracketIndex = host.IndexOf(']');
            if (endBracketIndex < 0)
            {
                // Invalid format
                return false;
            }
            if (colonIndex < endBracketIndex)
            {
                // No port, just the IPv6 Host
                colonIndex = -1;
            }
        }

        if (colonIndex > 0)
        {
            host = host.Subsegment(0, colonIndex);
        }

        foreach (var allowedHost in _hosts)
        {
            if (StringSegment.Equals(allowedHost, host, StringComparison.OrdinalIgnoreCase))
            {
                return true;
            }

            // Sub-domain wildcards: *.example.com
            if (allowedHost.StartsWith("*.", StringComparison.Ordinal) && host.Length >= allowedHost.Length)
            {
                // .example.com
                var allowedRoot = new StringSegment(allowedHost, 1, allowedHost.Length - 1);

                var hostRoot = host.Subsegment(host.Length - allowedRoot.Length, allowedRoot.Length);
                if (hostRoot.Equals(allowedRoot, StringComparison.OrdinalIgnoreCase))
                {
                    return true;
                }
            }
        }

        return false;
    }
}
```

<span data-ttu-id="9ca27-373">Зарегистрируйте предыдущий `HostFilteringMiddleware` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="9ca27-373">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="9ca27-374">Обратите внимание, что [порядок регистрации ПО промежуточного слоя](xref:fundamentals/middleware/index#ordering) важен.</span><span class="sxs-lookup"><span data-stu-id="9ca27-374">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#ordering) is important.</span></span> <span data-ttu-id="9ca27-375">Регистрация должна выполняться сразу после регистрации диагностического ПО промежуточного слоя (например, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="9ca27-375">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseBrowserLink();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
    }

    app.UseMiddleware<HostFilteringMiddleware>();

    app.UseMvcWithDefaultRoute();
}
```

<span data-ttu-id="9ca27-376">Предыдущее ПО промежуточного слоя ожидает ключ `AllowedHosts` в *appsettings.\<EnvironmentName >.json*.</span><span class="sxs-lookup"><span data-stu-id="9ca27-376">The preceding middleware expects an `AllowedHosts` key in *appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="9ca27-377">Значение этого ключа — разделенный точками с запятой список имен узлов без номеров портов.</span><span class="sxs-lookup"><span data-stu-id="9ca27-377">This key's value is a semicolon-delimited list of host names without the port numbers.</span></span> <span data-ttu-id="9ca27-378">Включите пару "ключ-значение" `AllowedHosts` в *appsettings. Production.JSON*:</span><span class="sxs-lookup"><span data-stu-id="9ca27-378">Include the `AllowedHosts` key-value pair in *appsettings.Production.json*:</span></span>

```json
{
  "AllowedHosts": "example.com"
}
```

<span data-ttu-id="9ca27-379">*appsettings.Development.json* (файл конфигурации localhost):</span><span class="sxs-lookup"><span data-stu-id="9ca27-379">*appsettings.Development.json* (localhost configuration file):</span></span>

```json
{
  "AllowedHosts": "localhost"
}
```

## <a name="additional-resources"></a><span data-ttu-id="9ca27-380">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="9ca27-380">Additional resources</span></span>

* [<span data-ttu-id="9ca27-381">Принудительное использование HTTPS</span><span class="sxs-lookup"><span data-stu-id="9ca27-381">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="9ca27-382">Исходный код Kestrel</span><span class="sxs-lookup"><span data-stu-id="9ca27-382">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
