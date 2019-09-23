---
title: Реализации веб-сервера Kestrel в ASP.NET Core
author: guardrex
description: Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/13/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: b1c28f084e67d9cce74258433aa0c884878c2520
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011164"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="af94b-103">Реализации веб-сервера Kestrel в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af94b-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="af94b-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Люк Лэтэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="af94b-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="af94b-105">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="af94b-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="af94b-106">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="af94b-107">Kestrel поддерживается в следующих сценариях.</span><span class="sxs-lookup"><span data-stu-id="af94b-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="af94b-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="af94b-108">HTTPS</span></span>
* <span data-ttu-id="af94b-109">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="af94b-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="af94b-110">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="af94b-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="af94b-111">HTTP/2 (за исключением macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="af94b-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="af94b-112">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="af94b-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="af94b-113">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="af94b-114">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af94b-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="af94b-115">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="af94b-115">HTTP/2 support</span></span>

<span data-ttu-id="af94b-116">Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) доступен для приложений ASP.NET Core, если выполнены следующие базовые требования:</span><span class="sxs-lookup"><span data-stu-id="af94b-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="af94b-117">Операционная система&dagger;:</span><span class="sxs-lookup"><span data-stu-id="af94b-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="af94b-118">Windows Server 2016 / Windows 10 или более поздних версий&Dagger;</span><span class="sxs-lookup"><span data-stu-id="af94b-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="af94b-119">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="af94b-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="af94b-120">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="af94b-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="af94b-121">Подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3).</span><span class="sxs-lookup"><span data-stu-id="af94b-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="af94b-122">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="af94b-123">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="af94b-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="af94b-124">&Dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="af94b-125">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="af94b-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="af94b-126">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="af94b-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="af94b-127">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="af94b-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="af94b-128">По умолчанию протокол HTTP/2 отключен.</span><span class="sxs-lookup"><span data-stu-id="af94b-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="af94b-129">Дополнительные сведения о конфигурации см. в разделах [о параметрах Kestrel](#kestrel-options) и [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="af94b-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="af94b-130">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="af94b-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="af94b-131">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как [IIS](https://www.iis.net/), [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="af94b-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="af94b-132">Обратный прокси-сервер получает HTTP-запросы из сети и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="af94b-133">Kestrel используется в качестве веб-сервера перехода (с выходом в Интернет).</span><span class="sxs-lookup"><span data-stu-id="af94b-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="af94b-135">Kestrel используется в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="af94b-137">Любая из этих конфигураций — с обратным прокси-сервером и без него — является поддерживаемой конфигурацией для размещения.</span><span class="sxs-lookup"><span data-stu-id="af94b-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="af94b-138">Kestrel, используемый в качестве пограничного сервера без обратного прокси-сервера, не поддерживает общий доступ нескольких процессов к одним и тем же IP-адресам и портам.</span><span class="sxs-lookup"><span data-stu-id="af94b-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="af94b-139">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка `Host` запросов.</span><span class="sxs-lookup"><span data-stu-id="af94b-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="af94b-140">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="af94b-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="af94b-141">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным.</span><span class="sxs-lookup"><span data-stu-id="af94b-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="af94b-142">Обратный прокси-сервер:</span><span class="sxs-lookup"><span data-stu-id="af94b-142">A reverse proxy:</span></span>

* <span data-ttu-id="af94b-143">Может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="af94b-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="af94b-144">Предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="af94b-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="af94b-145">Может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="af94b-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="af94b-146">Упрощает настройку балансировки нагрузки и безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="af94b-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="af94b-147">Сертификат X.509 требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложения во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="af94b-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="af94b-148">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="af94b-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="af94b-149">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af94b-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="af94b-150">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="af94b-151">В файле *Program.cs* код шаблона вызывает метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, который в фоновом режиме вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>.</span><span class="sxs-lookup"><span data-stu-id="af94b-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="af94b-152">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder` и `ConfigureWebHostDefaults`, используйте `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="af94b-152">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseStartup<Startup>();
        });
```

<span data-ttu-id="af94b-153">Если приложение не вызывает `CreateDefaultBuilder`, чтобы настроить узел, вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **перед** вызовом `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="af94b-153">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new HostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel(serverOptions =>
            {
                // Set properties and call methods on options
            })
            .UseIISIntegration()
            .UseStartup<Startup>();
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="af94b-154">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="af94b-154">Kestrel options</span></span>

