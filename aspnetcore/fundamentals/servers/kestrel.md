---
title: Реализации веб-сервера Kestrel в ASP.NET Core
author: rick-anderson
description: Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core.
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/13/2018
uid: fundamentals/servers/kestrel
ms.openlocfilehash: 4006057b8fcef9c28274bc52a311f15bff92ffb0
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49326151"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="aa904-103">Реализации веб-сервера Kestrel в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa904-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="aa904-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross) и [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter)</span><span class="sxs-lookup"><span data-stu-id="aa904-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), and [Stephen Halter](https://twitter.com/halter73)</span></span>

<span data-ttu-id="aa904-105">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="aa904-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="aa904-106">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa904-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="aa904-107">Kestrel поддерживает следующие функции:</span><span class="sxs-lookup"><span data-stu-id="aa904-107">Kestrel supports the following features:</span></span>

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="aa904-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="aa904-108">HTTPS</span></span>
* <span data-ttu-id="aa904-109">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="aa904-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="aa904-110">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="aa904-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="aa904-111">HTTP/2 (за исключением macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="aa904-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="aa904-112">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="aa904-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="aa904-113">HTTPS</span><span class="sxs-lookup"><span data-stu-id="aa904-113">HTTPS</span></span>
* <span data-ttu-id="aa904-114">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="aa904-114">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="aa904-115">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="aa904-115">Unix sockets for high performance behind Nginx</span></span>

::: moniker-end

<span data-ttu-id="aa904-116">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa904-116">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="aa904-117">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="aa904-117">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.2"

## <a name="http2-support"></a><span data-ttu-id="aa904-118">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="aa904-118">HTTP/2 support</span></span>

<span data-ttu-id="aa904-119">Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) доступен для приложений ASP.NET Core, если выполнены следующие базовые требования:</span><span class="sxs-lookup"><span data-stu-id="aa904-119">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="aa904-120">Операционная система&dagger;:</span><span class="sxs-lookup"><span data-stu-id="aa904-120">Operating system&dagger;</span></span>
  * <span data-ttu-id="aa904-121">Windows Server 2012 R2/Windows 8.1 или более поздней версии;</span><span class="sxs-lookup"><span data-stu-id="aa904-121">Windows Server 2012 R2/Windows 8.1 or later</span></span>
  * <span data-ttu-id="aa904-122">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="aa904-122">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="aa904-123">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="aa904-123">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="aa904-124">Подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3).</span><span class="sxs-lookup"><span data-stu-id="aa904-124">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="aa904-125">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="aa904-125">TLS 1.2 or later connection</span></span>

<span data-ttu-id="aa904-126">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="aa904-126">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="aa904-127">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="aa904-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="aa904-128">По умолчанию протокол HTTP/2 отключен.</span><span class="sxs-lookup"><span data-stu-id="aa904-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="aa904-129">Дополнительные сведения о конфигурации, см. в разделах [о параметрах Kestrel](#kestrel-options) и [о конфигурации конечной точки](#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="aa904-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [Endpoint configuration](#endpoint-configuration) sections.</span></span>

::: moniker-end

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="aa904-130">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="aa904-130">When to use Kestrel with a reverse proxy</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-131">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="aa904-131">You can use Kestrel by itself or with a *reverse proxy server*, such as IIS, Nginx, or Apache.</span></span> <span data-ttu-id="aa904-132">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="aa904-132">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="aa904-135">Любая из этих конфигураций &mdash; с обратным прокси-сервером и без него &mdash; является допустимой и поддерживаемой для размещения основных компонентов приложений ASP.NET версии 2.0 и выше.</span><span class="sxs-lookup"><span data-stu-id="aa904-135">Either configuration&mdash;with or without a reverse proxy server&mdash;is a valid and supported hosting configuration for ASP.NET Core 2.0 or later apps.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aa904-136">Если приложение принимает запросы только от внутренней сети, можно использовать Kestrel напрямую как сервер приложения.</span><span class="sxs-lookup"><span data-stu-id="aa904-136">If an app accepts requests only from an internal network, Kestrel can be used directly as the app's server.</span></span>

![Kestrel взаимодействует с вашей внутренней сетью напрямую.](kestrel/_static/kestrel-to-internal.png)

<span data-ttu-id="aa904-138">Если приложение имеет доступ к Интернету, необходимо использовать IIS, Nginx или Apache в качестве *обратного прокси-сервера*.</span><span class="sxs-lookup"><span data-stu-id="aa904-138">If you expose your app to the Internet, use IIS, Nginx, or Apache as a *reverse proxy server*.</span></span> <span data-ttu-id="aa904-139">Обратный прокси-сервер получает HTTP-запросы из Интернета и пересылает их в Kestrel после определенной предварительной обработки.</span><span class="sxs-lookup"><span data-stu-id="aa904-139">A reverse proxy server receives HTTP requests from the Internet and forwards them to Kestrel after some preliminary handling.</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="aa904-141">По соображениям безопасности для развертываний на общедоступном пограничном сервере (открытом для интернет-трафика) нужен обратный прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="aa904-141">A reverse proxy is required for public-facing edge server deployments (exposed to traffic from the Internet) for security reasons.</span></span> <span data-ttu-id="aa904-142">В версиях Kestrel 1.x нет полного набора функций защиты от атак, например надлежащего времени ожидания, ограничений по размеру и ограничений количества одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="aa904-142">The 1.x versions of Kestrel don't have a full complement of defenses against attacks, such as appropriate timeouts, size limits, and concurrent connection limits.</span></span>

::: moniker-end

<span data-ttu-id="aa904-143">Обратный прокси-сервер требуется в ситуациях, когда несколько приложений совместно используют один IP-адрес и порт и выполняются на одном сервере.</span><span class="sxs-lookup"><span data-stu-id="aa904-143">A reverse proxy scenario exists when there are multiple apps that share the same IP and port running on a single server.</span></span> <span data-ttu-id="aa904-144">Kestrel не поддерживает этот сценарий, поскольку не поддерживает совместное использование одного IP-адреса и порта несколькими процессами.</span><span class="sxs-lookup"><span data-stu-id="aa904-144">Kestrel doesn't support this scenario because Kestrel doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="aa904-145">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка узла запросов.</span><span class="sxs-lookup"><span data-stu-id="aa904-145">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' host header.</span></span> <span data-ttu-id="aa904-146">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="aa904-146">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="aa904-147">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным:</span><span class="sxs-lookup"><span data-stu-id="aa904-147">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice:</span></span>

* <span data-ttu-id="aa904-148">Он может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="aa904-148">It can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="aa904-149">Он предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="aa904-149">It provides an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="aa904-150">Он может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="aa904-150">It might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="aa904-151">Он упрощает балансировку нагрузки и настройку SSL.</span><span class="sxs-lookup"><span data-stu-id="aa904-151">It simplifies load balancing and SSL configuration.</span></span> <span data-ttu-id="aa904-152">SSL-сертификат требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложений во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="aa904-152">Only the reverse proxy server requires an SSL certificate, and that server can communicate with your app servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="aa904-153">Если не используется обратный прокси-сервер с включенной фильтрацией узла, необходимо включить [фильтрацию узла](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="aa904-153">If not using a reverse proxy with host filtering enabled, [host filtering](#host-filtering) must be enabled.</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="aa904-154">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa904-154">How to use Kestrel in ASP.NET Core apps</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-155">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="aa904-155">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="aa904-156">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa904-156">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="aa904-157">В файле *Program.cs* код шаблона вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), который в фоновом режиме вызывает [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel).</span><span class="sxs-lookup"><span data-stu-id="aa904-157">In *Program.cs*, the template code calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="aa904-158">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder`, используйте `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="aa904-158">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            // Set properties and call methods on options
        });
```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

<span data-ttu-id="aa904-159">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder`, вызовите [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span><span class="sxs-lookup"><span data-stu-id="aa904-159">To provide additional configuration after calling `CreateDefaultBuilder`, call [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel):</span></span>

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            // Set properties and call methods on options
        })
        .Build();
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aa904-160">Установите пакет NuGet [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/).</span><span class="sxs-lookup"><span data-stu-id="aa904-160">Install the [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) NuGet package.</span></span>