<span data-ttu-id="af94b-155">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="af94b-155">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="af94b-156">Задать ограничения для свойства <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> в классе <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="af94b-156">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="af94b-157">Свойство `Limits` содержит экземпляр класса <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="af94b-157">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="af94b-158">В следующих примерах используется пространство имен <xref:Microsoft.AspNetCore.Server.Kestrel.Core>.</span><span class="sxs-lookup"><span data-stu-id="af94b-158">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="af94b-159">Время ожидания проверки на активность</span><span class="sxs-lookup"><span data-stu-id="af94b-159">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="af94b-160">Получает или задает [время ожидания проверки на активность](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="af94b-160">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="af94b-161">Значение по умолчанию — 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="af94b-161">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="af94b-162">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="af94b-162">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="af94b-163">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="af94b-163">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="af94b-164">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="af94b-164">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="af94b-165">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="af94b-165">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="af94b-166">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="af94b-166">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="af94b-167">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="af94b-167">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="af94b-168">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="af94b-168">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="af94b-169">Чтобы переопределить это ограничение в приложении MVC ASP.NET Core, мы рекомендуем использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="af94b-169">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="af94b-170">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="af94b-170">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="af94b-171">Переопределите параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="af94b-171">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="af94b-172">Если приложение пытается настроить ограничение для запроса после того, как оно начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="af94b-172">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="af94b-173">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="af94b-173">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="af94b-174">При запуске приложения [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), который обслуживает [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module), ограничение на размер текста запроса Kestrel будет отключено, так как оно уже задается IIS.</span><span class="sxs-lookup"><span data-stu-id="af94b-174">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="af94b-175">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="af94b-175">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="af94b-176">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="af94b-176">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="af94b-177">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="af94b-177">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="af94b-178">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="af94b-178">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="af94b-179">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="af94b-179">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="af94b-180">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="af94b-180">A minimum rate also applies to the response.</span></span> <span data-ttu-id="af94b-181">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="af94b-181">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="af94b-182">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="af94b-182">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="af94b-183">Переопределите ограничения минимальной скорости для каждого запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="af94b-183">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="af94b-184">Возможности <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>, указанные в предыдущем примере, отсутствуют в `HttpContext.Features` для запросов HTTP/2, так как изменение ограничений скорости для каждого запроса не поддерживается для HTTP/2 из-за поддержки протокола мультиплексирования запроса.</span><span class="sxs-lookup"><span data-stu-id="af94b-184">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="af94b-185">Но возможности <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> все еще присутствуют в `HttpContext.Features` для запросов HTTP/2, так как ограничение скорости чтения может быть *полностью отключено* для отдельных запросов. Чтобы сделать это, задайте для параметра `IHttpMinRequestBodyDataRateFeature.MinDataRate` значение `null` (даже для запроса HTTP/2).</span><span class="sxs-lookup"><span data-stu-id="af94b-185">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="af94b-186">При попытке чтения свойства `IHttpMinRequestBodyDataRateFeature.MinDataRate` или при попытке задать для него значение, отличное от `null`, возникнет исключение `NotSupportedException` для запроса HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-186">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="af94b-187">Ограничения скорости на уровне сервера, которые настроены с помощью `KestrelServerOptions.Limits`, по-прежнему применяются к подключениям HTTP/1.x и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-187">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="af94b-188">Время ожидания для заголовков запросов</span><span class="sxs-lookup"><span data-stu-id="af94b-188">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="af94b-189">Получает или задает максимальное время, которое сервер уделяет получению заголовков запросов.</span><span class="sxs-lookup"><span data-stu-id="af94b-189">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="af94b-190">Значение по умолчанию — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="af94b-190">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="af94b-191">Максимальное число потоков на подключение</span><span class="sxs-lookup"><span data-stu-id="af94b-191">Maximum streams per connection</span></span>

<span data-ttu-id="af94b-192">`Http2.MaxStreamsPerConnection` ограничивает количество параллельных потоков запросов для одного соединения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-192">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="af94b-193">Потоки сверх этого числа будут отклонены.</span><span class="sxs-lookup"><span data-stu-id="af94b-193">Excess streams are refused.</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="af94b-194">Значение по умолчанию — 100.</span><span class="sxs-lookup"><span data-stu-id="af94b-194">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="af94b-195">Размер таблицы заголовка</span><span class="sxs-lookup"><span data-stu-id="af94b-195">Header table size</span></span>

<span data-ttu-id="af94b-196">Декодер HPACK распаковывает заголовки HTTP для подключений HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-196">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="af94b-197">`Http2.HeaderTableSize` ограничивает размер таблицы сжатия заголовка, которую использует декодер HPACK.</span><span class="sxs-lookup"><span data-stu-id="af94b-197">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="af94b-198">Это значение указывается в октетах и должно быть больше нуля (0).</span><span class="sxs-lookup"><span data-stu-id="af94b-198">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="af94b-199">Значение по умолчанию — 4096.</span><span class="sxs-lookup"><span data-stu-id="af94b-199">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="af94b-200">Максимальный размер кадра</span><span class="sxs-lookup"><span data-stu-id="af94b-200">Maximum frame size</span></span>

<span data-ttu-id="af94b-201">`Http2.MaxFrameSize` указывает максимально допустимый размер полезных данных в кадре подключения HTTP/2, получаемых или отправляемых сервером.</span><span class="sxs-lookup"><span data-stu-id="af94b-201">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="af94b-202">Это значение указывается в октетах и должно находиться в пределах от 2^14 (16 384) до 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="af94b-202">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="af94b-203">Значение по умолчанию — 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="af94b-203">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="af94b-204">Максимальный размер запроса заголовка</span><span class="sxs-lookup"><span data-stu-id="af94b-204">Maximum request header size</span></span>

<span data-ttu-id="af94b-205">`Http2.MaxRequestHeaderFieldSize` указывает максимально допустимый размер значений заголовка запроса (в октетах).</span><span class="sxs-lookup"><span data-stu-id="af94b-205">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="af94b-206">Это ограничение применяется к имени и значению в их сжатых и несжатых представлениях.</span><span class="sxs-lookup"><span data-stu-id="af94b-206">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="af94b-207">Значение должно быть больше нуля.</span><span class="sxs-lookup"><span data-stu-id="af94b-207">The value must be greater than zero (0).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="af94b-208">Значение по умолчанию — 8 192.</span><span class="sxs-lookup"><span data-stu-id="af94b-208">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="af94b-209">Размер окна начального подключения</span><span class="sxs-lookup"><span data-stu-id="af94b-209">Initial connection window size</span></span>

<span data-ttu-id="af94b-210">`Http2.InitialConnectionWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для всех запросов (потоков) на каждое соединение.</span><span class="sxs-lookup"><span data-stu-id="af94b-210">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="af94b-211">Размеры запросов также ограничиваются параметром `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="af94b-211">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="af94b-212">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="af94b-212">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="af94b-213">Значение по умолчанию — 128 КБ (131 072).</span><span class="sxs-lookup"><span data-stu-id="af94b-213">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="af94b-214">Размер окна начального потока</span><span class="sxs-lookup"><span data-stu-id="af94b-214">Initial stream window size</span></span>

<span data-ttu-id="af94b-215">`Http2.InitialStreamWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для каждого запроса (потока).</span><span class="sxs-lookup"><span data-stu-id="af94b-215">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="af94b-216">Размеры запросов также ограничиваются параметром `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="af94b-216">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="af94b-217">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="af94b-217">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="af94b-218">Значение по умолчанию — 96 КБ (98 304).</span><span class="sxs-lookup"><span data-stu-id="af94b-218">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="af94b-219">Синхронные операции ввода-вывода</span><span class="sxs-lookup"><span data-stu-id="af94b-219">Synchronous IO</span></span>

<span data-ttu-id="af94b-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> определяет, разрешены ли синхронные операции ввода-вывода для запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="af94b-220"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="af94b-221">Значение по умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="af94b-221">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="af94b-222">Выполнение большого числа заблокированных операций синхронного ввода-вывода может привести к перегрузке пула потоков и зависанию приложения.</span><span class="sxs-lookup"><span data-stu-id="af94b-222">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="af94b-223">Включайте `AllowSynchronousIO`, только если вы используете библиотеку, которая не поддерживает асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="af94b-223">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="af94b-224">В примере ниже включаются синхронные операции ввода-вывода:</span><span class="sxs-lookup"><span data-stu-id="af94b-224">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="af94b-225">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="af94b-225">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="af94b-226">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="af94b-226">Endpoint configuration</span></span>

<span data-ttu-id="af94b-227">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="af94b-227">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="af94b-228">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="af94b-228">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="af94b-229">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="af94b-229">Specify URLs using the:</span></span>

* <span data-ttu-id="af94b-230">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="af94b-230">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="af94b-231">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-231">`--urls` command-line argument.</span></span>
* <span data-ttu-id="af94b-232">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-232">`urls` host configuration key.</span></span>
* <span data-ttu-id="af94b-233">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-233">`UseUrls` extension method.</span></span>

<span data-ttu-id="af94b-234">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-234">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="af94b-235">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="af94b-235">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="af94b-236">Дополнительные сведения о таких подходах см. в разделах [URL-адреса сервера](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="af94b-236">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="af94b-237">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="af94b-237">A development certificate is created:</span></span>

* <span data-ttu-id="af94b-238">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="af94b-238">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="af94b-239">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-239">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="af94b-240">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="af94b-240">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="af94b-241">Шаблоны проектов настраивают приложения так, чтобы они запускались на базе HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="af94b-241">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="af94b-242">Вызовите методы <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> или <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> из <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, чтобы настроить префиксы URL-адресов и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-242">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="af94b-243">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-243">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="af94b-244">Конфигурация `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="af94b-244">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="af94b-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="af94b-245">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="af94b-246">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-246">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="af94b-247">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-247">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="af94b-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="af94b-248">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="af94b-249">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-249">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="af94b-250">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-250">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    // certificate is an X509Certificate2
                    listenOptions.ServerCertificate = certificate;
                });
            })
            .UseStartup<Startup>();
        });
}
```

### <a name="configureiconfiguration"></a><span data-ttu-id="af94b-251">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="af94b-251">Configure(IConfiguration)</span></span>

<span data-ttu-id="af94b-252">Создает загрузчик конфигурации для настройки Kestrel, который принимает <xref:Microsoft.Extensions.Configuration.IConfiguration> в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="af94b-252">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="af94b-253">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-253">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="af94b-254">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="af94b-254">ListenOptions.UseHttps</span></span>

<span data-ttu-id="af94b-255">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-255">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="af94b-256">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="af94b-256">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="af94b-257">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-257">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="af94b-258">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="af94b-258">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="af94b-259">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="af94b-259">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="af94b-260">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="af94b-260">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="af94b-261">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="af94b-261">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="af94b-262">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="af94b-262">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="af94b-263">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="af94b-263">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="af94b-264">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-264">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="af94b-265">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-265">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="af94b-266">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="af94b-266">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="af94b-267">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-267">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="af94b-268">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="af94b-268">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="af94b-269">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-269">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="af94b-270">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-270">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="af94b-271">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="af94b-271">Supported configurations described next:</span></span>

* <span data-ttu-id="af94b-272">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="af94b-272">No configuration</span></span>
* <span data-ttu-id="af94b-273">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="af94b-273">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="af94b-274">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="af94b-274">Change the defaults in code</span></span>

<span data-ttu-id="af94b-275">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-275">*No configuration*</span></span>

<span data-ttu-id="af94b-276">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-276">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="af94b-277">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-277">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="af94b-278">`CreateDefaultBuilder` по умолчанию вызывает `Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-278">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="af94b-279">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-279">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="af94b-280">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="af94b-280">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="af94b-281">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="af94b-281">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="af94b-282">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="af94b-282">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="af94b-283">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию** , или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="af94b-283">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="af94b-284">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="af94b-284">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="af94b-285">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="af94b-285">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="af94b-286">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="af94b-286">Schema notes:</span></span>

* <span data-ttu-id="af94b-287">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="af94b-287">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="af94b-288">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="af94b-288">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="af94b-289">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-289">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="af94b-290">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="af94b-290">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="af94b-291">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="af94b-291">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="af94b-292">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-292">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="af94b-293">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="af94b-293">The `Certificate` section is optional.</span></span> <span data-ttu-id="af94b-294">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="af94b-294">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="af94b-295">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="af94b-295">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="af94b-296">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="af94b-296">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="af94b-297">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="af94b-297">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="af94b-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, listenOptions => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="af94b-298">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseKestrel((context, serverOptions) =>
            {
                serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                    .Endpoint("HTTPS", listenOptions =>
                    {
                        listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                    });
            });
        });
```

<span data-ttu-id="af94b-299">Можно обратиться напрямую к `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="af94b-299">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="af94b-300">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="af94b-300">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="af94b-301">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("{SECTION}"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="af94b-301">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="af94b-302">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="af94b-302">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="af94b-303">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-303">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="af94b-304">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="af94b-304">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="af94b-305">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-305">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="af94b-306">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="af94b-306">*Change the defaults in code*</span></span>

<span data-ttu-id="af94b-307">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="af94b-307">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="af94b-308">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-308">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ConfigureEndpointDefaults(listenOptions =>
                {
                    // Configure endpoint defaults
                });

                serverOptions.ConfigureHttpsDefaults(listenOptions =>
                {
                    listenOptions.SslProtocols = SslProtocols.Tls12;
                });
            });
        });