<span data-ttu-id="aa904-161">Вызовите метод расширения [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) для [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) в методе `Main`, указав все необходимые [параметры Kestrel](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1), как показано в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="aa904-161">Call the [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) extension method on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder?view=aspnetcore-1.1) in the `Main` method, specifying any [Kestrel options](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1) required, as shown in the next section.</span></span>

[!code-csharp[](kestrel/samples/1.x/KestrelSample/Program.cs?name=snippet_Main&highlight=13-19)]

::: moniker-end

## <a name="kestrel-options"></a><span data-ttu-id="aa904-162">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="aa904-162">Kestrel options</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-163">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="aa904-163">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span> <span data-ttu-id="aa904-164">Несколько важных ограничений, которые можно настроить:</span><span class="sxs-lookup"><span data-stu-id="aa904-164">A few important limits that can be customized:</span></span>

* <span data-ttu-id="aa904-165">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="aa904-165">Maximum client connections</span></span>
* <span data-ttu-id="aa904-166">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="aa904-166">Maximum request body size</span></span>
* <span data-ttu-id="aa904-167">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="aa904-167">Minimum request body data rate</span></span>

<span data-ttu-id="aa904-168">Задайте эти и другие ограничения в свойстве [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) класса [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions).</span><span class="sxs-lookup"><span data-stu-id="aa904-168">Set these and other constraints on the [Limits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.limits) property of the [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) class.</span></span> <span data-ttu-id="aa904-169">Свойство `Limits` содержит экземпляр класса [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits).</span><span class="sxs-lookup"><span data-stu-id="aa904-169">The `Limits` property holds an instance of the [KestrelServerLimits](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits) class.</span></span>

### <a name="maximum-client-connections"></a><span data-ttu-id="aa904-170">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="aa904-170">Maximum client connections</span></span>

[<span data-ttu-id="aa904-171">MaxConcurrentConnections</span><span class="sxs-lookup"><span data-stu-id="aa904-171">MaxConcurrentConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentconnections)  
[<span data-ttu-id="aa904-172">MaxConcurrentUpgradedConnections</span><span class="sxs-lookup"><span data-stu-id="aa904-172">MaxConcurrentUpgradedConnections</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxconcurrentupgradedconnections)

::: moniker-end

<span data-ttu-id="aa904-173">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="aa904-173">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentConnections = 100;
        });