```

<span data-ttu-id="af94b-309">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="af94b-309">*Kestrel support for SNI*</span></span>

<span data-ttu-id="af94b-310">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="af94b-310">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="af94b-311">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-311">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="af94b-312">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="af94b-312">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="af94b-313">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="af94b-313">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="af94b-314">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-314">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="af94b-315">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="af94b-315">SNI support requires:</span></span>

* <span data-ttu-id="af94b-316">Запуск на целевой платформе `netcoreapp2.1` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-316">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="af94b-317">В `net461` или более поздней версии обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="af94b-317">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="af94b-318">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="af94b-318">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="af94b-319">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-319">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="af94b-320">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-320">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(serverOptions =>
            {
                serverOptions.ListenAnyIP(5005, listenOptions =>
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
            .UseStartup<Startup>();
        });
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="af94b-321">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="af94b-321">Bind to a TCP socket</span></span>

<span data-ttu-id="af94b-322">Метод <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> выполняет привязку к TCP-сокету, а лямбда-выражение параметров позволяет настроить конфигурацию сертификата X.509:</span><span class="sxs-lookup"><span data-stu-id="af94b-322">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="af94b-323">В этом примере настраивается HTTPS для конечной точки с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="af94b-323">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="af94b-324">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="af94b-324">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="af94b-325">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="af94b-325">Bind to a Unix socket</span></span>

<span data-ttu-id="af94b-326">Вы можете прослушивать сокет UNIX с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, чтобы улучшить производительность Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="af94b-326">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="af94b-327">Порт 0</span><span class="sxs-lookup"><span data-stu-id="af94b-327">Port 0</span></span>

<span data-ttu-id="af94b-328">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="af94b-328">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="af94b-329">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="af94b-329">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="af94b-330">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="af94b-330">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="af94b-331">Ограничения</span><span class="sxs-lookup"><span data-stu-id="af94b-331">Limitations</span></span>

<span data-ttu-id="af94b-332">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="af94b-332">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="af94b-333">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-333">`--urls` command-line argument</span></span>
* <span data-ttu-id="af94b-334">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-334">`urls` host configuration key</span></span>
* <span data-ttu-id="af94b-335">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="af94b-335">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="af94b-336">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-336">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="af94b-337">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="af94b-337">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="af94b-338">С этими подходами нельзя использовать HTTPS, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="af94b-338">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="af94b-339">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-339">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="af94b-340">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="af94b-340">IIS endpoint configuration</span></span>

<span data-ttu-id="af94b-341">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-341">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="af94b-342">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="af94b-342">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="af94b-343">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="af94b-343">ListenOptions.Protocols</span></span>

<span data-ttu-id="af94b-344">Свойство `Protocols` устанавливает протоколы HTTP (`HttpProtocols`), разрешенные для конечной точки подключения или для сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-344">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="af94b-345">Значение свойства `Protocols` должно входить в перечисление `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="af94b-345">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="af94b-346">Значение перечисления `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="af94b-346">`HttpProtocols` enum value</span></span> | <span data-ttu-id="af94b-347">Допустимый протокол подключения</span><span class="sxs-lookup"><span data-stu-id="af94b-347">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="af94b-348">Только HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-348">HTTP/1.1 only.</span></span> <span data-ttu-id="af94b-349">Можно использовать с протоколом TLS или без него.</span><span class="sxs-lookup"><span data-stu-id="af94b-349">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="af94b-350">Только HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-350">HTTP/2 only.</span></span> <span data-ttu-id="af94b-351">Может использоваться без TLS только в том случае, если клиент поддерживает [режим предварительного знания](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="af94b-351">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="af94b-352">HTTP/1.1 и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-352">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="af94b-353">Для использования HTTP/2 требуется, чтобы клиент выбрал HTTP/2 при подтверждении [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3); в противном случае используется подключение по умолчанию HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-353">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="af94b-354">Значение `ListenOptions.Protocols` по умолчанию для любой конечной точки равно `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="af94b-354">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="af94b-355">Ограничения TLS для HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="af94b-355">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="af94b-356">TSL 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-356">TLS version 1.2 or later</span></span>
* <span data-ttu-id="af94b-357">Повторное согласование отключено.</span><span class="sxs-lookup"><span data-stu-id="af94b-357">Renegotiation disabled</span></span>
* <span data-ttu-id="af94b-358">Сжатие отключено.</span><span class="sxs-lookup"><span data-stu-id="af94b-358">Compression disabled</span></span>
* <span data-ttu-id="af94b-359">Минимальные размеры обмена временными ключами:</span><span class="sxs-lookup"><span data-stu-id="af94b-359">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="af94b-360">эллиптическая кривая Диффи-Хелмана (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; не менее 224 бит;</span><span class="sxs-lookup"><span data-stu-id="af94b-360">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="af94b-361">конечное поле Диффи-Хелмана (DHE) &lbrack;`TLS12`&rbrack; &ndash; не менее 2048 бит;</span><span class="sxs-lookup"><span data-stu-id="af94b-361">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="af94b-362">Набор шифров не внесен в список блокировок.</span><span class="sxs-lookup"><span data-stu-id="af94b-362">Cipher suite not blacklisted</span></span>

<span data-ttu-id="af94b-363">По умолчанию поддерживается `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; с эллиптической кривой P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="af94b-363">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="af94b-364">Следующий пример разрешает подключения HTTP/1.1 и HTTP/2 через порт 8000.</span><span class="sxs-lookup"><span data-stu-id="af94b-364">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="af94b-365">Эти подключения шифруются по протоколу TLS с использованием предоставленного сертификата:</span><span class="sxs-lookup"><span data-stu-id="af94b-365">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="af94b-366">Также вы можете использовать ПО промежуточного слоя подключения для фильтрации подтверждений TLS для каждого соединения по конкретным шифрам:</span><span class="sxs-lookup"><span data-stu-id="af94b-366">Optionally use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseConnectionHandler<TlsFilterConnectionHandler>();
    });
});
```

```csharp
using System;
using System.Buffers;
using System.Security.Authentication;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Connections;
using Microsoft.AspNetCore.Connections.Features;

public class TlsFilterConnectionHandler : ConnectionHandler
{
    public override async Task OnConnectedAsync(ConnectionContext connection)
    {
        var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

        while (true)
        {
            var result = await connection.Transport.Input.ReadAsync();
            var buffer = result.Buffer;

            if (!buffer.IsEmpty)
            {
                await connection.Transport.Output.WriteAsync(buffer.ToArray());
            }
            else if (result.IsCompleted)
            {
                break;
            }

            connection.Transport.Input.AdvanceTo(buffer.End);
        }
    }
}
```

<span data-ttu-id="af94b-367">*Выбор протокола из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-367">*Set the protocol from configuration*</span></span>

<span data-ttu-id="af94b-368">`CreateDefaultBuilder` по умолчанию вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-368">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="af94b-369">В следующем примере *appsettings.json* для всех конечных точек устанавливается протокол подключения по умолчанию HTTP/1.1:</span><span class="sxs-lookup"><span data-stu-id="af94b-369">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="af94b-370">В следующем примере *appsettings.json* для отдельной конечной точки устанавливается протокол подключения HTTP/1.1:</span><span class="sxs-lookup"><span data-stu-id="af94b-370">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1"
      }
    }
  }
}
```

<span data-ttu-id="af94b-371">Указанные в коде протоколы переопределяют значения, заданные в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-371">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="af94b-372">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="af94b-372">Transport configuration</span></span>

<span data-ttu-id="af94b-373">Для проектов, где требуется использовать Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="af94b-373">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="af94b-374">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="af94b-374">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="af94b-375">Вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> для `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="af94b-375">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

   ```csharp
   public class Program
   {
       public static void Main(string[] args)
       {
           CreateHostBuilder(args).Build().Run();
       }

       public static IHostBuilder CreateHostBuilder(string[] args) =>
           Host.CreateDefaultBuilder(args)
               .ConfigureWebHostDefaults(webBuilder =>
               {
                   webBuilder.UseLibuv();
                   webBuilder.UseStartup<Startup>();
               });
   }
   ```

### <a name="url-prefixes"></a><span data-ttu-id="af94b-376">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="af94b-376">URL prefixes</span></span>

<span data-ttu-id="af94b-377">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="af94b-377">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="af94b-378">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="af94b-378">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="af94b-379">Kestrel не поддерживает HTTP при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-379">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="af94b-380">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-380">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="af94b-381">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="af94b-381">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="af94b-382">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-382">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="af94b-383">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="af94b-383">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="af94b-384">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-384">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="af94b-385">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="af94b-385">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="af94b-386">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="af94b-386">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="af94b-387">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="af94b-387">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="af94b-388">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="af94b-388">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="af94b-389">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-389">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="af94b-390">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="af94b-390">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="af94b-391">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="af94b-391">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="af94b-392">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="af94b-392">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="af94b-393">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="af94b-393">Host filtering</span></span>

<span data-ttu-id="af94b-394">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="af94b-394">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="af94b-395">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="af94b-395">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="af94b-396">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="af94b-396">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="af94b-397">Заголовки `Host` не проверяются.</span><span class="sxs-lookup"><span data-stu-id="af94b-397">`Host` headers aren't validated.</span></span>

<span data-ttu-id="af94b-398">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="af94b-398">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="af94b-399">ПО промежуточного слоя фильтрации узлов предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который неявно предоставляется для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-399">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="af94b-400">ПО промежуточного слоя добавляется в <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который вызывает <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="af94b-400">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="af94b-401">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-401">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="af94b-402">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="af94b-402">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="af94b-403">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="af94b-403">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="af94b-404">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="af94b-404">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="af94b-405">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) имеет также параметр <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="af94b-405">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="af94b-406">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="af94b-406">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="af94b-407">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок `Host` не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="af94b-407">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="af94b-408">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок `Host` пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="af94b-408">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="af94b-409">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в статье <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="af94b-409">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="af94b-410">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="af94b-410">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="af94b-411">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-411">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="af94b-412">Kestrel поддерживается в следующих сценариях.</span><span class="sxs-lookup"><span data-stu-id="af94b-412">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="af94b-413">HTTPS</span><span class="sxs-lookup"><span data-stu-id="af94b-413">HTTPS</span></span>
* <span data-ttu-id="af94b-414">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="af94b-414">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="af94b-415">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="af94b-415">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="af94b-416">HTTP/2 (за исключением macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="af94b-416">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="af94b-417">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="af94b-417">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="af94b-418">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-418">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="af94b-419">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af94b-419">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="af94b-420">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="af94b-420">HTTP/2 support</span></span>

<span data-ttu-id="af94b-421">Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) доступен для приложений ASP.NET Core, если выполнены следующие базовые требования:</span><span class="sxs-lookup"><span data-stu-id="af94b-421">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="af94b-422">Операционная система&dagger;:</span><span class="sxs-lookup"><span data-stu-id="af94b-422">Operating system&dagger;</span></span>
  * <span data-ttu-id="af94b-423">Windows Server 2016 / Windows 10 или более поздних версий&Dagger;</span><span class="sxs-lookup"><span data-stu-id="af94b-423">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="af94b-424">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="af94b-424">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="af94b-425">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="af94b-425">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="af94b-426">Подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3).</span><span class="sxs-lookup"><span data-stu-id="af94b-426">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="af94b-427">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-427">TLS 1.2 or later connection</span></span>

<span data-ttu-id="af94b-428">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="af94b-428">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="af94b-429">&Dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-429">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="af94b-430">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="af94b-430">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="af94b-431">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="af94b-431">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="af94b-432">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="af94b-432">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="af94b-433">По умолчанию протокол HTTP/2 отключен.</span><span class="sxs-lookup"><span data-stu-id="af94b-433">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="af94b-434">Дополнительные сведения о конфигурации см. в разделах [о параметрах Kestrel](#kestrel-options) и [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="af94b-434">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="af94b-435">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="af94b-435">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="af94b-436">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как [IIS](https://www.iis.net/), [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="af94b-436">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="af94b-437">Обратный прокси-сервер получает HTTP-запросы из сети и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-437">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="af94b-438">Kestrel используется в качестве веб-сервера перехода (с выходом в Интернет).</span><span class="sxs-lookup"><span data-stu-id="af94b-438">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="af94b-440">Kestrel используется в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-440">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="af94b-442">Любая из этих конфигураций — с обратным прокси-сервером и без него — является поддерживаемой конфигурацией для размещения.</span><span class="sxs-lookup"><span data-stu-id="af94b-442">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="af94b-443">Kestrel, используемый в качестве пограничного сервера без обратного прокси-сервера, не поддерживает общий доступ нескольких процессов к одним и тем же IP-адресам и портам.</span><span class="sxs-lookup"><span data-stu-id="af94b-443">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="af94b-444">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка `Host` запросов.</span><span class="sxs-lookup"><span data-stu-id="af94b-444">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="af94b-445">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="af94b-445">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="af94b-446">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным.</span><span class="sxs-lookup"><span data-stu-id="af94b-446">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="af94b-447">Обратный прокси-сервер:</span><span class="sxs-lookup"><span data-stu-id="af94b-447">A reverse proxy:</span></span>