```

::: moniker-end

<span data-ttu-id="aa904-174">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="aa904-174">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="aa904-175">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="aa904-175">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-176">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="aa904-176">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="aa904-177">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="aa904-177">Maximum request body size</span></span>

[<span data-ttu-id="aa904-178">MaxRequestBodySize</span><span class="sxs-lookup"><span data-stu-id="aa904-178">MaxRequestBodySize</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize)

<span data-ttu-id="aa904-179">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="aa904-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="aa904-180">Чтобы переопределить это ограничение в приложении ASP.NET Core MVC, рекомендуется использовать атрибут [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) в методе действия:</span><span class="sxs-lookup"><span data-stu-id="aa904-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the [RequestSizeLimit](/dotnet/api/microsoft.aspnetcore.mvc.requestsizelimitattribute) attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

::: moniker-end

<span data-ttu-id="aa904-181">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="aa904-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="aa904-182">Можно переопределить параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="aa904-182">You can override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-183">При попытке настроить ограничение для запроса после того, как приложение начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="aa904-183">An exception is thrown if you attempt to configure the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="aa904-184">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="aa904-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="aa904-185">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="aa904-185">Minimum request body data rate</span></span>

[<span data-ttu-id="aa904-186">MinRequestBodyDataRate</span><span class="sxs-lookup"><span data-stu-id="aa904-186">MinRequestBodyDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minrequestbodydatarate)  
[<span data-ttu-id="aa904-187">MinResponseDataRate</span><span class="sxs-lookup"><span data-stu-id="aa904-187">MinResponseDataRate</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.minresponsedatarate)

<span data-ttu-id="aa904-188">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="aa904-188">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="aa904-189">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="aa904-189">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="aa904-190">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="aa904-190">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="aa904-191">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="aa904-191">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="aa904-192">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="aa904-192">A minimum rate also applies to the response.</span></span> <span data-ttu-id="aa904-193">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="aa904-193">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="aa904-194">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="aa904-194">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            options.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

<span data-ttu-id="aa904-195">Вы можете настроить скорости для отдельного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="aa904-195">You can configure the rates per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=5-8)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="aa904-196">Максимальное число потоков на подключение</span><span class="sxs-lookup"><span data-stu-id="aa904-196">Maximum streams per connection</span></span>

<span data-ttu-id="aa904-197">`Http2.MaxStreamsPerConnection` ограничивает количество параллельных потоков запросов для одного соединения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="aa904-197">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="aa904-198">Потоки сверх этого числа будут отклонены.</span><span class="sxs-lookup"><span data-stu-id="aa904-198">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="aa904-199">Значение по умолчанию — 100.</span><span class="sxs-lookup"><span data-stu-id="aa904-199">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="aa904-200">Размер таблицы заголовка</span><span class="sxs-lookup"><span data-stu-id="aa904-200">Header table size</span></span>

<span data-ttu-id="aa904-201">Декодер HPACK распаковывает заголовки HTTP для подключений HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="aa904-201">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="aa904-202">`Http2.HeaderTableSize` ограничивает размер таблицы сжатия заголовка, которую использует декодер HPACK.</span><span class="sxs-lookup"><span data-stu-id="aa904-202">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="aa904-203">Это значение указывается в октетах и должно быть больше нуля (0).</span><span class="sxs-lookup"><span data-stu-id="aa904-203">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="aa904-204">Значение по умолчанию — 4096.</span><span class="sxs-lookup"><span data-stu-id="aa904-204">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="aa904-205">Максимальный размер кадра</span><span class="sxs-lookup"><span data-stu-id="aa904-205">Maximum frame size</span></span>

<span data-ttu-id="aa904-206">`Http2.MaxFrameSize` указывает максимальный размер полезных данных в получаемом кадре подключения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="aa904-206">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="aa904-207">Это значение указывается в октетах и должно находиться в пределах от 2^14 (16 384) до 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="aa904-207">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="aa904-208">Значение по умолчанию — 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="aa904-208">The default value is 2^14 (16,384).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-209">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="aa904-209">For information about other Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="aa904-210">KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="aa904-210">KestrelServerOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions)
* [<span data-ttu-id="aa904-211">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="aa904-211">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits)
* [<span data-ttu-id="aa904-212">ListenOptions</span><span class="sxs-lookup"><span data-stu-id="aa904-212">ListenOptions</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions)

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aa904-213">Сведения о параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="aa904-213">For information about Kestrel options and limits, see:</span></span>

* [<span data-ttu-id="aa904-214">Класс KestrelServerOptions</span><span class="sxs-lookup"><span data-stu-id="aa904-214">KestrelServerOptions class</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserveroptions?view=aspnetcore-1.1)
* [<span data-ttu-id="aa904-215">KestrelServerLimits</span><span class="sxs-lookup"><span data-stu-id="aa904-215">KestrelServerLimits</span></span>](/dotnet/api/microsoft.aspnetcore.server.kestrel.kestrelserverlimits?view=aspnetcore-1.1)

::: moniker-end

## <a name="endpoint-configuration"></a><span data-ttu-id="aa904-216">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="aa904-216">Endpoint configuration</span></span>

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aa904-217">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="aa904-217">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="aa904-218">Вызовите метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) или [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) в классе [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions), чтобы настроить префиксы URL-адреса и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa904-218">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span> <span data-ttu-id="aa904-219">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье.</span><span class="sxs-lookup"><span data-stu-id="aa904-219">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section.</span></span>

<span data-ttu-id="aa904-220">Ключ конфигурации узла `urls` должен поступать из конфигурации узла, а не конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="aa904-220">The `urls` host configuration key must come from the host configuration, not the app configuration.</span></span> <span data-ttu-id="aa904-221">Добавление ключа и значения `urls` в *appsettings.json* не влияет на конфигурацию узла, так как узел полностью инициализируется к моменту считывания конфигурации из файла конфигурации.</span><span class="sxs-lookup"><span data-stu-id="aa904-221">Adding a `urls` key and value to *appsettings.json* doesn't affect host configuration because the host is completely initialized by the time the configuration is read from the configuration file.</span></span> <span data-ttu-id="aa904-222">Тем не менее ключ `urls` в *appsettings.json* можно использовать с [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в конструкторе узла для настройки узла:</span><span class="sxs-lookup"><span data-stu-id="aa904-222">However, a `urls` key in *appsettings.json* can be used with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) on the host builder to configure the host:</span></span>

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

<span data-ttu-id="aa904-223">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="aa904-223">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="aa904-224">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="aa904-224">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="aa904-225">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="aa904-225">A development certificate is created:</span></span>

* <span data-ttu-id="aa904-226">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="aa904-226">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="aa904-227">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="aa904-227">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="aa904-228">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="aa904-228">Some browsers require that you grant explicit permission to the browser to trust the local development certificate.</span></span>

<span data-ttu-id="aa904-229">Шаблоны проектов на ASP.NET Core 2.1 и более поздних версий настраивают приложения так, чтобы они запускались на HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="aa904-229">ASP.NET Core 2.1 and later project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="aa904-230">Вызовите метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) или [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) в классе [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions), чтобы настроить префиксы URL-адреса и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa904-230">Call [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) or [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) methods on [KestrelServerOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions) to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="aa904-231">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="aa904-231">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="aa904-232">Конфигурация `KestrelServerOptions` в ASP.NET Core 2.1:</span><span class="sxs-lookup"><span data-stu-id="aa904-232">ASP.NET Core 2.1 `KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionltlistenoptionsgt"></a><span data-ttu-id="aa904-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="aa904-233">ConfigureEndpointDefaults(Action&lt;ListenOptions&gt;)</span></span>