* <span data-ttu-id="af94b-448">Может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="af94b-448">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="af94b-449">Предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="af94b-449">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="af94b-450">Может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="af94b-450">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="af94b-451">Упрощает настройку балансировки нагрузки и безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="af94b-451">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="af94b-452">Сертификат X.509 требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложения во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="af94b-452">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="af94b-453">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="af94b-453">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="af94b-454">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af94b-454">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="af94b-455">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="af94b-455">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="af94b-456">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-456">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="af94b-457">В файле *Program.cs* код шаблона вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который в фоновом режиме вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>.</span><span class="sxs-lookup"><span data-stu-id="af94b-457">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="af94b-458">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder`, используйте `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="af94b-458">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="af94b-459">Если приложение не вызывает `CreateDefaultBuilder`, чтобы настроить узел, вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **перед** вызовом `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="af94b-459">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

```csharp
public static void Main(string[] args)
{
    var host = new WebHostBuilder()
        .UseContentRoot(Directory.GetCurrentDirectory())
        .UseKestrel()
        .UseIISIntegration()
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        })
        .Build();

    host.Run();
}
```

## <a name="kestrel-options"></a><span data-ttu-id="af94b-460">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="af94b-460">Kestrel options</span></span>

<span data-ttu-id="af94b-461">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="af94b-461">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="af94b-462">Задать ограничения для свойства <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> в классе <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="af94b-462">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="af94b-463">Свойство `Limits` содержит экземпляр класса <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="af94b-463">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="af94b-464">В следующих примерах используется пространство имен <xref:Microsoft.AspNetCore.Server.Kestrel.Core>.</span><span class="sxs-lookup"><span data-stu-id="af94b-464">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="af94b-465">Время ожидания проверки на активность</span><span class="sxs-lookup"><span data-stu-id="af94b-465">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="af94b-466">Получает или задает [время ожидания проверки на активность](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="af94b-466">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="af94b-467">Значение по умолчанию — 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="af94b-467">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="af94b-468">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="af94b-468">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="af94b-469">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="af94b-469">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="af94b-470">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="af94b-470">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="af94b-471">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="af94b-471">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="af94b-472">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="af94b-472">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="af94b-473">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="af94b-473">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="af94b-474">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="af94b-474">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="af94b-475">Чтобы переопределить это ограничение в приложении MVC ASP.NET Core, мы рекомендуем использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="af94b-475">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="af94b-476">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="af94b-476">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="af94b-477">Переопределите параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="af94b-477">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="af94b-478">Если приложение пытается настроить ограничение для запроса после того, как оно начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="af94b-478">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="af94b-479">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="af94b-479">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="af94b-480">При запуске приложения [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), который обслуживает [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module), ограничение на размер текста запроса Kestrel будет отключено, так как оно уже задается IIS.</span><span class="sxs-lookup"><span data-stu-id="af94b-480">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="af94b-481">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="af94b-481">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="af94b-482">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="af94b-482">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="af94b-483">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="af94b-483">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="af94b-484">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="af94b-484">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="af94b-485">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="af94b-485">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="af94b-486">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="af94b-486">A minimum rate also applies to the response.</span></span> <span data-ttu-id="af94b-487">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="af94b-487">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="af94b-488">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="af94b-488">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="af94b-489">Переопределите ограничения минимальной скорости для каждого запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="af94b-489">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="af94b-490">В `HttpContext.Features` для запросов HTTP/2 отсутствуют возможности настройки скорости, указанные в предыдущем примере, так как изменение ограничений скорости для каждого запроса не поддерживается для HTTP/2 из-за поддержки протокола мультиплексирования запроса.</span><span class="sxs-lookup"><span data-stu-id="af94b-490">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="af94b-491">Ограничения скорости на уровне сервера, которые настроены с помощью `KestrelServerOptions.Limits`, по-прежнему применяются к подключениям HTTP/1.x и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-491">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="af94b-492">Время ожидания для заголовков запросов</span><span class="sxs-lookup"><span data-stu-id="af94b-492">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="af94b-493">Получает или задает максимальное время, которое сервер уделяет получению заголовков запросов.</span><span class="sxs-lookup"><span data-stu-id="af94b-493">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="af94b-494">Значение по умолчанию — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="af94b-494">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="af94b-495">Максимальное число потоков на подключение</span><span class="sxs-lookup"><span data-stu-id="af94b-495">Maximum streams per connection</span></span>

<span data-ttu-id="af94b-496">`Http2.MaxStreamsPerConnection` ограничивает количество параллельных потоков запросов для одного соединения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-496">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="af94b-497">Потоки сверх этого числа будут отклонены.</span><span class="sxs-lookup"><span data-stu-id="af94b-497">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="af94b-498">Значение по умолчанию — 100.</span><span class="sxs-lookup"><span data-stu-id="af94b-498">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="af94b-499">Размер таблицы заголовка</span><span class="sxs-lookup"><span data-stu-id="af94b-499">Header table size</span></span>

<span data-ttu-id="af94b-500">Декодер HPACK распаковывает заголовки HTTP для подключений HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-500">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="af94b-501">`Http2.HeaderTableSize` ограничивает размер таблицы сжатия заголовка, которую использует декодер HPACK.</span><span class="sxs-lookup"><span data-stu-id="af94b-501">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="af94b-502">Это значение указывается в октетах и должно быть больше нуля (0).</span><span class="sxs-lookup"><span data-stu-id="af94b-502">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="af94b-503">Значение по умолчанию — 4096.</span><span class="sxs-lookup"><span data-stu-id="af94b-503">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="af94b-504">Максимальный размер кадра</span><span class="sxs-lookup"><span data-stu-id="af94b-504">Maximum frame size</span></span>

<span data-ttu-id="af94b-505">`Http2.MaxFrameSize` указывает максимальный размер полезных данных в получаемом кадре подключения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-505">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="af94b-506">Это значение указывается в октетах и должно находиться в пределах от 2^14 (16 384) до 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="af94b-506">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="af94b-507">Значение по умолчанию — 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="af94b-507">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="af94b-508">Максимальный размер запроса заголовка</span><span class="sxs-lookup"><span data-stu-id="af94b-508">Maximum request header size</span></span>

<span data-ttu-id="af94b-509">`Http2.MaxRequestHeaderFieldSize` указывает максимально допустимый размер значений заголовка запроса (в октетах).</span><span class="sxs-lookup"><span data-stu-id="af94b-509">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="af94b-510">Это ограничение применяется к имени и значению в их сжатых и несжатых представлениях.</span><span class="sxs-lookup"><span data-stu-id="af94b-510">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="af94b-511">Значение должно быть больше нуля.</span><span class="sxs-lookup"><span data-stu-id="af94b-511">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="af94b-512">Значение по умолчанию — 8 192.</span><span class="sxs-lookup"><span data-stu-id="af94b-512">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="af94b-513">Размер окна начального подключения</span><span class="sxs-lookup"><span data-stu-id="af94b-513">Initial connection window size</span></span>

<span data-ttu-id="af94b-514">`Http2.InitialConnectionWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для всех запросов (потоков) на каждое соединение.</span><span class="sxs-lookup"><span data-stu-id="af94b-514">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="af94b-515">Размеры запросов также ограничиваются параметром `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="af94b-515">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="af94b-516">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="af94b-516">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="af94b-517">Значение по умолчанию — 128 КБ (131 072).</span><span class="sxs-lookup"><span data-stu-id="af94b-517">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="af94b-518">Размер окна начального потока</span><span class="sxs-lookup"><span data-stu-id="af94b-518">Initial stream window size</span></span>

<span data-ttu-id="af94b-519">`Http2.InitialStreamWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для каждого запроса (потока).</span><span class="sxs-lookup"><span data-stu-id="af94b-519">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="af94b-520">Размеры запросов также ограничиваются параметром `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="af94b-520">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="af94b-521">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="af94b-521">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="af94b-522">Значение по умолчанию — 96 КБ (98 304).</span><span class="sxs-lookup"><span data-stu-id="af94b-522">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="af94b-523">Синхронные операции ввода-вывода</span><span class="sxs-lookup"><span data-stu-id="af94b-523">Synchronous IO</span></span>

<span data-ttu-id="af94b-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> определяет, разрешены ли синхронные операции ввода-вывода для запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="af94b-524"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="af94b-525">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="af94b-525">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="af94b-526">Выполнение большого числа заблокированных операций синхронного ввода-вывода может привести к перегрузке пула потоков и зависанию приложения.</span><span class="sxs-lookup"><span data-stu-id="af94b-526">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="af94b-527">Включайте `AllowSynchronousIO`, только если вы используете библиотеку, которая не поддерживает асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="af94b-527">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="af94b-528">В примере ниже включаются синхронные операции ввода-вывода:</span><span class="sxs-lookup"><span data-stu-id="af94b-528">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="af94b-529">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="af94b-529">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="af94b-530">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="af94b-530">Endpoint configuration</span></span>

<span data-ttu-id="af94b-531">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="af94b-531">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="af94b-532">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="af94b-532">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="af94b-533">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="af94b-533">Specify URLs using the:</span></span>

* <span data-ttu-id="af94b-534">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="af94b-534">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="af94b-535">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-535">`--urls` command-line argument.</span></span>
* <span data-ttu-id="af94b-536">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-536">`urls` host configuration key.</span></span>
* <span data-ttu-id="af94b-537">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-537">`UseUrls` extension method.</span></span>

<span data-ttu-id="af94b-538">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-538">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="af94b-539">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="af94b-539">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="af94b-540">Дополнительные сведения о таких подходах см. в разделах [URL-адреса сервера](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="af94b-540">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="af94b-541">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="af94b-541">A development certificate is created:</span></span>

* <span data-ttu-id="af94b-542">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="af94b-542">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="af94b-543">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-543">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="af94b-544">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="af94b-544">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="af94b-545">Шаблоны проектов настраивают приложения так, чтобы они запускались на базе HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="af94b-545">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="af94b-546">Вызовите методы <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> или <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> из <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, чтобы настроить префиксы URL-адресов и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-546">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="af94b-547">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-547">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="af94b-548">Конфигурация `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="af94b-548">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="af94b-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="af94b-549">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="af94b-550">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-550">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="af94b-551">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-551">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="af94b-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="af94b-552">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="af94b-553">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-553">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="af94b-554">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-554">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="af94b-555">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="af94b-555">Configure(IConfiguration)</span></span>

<span data-ttu-id="af94b-556">Создает загрузчик конфигурации для настройки Kestrel, который принимает <xref:Microsoft.Extensions.Configuration.IConfiguration> в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="af94b-556">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="af94b-557">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-557">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="af94b-558">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="af94b-558">ListenOptions.UseHttps</span></span>

<span data-ttu-id="af94b-559">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-559">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="af94b-560">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="af94b-560">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="af94b-561">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-561">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="af94b-562">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="af94b-562">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="af94b-563">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="af94b-563">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="af94b-564">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="af94b-564">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="af94b-565">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="af94b-565">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="af94b-566">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="af94b-566">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="af94b-567">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="af94b-567">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="af94b-568">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-568">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="af94b-569">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-569">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="af94b-570">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="af94b-570">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="af94b-571">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-571">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="af94b-572">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="af94b-572">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="af94b-573">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-573">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="af94b-574">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-574">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="af94b-575">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="af94b-575">Supported configurations described next:</span></span>

* <span data-ttu-id="af94b-576">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="af94b-576">No configuration</span></span>
* <span data-ttu-id="af94b-577">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="af94b-577">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="af94b-578">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="af94b-578">Change the defaults in code</span></span>

<span data-ttu-id="af94b-579">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-579">*No configuration*</span></span>

<span data-ttu-id="af94b-580">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-580">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="af94b-581">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-581">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="af94b-582">`CreateDefaultBuilder` по умолчанию вызывает `Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-582">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="af94b-583">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-583">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="af94b-584">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="af94b-584">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="af94b-585">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="af94b-585">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="af94b-586">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="af94b-586">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="af94b-587">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию** , или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="af94b-587">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="af94b-588">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="af94b-588">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="af94b-589">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="af94b-589">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="af94b-590">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="af94b-590">Schema notes:</span></span>

* <span data-ttu-id="af94b-591">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="af94b-591">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="af94b-592">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="af94b-592">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="af94b-593">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-593">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="af94b-594">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="af94b-594">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="af94b-595">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="af94b-595">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="af94b-596">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-596">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="af94b-597">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="af94b-597">The `Certificate` section is optional.</span></span> <span data-ttu-id="af94b-598">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="af94b-598">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="af94b-599">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="af94b-599">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="af94b-600">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="af94b-600">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="af94b-601">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="af94b-601">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="af94b-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, listenOptions => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="af94b-602">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="af94b-603">Можно обратиться напрямую к `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="af94b-603">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="af94b-604">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="af94b-604">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="af94b-605">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("{SECTION}"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="af94b-605">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="af94b-606">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="af94b-606">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="af94b-607">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-607">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="af94b-608">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="af94b-608">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="af94b-609">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-609">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="af94b-610">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="af94b-610">*Change the defaults in code*</span></span>

<span data-ttu-id="af94b-611">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="af94b-611">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="af94b-612">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-612">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="af94b-613">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="af94b-613">*Kestrel support for SNI*</span></span>

<span data-ttu-id="af94b-614">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="af94b-614">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="af94b-615">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-615">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="af94b-616">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="af94b-616">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="af94b-617">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="af94b-617">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="af94b-618">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-618">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="af94b-619">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="af94b-619">SNI support requires:</span></span>

* <span data-ttu-id="af94b-620">Запуск на целевой платформе `netcoreapp2.1` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-620">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="af94b-621">В `net461` или более поздней версии обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="af94b-621">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="af94b-622">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="af94b-622">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="af94b-623">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-623">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="af94b-624">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-624">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="af94b-625">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="af94b-625">Bind to a TCP socket</span></span>

<span data-ttu-id="af94b-626">Метод <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> выполняет привязку к TCP-сокету, а лямбда-выражение параметров позволяет настроить конфигурацию сертификата X.509:</span><span class="sxs-lookup"><span data-stu-id="af94b-626">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="af94b-627">В этом примере настраивается HTTPS для конечной точки с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="af94b-627">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="af94b-628">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="af94b-628">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="af94b-629">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="af94b-629">Bind to a Unix socket</span></span>

<span data-ttu-id="af94b-630">Вы можете прослушивать сокет UNIX с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, чтобы улучшить производительность Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="af94b-630">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="af94b-631">Порт 0</span><span class="sxs-lookup"><span data-stu-id="af94b-631">Port 0</span></span>

<span data-ttu-id="af94b-632">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="af94b-632">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="af94b-633">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="af94b-633">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="af94b-634">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="af94b-634">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="af94b-635">Ограничения</span><span class="sxs-lookup"><span data-stu-id="af94b-635">Limitations</span></span>

<span data-ttu-id="af94b-636">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="af94b-636">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="af94b-637">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-637">`--urls` command-line argument</span></span>
* <span data-ttu-id="af94b-638">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-638">`urls` host configuration key</span></span>
* <span data-ttu-id="af94b-639">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="af94b-639">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="af94b-640">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-640">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="af94b-641">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="af94b-641">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="af94b-642">С этими подходами нельзя использовать HTTPS, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="af94b-642">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="af94b-643">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-643">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="af94b-644">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="af94b-644">IIS endpoint configuration</span></span>

<span data-ttu-id="af94b-645">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-645">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="af94b-646">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="af94b-646">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="af94b-647">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="af94b-647">ListenOptions.Protocols</span></span>

<span data-ttu-id="af94b-648">Свойство `Protocols` устанавливает протоколы HTTP (`HttpProtocols`), разрешенные для конечной точки подключения или для сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-648">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="af94b-649">Значение свойства `Protocols` должно входить в перечисление `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="af94b-649">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="af94b-650">Значение перечисления `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="af94b-650">`HttpProtocols` enum value</span></span> | <span data-ttu-id="af94b-651">Допустимый протокол подключения</span><span class="sxs-lookup"><span data-stu-id="af94b-651">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="af94b-652">Только HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-652">HTTP/1.1 only.</span></span> <span data-ttu-id="af94b-653">Можно использовать с протоколом TLS или без него.</span><span class="sxs-lookup"><span data-stu-id="af94b-653">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="af94b-654">Только HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-654">HTTP/2 only.</span></span> <span data-ttu-id="af94b-655">Может использоваться без TLS только в том случае, если клиент поддерживает [режим предварительного знания](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="af94b-655">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="af94b-656">HTTP/1.1 и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="af94b-656">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="af94b-657">Для использования HTTP/2 требуется протокол TLS и подключение с [согласованием протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3); при их отсутствии используется подключение по умолчанию HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-657">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="af94b-658">По умолчанию используется протокол HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="af94b-658">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="af94b-659">Ограничения TLS для HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="af94b-659">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="af94b-660">TSL 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-660">TLS version 1.2 or later</span></span>
* <span data-ttu-id="af94b-661">Повторное согласование отключено.</span><span class="sxs-lookup"><span data-stu-id="af94b-661">Renegotiation disabled</span></span>
* <span data-ttu-id="af94b-662">Сжатие отключено.</span><span class="sxs-lookup"><span data-stu-id="af94b-662">Compression disabled</span></span>
* <span data-ttu-id="af94b-663">Минимальные размеры обмена временными ключами:</span><span class="sxs-lookup"><span data-stu-id="af94b-663">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="af94b-664">эллиптическая кривая Диффи-Хелмана (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; не менее 224 бит;</span><span class="sxs-lookup"><span data-stu-id="af94b-664">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="af94b-665">конечное поле Диффи-Хелмана (DHE) &lbrack;`TLS12`&rbrack; &ndash; не менее 2048 бит;</span><span class="sxs-lookup"><span data-stu-id="af94b-665">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="af94b-666">Набор шифров не внесен в список блокировок.</span><span class="sxs-lookup"><span data-stu-id="af94b-666">Cipher suite not blacklisted</span></span>

<span data-ttu-id="af94b-667">По умолчанию поддерживается `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; с эллиптической кривой P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="af94b-667">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="af94b-668">Следующий пример разрешает подключения HTTP/1.1 и HTTP/2 через порт 8000.</span><span class="sxs-lookup"><span data-stu-id="af94b-668">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="af94b-669">Эти подключения шифруются по протоколу TLS с использованием предоставленного сертификата:</span><span class="sxs-lookup"><span data-stu-id="af94b-669">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="af94b-670">Также вы можете создать реализацию `IConnectionAdapter` для фильтрации подтверждений TLS для каждого соединения по конкретным шифрам:</span><span class="sxs-lookup"><span data-stu-id="af94b-670">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

```csharp
.ConfigureKestrel((context, serverOptions) =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.Protocols = HttpProtocols.Http1AndHttp2;
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.ConnectionAdapters.Add(new TlsFilterAdapter());
    });
});
```

```csharp
private class TlsFilterAdapter : IConnectionAdapter
{
    public bool IsHttps => false;