<span data-ttu-id="aa904-234">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="aa904-234">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="aa904-235">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="aa904-235">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configurehttpsdefaultsactionlthttpsconnectionadapteroptionsgt"></a><span data-ttu-id="aa904-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span><span class="sxs-lookup"><span data-stu-id="aa904-236">ConfigureHttpsDefaults(Action&lt;HttpsConnectionAdapterOptions&gt;)</span></span>

<span data-ttu-id="aa904-237">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="aa904-237">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="aa904-238">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="aa904-238">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

### <a name="configureiconfiguration"></a><span data-ttu-id="aa904-239">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="aa904-239">Configure(IConfiguration)</span></span>

<span data-ttu-id="aa904-240">Создает загрузчик конфигурации для настройки Kestrel, который принимает [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="aa904-240">Creates a configuration loader for setting up Kestrel that takes an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) as input.</span></span> <span data-ttu-id="aa904-241">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="aa904-241">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="aa904-242">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="aa904-242">ListenOptions.UseHttps</span></span>

<span data-ttu-id="aa904-243">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="aa904-243">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="aa904-244">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="aa904-244">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="aa904-245">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa904-245">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="aa904-246">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="aa904-246">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="aa904-247">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="aa904-247">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="aa904-248">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="aa904-248">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="aa904-249">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="aa904-249">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="aa904-250">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="aa904-250">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="aa904-251">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="aa904-251">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="aa904-252">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="aa904-252">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="aa904-253">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="aa904-253">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="aa904-254">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="aa904-254">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="aa904-255">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="aa904-255">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="aa904-256">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="aa904-256">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="aa904-257">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="aa904-257">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="aa904-258">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa904-258">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="aa904-259">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="aa904-259">Supported configurations described next:</span></span>

* <span data-ttu-id="aa904-260">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="aa904-260">No configuration</span></span>
* <span data-ttu-id="aa904-261">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="aa904-261">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="aa904-262">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="aa904-262">Change the defaults in code</span></span>

<span data-ttu-id="aa904-263">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="aa904-263">*No configuration*</span></span>

<span data-ttu-id="aa904-264">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="aa904-264">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<span data-ttu-id="aa904-265">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="aa904-265">Specify URLs using the:</span></span>

* <span data-ttu-id="aa904-266">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="aa904-266">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="aa904-267">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-267">`--urls` command-line argument.</span></span>
* <span data-ttu-id="aa904-268">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-268">`urls` host configuration key.</span></span>
* <span data-ttu-id="aa904-269">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-269">`UseUrls` extension method.</span></span>

<span data-ttu-id="aa904-270">Дополнительные сведения см. в разделах [URL-адреса серверов](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="aa904-270">For more information, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="aa904-271">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="aa904-271">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="aa904-272">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="aa904-272">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="aa904-273">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="aa904-273">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="aa904-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` по умолчанию, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa904-274">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="aa904-275">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa904-275">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="aa904-276">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="aa904-276">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="aa904-277">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="aa904-277">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="aa904-278">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="aa904-278">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="aa904-279">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию** , или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="aa904-279">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="aa904-280">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="aa904-280">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="aa904-281">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="aa904-281">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; defaults to My>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="aa904-282">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="aa904-282">Schema notes:</span></span>

* <span data-ttu-id="aa904-283">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="aa904-283">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="aa904-284">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="aa904-284">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="aa904-285">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="aa904-285">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="aa904-286">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="aa904-286">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="aa904-287">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="aa904-287">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="aa904-288">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="aa904-288">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="aa904-289">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="aa904-289">The `Certificate` section is optional.</span></span> <span data-ttu-id="aa904-290">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="aa904-290">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="aa904-291">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="aa904-291">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="aa904-292">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="aa904-292">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="aa904-293">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="aa904-293">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="aa904-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, options => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="aa904-294">`options.Configure(context.Configuration.GetSection("Kestrel"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, options => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

  ```csharp
  options.Configure(context.Configuration.GetSection("Kestrel"))
      .Endpoint("HTTPS", opt =>
      {
          opt.HttpsOptions.SslProtocols = SslProtocols.Tls12;
      });
  ```

  <span data-ttu-id="aa904-295">Можно также напрямую использовать `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span><span class="sxs-lookup"><span data-stu-id="aa904-295">You can also directly access `KestrelServerOptions.ConfigurationLoader` to keep iterating on the existing loader, such as the one provided by [WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder).</span></span>

* <span data-ttu-id="aa904-296">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="aa904-296">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="aa904-297">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("Kestrel"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="aa904-297">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("Kestrel"))` again with another section.</span></span> <span data-ttu-id="aa904-298">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="aa904-298">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="aa904-299">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa904-299">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="aa904-300">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="aa904-300">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="aa904-301">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="aa904-301">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="aa904-302">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="aa904-302">*Change the defaults in code*</span></span>

<span data-ttu-id="aa904-303">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="aa904-303">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="aa904-304">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="aa904-304">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="aa904-305">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="aa904-305">*Kestrel support for SNI*</span></span>

<span data-ttu-id="aa904-306">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="aa904-306">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="aa904-307">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="aa904-307">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="aa904-308">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="aa904-308">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="aa904-309">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="aa904-309">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="aa904-310">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="aa904-310">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="aa904-311">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="aa904-311">SNI support requires:</span></span>

* <span data-ttu-id="aa904-312">Запуск на целевой платформе `netcoreapp2.1`.</span><span class="sxs-lookup"><span data-stu-id="aa904-312">Running on target framework `netcoreapp2.1`.</span></span> <span data-ttu-id="aa904-313">В `netcoreapp2.0` и `net461` обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="aa904-313">On `netcoreapp2.0` and `net461`, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="aa904-314">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="aa904-314">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="aa904-315">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa904-315">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="aa904-316">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="aa904-316">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
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
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
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

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHost BuildWebHost(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
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
                    var certs = new Dictionary<string, X509Certificate2>(
                        StringComparer.OrdinalIgnoreCase);
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
        })
        .Build();
```

::: moniker-end

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="aa904-317">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="aa904-317">Bind to a TCP socket</span></span>

<span data-ttu-id="aa904-318">Метод [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) привязан к TCP-сокету, а лямбда-выражение параметров позволяет настроить SSL-сертификат:</span><span class="sxs-lookup"><span data-stu-id="aa904-318">The [Listen](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listen) method binds to a TCP socket, and an options lambda permits SSL certificate configuration:</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.Listen(IPAddress.Loopback, 5000);
            options.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-319">В примере настраивается SSL для конечной точки с помощью [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span><span class="sxs-lookup"><span data-stu-id="aa904-319">The example configures SSL for an endpoint with [ListenOptions](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.listenoptions).</span></span> <span data-ttu-id="aa904-320">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="aa904-320">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="aa904-321">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="aa904-321">Bind to a Unix socket</span></span>

<span data-ttu-id="aa904-322">Вы можете прослушивать сокет UNIX с помощью [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) для улучшения производительности Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="aa904-322">Listen on a Unix socket with [ListenUnixSocket](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserveroptions.listenunixsocket) for improved performance with Nginx, as shown in this example:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(options =>
        {
            options.ListenUnixSocket("/tmp/kestrel-test.sock");
            options.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="port-0"></a><span data-ttu-id="aa904-323">Порт 0</span><span class="sxs-lookup"><span data-stu-id="aa904-323">Port 0</span></span>

<span data-ttu-id="aa904-324">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="aa904-324">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="aa904-325">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="aa904-325">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="aa904-326">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="aa904-326">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="aa904-327">Ограничения</span><span class="sxs-lookup"><span data-stu-id="aa904-327">Limitations</span></span>

<span data-ttu-id="aa904-328">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="aa904-328">Configure endpoints with the following approaches:</span></span>

* <span data-ttu-id="aa904-329">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls).</span><span class="sxs-lookup"><span data-stu-id="aa904-329">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls)</span></span>
* <span data-ttu-id="aa904-330">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-330">`--urls` command-line argument</span></span>
* <span data-ttu-id="aa904-331">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-331">`urls` host configuration key</span></span>
* <span data-ttu-id="aa904-332">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="aa904-332">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="aa904-333">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa904-333">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="aa904-334">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="aa904-334">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="aa904-335">С этими подходами нельзя использовать SSL, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="aa904-335">SSL can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="aa904-336">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-336">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="aa904-337">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="aa904-337">IIS endpoint configuration</span></span>

<span data-ttu-id="aa904-338">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-338">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="aa904-339">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="aa904-339">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aa904-340">По умолчанию платформа ASP.NET Core привязана к `http://localhost:5000`.</span><span class="sxs-lookup"><span data-stu-id="aa904-340">By default, ASP.NET Core binds to `http://localhost:5000`.</span></span> <span data-ttu-id="aa904-341">Настройте префиксы URL-адресов и порты для Kestrel, используя:</span><span class="sxs-lookup"><span data-stu-id="aa904-341">Configure URL prefixes and ports for Kestrel using:</span></span>