    public Task<IAdaptedConnection> OnConnectionAsync(ConnectionAdapterContext context)
    {
        var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

        // Throw NotSupportedException for any cipher algorithm that the app doesn't
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

<span data-ttu-id="af94b-671">*Выбор протокола из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-671">*Set the protocol from configuration*</span></span>

<span data-ttu-id="af94b-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> по умолчанию вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-672"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="af94b-673">В следующем примере *appsettings.json* для всех конечных точек Kestrel устанавливается протокол подключения по умолчанию (HTTP/1.1 и HTTP/2):</span><span class="sxs-lookup"><span data-stu-id="af94b-673">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="af94b-674">В следующем примере файла конфигурации задается протокол соединения для конкретной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="af94b-674">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http1AndHttp2"
      }
    }
  }
}
```

<span data-ttu-id="af94b-675">Указанные в коде протоколы переопределяют значения, заданные в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-675">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="af94b-676">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="af94b-676">Transport configuration</span></span>

<span data-ttu-id="af94b-677">После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах.</span><span class="sxs-lookup"><span data-stu-id="af94b-677">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="af94b-678">Это критическое изменение для приложений ASP.NET Core 2.0, которые обновляются до версии 2.1, если они вызывают <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> и зависят от одного из следующих пакетов:</span><span class="sxs-lookup"><span data-stu-id="af94b-678">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="af94b-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (прямая ссылка на пакет)</span><span class="sxs-lookup"><span data-stu-id="af94b-679">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="af94b-680">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="af94b-680">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="af94b-681">Для проектов, где требуется использовать Libuv:</span><span class="sxs-lookup"><span data-stu-id="af94b-681">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="af94b-682">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="af94b-682">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="af94b-683">Вызов <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="af94b-683">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="af94b-684">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="af94b-684">URL prefixes</span></span>

<span data-ttu-id="af94b-685">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="af94b-685">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="af94b-686">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="af94b-686">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="af94b-687">Kestrel не поддерживает HTTP при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-687">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="af94b-688">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-688">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="af94b-689">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="af94b-689">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="af94b-690">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-690">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="af94b-691">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="af94b-691">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="af94b-692">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-692">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="af94b-693">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="af94b-693">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="af94b-694">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="af94b-694">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="af94b-695">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="af94b-695">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="af94b-696">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="af94b-696">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="af94b-697">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-697">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="af94b-698">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="af94b-698">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="af94b-699">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="af94b-699">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="af94b-700">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="af94b-700">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="af94b-701">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="af94b-701">Host filtering</span></span>

<span data-ttu-id="af94b-702">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="af94b-702">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="af94b-703">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="af94b-703">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="af94b-704">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="af94b-704">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="af94b-705">Заголовки `Host` не проверяются.</span><span class="sxs-lookup"><span data-stu-id="af94b-705">`Host` headers aren't validated.</span></span>

<span data-ttu-id="af94b-706">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="af94b-706">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="af94b-707">ПО промежуточного слоя фильтрации узла предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или 2.2).</span><span class="sxs-lookup"><span data-stu-id="af94b-707">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="af94b-708">ПО промежуточного слоя добавляется в <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который вызывает <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="af94b-708">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="af94b-709">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-709">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="af94b-710">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="af94b-710">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="af94b-711">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="af94b-711">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="af94b-712">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="af94b-712">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="af94b-713">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) имеет также параметр <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="af94b-713">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="af94b-714">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="af94b-714">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="af94b-715">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок `Host` не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="af94b-715">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="af94b-716">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок `Host` пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="af94b-716">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="af94b-717">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в статье <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="af94b-717">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="af94b-718">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="af94b-718">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="af94b-719">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-719">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="af94b-720">Kestrel поддерживается в следующих сценариях.</span><span class="sxs-lookup"><span data-stu-id="af94b-720">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="af94b-721">HTTPS</span><span class="sxs-lookup"><span data-stu-id="af94b-721">HTTPS</span></span>
* <span data-ttu-id="af94b-722">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="af94b-722">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="af94b-723">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="af94b-723">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="af94b-724">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="af94b-724">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="af94b-725">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="af94b-725">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="af94b-726">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="af94b-726">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="af94b-727">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как [IIS](https://www.iis.net/), [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="af94b-727">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="af94b-728">Обратный прокси-сервер получает HTTP-запросы из сети и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-728">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="af94b-729">Kestrel используется в качестве веб-сервера перехода (с выходом в Интернет).</span><span class="sxs-lookup"><span data-stu-id="af94b-729">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="af94b-731">Kestrel используется в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-731">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="af94b-733">Любая из этих конфигураций — с обратным прокси-сервером и без него — является поддерживаемой конфигурацией для размещения.</span><span class="sxs-lookup"><span data-stu-id="af94b-733">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="af94b-734">Kestrel, используемый в качестве пограничного сервера без обратного прокси-сервера, не поддерживает общий доступ нескольких процессов к одним и тем же IP-адресам и портам.</span><span class="sxs-lookup"><span data-stu-id="af94b-734">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="af94b-735">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка `Host` запросов.</span><span class="sxs-lookup"><span data-stu-id="af94b-735">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="af94b-736">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="af94b-736">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="af94b-737">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным.</span><span class="sxs-lookup"><span data-stu-id="af94b-737">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="af94b-738">Обратный прокси-сервер:</span><span class="sxs-lookup"><span data-stu-id="af94b-738">A reverse proxy:</span></span>

* <span data-ttu-id="af94b-739">Может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="af94b-739">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="af94b-740">Предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="af94b-740">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="af94b-741">Может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="af94b-741">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="af94b-742">Упрощает настройку балансировки нагрузки и безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="af94b-742">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="af94b-743">Сертификат X.509 требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложения во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="af94b-743">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="af94b-744">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="af94b-744">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="af94b-745">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="af94b-745">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="af94b-746">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="af94b-746">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="af94b-747">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-747">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="af94b-748">В файле *Program.cs* код шаблона вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который в фоновом режиме вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>.</span><span class="sxs-lookup"><span data-stu-id="af94b-748">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="af94b-749">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="af94b-749">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

## <a name="kestrel-options"></a><span data-ttu-id="af94b-750">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="af94b-750">Kestrel options</span></span>

<span data-ttu-id="af94b-751">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="af94b-751">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="af94b-752">Задать ограничения для свойства <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> в классе <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="af94b-752">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="af94b-753">Свойство `Limits` содержит экземпляр класса <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="af94b-753">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="af94b-754">В следующих примерах используется пространство имен <xref:Microsoft.AspNetCore.Server.Kestrel.Core>.</span><span class="sxs-lookup"><span data-stu-id="af94b-754">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

### <a name="keep-alive-timeout"></a><span data-ttu-id="af94b-755">Время ожидания проверки на активность</span><span class="sxs-lookup"><span data-stu-id="af94b-755">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="af94b-756">Получает или задает [время ожидания проверки на активность](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="af94b-756">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="af94b-757">Значение по умолчанию — 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="af94b-757">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="af94b-758">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="af94b-758">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="af94b-759">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="af94b-759">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="af94b-760">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="af94b-760">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="af94b-761">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="af94b-761">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="af94b-762">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="af94b-762">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="af94b-763">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="af94b-763">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="af94b-764">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="af94b-764">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="af94b-765">Чтобы переопределить это ограничение в приложении MVC ASP.NET Core, мы рекомендуем использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="af94b-765">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="af94b-766">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="af94b-766">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="af94b-767">Переопределите параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="af94b-767">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="af94b-768">Если приложение пытается настроить ограничение для запроса после того, как оно начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="af94b-768">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="af94b-769">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="af94b-769">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="af94b-770">При запуске приложения [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), который обслуживает [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module), ограничение на размер текста запроса Kestrel будет отключено, так как оно уже задается IIS.</span><span class="sxs-lookup"><span data-stu-id="af94b-770">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="af94b-771">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="af94b-771">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="af94b-772">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="af94b-772">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="af94b-773">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="af94b-773">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="af94b-774">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="af94b-774">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="af94b-775">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="af94b-775">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="af94b-776">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="af94b-776">A minimum rate also applies to the response.</span></span> <span data-ttu-id="af94b-777">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="af94b-777">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="af94b-778">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="af94b-778">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MinRequestBodyDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
            serverOptions.Limits.MinResponseDataRate =
                new MinDataRate(bytesPerSecond: 100, gracePeriod: TimeSpan.FromSeconds(10));
        });
```

### <a name="request-headers-timeout"></a><span data-ttu-id="af94b-779">Время ожидания для заголовков запросов</span><span class="sxs-lookup"><span data-stu-id="af94b-779">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="af94b-780">Получает или задает максимальное время, которое сервер уделяет получению заголовков запросов.</span><span class="sxs-lookup"><span data-stu-id="af94b-780">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="af94b-781">Значение по умолчанию — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="af94b-781">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="af94b-782">Синхронные операции ввода-вывода</span><span class="sxs-lookup"><span data-stu-id="af94b-782">Synchronous IO</span></span>

<span data-ttu-id="af94b-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> определяет, разрешены ли синхронные операции ввода-вывода для запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="af94b-783"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="af94b-784">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="af94b-784">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="af94b-785">Выполнение большого числа заблокированных операций синхронного ввода-вывода может привести к перегрузке пула потоков и зависанию приложения.</span><span class="sxs-lookup"><span data-stu-id="af94b-785">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="af94b-786">Включайте `AllowSynchronousIO`, только если вы используете библиотеку, которая не поддерживает асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="af94b-786">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="af94b-787">В примере ниже отключаются синхронные операции ввода-вывода:</span><span class="sxs-lookup"><span data-stu-id="af94b-787">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="af94b-788">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="af94b-788">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="af94b-789">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="af94b-789">Endpoint configuration</span></span>

<span data-ttu-id="af94b-790">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="af94b-790">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="af94b-791">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="af94b-791">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="af94b-792">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="af94b-792">Specify URLs using the:</span></span>

* <span data-ttu-id="af94b-793">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="af94b-793">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="af94b-794">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-794">`--urls` command-line argument.</span></span>
* <span data-ttu-id="af94b-795">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-795">`urls` host configuration key.</span></span>
* <span data-ttu-id="af94b-796">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-796">`UseUrls` extension method.</span></span>

<span data-ttu-id="af94b-797">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-797">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="af94b-798">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000; http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="af94b-798">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="af94b-799">Дополнительные сведения о таких подходах см. в разделах [URL-адреса сервера](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="af94b-799">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="af94b-800">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="af94b-800">A development certificate is created:</span></span>

* <span data-ttu-id="af94b-801">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="af94b-801">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="af94b-802">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-802">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="af94b-803">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="af94b-803">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="af94b-804">Шаблоны проектов настраивают приложения так, чтобы они запускались на базе HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="af94b-804">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="af94b-805">Вызовите методы <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> или <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> из <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, чтобы настроить префиксы URL-адресов и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-805">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="af94b-806">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-806">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="af94b-807">Конфигурация `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="af94b-807">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="af94b-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="af94b-808">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="af94b-809">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-809">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="af94b-810">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-810">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
        });
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="af94b-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="af94b-811">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="af94b-812">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-812">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="af94b-813">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="af94b-813">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                // certificate is an X509Certificate2
                listenOptions.ServerCertificate = certificate;
            });
        });