* <span data-ttu-id="aa904-342">Метод расширения [UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1)</span><span class="sxs-lookup"><span data-stu-id="aa904-342">[UseUrls](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useurls?view=aspnetcore-1.1) extension method</span></span>
* <span data-ttu-id="aa904-343">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-343">`--urls` command-line argument</span></span>
* <span data-ttu-id="aa904-344">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-344">`urls` host configuration key</span></span>
* <span data-ttu-id="aa904-345">Система конфигурации ASP.NET Core, включая переменную среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="aa904-345">ASP.NET Core configuration system, including `ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="aa904-346">Дополнительные сведения об этих методах см. в разделе [Размещение](xref:fundamentals/host/index).</span><span class="sxs-lookup"><span data-stu-id="aa904-346">For more information on these methods, see [Hosting](xref:fundamentals/host/index).</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="aa904-347">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="aa904-347">IIS endpoint configuration</span></span>

<span data-ttu-id="aa904-348">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-348">When using IIS, the URL bindings for IIS override bindings set by `UseUrls`.</span></span> <span data-ttu-id="aa904-349">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="aa904-349">For more information, see the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) topic.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

### <a name="listenoptionsprotocols"></a><span data-ttu-id="aa904-350">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="aa904-350">ListenOptions.Protocols</span></span>

<span data-ttu-id="aa904-351">Свойство `Protocols` устанавливает протоколы HTTP (`HttpProtocols`), разрешенные для конечной точки подключения или для сервера.</span><span class="sxs-lookup"><span data-stu-id="aa904-351">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="aa904-352">Значение свойства `Protocols` должно входить в перечисление `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="aa904-352">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="aa904-353">Значение перечисления `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="aa904-353">`HttpProtocols` enum value</span></span> | <span data-ttu-id="aa904-354">Допустимый протокол подключения</span><span class="sxs-lookup"><span data-stu-id="aa904-354">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="aa904-355">Только HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="aa904-355">HTTP/1.1 only.</span></span> <span data-ttu-id="aa904-356">Можно использовать с протоколом TLS или без него.</span><span class="sxs-lookup"><span data-stu-id="aa904-356">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="aa904-357">Только HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="aa904-357">HTTP/2 only.</span></span> <span data-ttu-id="aa904-358">В основном используется с протоколом TLS.</span><span class="sxs-lookup"><span data-stu-id="aa904-358">Primarily used with TLS.</span></span> <span data-ttu-id="aa904-359">Может использоваться без TLS только в том случае, если клиент поддерживает [режим предварительного знания](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="aa904-359">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="aa904-360">HTTP/1.1 и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="aa904-360">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="aa904-361">Для согласования HTTP/2 требуется протокол TLS и подключение с [согласованием протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3); при их отсутствии используется подключение по умолчанию HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="aa904-361">Requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection to negotiate HTTP/2; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="aa904-362">По умолчанию используется протокол HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="aa904-362">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="aa904-363">Ограничения TLS для HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="aa904-363">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="aa904-364">TSL 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="aa904-364">TLS version 1.2 or later</span></span>
* <span data-ttu-id="aa904-365">Повторное согласование отключено.</span><span class="sxs-lookup"><span data-stu-id="aa904-365">Renegotiation disabled</span></span>
* <span data-ttu-id="aa904-366">Сжатие отключено.</span><span class="sxs-lookup"><span data-stu-id="aa904-366">Compression disabled</span></span>
* <span data-ttu-id="aa904-367">Минимальные размеры обмена временными ключами:</span><span class="sxs-lookup"><span data-stu-id="aa904-367">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="aa904-368">эллиптическая кривая Диффи-Хелмана (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; не менее 224 бит;</span><span class="sxs-lookup"><span data-stu-id="aa904-368">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="aa904-369">конечное поле Диффи-Хелмана (DHE) &lbrack;`TLS12`&rbrack; &ndash; не менее 2048 бит;</span><span class="sxs-lookup"><span data-stu-id="aa904-369">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="aa904-370">Набор шифров не внесен в черный список.</span><span class="sxs-lookup"><span data-stu-id="aa904-370">Cipher suite not blacklisted</span></span>

<span data-ttu-id="aa904-371">По умолчанию поддерживается `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; с эллиптической кривой P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="aa904-371">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="aa904-372">Следующий пример разрешает подключения HTTP/1.1 и HTTP/2 через порт 8000.</span><span class="sxs-lookup"><span data-stu-id="aa904-372">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="aa904-373">Эти подключения шифруются по протоколу TLS с использованием предоставленного сертификата:</span><span class="sxs-lookup"><span data-stu-id="aa904-373">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        }
```

<span data-ttu-id="aa904-374">Также вы можете создать реализацию `IConnectionAdapter` для фильтрации подтверждений TLS для каждого соединения по конкретным шифрам:</span><span class="sxs-lookup"><span data-stu-id="aa904-374">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, options) =>
        {
            options.Listen(IPAddress.Any, 8000, listenOptions =>
            {
                listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
                listenOptions.UseHttps("testCert.pfx", "testPassword");
                listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
            });
        }
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that you don't 
        // wish to support. Alternatively, define and compare 
        // ITlsHandshakeFeature.CipherAlgorithm to a list of acceptable cipher 
        // suites.
        //
        // A ITlsHandshakeFeature.CipherAlgorithm of CipherAlgorithmType.Null 
        // indicates that no cipher algorithm supported by Kestrel matches the 
        // requested algorithm(s).
        if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
        {
            throw new NotSupportedException("Prohibited cipher: " + tlsFeature.CipherAlgorithm);
        }

        return Task.FromResult<IAdaptedConnection>(new AdaptedConnection(context.ConnectionStream));
    }

    private class AdaptedConnection : IAdaptedConnection
    {
        public AdaptedConnection(Stream adaptedStream)
        {
            ConnectionStream = adaptedStream;
        }

        public Stream ConnectionStream { get; }

        public void Dispose()
        {
        }
    }
}
```

<span data-ttu-id="aa904-375">*Выбор протокола из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="aa904-375">*Set the protocol from configuration*</span></span>

<span data-ttu-id="aa904-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` по умолчанию, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="aa904-376">[WebHost.CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="aa904-377">В следующем примере *appsettings.json* для всех конечных точек Kestrel устанавливается протокол подключения по умолчанию (HTTP/1.1 и HTTP/2):</span><span class="sxs-lookup"><span data-stu-id="aa904-377">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndPointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="aa904-378">В следующем примере файла конфигурации задается протокол соединения для конкретной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="aa904-378">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "EndPoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="aa904-379">Указанные в коде протоколы переопределяют значения, заданные в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="aa904-379">Protocols specified in code override values set by configuration.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

## <a name="transport-configuration"></a><span data-ttu-id="aa904-380">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="aa904-380">Transport configuration</span></span>

<span data-ttu-id="aa904-381">После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах.</span><span class="sxs-lookup"><span data-stu-id="aa904-381">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="aa904-382">Это критическое изменение для приложений ASP.NET Core 2.0, обновляющихся до версии 2.1, которые вызывают [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) и зависят от одного из следующих пакетов:</span><span class="sxs-lookup"><span data-stu-id="aa904-382">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv) and depend on either of the following packages:</span></span>

* <span data-ttu-id="aa904-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (прямая ссылка на пакет)</span><span class="sxs-lookup"><span data-stu-id="aa904-383">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="aa904-384">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="aa904-384">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="aa904-385">Для ASP.NET Core 2.1 или более поздней версии проекты, которые используют [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) и требуют использования Libuv:</span><span class="sxs-lookup"><span data-stu-id="aa904-385">For ASP.NET Core 2.1 or later projects that use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) and require the use of Libuv:</span></span>

* <span data-ttu-id="aa904-386">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="aa904-386">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv" 
                      Version="<LATEST_VERSION>" />
    ```

* <span data-ttu-id="aa904-387">Вызывают [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span><span class="sxs-lookup"><span data-stu-id="aa904-387">Call [WebHostBuilderLibuvExtensions.UseLibuv](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderlibuvextensions.uselibuv):</span></span>

    ```csharp
    public class Program
    {
        public static void Main(string[] args)
        {
            CreateWebHostBuilder(args).Build().Run();
        }

        public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
            WebHost.CreateDefaultBuilder(args)
                .UseLibuv()
                .UseStartup<Startup>();
    }
    ```

::: moniker-end

### <a name="url-prefixes"></a><span data-ttu-id="aa904-388">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="aa904-388">URL prefixes</span></span>

<span data-ttu-id="aa904-389">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="aa904-389">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-390">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="aa904-390">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="aa904-391">Kestrel не поддерживает SSL при настройке привязки URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="aa904-391">Kestrel doesn't support SSL when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="aa904-392">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-392">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="aa904-393">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="aa904-393">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="aa904-394">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-394">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="aa904-395">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="aa904-395">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="aa904-396">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-396">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="aa904-397">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="aa904-397">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="aa904-398">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa904-398">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="aa904-399">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="aa904-399">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="aa904-400">Если не используется обратный прокси-сервер с включенной фильтрацией узла, необходимо включить [фильтрацию узла](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="aa904-400">If not using a reverse proxy with host filtering enabled, enable [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="aa904-401">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-401">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="aa904-402">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa904-402">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="aa904-403">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="aa904-403">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="aa904-404">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="aa904-404">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

* <span data-ttu-id="aa904-405">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-405">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  https://65.55.39.10:443/
  ```

  <span data-ttu-id="aa904-406">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="aa904-406">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="aa904-407">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-407">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  https://[0:0:0:0:0:ffff:4137:270a]:443/
  ```

  <span data-ttu-id="aa904-408">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="aa904-408">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="aa904-409">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-409">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  https://contoso.com:443/
  https://*:443/
  ```

  <span data-ttu-id="aa904-410">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="aa904-410">Host names, `*`, and `+` aren't special.</span></span> <span data-ttu-id="aa904-411">Все, что не распознается как IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa904-411">Anything that isn't a recognized IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="aa904-412">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [WebListener](xref:fundamentals/servers/weblistener) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="aa904-412">To bind different host names to different ASP.NET Core apps on the same port, use [WebListener](xref:fundamentals/servers/weblistener) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

* <span data-ttu-id="aa904-413">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="aa904-413">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="aa904-414">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="aa904-414">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="aa904-415">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="aa904-415">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="aa904-416">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="aa904-416">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

* <span data-ttu-id="aa904-417">Сокет UNIX</span><span class="sxs-lookup"><span data-stu-id="aa904-417">Unix socket</span></span>

  ```
  http://unix:/run/dan-live.sock
  ```

<span data-ttu-id="aa904-418">**Порт 0**</span><span class="sxs-lookup"><span data-stu-id="aa904-418">**Port 0**</span></span>

<span data-ttu-id="aa904-419">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="aa904-419">When the port number is `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="aa904-420">Привязка к порту `0` разрешена для любого имени узла или IP-адреса, кроме `localhost`.</span><span class="sxs-lookup"><span data-stu-id="aa904-420">Binding to port `0` is allowed for any host name or IP except for `localhost`.</span></span>

<span data-ttu-id="aa904-421">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="aa904-421">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Now listening on: http://127.0.0.1:48508
```

<span data-ttu-id="aa904-422">**Префиксы URL-адресов для SSL**</span><span class="sxs-lookup"><span data-stu-id="aa904-422">**URL prefixes for SSL**</span></span>