```

### <a name="configureiconfiguration"></a><span data-ttu-id="af94b-814">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="af94b-814">Configure(IConfiguration)</span></span>

<span data-ttu-id="af94b-815">Создает загрузчик конфигурации для настройки Kestrel, который принимает <xref:Microsoft.Extensions.Configuration.IConfiguration> в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="af94b-815">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="af94b-816">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-816">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="af94b-817">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="af94b-817">ListenOptions.UseHttps</span></span>

<span data-ttu-id="af94b-818">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-818">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="af94b-819">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="af94b-819">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="af94b-820">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-820">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="af94b-821">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="af94b-821">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="af94b-822">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="af94b-822">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="af94b-823">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="af94b-823">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="af94b-824">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="af94b-824">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="af94b-825">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="af94b-825">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="af94b-826">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="af94b-826">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="af94b-827">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-827">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="af94b-828">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="af94b-828">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="af94b-829">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="af94b-829">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="af94b-830">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-830">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="af94b-831">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="af94b-831">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="af94b-832">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="af94b-832">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="af94b-833">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-833">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="af94b-834">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="af94b-834">Supported configurations described next:</span></span>

* <span data-ttu-id="af94b-835">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="af94b-835">No configuration</span></span>
* <span data-ttu-id="af94b-836">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="af94b-836">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="af94b-837">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="af94b-837">Change the defaults in code</span></span>

<span data-ttu-id="af94b-838">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-838">*No configuration*</span></span>

<span data-ttu-id="af94b-839">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="af94b-839">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="af94b-840">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="af94b-840">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="af94b-841">`CreateDefaultBuilder` по умолчанию вызывает `Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-841">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="af94b-842">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-842">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="af94b-843">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="af94b-843">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="af94b-844">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="af94b-844">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="af94b-845">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="af94b-845">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="af94b-846">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию** , или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="af94b-846">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
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
          "Store": "<certificate store; required>",
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

<span data-ttu-id="af94b-847">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="af94b-847">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="af94b-848">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="af94b-848">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="af94b-849">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="af94b-849">Schema notes:</span></span>

* <span data-ttu-id="af94b-850">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="af94b-850">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="af94b-851">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="af94b-851">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="af94b-852">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-852">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="af94b-853">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="af94b-853">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="af94b-854">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="af94b-854">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="af94b-855">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-855">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="af94b-856">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="af94b-856">The `Certificate` section is optional.</span></span> <span data-ttu-id="af94b-857">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="af94b-857">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="af94b-858">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="af94b-858">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="af94b-859">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="af94b-859">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="af94b-860">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="af94b-860">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="af94b-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, listenOptions => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="af94b-861">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
                .Endpoint("HTTPS", listenOptions =>
                {
                    listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
                });
        });
```

<span data-ttu-id="af94b-862">Можно обратиться напрямую к `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="af94b-862">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="af94b-863">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="af94b-863">The configuration section for each endpoint is a available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="af94b-864">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("{SECTION}"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="af94b-864">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="af94b-865">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="af94b-865">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="af94b-866">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-866">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="af94b-867">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="af94b-867">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="af94b-868">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="af94b-868">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="af94b-869">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="af94b-869">*Change the defaults in code*</span></span>

<span data-ttu-id="af94b-870">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="af94b-870">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="af94b-871">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="af94b-871">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ConfigureEndpointDefaults(listenOptions =>
            {
                // Configure endpoint defaults
            });
            
            serverOptions.ConfigureHttpsDefaults(listenOptions =>
            {
                listenOptions.SslProtocols = SslProtocols.Tls12;
            });
        });
```

<span data-ttu-id="af94b-872">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="af94b-872">*Kestrel support for SNI*</span></span>

<span data-ttu-id="af94b-873">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="af94b-873">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="af94b-874">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-874">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="af94b-875">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="af94b-875">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="af94b-876">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="af94b-876">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="af94b-877">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="af94b-877">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="af94b-878">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="af94b-878">SNI support requires:</span></span>

* <span data-ttu-id="af94b-879">Запуск на целевой платформе `netcoreapp2.1` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="af94b-879">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="af94b-880">В `net461` или более поздней версии обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="af94b-880">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="af94b-881">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="af94b-881">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="af94b-882">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-882">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="af94b-883">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="af94b-883">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel((context, serverOptions) =>
        {
            serverOptions.ListenAnyIP(5005, listenOptions =>
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

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="af94b-884">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="af94b-884">Bind to a TCP socket</span></span>

<span data-ttu-id="af94b-885">Метод <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> выполняет привязку к TCP-сокету, а лямбда-выражение параметров позволяет настроить конфигурацию сертификата X.509:</span><span class="sxs-lookup"><span data-stu-id="af94b-885">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

```csharp
public static void Main(string[] args)
{
    CreateWebHostBuilder(args).Build().Run();
}

public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Listen(IPAddress.Loopback, 5000);
            serverOptions.Listen(IPAddress.Loopback, 5001, listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testPassword");
            });
        });
```

<span data-ttu-id="af94b-886">В этом примере настраивается HTTPS для конечной точки с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="af94b-886">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="af94b-887">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="af94b-887">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="af94b-888">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="af94b-888">Bind to a Unix socket</span></span>

<span data-ttu-id="af94b-889">Вы можете прослушивать сокет UNIX с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, чтобы улучшить производительность Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="af94b-889">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock");
            serverOptions.ListenUnixSocket("/tmp/kestrel-test.sock", listenOptions =>
            {
                listenOptions.UseHttps("testCert.pfx", "testpassword");
            });
        });
```

### <a name="port-0"></a><span data-ttu-id="af94b-890">Порт 0</span><span class="sxs-lookup"><span data-stu-id="af94b-890">Port 0</span></span>

<span data-ttu-id="af94b-891">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="af94b-891">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="af94b-892">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="af94b-892">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="af94b-893">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="af94b-893">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="af94b-894">Ограничения</span><span class="sxs-lookup"><span data-stu-id="af94b-894">Limitations</span></span>

<span data-ttu-id="af94b-895">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="af94b-895">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="af94b-896">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-896">`--urls` command-line argument</span></span>
* <span data-ttu-id="af94b-897">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-897">`urls` host configuration key</span></span>
* <span data-ttu-id="af94b-898">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="af94b-898">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="af94b-899">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="af94b-899">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="af94b-900">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="af94b-900">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="af94b-901">С этими подходами нельзя использовать HTTPS, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="af94b-901">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="af94b-902">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-902">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="af94b-903">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="af94b-903">IIS endpoint configuration</span></span>

<span data-ttu-id="af94b-904">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-904">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="af94b-905">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="af94b-905">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="af94b-906">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="af94b-906">Transport configuration</span></span>

<span data-ttu-id="af94b-907">После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах.</span><span class="sxs-lookup"><span data-stu-id="af94b-907">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="af94b-908">Это критическое изменение для приложений ASP.NET Core 2.0, которые обновляются до версии 2.1, если они вызывают <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> и зависят от одного из следующих пакетов:</span><span class="sxs-lookup"><span data-stu-id="af94b-908">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="af94b-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (прямая ссылка на пакет)</span><span class="sxs-lookup"><span data-stu-id="af94b-909">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="af94b-910">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="af94b-910">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="af94b-911">Для проектов, где требуется использовать Libuv:</span><span class="sxs-lookup"><span data-stu-id="af94b-911">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="af94b-912">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="af94b-912">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="af94b-913">Вызов <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="af94b-913">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="af94b-914">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="af94b-914">URL prefixes</span></span>

<span data-ttu-id="af94b-915">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="af94b-915">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="af94b-916">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="af94b-916">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="af94b-917">Kestrel не поддерживает HTTP при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="af94b-917">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="af94b-918">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-918">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="af94b-919">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="af94b-919">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="af94b-920">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-920">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="af94b-921">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="af94b-921">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="af94b-922">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-922">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="af94b-923">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="af94b-923">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="af94b-924">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="af94b-924">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="af94b-925">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="af94b-925">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="af94b-926">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="af94b-926">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="af94b-927">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="af94b-927">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="af94b-928">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="af94b-928">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="af94b-929">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="af94b-929">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="af94b-930">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="af94b-930">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="af94b-931">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="af94b-931">Host filtering</span></span>

<span data-ttu-id="af94b-932">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="af94b-932">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="af94b-933">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="af94b-933">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="af94b-934">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="af94b-934">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="af94b-935">Заголовки `Host` не проверяются.</span><span class="sxs-lookup"><span data-stu-id="af94b-935">`Host` headers aren't validated.</span></span>

<span data-ttu-id="af94b-936">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="af94b-936">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="af94b-937">ПО промежуточного слоя фильтрации узла предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или 2.2).</span><span class="sxs-lookup"><span data-stu-id="af94b-937">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="af94b-938">ПО промежуточного слоя добавляется в <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который вызывает <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="af94b-938">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="af94b-939">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="af94b-939">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="af94b-940">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="af94b-940">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="af94b-941">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="af94b-941">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="af94b-942">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="af94b-942">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="af94b-943">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) имеет также параметр <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="af94b-943">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="af94b-944">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="af94b-944">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="af94b-945">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок `Host` не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="af94b-945">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="af94b-946">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок `Host` пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="af94b-946">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="af94b-947">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в статье <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="af94b-947">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="af94b-948">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="af94b-948">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="af94b-949">RFC 7230: синтаксис и маршрутизация сообщений (раздел 5.4: узел)</span><span class="sxs-lookup"><span data-stu-id="af94b-949">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