<span data-ttu-id="aa904-423">При вызове метода расширения `UseHttps` укажите префиксы URL-адресов с `https:`:</span><span class="sxs-lookup"><span data-stu-id="aa904-423">If calling the `UseHttps` extension method, be sure to include URL prefixes with `https:`:</span></span>

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
> <span data-ttu-id="aa904-424">HTTPS и HTTP не могут размещаться на одном порте.</span><span class="sxs-lookup"><span data-stu-id="aa904-424">HTTPS and HTTP can't be hosted on the same port.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

::: moniker-end

## <a name="host-filtering"></a><span data-ttu-id="aa904-425">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="aa904-425">Host filtering</span></span>

<span data-ttu-id="aa904-426">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="aa904-426">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="aa904-427">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="aa904-427">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="aa904-428">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="aa904-428">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="aa904-429">Эта информация не используется для проверки заголовков запроса `Host`.</span><span class="sxs-lookup"><span data-stu-id="aa904-429">None of this information is used to validate request `Host` headers.</span></span>

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="aa904-430">В качестве обходного решения можно выполнить размещение за обратным прокси-сервером с фильтрацией заголовков узла.</span><span class="sxs-lookup"><span data-stu-id="aa904-430">As a workaround, host behind a reverse proxy with host header filtering.</span></span> <span data-ttu-id="aa904-431">Это единственный поддерживаемый сценарий для Kestrel в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="aa904-431">This is the only supported scenario for Kestrel in ASP.NET Core 1.x.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="aa904-432">В качестве обходного решения используйте ПО промежуточного слоя, чтобы фильтровать запросы по заголовку `Host`:</span><span class="sxs-lookup"><span data-stu-id="aa904-432">As a workaround, use middleware to filter requests by the `Host` header:</span></span>

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

<span data-ttu-id="aa904-433">Зарегистрируйте предыдущий `HostFilteringMiddleware` в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="aa904-433">Register the preceding `HostFilteringMiddleware` in `Startup.Configure`.</span></span> <span data-ttu-id="aa904-434">Обратите внимание, что [порядок регистрации ПО промежуточного слоя](xref:fundamentals/middleware/index#order) важен.</span><span class="sxs-lookup"><span data-stu-id="aa904-434">Note that the [ordering of middleware registration](xref:fundamentals/middleware/index#order) is important.</span></span> <span data-ttu-id="aa904-435">Регистрация должна выполняться сразу после регистрации диагностического ПО промежуточного слоя (например, `app.UseExceptionHandler`).</span><span class="sxs-lookup"><span data-stu-id="aa904-435">Registration should occur immediately after Diagnostic Middleware registration (for example, `app.UseExceptionHandler`).</span></span>

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

<span data-ttu-id="aa904-436">ПО промежуточного слоя ожидает ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="aa904-436">The middleware expects an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="aa904-437">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="aa904-437">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="aa904-438">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="aa904-438">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="aa904-439">ПО промежуточного слоя фильтрации узла предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="aa904-439">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span> <span data-ttu-id="aa904-440">ПО промежуточного слоя добавляется методом [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), который вызывает [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span><span class="sxs-lookup"><span data-stu-id="aa904-440">The middleware is added by [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), which calls [AddHostFiltering](/dotnet/api/microsoft.aspnetcore.builder.hostfilteringservicesextensions.addhostfiltering):</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="aa904-441">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="aa904-441">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="aa904-442">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="aa904-442">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="aa904-443">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="aa904-443">The value is a semicolon-delimited list of host names without port numbers:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="aa904-444">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="aa904-444">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="aa904-445">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) также имеет параметр [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts).</span><span class="sxs-lookup"><span data-stu-id="aa904-445">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an [ForwardedHeadersOptions.AllowedHosts](/dotnet/api/microsoft.aspnetcore.builder.forwardedheadersoptions.allowedhosts) option.</span></span> <span data-ttu-id="aa904-446">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="aa904-446">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="aa904-447">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок узла не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="aa904-447">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the Host header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="aa904-448">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок узла пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="aa904-448">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the Host header is directly forwarded.</span></span>
>
> <span data-ttu-id="aa904-449">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в разделе [Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки](xref:host-and-deploy/proxy-load-balancer).</span><span class="sxs-lookup"><span data-stu-id="aa904-449">For more information on Forwarded Headers Middleware, see [Configure ASP.NET Core to work with proxy servers and load balancers](xref:host-and-deploy/proxy-load-balancer).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="aa904-450">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="aa904-450">Additional resources</span></span>

* [<span data-ttu-id="aa904-451">Принудительное использование HTTPS</span><span class="sxs-lookup"><span data-stu-id="aa904-451">Enforce HTTPS</span></span>](xref:security/enforcing-ssl)
* [<span data-ttu-id="aa904-452">Исходный код Kestrel</span><span class="sxs-lookup"><span data-stu-id="aa904-452">Kestrel source code</span></span>](https://github.com/aspnet/KestrelHttpServer)
* [<span data-ttu-id="aa904-453">RFC 7230. Синтаксис и маршрутизация сообщений (раздел 5.4. Узел)</span><span class="sxs-lookup"><span data-stu-id="aa904-453">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
* [<span data-ttu-id="aa904-454">Настройка ASP.NET Core для работы с прокси-серверами и подсистемами балансировки нагрузки</span><span class="sxs-lookup"><span data-stu-id="aa904-454">Configure ASP.NET Core to work with proxy servers and load balancers</span></span>](xref:host-and-deploy/proxy-load-balancer)
