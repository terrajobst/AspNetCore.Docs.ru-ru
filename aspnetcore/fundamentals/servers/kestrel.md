---
title: Реализации веб-сервера Kestrel в ASP.NET Core
author: guardrex
description: Общие сведения о Kestrel — кроссплатформенном веб-сервере для ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/29/2019
uid: fundamentals/servers/kestrel
ms.openlocfilehash: beaf6ac49359adfdc2dc24221eab04cc853646a9
ms.sourcegitcommit: de0fc77487a4d342bcc30965ec5c142d10d22c03
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/30/2019
ms.locfileid: "73143443"
---
# <a name="kestrel-web-server-implementation-in-aspnet-core"></a><span data-ttu-id="3f256-103">Реализации веб-сервера Kestrel в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f256-103">Kestrel web server implementation in ASP.NET Core</span></span>

<span data-ttu-id="3f256-104">Авторы: [Том Дикстра](https://github.com/tdykstra) (Tom Dykstra), [Крис Росс](https://github.com/Tratcher) (Chris Ross), [Стивен Хальтер](https://twitter.com/halter73) (Stephen Halter) и [Люк Лэтэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="3f256-104">By [Tom Dykstra](https://github.com/tdykstra), [Chris Ross](https://github.com/Tratcher), [Stephen Halter](https://twitter.com/halter73), and [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3f256-105">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-105">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3f256-106">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-106">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3f256-107">Kestrel поддерживается в следующих сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f256-107">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3f256-108">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f256-108">HTTPS</span></span>
* <span data-ttu-id="3f256-109">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3f256-109">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3f256-110">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="3f256-110">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="3f256-111">HTTP/2 (за исключением macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="3f256-111">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="3f256-112">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="3f256-112">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="3f256-113">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-113">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3f256-114">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f256-114">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f256-115">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f256-115">HTTP/2 support</span></span>

<span data-ttu-id="3f256-116">Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) доступен для приложений ASP.NET Core, если выполнены следующие базовые требования:</span><span class="sxs-lookup"><span data-stu-id="3f256-116">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="3f256-117">Операционная система&dagger;:</span><span class="sxs-lookup"><span data-stu-id="3f256-117">Operating system&dagger;</span></span>
  * <span data-ttu-id="3f256-118">Windows Server 2016 / Windows 10 или более поздних версий&Dagger;</span><span class="sxs-lookup"><span data-stu-id="3f256-118">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="3f256-119">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="3f256-119">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="3f256-120">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="3f256-120">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="3f256-121">Подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3).</span><span class="sxs-lookup"><span data-stu-id="3f256-121">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="3f256-122">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-122">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f256-123">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="3f256-123">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="3f256-124">&Dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-124">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="3f256-125">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="3f256-125">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="3f256-126">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="3f256-126">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="3f256-127">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3f256-127">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="3f256-128">По умолчанию протокол HTTP/2 отключен.</span><span class="sxs-lookup"><span data-stu-id="3f256-128">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="3f256-129">Дополнительные сведения о конфигурации см. в разделах [о параметрах Kestrel](#kestrel-options) и [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="3f256-129">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3f256-130">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="3f256-130">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3f256-131">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как [IIS](https://www.iis.net/), [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3f256-131">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3f256-132">Обратный прокси-сервер получает HTTP-запросы из сети и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-132">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3f256-133">Kestrel используется в качестве веб-сервера перехода (с выходом в Интернет).</span><span class="sxs-lookup"><span data-stu-id="3f256-133">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3f256-135">Kestrel используется в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-135">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3f256-137">Любая из этих конфигураций — с обратным прокси-сервером и без него — является поддерживаемой конфигурацией для размещения.</span><span class="sxs-lookup"><span data-stu-id="3f256-137">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3f256-138">Kestrel, используемый в качестве пограничного сервера без обратного прокси-сервера, не поддерживает общий доступ нескольких процессов к одним и тем же IP-адресам и портам.</span><span class="sxs-lookup"><span data-stu-id="3f256-138">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3f256-139">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка `Host` запросов.</span><span class="sxs-lookup"><span data-stu-id="3f256-139">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3f256-140">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="3f256-140">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3f256-141">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным.</span><span class="sxs-lookup"><span data-stu-id="3f256-141">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3f256-142">Обратный прокси-сервер:</span><span class="sxs-lookup"><span data-stu-id="3f256-142">A reverse proxy:</span></span>

* <span data-ttu-id="3f256-143">Может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="3f256-143">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3f256-144">Предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="3f256-144">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3f256-145">Может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="3f256-145">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3f256-146">Упрощает настройку балансировки нагрузки и безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f256-146">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3f256-147">Сертификат X.509 требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложения во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f256-147">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3f256-148">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f256-148">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3f256-149">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f256-149">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3f256-150">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-150">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3f256-151">В файле *Program.cs* код шаблона вызывает метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, который в фоновом режиме вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>.</span><span class="sxs-lookup"><span data-stu-id="3f256-151">In *Program.cs*, the template code calls <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3f256-152">Дополнительные сведения о `CreateDefaultBuilder` и построении узла см. в разделе *Настройка узла* и *Параметры построителя по умолчанию* разделы <xref:fundamentals/host/generic-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="3f256-152">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* and *Default builder settings* sections of <xref:fundamentals/host/generic-host#set-up-a-host>.</span></span>

<span data-ttu-id="3f256-153">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder` и `ConfigureWebHostDefaults`, используйте `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="3f256-153">To provide additional configuration after calling `CreateDefaultBuilder` and `ConfigureWebHostDefaults`, use `ConfigureKestrel`:</span></span>

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

<span data-ttu-id="3f256-154">Если приложение не вызывает `CreateDefaultBuilder`, чтобы настроить узел, вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **перед** вызовом `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="3f256-154">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="3f256-155">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="3f256-155">Kestrel options</span></span>

<span data-ttu-id="3f256-156">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="3f256-156">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3f256-157">Задать ограничения для свойства <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> в классе <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f256-157">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3f256-158">Свойство `Limits` содержит экземпляр класса <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="3f256-158">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3f256-159">В следующих примерах используется пространство имен <xref:Microsoft.AspNetCore.Server.Kestrel.Core>.</span><span class="sxs-lookup"><span data-stu-id="3f256-159">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="3f256-160">Параметры Kestrel, которые настраиваются в C# коде в следующих примерах, можно также задать с помощью [поставщика конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-160">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3f256-161">Например, поставщик конфигурации файла может загрузить конфигурацию Kestrel из файла *appsettings.JSON* или *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-161">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    },
    "DisableStringReuse": true
  }
}
```

<span data-ttu-id="3f256-162">Воспользуйтесь **одним** из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="3f256-162">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="3f256-163">Настройте Kestrel в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f256-163">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="3f256-164">Внедрение экземпляра `IConfiguration` в класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3f256-164">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="3f256-165">В следующем примере предполагается, что введенная конфигурация назначается свойству `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="3f256-165">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="3f256-166">В `Startup.ConfigureServices` загрузите раздел конфигурации `Kestrel` в конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-166">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="3f256-167">Настройте Kestrel при сборке узла:</span><span class="sxs-lookup"><span data-stu-id="3f256-167">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="3f256-168">В *Program.cs* загрузите раздел конфигурации `Kestrel` в конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-168">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IHostBuilder CreateHostBuilder(string[] args) =>
      Host.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .ConfigureWebHostDefaults(webBuilder =>
          {
              webBuilder.UseStartup<Startup>();
          });
  ```

<span data-ttu-id="3f256-169">Оба предыдущих подхода работают с любым [поставщиком конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-169">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="3f256-170">Время ожидания проверки на активность</span><span class="sxs-lookup"><span data-stu-id="3f256-170">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3f256-171">Получает или задает [время ожидания проверки на активность](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="3f256-171">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3f256-172">Значение по умолчанию — 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="3f256-172">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=19-20)]

### <a name="maximum-client-connections"></a><span data-ttu-id="3f256-173">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="3f256-173">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3f256-174">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="3f256-174">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="3f256-175">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f256-175">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3f256-176">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="3f256-176">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="3f256-177">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="3f256-177">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3f256-178">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="3f256-178">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3f256-179">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="3f256-179">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3f256-180">Чтобы переопределить это ограничение в приложении MVC ASP.NET Core, мы рекомендуем использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="3f256-180">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3f256-181">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="3f256-181">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3f256-182">Переопределите параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="3f256-182">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3f256-183">Если приложение пытается настроить ограничение для запроса после того, как оно начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3f256-183">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3f256-184">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="3f256-184">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3f256-185">При запуске приложения [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), который обслуживает [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module), ограничение на размер текста запроса Kestrel будет отключено, так как оно уже задается IIS.</span><span class="sxs-lookup"><span data-stu-id="3f256-185">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3f256-186">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="3f256-186">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3f256-187">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="3f256-187">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3f256-188">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="3f256-188">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3f256-189">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="3f256-189">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3f256-190">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="3f256-190">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3f256-191">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="3f256-191">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3f256-192">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="3f256-192">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3f256-193">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f256-193">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-11)]

<span data-ttu-id="3f256-194">Переопределите ограничения минимальной скорости для каждого запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="3f256-194">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="3f256-195">Возможности <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature>, указанные в предыдущем примере, отсутствуют в `HttpContext.Features` для запросов HTTP/2, так как изменение ограничений скорости для каждого запроса не поддерживается для HTTP/2 из-за поддержки протокола мультиплексирования запроса.</span><span class="sxs-lookup"><span data-stu-id="3f256-195">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinResponseDataRateFeature> referenced in the prior sample is not present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis is generally not supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="3f256-196">Но возможности <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> все еще присутствуют в `HttpContext.Features` для запросов HTTP/2, так как ограничение скорости чтения может быть *полностью отключено* для отдельных запросов. Чтобы сделать это, задайте для параметра `IHttpMinRequestBodyDataRateFeature.MinDataRate` значение `null` (даже для запроса HTTP/2).</span><span class="sxs-lookup"><span data-stu-id="3f256-196">However, the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.Features.IHttpMinRequestBodyDataRateFeature> is still present `HttpContext.Features` for HTTP/2 requests, because the read rate limit can still be *disabled entirely* on a per-request basis by setting `IHttpMinRequestBodyDataRateFeature.MinDataRate` to `null` even for an HTTP/2 request.</span></span> <span data-ttu-id="3f256-197">При попытке чтения свойства `IHttpMinRequestBodyDataRateFeature.MinDataRate` или при попытке задать для него значение, отличное от `null`, возникнет исключение `NotSupportedException` для запроса HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-197">Attempting to read `IHttpMinRequestBodyDataRateFeature.MinDataRate` or attempting to set it to a value other than `null` will result in a `NotSupportedException` being thrown given an HTTP/2 request.</span></span>

<span data-ttu-id="3f256-198">Ограничения скорости на уровне сервера, которые настроены с помощью `KestrelServerOptions.Limits`, по-прежнему применяются к подключениям HTTP/1.x и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-198">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="3f256-199">Время ожидания для заголовков запросов</span><span class="sxs-lookup"><span data-stu-id="3f256-199">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3f256-200">Получает или задает максимальное время, которое сервер уделяет получению заголовков запросов.</span><span class="sxs-lookup"><span data-stu-id="3f256-200">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3f256-201">Значение по умолчанию — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="3f256-201">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=21-22)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="3f256-202">Максимальное число потоков на подключение</span><span class="sxs-lookup"><span data-stu-id="3f256-202">Maximum streams per connection</span></span>

<span data-ttu-id="3f256-203">`Http2.MaxStreamsPerConnection` ограничивает количество параллельных потоков запросов для одного соединения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-203">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="3f256-204">Потоки сверх этого числа будут отклонены.</span><span class="sxs-lookup"><span data-stu-id="3f256-204">Excess streams are refused.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
});
```

<span data-ttu-id="3f256-205">Значение по умолчанию — 100.</span><span class="sxs-lookup"><span data-stu-id="3f256-205">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="3f256-206">Размер таблицы заголовка</span><span class="sxs-lookup"><span data-stu-id="3f256-206">Header table size</span></span>

<span data-ttu-id="3f256-207">Декодер HPACK распаковывает заголовки HTTP для подключений HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-207">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="3f256-208">`Http2.HeaderTableSize` ограничивает размер таблицы сжатия заголовка, которую использует декодер HPACK.</span><span class="sxs-lookup"><span data-stu-id="3f256-208">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="3f256-209">Это значение указывается в октетах и должно быть больше нуля (0).</span><span class="sxs-lookup"><span data-stu-id="3f256-209">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.HeaderTableSize = 4096;
});
```

<span data-ttu-id="3f256-210">Значение по умолчанию — 4096.</span><span class="sxs-lookup"><span data-stu-id="3f256-210">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="3f256-211">Максимальный размер кадра</span><span class="sxs-lookup"><span data-stu-id="3f256-211">Maximum frame size</span></span>

<span data-ttu-id="3f256-212">`Http2.MaxFrameSize` указывает максимально допустимый размер полезных данных в кадре подключения HTTP/2, получаемых или отправляемых сервером.</span><span class="sxs-lookup"><span data-stu-id="3f256-212">`Http2.MaxFrameSize` indicates the maximum allowed size of an HTTP/2 connection frame payload received or sent by the server.</span></span> <span data-ttu-id="3f256-213">Это значение указывается в октетах и должно находиться в пределах от 2^14 (16 384) до 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="3f256-213">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxFrameSize = 16384;
});
```

<span data-ttu-id="3f256-214">Значение по умолчанию — 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="3f256-214">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="3f256-215">Максимальный размер запроса заголовка</span><span class="sxs-lookup"><span data-stu-id="3f256-215">Maximum request header size</span></span>

<span data-ttu-id="3f256-216">`Http2.MaxRequestHeaderFieldSize` указывает максимально допустимый размер значений заголовка запроса (в октетах).</span><span class="sxs-lookup"><span data-stu-id="3f256-216">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="3f256-217">Это ограничение применяется к имени и значению в их сжатых и несжатых представлениях.</span><span class="sxs-lookup"><span data-stu-id="3f256-217">This limit applies to both name and value in their compressed and uncompressed representations.</span></span> <span data-ttu-id="3f256-218">Значение должно быть больше нуля.</span><span class="sxs-lookup"><span data-stu-id="3f256-218">The value must be greater than zero (0).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
});
```

<span data-ttu-id="3f256-219">Значение по умолчанию — 8 192.</span><span class="sxs-lookup"><span data-stu-id="3f256-219">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="3f256-220">Размер окна начального подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-220">Initial connection window size</span></span>

<span data-ttu-id="3f256-221">`Http2.InitialConnectionWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для всех запросов (потоков) на каждое соединение.</span><span class="sxs-lookup"><span data-stu-id="3f256-221">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="3f256-222">Размеры запросов также ограничиваются параметром `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f256-222">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3f256-223">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f256-223">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
});
```

<span data-ttu-id="3f256-224">Значение по умолчанию — 128 КБ (131 072).</span><span class="sxs-lookup"><span data-stu-id="3f256-224">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="3f256-225">Размер окна начального потока</span><span class="sxs-lookup"><span data-stu-id="3f256-225">Initial stream window size</span></span>

<span data-ttu-id="3f256-226">`Http2.InitialStreamWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для каждого запроса (потока).</span><span class="sxs-lookup"><span data-stu-id="3f256-226">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="3f256-227">Размеры запросов также ограничиваются параметром `Http2.InitialConnectionWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f256-227">Requests are also limited by `Http2.InitialConnectionWindowSize`.</span></span> <span data-ttu-id="3f256-228">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f256-228">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
});
```

<span data-ttu-id="3f256-229">Значение по умолчанию — 96 КБ (98 304).</span><span class="sxs-lookup"><span data-stu-id="3f256-229">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="3f256-230">Синхронные операции ввода-вывода</span><span class="sxs-lookup"><span data-stu-id="3f256-230">Synchronous IO</span></span>

<span data-ttu-id="3f256-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> определяет, разрешены ли синхронные операции ввода-вывода для запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="3f256-231"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3f256-232">Значение по умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="3f256-232">The default value is `false`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f256-233">Выполнение большого числа заблокированных операций синхронного ввода-вывода может привести к перегрузке пула потоков и зависанию приложения.</span><span class="sxs-lookup"><span data-stu-id="3f256-233">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3f256-234">Включайте `AllowSynchronousIO`, только если вы используете библиотеку, которая не поддерживает асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="3f256-234">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3f256-235">В примере ниже включаются синхронные операции ввода-вывода:</span><span class="sxs-lookup"><span data-stu-id="3f256-235">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="3f256-236">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="3f256-236">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3f256-237">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="3f256-237">Endpoint configuration</span></span>

<span data-ttu-id="3f256-238">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="3f256-238">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3f256-239">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="3f256-239">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3f256-240">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="3f256-240">Specify URLs using the:</span></span>

* <span data-ttu-id="3f256-241">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f256-241">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3f256-242">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-242">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3f256-243">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-243">`urls` host configuration key.</span></span>
* <span data-ttu-id="3f256-244">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-244">`UseUrls` extension method.</span></span>

<span data-ttu-id="3f256-245">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-245">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3f256-246">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3f256-246">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3f256-247">Дополнительные сведения о таких подходах см. в разделах [URL-адреса сервера](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f256-247">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3f256-248">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="3f256-248">A development certificate is created:</span></span>

* <span data-ttu-id="3f256-249">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="3f256-249">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3f256-250">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-250">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3f256-251">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="3f256-251">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3f256-252">Шаблоны проектов настраивают приложения так, чтобы они запускались на базе HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3f256-252">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3f256-253">Вызовите методы <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> или <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> из <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, чтобы настроить префиксы URL-адресов и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-253">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3f256-254">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-254">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3f256-255">Конфигурация `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="3f256-255">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3f256-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="3f256-256">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3f256-257">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-257">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3f256-258">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="3f256-258">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureEndpointDefaults(listenOptions =>
    {
        // Configure endpoint defaults
    });
});
```

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3f256-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="3f256-259">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3f256-260">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-260">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3f256-261">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="3f256-261">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        // certificate is an X509Certificate2
        listenOptions.ServerCertificate = certificate;
    });
});
```

### <a name="configureiconfiguration"></a><span data-ttu-id="3f256-262">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="3f256-262">Configure(IConfiguration)</span></span>

<span data-ttu-id="3f256-263">Создает загрузчик конфигурации для настройки Kestrel, который принимает <xref:Microsoft.Extensions.Configuration.IConfiguration> в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="3f256-263">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3f256-264">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-264">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3f256-265">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="3f256-265">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3f256-266">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-266">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3f256-267">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="3f256-267">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3f256-268">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-268">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3f256-269">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="3f256-269">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="3f256-270">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="3f256-270">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3f256-271">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="3f256-271">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3f256-272">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="3f256-272">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3f256-273">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f256-273">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3f256-274">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f256-274">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3f256-275">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-275">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3f256-276">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-276">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3f256-277">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="3f256-277">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3f256-278">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-278">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3f256-279">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="3f256-279">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3f256-280">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-280">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3f256-281">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-281">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3f256-282">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="3f256-282">Supported configurations described next:</span></span>

* <span data-ttu-id="3f256-283">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="3f256-283">No configuration</span></span>
* <span data-ttu-id="3f256-284">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="3f256-284">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3f256-285">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="3f256-285">Change the defaults in code</span></span>

<span data-ttu-id="3f256-286">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-286">*No configuration*</span></span>

<span data-ttu-id="3f256-287">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-287">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3f256-288">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-288">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3f256-289">`CreateDefaultBuilder` по умолчанию вызывает `Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-289">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3f256-290">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-290">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3f256-291">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="3f256-291">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3f256-292">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-292">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3f256-293">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="3f256-293">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3f256-294">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию**, или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="3f256-294">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="3f256-295">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="3f256-295">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3f256-296">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3f256-296">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3f256-297">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="3f256-297">Schema notes:</span></span>

* <span data-ttu-id="3f256-298">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="3f256-298">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3f256-299">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="3f256-299">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3f256-300">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-300">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3f256-301">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="3f256-301">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3f256-302">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="3f256-302">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3f256-303">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-303">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3f256-304">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="3f256-304">The `Certificate` section is optional.</span></span> <span data-ttu-id="3f256-305">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f256-305">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3f256-306">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="3f256-306">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3f256-307">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="3f256-307">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3f256-308">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="3f256-308">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3f256-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, listenOptions => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="3f256-309">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

```csharp
webBuilder.UseKestrel((context, serverOptions) =>
{
    serverOptions.Configure(context.Configuration.GetSection("Kestrel"))
        .Endpoint("HTTPS", listenOptions =>
        {
            listenOptions.HttpsOptions.SslProtocols = SslProtocols.Tls12;
        });
});
```

<span data-ttu-id="3f256-310">Можно обратиться напрямую к `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="3f256-310">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3f256-311">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="3f256-311">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3f256-312">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("{SECTION}"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="3f256-312">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3f256-313">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="3f256-313">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3f256-314">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-314">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3f256-315">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="3f256-315">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3f256-316">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-316">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3f256-317">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="3f256-317">*Change the defaults in code*</span></span>

<span data-ttu-id="3f256-318">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="3f256-318">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3f256-319">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-319">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

```csharp
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
```

<span data-ttu-id="3f256-320">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="3f256-320">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3f256-321">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="3f256-321">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3f256-322">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-322">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3f256-323">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="3f256-323">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3f256-324">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="3f256-324">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3f256-325">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-325">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3f256-326">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="3f256-326">SNI support requires:</span></span>

* <span data-ttu-id="3f256-327">Запуск на целевой платформе `netcoreapp2.1` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-327">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3f256-328">В `net461` или более поздней версии обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="3f256-328">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3f256-329">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="3f256-329">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3f256-330">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-330">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3f256-331">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-331">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

```csharp
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
});
```

### <a name="connection-logging"></a><span data-ttu-id="3f256-332">Ведение журнала подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-332">Connection logging</span></span>

<span data-ttu-id="3f256-333">Вызовите <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, чтобы выдать журналы уровня отладки для обмена данными на уровне байтов в рамках подключения.</span><span class="sxs-lookup"><span data-stu-id="3f256-333">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="3f256-334">Ведение журнала подключения полезно для устранения неполадок, связанных с низкоуровневым взаимодействием, например при TLS-шифровании и работе за прокси-серверами.</span><span class="sxs-lookup"><span data-stu-id="3f256-334">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="3f256-335">Если `UseConnectionLogging` поместить перед `UseHttps`, в журнале регистрируется зашифрованный трафик.</span><span class="sxs-lookup"><span data-stu-id="3f256-335">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="3f256-336">Если `UseConnectionLogging` поместить после `UseHttps`, в журнале регистрируется расшифрованный трафик.</span><span class="sxs-lookup"><span data-stu-id="3f256-336">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3f256-337">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="3f256-337">Bind to a TCP socket</span></span>

<span data-ttu-id="3f256-338">Метод <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> выполняет привязку к TCP-сокету, а лямбда-выражение параметров позволяет настроить конфигурацию сертификата X.509:</span><span class="sxs-lookup"><span data-stu-id="3f256-338">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="3f256-339">В этом примере настраивается HTTPS для конечной точки с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f256-339">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3f256-340">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="3f256-340">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3f256-341">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="3f256-341">Bind to a Unix socket</span></span>

<span data-ttu-id="3f256-342">Вы можете прослушивать сокет UNIX с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, чтобы улучшить производительность Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="3f256-342">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="3f256-343">Порт 0</span><span class="sxs-lookup"><span data-stu-id="3f256-343">Port 0</span></span>

<span data-ttu-id="3f256-344">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="3f256-344">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3f256-345">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="3f256-345">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/3.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3f256-346">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="3f256-346">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3f256-347">Ограничения</span><span class="sxs-lookup"><span data-stu-id="3f256-347">Limitations</span></span>

<span data-ttu-id="3f256-348">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="3f256-348">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3f256-349">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-349">`--urls` command-line argument</span></span>
* <span data-ttu-id="3f256-350">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-350">`urls` host configuration key</span></span>
* <span data-ttu-id="3f256-351">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f256-351">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3f256-352">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-352">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3f256-353">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="3f256-353">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3f256-354">С этими подходами нельзя использовать HTTPS, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="3f256-354">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3f256-355">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-355">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3f256-356">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="3f256-356">IIS endpoint configuration</span></span>

<span data-ttu-id="3f256-357">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-357">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3f256-358">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3f256-358">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="3f256-359">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="3f256-359">ListenOptions.Protocols</span></span>

<span data-ttu-id="3f256-360">Свойство `Protocols` устанавливает протоколы HTTP (`HttpProtocols`), разрешенные для конечной точки подключения или для сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-360">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="3f256-361">Значение свойства `Protocols` должно входить в перечисление `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="3f256-361">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="3f256-362">Значение перечисления `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="3f256-362">`HttpProtocols` enum value</span></span> | <span data-ttu-id="3f256-363">Допустимый протокол подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-363">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="3f256-364">Только HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-364">HTTP/1.1 only.</span></span> <span data-ttu-id="3f256-365">Можно использовать с протоколом TLS или без него.</span><span class="sxs-lookup"><span data-stu-id="3f256-365">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="3f256-366">Только HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-366">HTTP/2 only.</span></span> <span data-ttu-id="3f256-367">Может использоваться без TLS только в том случае, если клиент поддерживает [режим предварительного знания](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="3f256-367">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="3f256-368">HTTP/1.1 и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-368">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="3f256-369">Для использования HTTP/2 требуется, чтобы клиент выбрал HTTP/2 при подтверждении [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3); в противном случае используется подключение по умолчанию HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-369">HTTP/2 requires the client to select HTTP/2 in the TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="3f256-370">Значение `ListenOptions.Protocols` по умолчанию для любой конечной точки равно `HttpProtocols.Http1AndHttp2`.</span><span class="sxs-lookup"><span data-stu-id="3f256-370">The default `ListenOptions.Protocols` value for any endpoint is `HttpProtocols.Http1AndHttp2`.</span></span>

<span data-ttu-id="3f256-371">Ограничения TLS для HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="3f256-371">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="3f256-372">TSL 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-372">TLS version 1.2 or later</span></span>
* <span data-ttu-id="3f256-373">Повторное согласование отключено.</span><span class="sxs-lookup"><span data-stu-id="3f256-373">Renegotiation disabled</span></span>
* <span data-ttu-id="3f256-374">Сжатие отключено.</span><span class="sxs-lookup"><span data-stu-id="3f256-374">Compression disabled</span></span>
* <span data-ttu-id="3f256-375">Минимальные размеры обмена временными ключами:</span><span class="sxs-lookup"><span data-stu-id="3f256-375">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="3f256-376">эллиптическая кривая Диффи-Хелмана (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; не менее 224 бит;</span><span class="sxs-lookup"><span data-stu-id="3f256-376">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="3f256-377">конечное поле Диффи-Хелмана (DHE) &lbrack;`TLS12`&rbrack; &ndash; не менее 2048 бит;</span><span class="sxs-lookup"><span data-stu-id="3f256-377">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="3f256-378">Набор шифров не внесен в список блокировок.</span><span class="sxs-lookup"><span data-stu-id="3f256-378">Cipher suite not blacklisted</span></span>

<span data-ttu-id="3f256-379">По умолчанию поддерживается `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; с эллиптической кривой P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="3f256-379">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="3f256-380">Следующий пример разрешает подключения HTTP/1.1 и HTTP/2 через порт 8000.</span><span class="sxs-lookup"><span data-stu-id="3f256-380">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="3f256-381">Эти подключения шифруются по протоколу TLS с использованием предоставленного сертификата:</span><span class="sxs-lookup"><span data-stu-id="3f256-381">Connections are secured by TLS with a supplied certificate:</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
    });
});
```

<span data-ttu-id="3f256-382">При необходимости, используйте ПО промежуточного слоя подключения для фильтрации подтверждений TLS для каждого соединения по конкретным шифрам.</span><span class="sxs-lookup"><span data-stu-id="3f256-382">Use Connection Middleware to filter TLS handshakes on a per-connection basis for specific ciphers if required.</span></span>

<span data-ttu-id="3f256-383">Следующий пример вызывает <xref:System.NotSupportedException> для любого алгоритма шифрования, который не поддерживается приложением.</span><span class="sxs-lookup"><span data-stu-id="3f256-383">The following example throws <xref:System.NotSupportedException> for any cipher algorithm that the app doesn't support.</span></span> <span data-ttu-id="3f256-384">Также можно определить и сравнить [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) со списком приемлемых наборов шифров.</span><span class="sxs-lookup"><span data-stu-id="3f256-384">Alternatively, define and compare [ITlsHandshakeFeature.CipherAlgorithm](xref:Microsoft.AspNetCore.Connections.Features.ITlsHandshakeFeature.CipherAlgorithm) to a list of acceptable cipher suites.</span></span>

<span data-ttu-id="3f256-385">При использовании алгоритма шифрования [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) шифрование не используется.</span><span class="sxs-lookup"><span data-stu-id="3f256-385">No encryption is used with a [CipherAlgorithmType.Null](xref:System.Security.Authentication.CipherAlgorithmType) cipher algorithm.</span></span>

```csharp
// using System.Net;
// using Microsoft.AspNetCore.Connections;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.UseTlsFilter();
    });
});
```

```csharp
using System;
using System.Security.Authentication;
using Microsoft.AspNetCore.Connections.Features;

namespace Microsoft.AspNetCore.Connections
{
    public static class TlsFilterConnectionMiddlewareExtensions
    {
        public static IConnectionBuilder UseTlsFilter(
            this IConnectionBuilder builder)
        {
            return builder.Use((connection, next) =>
            {
                var tlsFeature = connection.Features.Get<ITlsHandshakeFeature>();

                if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
                {
                    throw new NotSupportedException("Prohibited cipher: " +
                        tlsFeature.CipherAlgorithm);
                }

                return next();
            });
        }
    }
}
```

<span data-ttu-id="3f256-386">Фильтрацию соединений также можно настроить с помощью лямбды <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder>:</span><span class="sxs-lookup"><span data-stu-id="3f256-386">Connection filtering can also be configured via an <xref:Microsoft.AspNetCore.Connections.IConnectionBuilder> lambda:</span></span>

```csharp
// using System;
// using System.Net;
// using System.Security.Authentication;
// using Microsoft.AspNetCore.Connections;
// using Microsoft.AspNetCore.Connections.Features;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseHttps("testCert.pfx", "testPassword");
        listenOptions.Use((context, next) =>
        {
            var tlsFeature = context.Features.Get<ITlsHandshakeFeature>();

            if (tlsFeature.CipherAlgorithm == CipherAlgorithmType.Null)
            {
                throw new NotSupportedException(
                    $"Prohibited cipher: {tlsFeature.CipherAlgorithm}");
            }

            return next();
        });
    });
});
```

<span data-ttu-id="3f256-387">В Linux для фильтрации подтверждений TLS по каждому соединению можно использовать <xref:System.Net.Security.CipherSuitesPolicy>:</span><span class="sxs-lookup"><span data-stu-id="3f256-387">On Linux, <xref:System.Net.Security.CipherSuitesPolicy> can be used to filter TLS handshakes on a per-connection basis:</span></span>

```csharp
// using System.Net.Security;
// using Microsoft.AspNetCore.Hosting;
// using Microsoft.AspNetCore.Server.Kestrel.Core;
// using Microsoft.Extensions.DependencyInjection;
// using Microsoft.Extensions.Hosting;

webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.ConfigureHttpsDefaults(listenOptions =>
    {
        listenOptions.OnAuthenticate = (context, sslOptions) =>
        {
            sslOptions.CipherSuitesPolicy = new CipherSuitesPolicy(
                new[]
                {
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_128_GCM_SHA256,
                    TlsCipherSuite.TLS_ECDHE_ECDSA_WITH_AES_256_GCM_SHA384,
                    // ...
                });
        };
    });
});
```

<span data-ttu-id="3f256-388">*Выбор протокола из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-388">*Set the protocol from configuration*</span></span>

<span data-ttu-id="3f256-389">`CreateDefaultBuilder` по умолчанию вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-389">`CreateDefaultBuilder` calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="3f256-390">В следующем примере *appsettings.json* для всех конечных точек устанавливается протокол подключения по умолчанию HTTP/1.1:</span><span class="sxs-lookup"><span data-stu-id="3f256-390">The following *appsettings.json* example establishes HTTP/1.1 as the default connection protocol for all endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1"
    }
  }
}
```

<span data-ttu-id="3f256-391">В следующем примере *appsettings.json* для отдельной конечной точки устанавливается протокол подключения HTTP/1.1:</span><span class="sxs-lookup"><span data-stu-id="3f256-391">The following *appsettings.json* example establishes the HTTP/1.1 connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="3f256-392">Указанные в коде протоколы переопределяют значения, заданные в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-392">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3f256-393">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="3f256-393">Transport configuration</span></span>

<span data-ttu-id="3f256-394">Для проектов, где требуется использовать Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span><span class="sxs-lookup"><span data-stu-id="3f256-394">For projects that require the use of Libuv (<xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>):</span></span>

* <span data-ttu-id="3f256-395">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="3f256-395">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

   ```xml
   <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                     Version="{VERSION}" />
   ```

* <span data-ttu-id="3f256-396">Вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> для `IWebHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="3f256-396">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> on the `IWebHostBuilder`:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="3f256-397">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="3f256-397">URL prefixes</span></span>

<span data-ttu-id="3f256-398">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="3f256-398">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3f256-399">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f256-399">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3f256-400">Kestrel не поддерживает HTTP при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-400">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3f256-401">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-401">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3f256-402">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="3f256-402">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3f256-403">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-403">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3f256-404">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="3f256-404">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3f256-405">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-405">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3f256-406">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="3f256-406">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3f256-407">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f256-407">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3f256-408">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="3f256-408">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3f256-409">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f256-409">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3f256-410">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-410">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3f256-411">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f256-411">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3f256-412">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="3f256-412">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3f256-413">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="3f256-413">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3f256-414">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="3f256-414">Host filtering</span></span>

<span data-ttu-id="3f256-415">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="3f256-415">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3f256-416">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="3f256-416">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3f256-417">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="3f256-417">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3f256-418">Заголовки `Host` не проверяются.</span><span class="sxs-lookup"><span data-stu-id="3f256-418">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3f256-419">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="3f256-419">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3f256-420">ПО промежуточного слоя фильтрации узлов предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который неявно предоставляется для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-420">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is implicitly provided for ASP.NET Core apps.</span></span> <span data-ttu-id="3f256-421">ПО промежуточного слоя добавляется в <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который вызывает <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="3f256-421">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3f256-422">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-422">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3f256-423">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="3f256-423">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3f256-424">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="3f256-424">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3f256-425">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-425">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3f256-426">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) имеет также параметр <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="3f256-426">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3f256-427">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="3f256-427">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3f256-428">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок `Host` не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="3f256-428">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3f256-429">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок `Host` пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="3f256-429">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3f256-430">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в статье <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3f256-430">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="3f256-431">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-431">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3f256-432">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-432">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3f256-433">Kestrel поддерживается в следующих сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f256-433">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3f256-434">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f256-434">HTTPS</span></span>
* <span data-ttu-id="3f256-435">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3f256-435">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3f256-436">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="3f256-436">Unix sockets for high performance behind Nginx</span></span>
* <span data-ttu-id="3f256-437">HTTP/2 (за исключением macOS&dagger;)</span><span class="sxs-lookup"><span data-stu-id="3f256-437">HTTP/2 (except on macOS&dagger;)</span></span>

<span data-ttu-id="3f256-438">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="3f256-438">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>

<span data-ttu-id="3f256-439">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-439">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3f256-440">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f256-440">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="http2-support"></a><span data-ttu-id="3f256-441">Поддержка HTTP/2</span><span class="sxs-lookup"><span data-stu-id="3f256-441">HTTP/2 support</span></span>

<span data-ttu-id="3f256-442">Протокол [HTTP/2](https://httpwg.org/specs/rfc7540.html) доступен для приложений ASP.NET Core, если выполнены следующие базовые требования:</span><span class="sxs-lookup"><span data-stu-id="3f256-442">[HTTP/2](https://httpwg.org/specs/rfc7540.html) is available for ASP.NET Core apps if the following base requirements are met:</span></span>

* <span data-ttu-id="3f256-443">Операционная система&dagger;:</span><span class="sxs-lookup"><span data-stu-id="3f256-443">Operating system&dagger;</span></span>
  * <span data-ttu-id="3f256-444">Windows Server 2016 / Windows 10 или более поздних версий&Dagger;</span><span class="sxs-lookup"><span data-stu-id="3f256-444">Windows Server 2016/Windows 10 or later&Dagger;</span></span>
  * <span data-ttu-id="3f256-445">Linux с OpenSSL 1.0.2 или более поздней версии (например, Ubuntu 16.04 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="3f256-445">Linux with OpenSSL 1.0.2 or later (for example, Ubuntu 16.04 or later)</span></span>
* <span data-ttu-id="3f256-446">Требуемая версия .NET Framework: .NET Core версии 2.2 или более поздней</span><span class="sxs-lookup"><span data-stu-id="3f256-446">Target framework: .NET Core 2.2 or later</span></span>
* <span data-ttu-id="3f256-447">Подключение с поддержкой [согласования протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3).</span><span class="sxs-lookup"><span data-stu-id="3f256-447">[Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection</span></span>
* <span data-ttu-id="3f256-448">Подключение TLS 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-448">TLS 1.2 or later connection</span></span>

<span data-ttu-id="3f256-449">&dagger; HTTP/2 будет поддерживаться для macOS в будущих выпусках.</span><span class="sxs-lookup"><span data-stu-id="3f256-449">&dagger;HTTP/2 will be supported on macOS in a future release.</span></span>
<span data-ttu-id="3f256-450">&Dagger;Для Kestrel предусмотрена ограниченная поддержка HTTP/2 в Windows Server 2012 R2 и Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-450">&Dagger;Kestrel has limited support for HTTP/2 on Windows Server 2012 R2 and Windows 8.1.</span></span> <span data-ttu-id="3f256-451">Поддержка ограничена из-за небольшого числа поддерживаемых комплектов шифров TLS, доступных для этих операционных систем.</span><span class="sxs-lookup"><span data-stu-id="3f256-451">Support is limited because the list of supported TLS cipher suites available on these operating systems is limited.</span></span> <span data-ttu-id="3f256-452">Для обеспечения безопасности TLS-подключений может потребоваться сертификат, созданный с использованием алгоритма ECDSA.</span><span class="sxs-lookup"><span data-stu-id="3f256-452">A certificate generated using an Elliptic Curve Digital Signature Algorithm (ECDSA) may be required to secure TLS connections.</span></span>

<span data-ttu-id="3f256-453">Если установлено подключение HTTP/2, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) возвращает `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="3f256-453">If an HTTP/2 connection is established, [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) reports `HTTP/2`.</span></span>

<span data-ttu-id="3f256-454">По умолчанию протокол HTTP/2 отключен.</span><span class="sxs-lookup"><span data-stu-id="3f256-454">HTTP/2 is disabled by default.</span></span> <span data-ttu-id="3f256-455">Дополнительные сведения о конфигурации см. в разделах [о параметрах Kestrel](#kestrel-options) и [ListenOptions.Protocols](#listenoptionsprotocols).</span><span class="sxs-lookup"><span data-stu-id="3f256-455">For more information on configuration, see the [Kestrel options](#kestrel-options) and [ListenOptions.Protocols](#listenoptionsprotocols) sections.</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3f256-456">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="3f256-456">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3f256-457">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как [IIS](https://www.iis.net/), [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3f256-457">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3f256-458">Обратный прокси-сервер получает HTTP-запросы из сети и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-458">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3f256-459">Kestrel используется в качестве веб-сервера перехода (с выходом в Интернет).</span><span class="sxs-lookup"><span data-stu-id="3f256-459">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3f256-461">Kestrel используется в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-461">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3f256-463">Любая из этих конфигураций — с обратным прокси-сервером и без него — является поддерживаемой конфигурацией для размещения.</span><span class="sxs-lookup"><span data-stu-id="3f256-463">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3f256-464">Kestrel, используемый в качестве пограничного сервера без обратного прокси-сервера, не поддерживает общий доступ нескольких процессов к одним и тем же IP-адресам и портам.</span><span class="sxs-lookup"><span data-stu-id="3f256-464">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3f256-465">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка `Host` запросов.</span><span class="sxs-lookup"><span data-stu-id="3f256-465">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3f256-466">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="3f256-466">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3f256-467">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным.</span><span class="sxs-lookup"><span data-stu-id="3f256-467">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3f256-468">Обратный прокси-сервер:</span><span class="sxs-lookup"><span data-stu-id="3f256-468">A reverse proxy:</span></span>

* <span data-ttu-id="3f256-469">Может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="3f256-469">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3f256-470">Предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="3f256-470">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3f256-471">Может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="3f256-471">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3f256-472">Упрощает настройку балансировки нагрузки и безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f256-472">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3f256-473">Сертификат X.509 требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложения во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f256-473">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3f256-474">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f256-474">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3f256-475">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f256-475">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3f256-476">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3f256-476">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="3f256-477">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-477">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3f256-478">В файле *Program.cs* код шаблона вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который в фоновом режиме вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>.</span><span class="sxs-lookup"><span data-stu-id="3f256-478">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_DefaultBuilder&highlight=7)]

<span data-ttu-id="3f256-479">Дополнительные сведения о `CreateDefaultBuilder` и построении узла, см. в разделе *Настройка узла*, разделы <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="3f256-479">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

<span data-ttu-id="3f256-480">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder`, используйте `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="3f256-480">To provide additional configuration after calling `CreateDefaultBuilder`, use `ConfigureKestrel`:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="3f256-481">Если приложение не вызывает `CreateDefaultBuilder`, чтобы настроить узел, вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **перед** вызовом `ConfigureKestrel`:</span><span class="sxs-lookup"><span data-stu-id="3f256-481">If the app doesn't call `CreateDefaultBuilder` to set up the host, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> **before** calling `ConfigureKestrel`:</span></span>

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

## <a name="kestrel-options"></a><span data-ttu-id="3f256-482">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="3f256-482">Kestrel options</span></span>

<span data-ttu-id="3f256-483">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="3f256-483">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3f256-484">Задать ограничения для свойства <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> в классе <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f256-484">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3f256-485">Свойство `Limits` содержит экземпляр класса <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="3f256-485">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3f256-486">В следующих примерах используется пространство имен <xref:Microsoft.AspNetCore.Server.Kestrel.Core>.</span><span class="sxs-lookup"><span data-stu-id="3f256-486">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="3f256-487">Параметры Kestrel, которые настраиваются в C# коде в следующих примерах, можно также задать с помощью [поставщика конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-487">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3f256-488">Например, поставщик конфигурации файла может загрузить конфигурацию Kestrel из файла *appsettings.JSON* или *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-488">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="3f256-489">Воспользуйтесь **одним** из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="3f256-489">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="3f256-490">Настройте Kestrel в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f256-490">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="3f256-491">Внедрение экземпляра `IConfiguration` в класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3f256-491">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="3f256-492">В следующем примере предполагается, что введенная конфигурация назначается свойству `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="3f256-492">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="3f256-493">В `Startup.ConfigureServices` загрузите раздел конфигурации `Kestrel` в конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-493">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="3f256-494">Настройте Kestrel при сборке узла:</span><span class="sxs-lookup"><span data-stu-id="3f256-494">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="3f256-495">В *Program.cs* загрузите раздел конфигурации `Kestrel` в конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-495">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="3f256-496">Оба предыдущих подхода работают с любым [поставщиком конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-496">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="3f256-497">Время ожидания проверки на активность</span><span class="sxs-lookup"><span data-stu-id="3f256-497">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3f256-498">Получает или задает [время ожидания проверки на активность](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="3f256-498">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3f256-499">Значение по умолчанию — 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="3f256-499">Defaults to 2 minutes.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=15)]

### <a name="maximum-client-connections"></a><span data-ttu-id="3f256-500">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="3f256-500">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3f256-501">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="3f256-501">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=3)]

<span data-ttu-id="3f256-502">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f256-502">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3f256-503">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="3f256-503">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=4)]

<span data-ttu-id="3f256-504">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="3f256-504">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3f256-505">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="3f256-505">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3f256-506">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="3f256-506">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3f256-507">Чтобы переопределить это ограничение в приложении MVC ASP.NET Core, мы рекомендуем использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="3f256-507">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3f256-508">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="3f256-508">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=5)]

<span data-ttu-id="3f256-509">Переопределите параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="3f256-509">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3f256-510">Если приложение пытается настроить ограничение для запроса после того, как оно начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3f256-510">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3f256-511">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="3f256-511">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3f256-512">При запуске приложения [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), который обслуживает [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module), ограничение на размер текста запроса Kestrel будет отключено, так как оно уже задается IIS.</span><span class="sxs-lookup"><span data-stu-id="3f256-512">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3f256-513">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="3f256-513">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3f256-514">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="3f256-514">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3f256-515">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="3f256-515">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3f256-516">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="3f256-516">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3f256-517">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="3f256-517">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3f256-518">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="3f256-518">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3f256-519">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="3f256-519">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3f256-520">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f256-520">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=6-9)]

<span data-ttu-id="3f256-521">Переопределите ограничения минимальной скорости для каждого запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="3f256-521">Override the minimum rate limits per request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=6-21)]

<span data-ttu-id="3f256-522">В `HttpContext.Features` для запросов HTTP/2 отсутствуют возможности настройки скорости, указанные в предыдущем примере, так как изменение ограничений скорости для каждого запроса не поддерживается для HTTP/2 из-за поддержки протокола мультиплексирования запроса.</span><span class="sxs-lookup"><span data-stu-id="3f256-522">Neither rate feature referenced in the prior sample are present in `HttpContext.Features` for HTTP/2 requests because modifying rate limits on a per-request basis isn't supported for HTTP/2 due to the protocol's support for request multiplexing.</span></span> <span data-ttu-id="3f256-523">Ограничения скорости на уровне сервера, которые настроены с помощью `KestrelServerOptions.Limits`, по-прежнему применяются к подключениям HTTP/1.x и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-523">Server-wide rate limits configured via `KestrelServerOptions.Limits` still apply to both HTTP/1.x and HTTP/2 connections.</span></span>

### <a name="request-headers-timeout"></a><span data-ttu-id="3f256-524">Время ожидания для заголовков запросов</span><span class="sxs-lookup"><span data-stu-id="3f256-524">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3f256-525">Получает или задает максимальное время, которое сервер уделяет получению заголовков запросов.</span><span class="sxs-lookup"><span data-stu-id="3f256-525">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3f256-526">Значение по умолчанию — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="3f256-526">Defaults to 30 seconds.</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_Limits&highlight=16)]

### <a name="maximum-streams-per-connection"></a><span data-ttu-id="3f256-527">Максимальное число потоков на подключение</span><span class="sxs-lookup"><span data-stu-id="3f256-527">Maximum streams per connection</span></span>

<span data-ttu-id="3f256-528">`Http2.MaxStreamsPerConnection` ограничивает количество параллельных потоков запросов для одного соединения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-528">`Http2.MaxStreamsPerConnection` limits the number of concurrent request streams per HTTP/2 connection.</span></span> <span data-ttu-id="3f256-529">Потоки сверх этого числа будут отклонены.</span><span class="sxs-lookup"><span data-stu-id="3f256-529">Excess streams are refused.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxStreamsPerConnection = 100;
        });
```

<span data-ttu-id="3f256-530">Значение по умолчанию — 100.</span><span class="sxs-lookup"><span data-stu-id="3f256-530">The default value is 100.</span></span>

### <a name="header-table-size"></a><span data-ttu-id="3f256-531">Размер таблицы заголовка</span><span class="sxs-lookup"><span data-stu-id="3f256-531">Header table size</span></span>

<span data-ttu-id="3f256-532">Декодер HPACK распаковывает заголовки HTTP для подключений HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-532">The HPACK decoder decompresses HTTP headers for HTTP/2 connections.</span></span> <span data-ttu-id="3f256-533">`Http2.HeaderTableSize` ограничивает размер таблицы сжатия заголовка, которую использует декодер HPACK.</span><span class="sxs-lookup"><span data-stu-id="3f256-533">`Http2.HeaderTableSize` limits the size of the header compression table that the HPACK decoder uses.</span></span> <span data-ttu-id="3f256-534">Это значение указывается в октетах и должно быть больше нуля (0).</span><span class="sxs-lookup"><span data-stu-id="3f256-534">The value is provided in octets and must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.HeaderTableSize = 4096;
        });
```

<span data-ttu-id="3f256-535">Значение по умолчанию — 4096.</span><span class="sxs-lookup"><span data-stu-id="3f256-535">The default value is 4096.</span></span>

### <a name="maximum-frame-size"></a><span data-ttu-id="3f256-536">Максимальный размер кадра</span><span class="sxs-lookup"><span data-stu-id="3f256-536">Maximum frame size</span></span>

<span data-ttu-id="3f256-537">`Http2.MaxFrameSize` указывает максимальный размер полезных данных в получаемом кадре подключения HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-537">`Http2.MaxFrameSize` indicates the maximum size of the HTTP/2 connection frame payload to receive.</span></span> <span data-ttu-id="3f256-538">Это значение указывается в октетах и должно находиться в пределах от 2^14 (16 384) до 2^24-1 (16 777 215).</span><span class="sxs-lookup"><span data-stu-id="3f256-538">The value is provided in octets and must be between 2^14 (16,384) and 2^24-1 (16,777,215).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxFrameSize = 16384;
        });
```

<span data-ttu-id="3f256-539">Значение по умолчанию — 2^14 (16 384).</span><span class="sxs-lookup"><span data-stu-id="3f256-539">The default value is 2^14 (16,384).</span></span>

### <a name="maximum-request-header-size"></a><span data-ttu-id="3f256-540">Максимальный размер запроса заголовка</span><span class="sxs-lookup"><span data-stu-id="3f256-540">Maximum request header size</span></span>

<span data-ttu-id="3f256-541">`Http2.MaxRequestHeaderFieldSize` указывает максимально допустимый размер значений заголовка запроса (в октетах).</span><span class="sxs-lookup"><span data-stu-id="3f256-541">`Http2.MaxRequestHeaderFieldSize` indicates the maximum allowed size in octets of request header values.</span></span> <span data-ttu-id="3f256-542">Это ограничение применяется к имени и значению в их сжатых и несжатых представлениях.</span><span class="sxs-lookup"><span data-stu-id="3f256-542">This limit applies to both name and value together in their compressed and uncompressed representations.</span></span> <span data-ttu-id="3f256-543">Значение должно быть больше нуля.</span><span class="sxs-lookup"><span data-stu-id="3f256-543">The value must be greater than zero (0).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.MaxRequestHeaderFieldSize = 8192;
        });
```

<span data-ttu-id="3f256-544">Значение по умолчанию — 8 192.</span><span class="sxs-lookup"><span data-stu-id="3f256-544">The default value is 8,192.</span></span>

### <a name="initial-connection-window-size"></a><span data-ttu-id="3f256-545">Размер окна начального подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-545">Initial connection window size</span></span>

<span data-ttu-id="3f256-546">`Http2.InitialConnectionWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для всех запросов (потоков) на каждое соединение.</span><span class="sxs-lookup"><span data-stu-id="3f256-546">`Http2.InitialConnectionWindowSize` indicates the maximum request body data in bytes the server buffers at one time aggregated across all requests (streams) per connection.</span></span> <span data-ttu-id="3f256-547">Размеры запросов также ограничиваются параметром `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f256-547">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3f256-548">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f256-548">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialConnectionWindowSize = 131072;
        });
```

<span data-ttu-id="3f256-549">Значение по умолчанию — 128 КБ (131 072).</span><span class="sxs-lookup"><span data-stu-id="3f256-549">The default value is 128 KB (131,072).</span></span>

### <a name="initial-stream-window-size"></a><span data-ttu-id="3f256-550">Размер окна начального потока</span><span class="sxs-lookup"><span data-stu-id="3f256-550">Initial stream window size</span></span>

<span data-ttu-id="3f256-551">`Http2.InitialStreamWindowSize` указывает максимальное значение данных тела запроса (в байтах), буферизируемое сервером за один раз, для каждого запроса (потока).</span><span class="sxs-lookup"><span data-stu-id="3f256-551">`Http2.InitialStreamWindowSize` indicates the maximum request body data in bytes the server buffers at one time per request (stream).</span></span> <span data-ttu-id="3f256-552">Размеры запросов также ограничиваются параметром `Http2.InitialStreamWindowSize`.</span><span class="sxs-lookup"><span data-stu-id="3f256-552">Requests are also limited by `Http2.InitialStreamWindowSize`.</span></span> <span data-ttu-id="3f256-553">Значение должно быть больше или равно 65 535 и меньше 2^31 (2 147 483 648).</span><span class="sxs-lookup"><span data-stu-id="3f256-553">The value must be greater than or equal to 65,535 and less than 2^31 (2,147,483,648).</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .ConfigureKestrel((context, serverOptions) =>
        {
            serverOptions.Limits.Http2.InitialStreamWindowSize = 98304;
        });
```

<span data-ttu-id="3f256-554">Значение по умолчанию — 96 КБ (98 304).</span><span class="sxs-lookup"><span data-stu-id="3f256-554">The default value is 96 KB (98,304).</span></span>

### <a name="synchronous-io"></a><span data-ttu-id="3f256-555">Синхронные операции ввода-вывода</span><span class="sxs-lookup"><span data-stu-id="3f256-555">Synchronous IO</span></span>

<span data-ttu-id="3f256-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> определяет, разрешены ли синхронные операции ввода-вывода для запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="3f256-556"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3f256-557">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="3f256-557">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f256-558">Выполнение большого числа заблокированных операций синхронного ввода-вывода может привести к перегрузке пула потоков и зависанию приложения.</span><span class="sxs-lookup"><span data-stu-id="3f256-558">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3f256-559">Включайте `AllowSynchronousIO`, только если вы используете библиотеку, которая не поддерживает асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="3f256-559">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3f256-560">В примере ниже включаются синхронные операции ввода-вывода:</span><span class="sxs-lookup"><span data-stu-id="3f256-560">The following example enables synchronous IO:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_SyncIO)]

<span data-ttu-id="3f256-561">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="3f256-561">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3f256-562">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="3f256-562">Endpoint configuration</span></span>

<span data-ttu-id="3f256-563">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="3f256-563">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3f256-564">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="3f256-564">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3f256-565">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="3f256-565">Specify URLs using the:</span></span>

* <span data-ttu-id="3f256-566">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f256-566">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3f256-567">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-567">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3f256-568">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-568">`urls` host configuration key.</span></span>
* <span data-ttu-id="3f256-569">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-569">`UseUrls` extension method.</span></span>

<span data-ttu-id="3f256-570">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-570">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3f256-571">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3f256-571">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3f256-572">Дополнительные сведения о таких подходах см. в разделах [URL-адреса сервера](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f256-572">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3f256-573">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="3f256-573">A development certificate is created:</span></span>

* <span data-ttu-id="3f256-574">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="3f256-574">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3f256-575">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-575">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3f256-576">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="3f256-576">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3f256-577">Шаблоны проектов настраивают приложения так, чтобы они запускались на базе HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3f256-577">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3f256-578">Вызовите методы <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> или <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> из <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, чтобы настроить префиксы URL-адресов и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-578">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3f256-579">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-579">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3f256-580">Конфигурация `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="3f256-580">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3f256-581">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="3f256-581">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3f256-582">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-582">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3f256-583">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="3f256-583">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3f256-584">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="3f256-584">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3f256-585">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-585">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3f256-586">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="3f256-586">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="3f256-587">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="3f256-587">Configure(IConfiguration)</span></span>

<span data-ttu-id="3f256-588">Создает загрузчик конфигурации для настройки Kestrel, который принимает <xref:Microsoft.Extensions.Configuration.IConfiguration> в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="3f256-588">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3f256-589">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-589">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3f256-590">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="3f256-590">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3f256-591">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-591">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3f256-592">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="3f256-592">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3f256-593">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-593">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3f256-594">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="3f256-594">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="3f256-595">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="3f256-595">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3f256-596">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="3f256-596">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3f256-597">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="3f256-597">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3f256-598">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f256-598">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3f256-599">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f256-599">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3f256-600">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-600">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3f256-601">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-601">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3f256-602">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="3f256-602">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3f256-603">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-603">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3f256-604">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="3f256-604">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3f256-605">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-605">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3f256-606">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-606">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3f256-607">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="3f256-607">Supported configurations described next:</span></span>

* <span data-ttu-id="3f256-608">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="3f256-608">No configuration</span></span>
* <span data-ttu-id="3f256-609">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="3f256-609">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3f256-610">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="3f256-610">Change the defaults in code</span></span>

<span data-ttu-id="3f256-611">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-611">*No configuration*</span></span>

<span data-ttu-id="3f256-612">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-612">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3f256-613">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-613">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3f256-614">`CreateDefaultBuilder` по умолчанию вызывает `Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-614">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3f256-615">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-615">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3f256-616">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="3f256-616">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3f256-617">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-617">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3f256-618">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="3f256-618">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3f256-619">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию**, или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="3f256-619">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="3f256-620">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="3f256-620">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3f256-621">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3f256-621">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3f256-622">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="3f256-622">Schema notes:</span></span>

* <span data-ttu-id="3f256-623">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="3f256-623">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3f256-624">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="3f256-624">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3f256-625">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-625">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3f256-626">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="3f256-626">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3f256-627">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="3f256-627">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3f256-628">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-628">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3f256-629">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="3f256-629">The `Certificate` section is optional.</span></span> <span data-ttu-id="3f256-630">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f256-630">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3f256-631">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="3f256-631">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3f256-632">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="3f256-632">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3f256-633">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="3f256-633">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3f256-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, listenOptions => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="3f256-634">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="3f256-635">Можно обратиться напрямую к `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="3f256-635">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3f256-636">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="3f256-636">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3f256-637">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("{SECTION}"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="3f256-637">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3f256-638">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="3f256-638">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3f256-639">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-639">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3f256-640">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="3f256-640">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3f256-641">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-641">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3f256-642">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="3f256-642">*Change the defaults in code*</span></span>

<span data-ttu-id="3f256-643">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="3f256-643">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3f256-644">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-644">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="3f256-645">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="3f256-645">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3f256-646">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="3f256-646">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3f256-647">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-647">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3f256-648">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="3f256-648">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3f256-649">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="3f256-649">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3f256-650">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-650">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3f256-651">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="3f256-651">SNI support requires:</span></span>

* <span data-ttu-id="3f256-652">Запуск на целевой платформе `netcoreapp2.1` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-652">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3f256-653">В `net461` или более поздней версии обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="3f256-653">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3f256-654">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="3f256-654">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3f256-655">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-655">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3f256-656">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-656">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="3f256-657">Ведение журнала подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-657">Connection logging</span></span>

<span data-ttu-id="3f256-658">Вызовите <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, чтобы выдать журналы уровня отладки для обмена данными на уровне байтов в рамках подключения.</span><span class="sxs-lookup"><span data-stu-id="3f256-658">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="3f256-659">Ведение журнала подключения полезно для устранения неполадок, связанных с низкоуровневым взаимодействием, например при TLS-шифровании и работе за прокси-серверами.</span><span class="sxs-lookup"><span data-stu-id="3f256-659">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="3f256-660">Если `UseConnectionLogging` поместить перед `UseHttps`, в журнале регистрируется зашифрованный трафик.</span><span class="sxs-lookup"><span data-stu-id="3f256-660">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="3f256-661">Если `UseConnectionLogging` поместить после `UseHttps`, в журнале регистрируется расшифрованный трафик.</span><span class="sxs-lookup"><span data-stu-id="3f256-661">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3f256-662">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="3f256-662">Bind to a TCP socket</span></span>

<span data-ttu-id="3f256-663">Метод <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> выполняет привязку к TCP-сокету, а лямбда-выражение параметров позволяет настроить конфигурацию сертификата X.509:</span><span class="sxs-lookup"><span data-stu-id="3f256-663">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_TCPSocket&highlight=9-16)]

<span data-ttu-id="3f256-664">В этом примере настраивается HTTPS для конечной точки с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f256-664">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3f256-665">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="3f256-665">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3f256-666">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="3f256-666">Bind to a Unix socket</span></span>

<span data-ttu-id="3f256-667">Вы можете прослушивать сокет UNIX с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, чтобы улучшить производительность Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="3f256-667">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Program.cs?name=snippet_UnixSocket)]

### <a name="port-0"></a><span data-ttu-id="3f256-668">Порт 0</span><span class="sxs-lookup"><span data-stu-id="3f256-668">Port 0</span></span>

<span data-ttu-id="3f256-669">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="3f256-669">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3f256-670">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="3f256-670">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3f256-671">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="3f256-671">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3f256-672">Ограничения</span><span class="sxs-lookup"><span data-stu-id="3f256-672">Limitations</span></span>

<span data-ttu-id="3f256-673">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="3f256-673">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3f256-674">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-674">`--urls` command-line argument</span></span>
* <span data-ttu-id="3f256-675">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-675">`urls` host configuration key</span></span>
* <span data-ttu-id="3f256-676">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f256-676">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3f256-677">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-677">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3f256-678">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="3f256-678">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3f256-679">С этими подходами нельзя использовать HTTPS, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="3f256-679">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3f256-680">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-680">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3f256-681">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="3f256-681">IIS endpoint configuration</span></span>

<span data-ttu-id="3f256-682">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-682">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3f256-683">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3f256-683">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

### <a name="listenoptionsprotocols"></a><span data-ttu-id="3f256-684">ListenOptions.Protocols</span><span class="sxs-lookup"><span data-stu-id="3f256-684">ListenOptions.Protocols</span></span>

<span data-ttu-id="3f256-685">Свойство `Protocols` устанавливает протоколы HTTP (`HttpProtocols`), разрешенные для конечной точки подключения или для сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-685">The `Protocols` property establishes the HTTP protocols (`HttpProtocols`) enabled on a connection endpoint or for the server.</span></span> <span data-ttu-id="3f256-686">Значение свойства `Protocols` должно входить в перечисление `HttpProtocols`.</span><span class="sxs-lookup"><span data-stu-id="3f256-686">Assign a value to the `Protocols` property from the `HttpProtocols` enum.</span></span>

| <span data-ttu-id="3f256-687">Значение перечисления `HttpProtocols`</span><span class="sxs-lookup"><span data-stu-id="3f256-687">`HttpProtocols` enum value</span></span> | <span data-ttu-id="3f256-688">Допустимый протокол подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-688">Connection protocol permitted</span></span> |
| -------------------------- | ----------------------------- |
| `Http1`                    | <span data-ttu-id="3f256-689">Только HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-689">HTTP/1.1 only.</span></span> <span data-ttu-id="3f256-690">Можно использовать с протоколом TLS или без него.</span><span class="sxs-lookup"><span data-stu-id="3f256-690">Can be used with or without TLS.</span></span> |
| `Http2`                    | <span data-ttu-id="3f256-691">Только HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-691">HTTP/2 only.</span></span> <span data-ttu-id="3f256-692">Может использоваться без TLS только в том случае, если клиент поддерживает [режим предварительного знания](https://tools.ietf.org/html/rfc7540#section-3.4).</span><span class="sxs-lookup"><span data-stu-id="3f256-692">May be used without TLS only if the client supports a [Prior Knowledge mode](https://tools.ietf.org/html/rfc7540#section-3.4).</span></span> |
| `Http1AndHttp2`            | <span data-ttu-id="3f256-693">HTTP/1.1 и HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3f256-693">HTTP/1.1 and HTTP/2.</span></span> <span data-ttu-id="3f256-694">Для использования HTTP/2 требуется протокол TLS и подключение с [согласованием протокола уровня приложений (ALPN)](https://tools.ietf.org/html/rfc7301#section-3); при их отсутствии используется подключение по умолчанию HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-694">HTTP/2 requires a TLS and [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) connection; otherwise, the connection defaults to HTTP/1.1.</span></span> |

<span data-ttu-id="3f256-695">По умолчанию используется протокол HTTP/1.1.</span><span class="sxs-lookup"><span data-stu-id="3f256-695">The default protocol is HTTP/1.1.</span></span>

<span data-ttu-id="3f256-696">Ограничения TLS для HTTP/2:</span><span class="sxs-lookup"><span data-stu-id="3f256-696">TLS restrictions for HTTP/2:</span></span>

* <span data-ttu-id="3f256-697">TSL 1.2 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-697">TLS version 1.2 or later</span></span>
* <span data-ttu-id="3f256-698">Повторное согласование отключено.</span><span class="sxs-lookup"><span data-stu-id="3f256-698">Renegotiation disabled</span></span>
* <span data-ttu-id="3f256-699">Сжатие отключено.</span><span class="sxs-lookup"><span data-stu-id="3f256-699">Compression disabled</span></span>
* <span data-ttu-id="3f256-700">Минимальные размеры обмена временными ключами:</span><span class="sxs-lookup"><span data-stu-id="3f256-700">Minimum ephemeral key exchange sizes:</span></span>
  * <span data-ttu-id="3f256-701">эллиптическая кривая Диффи-Хелмана (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; не менее 224 бит;</span><span class="sxs-lookup"><span data-stu-id="3f256-701">Elliptic curve Diffie-Hellman (ECDHE) &lbrack;[RFC4492](https://www.ietf.org/rfc/rfc4492.txt)&rbrack; &ndash; 224 bits minimum</span></span>
  * <span data-ttu-id="3f256-702">конечное поле Диффи-Хелмана (DHE) &lbrack;`TLS12`&rbrack; &ndash; не менее 2048 бит;</span><span class="sxs-lookup"><span data-stu-id="3f256-702">Finite field Diffie-Hellman (DHE) &lbrack;`TLS12`&rbrack; &ndash; 2048 bits minimum</span></span>
* <span data-ttu-id="3f256-703">Набор шифров не внесен в список блокировок.</span><span class="sxs-lookup"><span data-stu-id="3f256-703">Cipher suite not blacklisted</span></span>

<span data-ttu-id="3f256-704">По умолчанию поддерживается `TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; с эллиптической кривой P-256 &lbrack;`FIPS186`&rbrack;.</span><span class="sxs-lookup"><span data-stu-id="3f256-704">`TLS_ECDHE_RSA_WITH_AES_128_GCM_SHA256` &lbrack;`TLS-ECDHE`&rbrack; with the P-256 elliptic curve &lbrack;`FIPS186`&rbrack; is supported by default.</span></span>

<span data-ttu-id="3f256-705">Следующий пример разрешает подключения HTTP/1.1 и HTTP/2 через порт 8000.</span><span class="sxs-lookup"><span data-stu-id="3f256-705">The following example permits HTTP/1.1 and HTTP/2 connections on port 8000.</span></span> <span data-ttu-id="3f256-706">Эти подключения шифруются по протоколу TLS с использованием предоставленного сертификата:</span><span class="sxs-lookup"><span data-stu-id="3f256-706">Connections are secured by TLS with a supplied certificate:</span></span>

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

<span data-ttu-id="3f256-707">Также вы можете создать реализацию `IConnectionAdapter` для фильтрации подтверждений TLS для каждого соединения по конкретным шифрам:</span><span class="sxs-lookup"><span data-stu-id="3f256-707">Optionally create an `IConnectionAdapter` implementation to filter TLS handshakes on a per-connection basis for specific ciphers:</span></span>

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
        // No encryption is used with a CipherAlgorithmType.Null cipher algorithm.
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

<span data-ttu-id="3f256-708">*Выбор протокола из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-708">*Set the protocol from configuration*</span></span>

<span data-ttu-id="3f256-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> по умолчанию вызывает `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-709"><xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*> calls `serverOptions.Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span>

<span data-ttu-id="3f256-710">В следующем примере *appsettings.json* для всех конечных точек Kestrel устанавливается протокол подключения по умолчанию (HTTP/1.1 и HTTP/2):</span><span class="sxs-lookup"><span data-stu-id="3f256-710">In the following *appsettings.json* example, a default connection protocol (HTTP/1.1 and HTTP/2) is established for all of Kestrel's endpoints:</span></span>

```json
{
  "Kestrel": {
    "EndpointDefaults": {
      "Protocols": "Http1AndHttp2"
    }
  }
}
```

<span data-ttu-id="3f256-711">В следующем примере файла конфигурации задается протокол соединения для конкретной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="3f256-711">The following configuration file example establishes a connection protocol for a specific endpoint:</span></span>

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

<span data-ttu-id="3f256-712">Указанные в коде протоколы переопределяют значения, заданные в конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-712">Protocols specified in code override values set by configuration.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3f256-713">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="3f256-713">Transport configuration</span></span>

<span data-ttu-id="3f256-714">После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах.</span><span class="sxs-lookup"><span data-stu-id="3f256-714">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="3f256-715">Это критическое изменение для приложений ASP.NET Core 2.0, которые обновляются до версии 2.1, если они вызывают <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> и зависят от одного из следующих пакетов:</span><span class="sxs-lookup"><span data-stu-id="3f256-715">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="3f256-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (прямая ссылка на пакет)</span><span class="sxs-lookup"><span data-stu-id="3f256-716">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="3f256-717">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="3f256-717">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="3f256-718">Для проектов, где требуется использовать Libuv:</span><span class="sxs-lookup"><span data-stu-id="3f256-718">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="3f256-719">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="3f256-719">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="3f256-720">Вызов <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="3f256-720">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="3f256-721">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="3f256-721">URL prefixes</span></span>

<span data-ttu-id="3f256-722">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="3f256-722">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3f256-723">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f256-723">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3f256-724">Kestrel не поддерживает HTTP при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-724">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3f256-725">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-725">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3f256-726">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="3f256-726">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3f256-727">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-727">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3f256-728">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="3f256-728">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3f256-729">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-729">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3f256-730">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="3f256-730">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3f256-731">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f256-731">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3f256-732">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="3f256-732">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3f256-733">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f256-733">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3f256-734">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-734">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3f256-735">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f256-735">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3f256-736">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="3f256-736">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3f256-737">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="3f256-737">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3f256-738">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="3f256-738">Host filtering</span></span>

<span data-ttu-id="3f256-739">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="3f256-739">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3f256-740">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="3f256-740">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3f256-741">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="3f256-741">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3f256-742">Заголовки `Host` не проверяются.</span><span class="sxs-lookup"><span data-stu-id="3f256-742">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3f256-743">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="3f256-743">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3f256-744">ПО промежуточного слоя фильтрации узла предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или 2.2).</span><span class="sxs-lookup"><span data-stu-id="3f256-744">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="3f256-745">ПО промежуточного слоя добавляется в <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который вызывает <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="3f256-745">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3f256-746">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-746">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3f256-747">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="3f256-747">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3f256-748">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="3f256-748">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3f256-749">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-749">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3f256-750">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) имеет также параметр <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="3f256-750">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3f256-751">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="3f256-751">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3f256-752">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок `Host` не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="3f256-752">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3f256-753">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок `Host` пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="3f256-753">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3f256-754">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в статье <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3f256-754">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="3f256-755">Kestrel — это кроссплатформенный [веб-сервер для ASP.NET Core](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-755">Kestrel is a cross-platform [web server for ASP.NET Core](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="3f256-756">Веб-сервер Kestrel по умолчанию включается в шаблоны проектов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-756">Kestrel is the web server that's included by default in ASP.NET Core project templates.</span></span>

<span data-ttu-id="3f256-757">Kestrel поддерживается в следующих сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f256-757">Kestrel supports the following scenarios:</span></span>

* <span data-ttu-id="3f256-758">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3f256-758">HTTPS</span></span>
* <span data-ttu-id="3f256-759">Непрозрачное обновление для поддержки [WebSocket](https://github.com/aspnet/websockets)</span><span class="sxs-lookup"><span data-stu-id="3f256-759">Opaque upgrade used to enable [WebSockets](https://github.com/aspnet/websockets)</span></span>
* <span data-ttu-id="3f256-760">Сокеты UNIX для повышения производительности при работе за Nginx</span><span class="sxs-lookup"><span data-stu-id="3f256-760">Unix sockets for high performance behind Nginx</span></span>

<span data-ttu-id="3f256-761">Kestrel поддерживается на всех платформах и во всех версиях, поддерживаемых .NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f256-761">Kestrel is supported on all platforms and versions that .NET Core supports.</span></span>

<span data-ttu-id="3f256-762">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3f256-762">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/servers/kestrel/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-kestrel-with-a-reverse-proxy"></a><span data-ttu-id="3f256-763">Условия использования Kestrel с обратным прокси-сервером</span><span class="sxs-lookup"><span data-stu-id="3f256-763">When to use Kestrel with a reverse proxy</span></span>

<span data-ttu-id="3f256-764">Kestrel можно использовать отдельно или с *обратным прокси-сервером*, таким как [IIS](https://www.iis.net/), [Nginx](https://nginx.org) или [Apache](https://httpd.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="3f256-764">Kestrel can be used by itself or with a *reverse proxy server*, such as [Internet Information Services (IIS)](https://www.iis.net/), [Nginx](https://nginx.org), or [Apache](https://httpd.apache.org/).</span></span> <span data-ttu-id="3f256-765">Обратный прокси-сервер получает HTTP-запросы из сети и пересылает их в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-765">A reverse proxy server receives HTTP requests from the network and forwards them to Kestrel.</span></span>

<span data-ttu-id="3f256-766">Kestrel используется в качестве веб-сервера перехода (с выходом в Интернет).</span><span class="sxs-lookup"><span data-stu-id="3f256-766">Kestrel used as an edge (Internet-facing) web server:</span></span>

![Kestrel взаимодействует с Интернетом напрямую, без обратного прокси-сервера](kestrel/_static/kestrel-to-internet2.png)

<span data-ttu-id="3f256-768">Kestrel используется в конфигурации обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-768">Kestrel used in a reverse proxy configuration:</span></span>

![Kestrel взаимодействует с Интернетом косвенно, через обратный прокси-сервер, такой как IIS, Nginx или Apache.](kestrel/_static/kestrel-to-internet.png)

<span data-ttu-id="3f256-770">Любая из этих конфигураций — с обратным прокси-сервером и без него — является поддерживаемой конфигурацией для размещения.</span><span class="sxs-lookup"><span data-stu-id="3f256-770">Either configuration, with or without a reverse proxy server, is a supported hosting configuration.</span></span>

<span data-ttu-id="3f256-771">Kestrel, используемый в качестве пограничного сервера без обратного прокси-сервера, не поддерживает общий доступ нескольких процессов к одним и тем же IP-адресам и портам.</span><span class="sxs-lookup"><span data-stu-id="3f256-771">Kestrel used as an edge server without a reverse proxy server doesn't support sharing the same IP and port among multiple processes.</span></span> <span data-ttu-id="3f256-772">Когда Kestrel настроен на ожидание передачи данных от порта, Kestrel обрабатывает весь трафик для этого порта независимо от заголовка `Host` запросов.</span><span class="sxs-lookup"><span data-stu-id="3f256-772">When Kestrel is configured to listen on a port, Kestrel handles all of the traffic for that port regardless of requests' `Host` headers.</span></span> <span data-ttu-id="3f256-773">Поэтому обратный прокси-сервер, который может совместно использовать порты, способен пересылать запросы в Kestrel с уникальным IP-адресом и портом.</span><span class="sxs-lookup"><span data-stu-id="3f256-773">A reverse proxy that can share ports has the ability to forward requests to Kestrel on a unique IP and port.</span></span>

<span data-ttu-id="3f256-774">Даже если обратный прокси-сервер не требуется, его использование может оказаться удобным.</span><span class="sxs-lookup"><span data-stu-id="3f256-774">Even if a reverse proxy server isn't required, using a reverse proxy server might be a good choice.</span></span>

<span data-ttu-id="3f256-775">Обратный прокси-сервер:</span><span class="sxs-lookup"><span data-stu-id="3f256-775">A reverse proxy:</span></span>

* <span data-ttu-id="3f256-776">Может ограничить общедоступную контактную зону размещенных на нем приложений.</span><span class="sxs-lookup"><span data-stu-id="3f256-776">Can limit the exposed public surface area of the apps that it hosts.</span></span>
* <span data-ttu-id="3f256-777">Предоставляет дополнительный уровень конфигурации и защиты.</span><span class="sxs-lookup"><span data-stu-id="3f256-777">Provide an additional layer of configuration and defense.</span></span>
* <span data-ttu-id="3f256-778">Может лучше интегрироваться с существующей инфраструктурой.</span><span class="sxs-lookup"><span data-stu-id="3f256-778">Might integrate better with existing infrastructure.</span></span>
* <span data-ttu-id="3f256-779">Упрощает настройку балансировки нагрузки и безопасных подключений (HTTPS).</span><span class="sxs-lookup"><span data-stu-id="3f256-779">Simplify load balancing and secure communication (HTTPS) configuration.</span></span> <span data-ttu-id="3f256-780">Сертификат X.509 требуется только обратному прокси-серверу, а сам этот сервер может обмениваться данными с серверами приложения во внутренней сети по обычному протоколу HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f256-780">Only the reverse proxy server requires an X.509 certificate, and that server can communicate with the app's servers on the internal network using plain HTTP.</span></span>

> [!WARNING]
> <span data-ttu-id="3f256-781">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f256-781">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

## <a name="how-to-use-kestrel-in-aspnet-core-apps"></a><span data-ttu-id="3f256-782">Условия использования Kestrel в приложениях ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f256-782">How to use Kestrel in ASP.NET Core apps</span></span>

<span data-ttu-id="3f256-783">Пакет [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="3f256-783">The [Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span>

<span data-ttu-id="3f256-784">Шаблоны проектов ASP.NET Core используют Kestrel по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-784">ASP.NET Core project templates use Kestrel by default.</span></span> <span data-ttu-id="3f256-785">В файле *Program.cs* код шаблона вызывает метод <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который в фоновом режиме вызывает <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>.</span><span class="sxs-lookup"><span data-stu-id="3f256-785">In *Program.cs*, the template code calls <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*> behind the scenes.</span></span>

<span data-ttu-id="3f256-786">Чтобы задать дополнительную конфигурацию после вызова `CreateDefaultBuilder`, вызовите <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span><span class="sxs-lookup"><span data-stu-id="3f256-786">To provide additional configuration after calling `CreateDefaultBuilder`, call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderKestrelExtensions.UseKestrel*>:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            // Set properties and call methods on serverOptions
        });
```

<span data-ttu-id="3f256-787">Дополнительные сведения о `CreateDefaultBuilder` и построении узла, см. в разделе *Настройка узла*, разделы <xref:fundamentals/host/web-host#set-up-a-host>.</span><span class="sxs-lookup"><span data-stu-id="3f256-787">For more information on `CreateDefaultBuilder` and building the host, see the *Set up a host* section of <xref:fundamentals/host/web-host#set-up-a-host>.</span></span>

## <a name="kestrel-options"></a><span data-ttu-id="3f256-788">Параметры Kestrel</span><span class="sxs-lookup"><span data-stu-id="3f256-788">Kestrel options</span></span>

<span data-ttu-id="3f256-789">Веб-сервер Kestrel имеет ограничительные параметры конфигурации, которые удобно использовать в развертываниях с выходом в Интернет.</span><span class="sxs-lookup"><span data-stu-id="3f256-789">The Kestrel web server has constraint configuration options that are especially useful in Internet-facing deployments.</span></span>

<span data-ttu-id="3f256-790">Задать ограничения для свойства <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> в классе <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f256-790">Set constraints on the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Limits> property of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> class.</span></span> <span data-ttu-id="3f256-791">Свойство `Limits` содержит экземпляр класса <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>.</span><span class="sxs-lookup"><span data-stu-id="3f256-791">The `Limits` property holds an instance of the <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits> class.</span></span>

<span data-ttu-id="3f256-792">В следующих примерах используется пространство имен <xref:Microsoft.AspNetCore.Server.Kestrel.Core>.</span><span class="sxs-lookup"><span data-stu-id="3f256-792">The following examples use the <xref:Microsoft.AspNetCore.Server.Kestrel.Core> namespace:</span></span>

```csharp
using Microsoft.AspNetCore.Server.Kestrel.Core;
```

<span data-ttu-id="3f256-793">Параметры Kestrel, которые настраиваются в C# коде в следующих примерах, можно также задать с помощью [поставщика конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-793">Kestrel options, which are configured in C# code in the following examples, can also be set using a [configuration provider](xref:fundamentals/configuration/index).</span></span> <span data-ttu-id="3f256-794">Например, поставщик конфигурации файла может загрузить конфигурацию Kestrel из файла *appsettings.JSON* или *appsettings.{Environment}.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-794">For example, the File Configuration Provider can load Kestrel configuration from an *appsettings.json* or *appsettings.{Environment}.json* file:</span></span>

```json
{
  "Kestrel": {
    "Limits": {
      "MaxConcurrentConnections": 100,
      "MaxConcurrentUpgradedConnections": 100
    }
  }
}
```

<span data-ttu-id="3f256-795">Воспользуйтесь **одним** из перечисленных ниже подходов.</span><span class="sxs-lookup"><span data-stu-id="3f256-795">Use **one** of the following approaches:</span></span>

* <span data-ttu-id="3f256-796">Настройте Kestrel в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="3f256-796">Configure Kestrel in `Startup.ConfigureServices`:</span></span>

  1. <span data-ttu-id="3f256-797">Внедрение экземпляра `IConfiguration` в класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="3f256-797">Inject an instance of `IConfiguration` into the `Startup` class.</span></span> <span data-ttu-id="3f256-798">В следующем примере предполагается, что введенная конфигурация назначается свойству `Configuration`.</span><span class="sxs-lookup"><span data-stu-id="3f256-798">The following example assumes that the injected configuration is assigned to the `Configuration` property.</span></span>
  2. <span data-ttu-id="3f256-799">В `Startup.ConfigureServices` загрузите раздел конфигурации `Kestrel` в конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-799">In `Startup.ConfigureServices`, load the `Kestrel` section of configuration into Kestrel's configuration.</span></span>

     ```csharp
     // using Microsoft.Extensions.Configuration

     public void ConfigureServices(IServiceCollection services)
     {
         services.Configure<KestrelServerOptions>(
             Configuration.GetSection("Kestrel"));
     }
     ```

* <span data-ttu-id="3f256-800">Настройте Kestrel при сборке узла:</span><span class="sxs-lookup"><span data-stu-id="3f256-800">Configure Kestrel when building the host:</span></span>

  <span data-ttu-id="3f256-801">В *Program.cs* загрузите раздел конфигурации `Kestrel` в конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-801">In *Program.cs*, load the `Kestrel` section of configuration into Kestrel's configuration:</span></span>

  ```csharp
  // using Microsoft.Extensions.DependencyInjection;

  public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
      WebHost.CreateDefaultBuilder(args)
          .ConfigureServices((context, services) =>
          {
              services.Configure<KestrelServerOptions>(
                  context.Configuration.GetSection("Kestrel"));
          })
          .UseStartup<Startup>();
  ```

<span data-ttu-id="3f256-802">Оба предыдущих подхода работают с любым [поставщиком конфигурации](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="3f256-802">Both of the preceding approaches work with any [configuration provider](xref:fundamentals/configuration/index).</span></span>

### <a name="keep-alive-timeout"></a><span data-ttu-id="3f256-803">Время ожидания проверки на активность</span><span class="sxs-lookup"><span data-stu-id="3f256-803">Keep-alive timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.KeepAliveTimeout>

<span data-ttu-id="3f256-804">Получает или задает [время ожидания проверки на активность](https://tools.ietf.org/html/rfc7230#section-6.5).</span><span class="sxs-lookup"><span data-stu-id="3f256-804">Gets or sets the [keep-alive timeout](https://tools.ietf.org/html/rfc7230#section-6.5).</span></span> <span data-ttu-id="3f256-805">Значение по умолчанию — 2 минуты.</span><span class="sxs-lookup"><span data-stu-id="3f256-805">Defaults to 2 minutes.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.KeepAliveTimeout = TimeSpan.FromMinutes(2);
        });
```

### <a name="maximum-client-connections"></a><span data-ttu-id="3f256-806">максимальное число клиентских подключений;</span><span class="sxs-lookup"><span data-stu-id="3f256-806">Maximum client connections</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentConnections>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxConcurrentUpgradedConnections>

<span data-ttu-id="3f256-807">Максимальное число одновременно открытых подключений TCP для всего приложения можно задать с помощью следующего кода:</span><span class="sxs-lookup"><span data-stu-id="3f256-807">The maximum number of concurrent open TCP connections can be set for the entire app with the following code:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentConnections = 100;
        });
```

<span data-ttu-id="3f256-808">Существует отдельный предел по подключениям, измененным с HTTP или HTTPS на другой протокол (например, по запросу WebSocket).</span><span class="sxs-lookup"><span data-stu-id="3f256-808">There's a separate limit for connections that have been upgraded from HTTP or HTTPS to another protocol (for example, on a WebSockets request).</span></span> <span data-ttu-id="3f256-809">После изменения подключение не учитывается в пределе `MaxConcurrentConnections`.</span><span class="sxs-lookup"><span data-stu-id="3f256-809">After a connection is upgraded, it isn't counted against the `MaxConcurrentConnections` limit.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxConcurrentUpgradedConnections = 100;
        });
```

<span data-ttu-id="3f256-810">По умолчанию максимальное число подключений не ограничено (null).</span><span class="sxs-lookup"><span data-stu-id="3f256-810">The maximum number of connections is unlimited (null) by default.</span></span>

### <a name="maximum-request-body-size"></a><span data-ttu-id="3f256-811">максимальный размер текста запроса;</span><span class="sxs-lookup"><span data-stu-id="3f256-811">Maximum request body size</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MaxRequestBodySize>

<span data-ttu-id="3f256-812">По умолчанию максимальный размер текста запроса составляет 30 000 000 байт, что примерно соответствует 28,6 МБ.</span><span class="sxs-lookup"><span data-stu-id="3f256-812">The default maximum request body size is 30,000,000 bytes, which is approximately 28.6 MB.</span></span>

<span data-ttu-id="3f256-813">Чтобы переопределить это ограничение в приложении MVC ASP.NET Core, мы рекомендуем использовать атрибут <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> в методе действия:</span><span class="sxs-lookup"><span data-stu-id="3f256-813">The recommended approach to override the limit in an ASP.NET Core MVC app is to use the <xref:Microsoft.AspNetCore.Mvc.RequestSizeLimitAttribute> attribute on an action method:</span></span>

```csharp
[RequestSizeLimit(100000000)]
public IActionResult MyActionMethod()
```

<span data-ttu-id="3f256-814">Приведенный ниже пример показывает, как настроить ограничение для приложения и каждого запроса:</span><span class="sxs-lookup"><span data-stu-id="3f256-814">Here's an example that shows how to configure the constraint for the app on every request:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.MaxRequestBodySize = 10 * 1024;
        });
```

<span data-ttu-id="3f256-815">Переопределите параметр для конкретного запроса в ПО промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="3f256-815">Override the setting on a specific request in middleware:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Limits&highlight=3-4)]

<span data-ttu-id="3f256-816">Если приложение пытается настроить ограничение для запроса после того, как оно начало считывать запрос, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="3f256-816">An exception is thrown if the app configures the limit on a request after the app has started to read the request.</span></span> <span data-ttu-id="3f256-817">Доступно свойство `IsReadOnly`, указывающее, что свойство `MaxRequestBodySize` находится в состоянии только для чтения и настраивать ограничение слишком поздно.</span><span class="sxs-lookup"><span data-stu-id="3f256-817">There's an `IsReadOnly` property that indicates if the `MaxRequestBodySize` property is in read-only state, meaning it's too late to configure the limit.</span></span>

<span data-ttu-id="3f256-818">При запуске приложения [вне процесса](xref:host-and-deploy/iis/index#out-of-process-hosting-model), который обслуживает [модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module), ограничение на размер текста запроса Kestrel будет отключено, так как оно уже задается IIS.</span><span class="sxs-lookup"><span data-stu-id="3f256-818">When an app is run [out-of-process](xref:host-and-deploy/iis/index#out-of-process-hosting-model) behind the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), Kestrel's request body size limit is disabled because IIS already sets the limit.</span></span>

### <a name="minimum-request-body-data-rate"></a><span data-ttu-id="3f256-819">минимальная скорость передачи данных в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="3f256-819">Minimum request body data rate</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinResponseDataRate>

<span data-ttu-id="3f256-820">Kestrel каждую секунду проверяет, поступают ли данные с указанной скоростью в байтах в секунду.</span><span class="sxs-lookup"><span data-stu-id="3f256-820">Kestrel checks every second if data is arriving at the specified rate in bytes/second.</span></span> <span data-ttu-id="3f256-821">Если скорость падает ниже минимума, для соединения истекает время ожидания. Льготный период — это количество времени, которое Kestrel дает клиенту на увеличение его скорости отправки до минимального уровня; в течение этого периода скорость не проверяется.</span><span class="sxs-lookup"><span data-stu-id="3f256-821">If the rate drops below the minimum, the connection is timed out. The grace period is the amount of time that Kestrel gives the client to increase its send rate up to the minimum; the rate isn't checked during that time.</span></span> <span data-ttu-id="3f256-822">Льготный период помогает избежать разрыва соединений, которые первоначально отправляют данные с небольшой скоростью из-за медленного запуска TCP.</span><span class="sxs-lookup"><span data-stu-id="3f256-822">The grace period helps avoid dropping connections that are initially sending data at a slow rate due to TCP slow-start.</span></span>

<span data-ttu-id="3f256-823">Минимальная скорость по умолчанию составляет 240 байт/с, льготный период равен 5 секундам.</span><span class="sxs-lookup"><span data-stu-id="3f256-823">The default minimum rate is 240 bytes/second with a 5 second grace period.</span></span>

<span data-ttu-id="3f256-824">Минимальная скорость также применяется к отклику.</span><span class="sxs-lookup"><span data-stu-id="3f256-824">A minimum rate also applies to the response.</span></span> <span data-ttu-id="3f256-825">Код для задания лимита запросов и лимита откликов различается только наличием `RequestBody` или `Response` в именах свойств и интерфейсов.</span><span class="sxs-lookup"><span data-stu-id="3f256-825">The code to set the request limit and the response limit is the same except for having `RequestBody` or `Response` in the property and interface names.</span></span>

<span data-ttu-id="3f256-826">Ниже приведен пример, показывающий, как настроить минимальную скорость передачи данных в *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="3f256-826">Here's an example that shows how to configure the minimum data rates in *Program.cs*:</span></span>

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

### <a name="request-headers-timeout"></a><span data-ttu-id="3f256-827">Время ожидания для заголовков запросов</span><span class="sxs-lookup"><span data-stu-id="3f256-827">Request headers timeout</span></span>

<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.RequestHeadersTimeout>

<span data-ttu-id="3f256-828">Получает или задает максимальное время, которое сервер уделяет получению заголовков запросов.</span><span class="sxs-lookup"><span data-stu-id="3f256-828">Gets or sets the maximum amount of time the server spends receiving request headers.</span></span> <span data-ttu-id="3f256-829">Значение по умолчанию — 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="3f256-829">Defaults to 30 seconds.</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.Limits.RequestHeadersTimeout = TimeSpan.FromMinutes(1);
        });
```

### <a name="synchronous-io"></a><span data-ttu-id="3f256-830">Синхронные операции ввода-вывода</span><span class="sxs-lookup"><span data-stu-id="3f256-830">Synchronous IO</span></span>

<span data-ttu-id="3f256-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> определяет, разрешены ли синхронные операции ввода-вывода для запроса и ответа.</span><span class="sxs-lookup"><span data-stu-id="3f256-831"><xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.AllowSynchronousIO> controls whether synchronous IO is allowed for the request and response.</span></span> <span data-ttu-id="3f256-832">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="3f256-832">The  default value is `true`.</span></span>

> [!WARNING]
> <span data-ttu-id="3f256-833">Выполнение большого числа заблокированных операций синхронного ввода-вывода может привести к перегрузке пула потоков и зависанию приложения.</span><span class="sxs-lookup"><span data-stu-id="3f256-833">A large number of blocking synchronous IO operations can lead to thread pool starvation, which makes the app unresponsive.</span></span> <span data-ttu-id="3f256-834">Включайте `AllowSynchronousIO`, только если вы используете библиотеку, которая не поддерживает асинхронные операции ввода-вывода.</span><span class="sxs-lookup"><span data-stu-id="3f256-834">Only enable `AllowSynchronousIO` when using a library that doesn't support asynchronous IO.</span></span>

<span data-ttu-id="3f256-835">В примере ниже отключаются синхронные операции ввода-вывода:</span><span class="sxs-lookup"><span data-stu-id="3f256-835">The following example disables synchronous IO:</span></span>

```csharp
public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
    WebHost.CreateDefaultBuilder(args)
        .UseStartup<Startup>()
        .UseKestrel(serverOptions =>
        {
            serverOptions.AllowSynchronousIO = false;
        });
```

<span data-ttu-id="3f256-836">Сведения о других параметрах и ограничениях Kestrel см. в следующих разделах:</span><span class="sxs-lookup"><span data-stu-id="3f256-836">For information about other Kestrel options and limits, see:</span></span>

* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits>
* <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>

## <a name="endpoint-configuration"></a><span data-ttu-id="3f256-837">Конфигурация конечной точки</span><span class="sxs-lookup"><span data-stu-id="3f256-837">Endpoint configuration</span></span>

<span data-ttu-id="3f256-838">По умолчанию платформа ASP.NET Core привязана к:</span><span class="sxs-lookup"><span data-stu-id="3f256-838">By default, ASP.NET Core binds to:</span></span>

* `http://localhost:5000`
* <span data-ttu-id="3f256-839">`https://localhost:5001` (если присутствует локальный сертификат разработки)</span><span class="sxs-lookup"><span data-stu-id="3f256-839">`https://localhost:5001` (when a local development certificate is present)</span></span>

<span data-ttu-id="3f256-840">Укажите URL-адреса с помощью следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="3f256-840">Specify URLs using the:</span></span>

* <span data-ttu-id="3f256-841">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f256-841">`ASPNETCORE_URLS` environment variable.</span></span>
* <span data-ttu-id="3f256-842">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-842">`--urls` command-line argument.</span></span>
* <span data-ttu-id="3f256-843">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-843">`urls` host configuration key.</span></span>
* <span data-ttu-id="3f256-844">Метод расширения `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-844">`UseUrls` extension method.</span></span>

<span data-ttu-id="3f256-845">Значение, указанное с помощью этих подходов, может быть одной или несколькими конечными точками HTTP и HTTPS (HTTPS при наличии сертификата по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-845">The value provided using these approaches can be one or more HTTP and HTTPS endpoints (HTTPS if a default cert is available).</span></span> <span data-ttu-id="3f256-846">Настройте значение в виде списка с разделением точкой с запятой (например, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span><span class="sxs-lookup"><span data-stu-id="3f256-846">Configure the value as a semicolon-separated list (for example, `"Urls": "http://localhost:8000;http://localhost:8001"`).</span></span>

<span data-ttu-id="3f256-847">Дополнительные сведения о таких подходах см. в разделах [URL-адреса сервера](xref:fundamentals/host/web-host#server-urls) и [Переопределение конфигурации](xref:fundamentals/host/web-host#override-configuration).</span><span class="sxs-lookup"><span data-stu-id="3f256-847">For more information on these approaches, see [Server URLs](xref:fundamentals/host/web-host#server-urls) and [Override configuration](xref:fundamentals/host/web-host#override-configuration).</span></span>

<span data-ttu-id="3f256-848">Сертификат разработки создается, когда:</span><span class="sxs-lookup"><span data-stu-id="3f256-848">A development certificate is created:</span></span>

* <span data-ttu-id="3f256-849">установлен [пакет SDK для .NET Core](/dotnet/core/sdk);</span><span class="sxs-lookup"><span data-stu-id="3f256-849">When the [.NET Core SDK](/dotnet/core/sdk) is installed.</span></span>
* <span data-ttu-id="3f256-850">используется [средство dev-certs](xref:aspnetcore-2.1#https) для создания сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-850">The [dev-certs tool](xref:aspnetcore-2.1#https) is used to create a certificate.</span></span>

<span data-ttu-id="3f256-851">В некоторых браузерах требуется явное разрешение доверять локальному сертификату разработки.</span><span class="sxs-lookup"><span data-stu-id="3f256-851">Some browsers require granting explicit permission to trust the local development certificate.</span></span>

<span data-ttu-id="3f256-852">Шаблоны проектов настраивают приложения так, чтобы они запускались на базе HTTPS по умолчанию и включали [поддержку перенаправления HTTPS и HSTS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="3f256-852">Project templates configure apps to run on HTTPS by default and include [HTTPS redirection and HSTS support](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="3f256-853">Вызовите методы <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> или <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> из <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions>, чтобы настроить префиксы URL-адресов и порты для Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-853">Call <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> or <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> methods on <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions> to configure URL prefixes and ports for Kestrel.</span></span>

<span data-ttu-id="3f256-854">`UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` и переменная среды `ASPNETCORE_URLS` тоже работают, однако на них распространяются ограничения, указанные далее в этой статье (для конфигурации конечной точки HTTPS требуется сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-854">`UseUrls`, the `--urls` command-line argument, `urls` host configuration key, and the `ASPNETCORE_URLS` environment variable also work but have the limitations noted later in this section (a default certificate must be available for HTTPS endpoint configuration).</span></span>

<span data-ttu-id="3f256-855">Конфигурация `KestrelServerOptions`:</span><span class="sxs-lookup"><span data-stu-id="3f256-855">`KestrelServerOptions` configuration:</span></span>

### <a name="configureendpointdefaultsactionlistenoptions"></a><span data-ttu-id="3f256-856">ConfigureEndpointDefaults(Action\<ListenOptions>)</span><span class="sxs-lookup"><span data-stu-id="3f256-856">ConfigureEndpointDefaults(Action\<ListenOptions>)</span></span>

<span data-ttu-id="3f256-857">Указывает конфигурацию `Action` для выполнения каждой заданной конечной точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-857">Specifies a configuration `Action` to run for each specified endpoint.</span></span> <span data-ttu-id="3f256-858">Если вызвать `ConfigureEndpointDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="3f256-858">Calling `ConfigureEndpointDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configurehttpsdefaultsactionhttpsconnectionadapteroptions"></a><span data-ttu-id="3f256-859">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span><span class="sxs-lookup"><span data-stu-id="3f256-859">ConfigureHttpsDefaults(Action\<HttpsConnectionAdapterOptions>)</span></span>

<span data-ttu-id="3f256-860">Указывает конфигурацию `Action` для выполнения каждой конечной точки HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-860">Specifies a configuration `Action` to run for each HTTPS endpoint.</span></span> <span data-ttu-id="3f256-861">Если вызвать `ConfigureHttpsDefaults` несколько раз, предыдущие элементы `Action` будут заменены последним элементом `Action`.</span><span class="sxs-lookup"><span data-stu-id="3f256-861">Calling `ConfigureHttpsDefaults` multiple times replaces prior `Action`s with the last `Action` specified.</span></span>

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

### <a name="configureiconfiguration"></a><span data-ttu-id="3f256-862">Configure(IConfiguration)</span><span class="sxs-lookup"><span data-stu-id="3f256-862">Configure(IConfiguration)</span></span>

<span data-ttu-id="3f256-863">Создает загрузчик конфигурации для настройки Kestrel, который принимает <xref:Microsoft.Extensions.Configuration.IConfiguration> в качестве входных данных.</span><span class="sxs-lookup"><span data-stu-id="3f256-863">Creates a configuration loader for setting up Kestrel that takes an <xref:Microsoft.Extensions.Configuration.IConfiguration> as input.</span></span> <span data-ttu-id="3f256-864">Для Kestrel конфигурация должна быть ограничена разделом конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-864">The configuration must be scoped to the configuration section for Kestrel.</span></span>

### <a name="listenoptionsusehttps"></a><span data-ttu-id="3f256-865">ListenOptions.UseHttps</span><span class="sxs-lookup"><span data-stu-id="3f256-865">ListenOptions.UseHttps</span></span>

<span data-ttu-id="3f256-866">Настройте Kestrel для использования протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-866">Configure Kestrel to use HTTPS.</span></span>

<span data-ttu-id="3f256-867">Расширения `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="3f256-867">`ListenOptions.UseHttps` extensions:</span></span>

* <span data-ttu-id="3f256-868">`UseHttps` &ndash; Настройте Kestrel для использования протокола HTTPS с сертификатом по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-868">`UseHttps` &ndash; Configure Kestrel to use HTTPS with the default certificate.</span></span> <span data-ttu-id="3f256-869">Создает исключение, если сертификат по умолчанию не настроен.</span><span class="sxs-lookup"><span data-stu-id="3f256-869">Throws an exception if no default certificate is configured.</span></span>
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

<span data-ttu-id="3f256-870">Параметры `ListenOptions.UseHttps`:</span><span class="sxs-lookup"><span data-stu-id="3f256-870">`ListenOptions.UseHttps` parameters:</span></span>

* <span data-ttu-id="3f256-871">`filename` — это путь и имя файла сертификата, связанного с каталогом, где находятся файлы содержимого приложения.</span><span class="sxs-lookup"><span data-stu-id="3f256-871">`filename` is the path and file name of a certificate file, relative to the directory that contains the app's content files.</span></span>
* <span data-ttu-id="3f256-872">`password` — это пароль для доступа к данным сертификата X.509.</span><span class="sxs-lookup"><span data-stu-id="3f256-872">`password` is the password required to access the X.509 certificate data.</span></span>
* <span data-ttu-id="3f256-873">`configureOptions` — это `Action` для настройки `HttpsConnectionAdapterOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f256-873">`configureOptions` is an `Action` to configure the `HttpsConnectionAdapterOptions`.</span></span> <span data-ttu-id="3f256-874">Возвращает `ListenOptions`.</span><span class="sxs-lookup"><span data-stu-id="3f256-874">Returns the `ListenOptions`.</span></span>
* <span data-ttu-id="3f256-875">`storeName` — это хранилище сертификатов, из которого выполняется загрузка сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-875">`storeName` is the certificate store from which to load the certificate.</span></span>
* <span data-ttu-id="3f256-876">`subject` — это имя субъекта для сертификата.</span><span class="sxs-lookup"><span data-stu-id="3f256-876">`subject` is the subject name for the certificate.</span></span>
* <span data-ttu-id="3f256-877">`allowInvalid` указывает, следует ли учитывать недопустимые сертификаты, например самозаверяющие сертификаты.</span><span class="sxs-lookup"><span data-stu-id="3f256-877">`allowInvalid` indicates if invalid certificates should be considered, such as self-signed certificates.</span></span>
* <span data-ttu-id="3f256-878">`location` — это расположение хранилища, из которого загружается сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-878">`location` is the store location to load the certificate from.</span></span>
* <span data-ttu-id="3f256-879">`serverCertificate` — это сертификат X.509.</span><span class="sxs-lookup"><span data-stu-id="3f256-879">`serverCertificate` is the X.509 certificate.</span></span>

<span data-ttu-id="3f256-880">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="3f256-880">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="3f256-881">Как минимум необходимо указать сертификат по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-881">At a minimum, a default certificate must be provided.</span></span>

<span data-ttu-id="3f256-882">Поддерживаемые конфигурации, описанные далее:</span><span class="sxs-lookup"><span data-stu-id="3f256-882">Supported configurations described next:</span></span>

* <span data-ttu-id="3f256-883">Отсутствие конфигурации</span><span class="sxs-lookup"><span data-stu-id="3f256-883">No configuration</span></span>
* <span data-ttu-id="3f256-884">Замена сертификата по умолчанию из конфигурации</span><span class="sxs-lookup"><span data-stu-id="3f256-884">Replace the default certificate from configuration</span></span>
* <span data-ttu-id="3f256-885">Изменение значений по умолчанию в коде</span><span class="sxs-lookup"><span data-stu-id="3f256-885">Change the defaults in code</span></span>

<span data-ttu-id="3f256-886">*Отсутствие конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-886">*No configuration*</span></span>

<span data-ttu-id="3f256-887">Kestrel ожидает передачи данных через `http://localhost:5000` и `https://localhost:5001` (если доступен сертификат по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="3f256-887">Kestrel listens on `http://localhost:5000` and `https://localhost:5001` (if a default cert is available).</span></span>

<a name="configuration"></a>

<span data-ttu-id="3f256-888">*Замена сертификата по умолчанию из конфигурации*</span><span class="sxs-lookup"><span data-stu-id="3f256-888">*Replace the default certificate from configuration*</span></span>

<span data-ttu-id="3f256-889">`CreateDefaultBuilder` по умолчанию вызывает `Configure(context.Configuration.GetSection("Kestrel"))`, чтобы загрузить конфигурацию Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-889">`CreateDefaultBuilder` calls `Configure(context.Configuration.GetSection("Kestrel"))` by default to load Kestrel configuration.</span></span> <span data-ttu-id="3f256-890">Kestrel имеет доступ к схеме конфигурации параметров приложения HTTPS по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-890">A default HTTPS app settings configuration schema is available for Kestrel.</span></span> <span data-ttu-id="3f256-891">Настройте несколько конечных точек, включая URL-адреса и сертификаты для использования, либо из файла на диске, либо из хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="3f256-891">Configure multiple endpoints, including the URLs and the certificates to use, either from a file on disk or from a certificate store.</span></span>

<span data-ttu-id="3f256-892">В следующем примере *appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-892">In the following *appsettings.json* example:</span></span>

* <span data-ttu-id="3f256-893">Установите для **AllowInvalid** значение `true`, чтобы разрешить использование недопустимых сертификатов (например, самозаверяющих сертификатов).</span><span class="sxs-lookup"><span data-stu-id="3f256-893">Set **AllowInvalid** to `true` to permit the use of invalid certificates (for example, self-signed certificates).</span></span>
* <span data-ttu-id="3f256-894">Любая конечная точка HTTPS, которая не указывает сертификат (**HttpsDefaultCert** в следующем примере), будет использовать сертификат, определенный в разделе **Сертификаты** > **По умолчанию**, или сертификат разработки.</span><span class="sxs-lookup"><span data-stu-id="3f256-894">Any HTTPS endpoint that doesn't specify a certificate (**HttpsDefaultCert** in the example that follows) falls back to the cert defined under **Certificates** > **Default** or the development certificate.</span></span>

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

<span data-ttu-id="3f256-895">Вместо использования параметров **Path** и **Password** для узла сертификата можно указать сертификат с помощью полей хранилища сертификатов.</span><span class="sxs-lookup"><span data-stu-id="3f256-895">An alternative to using **Path** and **Password** for any certificate node is to specify the certificate using certificate store fields.</span></span> <span data-ttu-id="3f256-896">Например, сертификат из раздела **Сертификаты** > **По умолчанию** можно указать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3f256-896">For example, the **Certificates** > **Default** certificate can be specified as:</span></span>

```json
"Default": {
  "Subject": "<subject; required>",
  "Store": "<cert store; required>",
  "Location": "<location; defaults to CurrentUser>",
  "AllowInvalid": "<true or false; defaults to false>"
}
```

<span data-ttu-id="3f256-897">Примечания к схеме.</span><span class="sxs-lookup"><span data-stu-id="3f256-897">Schema notes:</span></span>

* <span data-ttu-id="3f256-898">Регистр букв в именах конечных точек не учитывается.</span><span class="sxs-lookup"><span data-stu-id="3f256-898">Endpoints names are case-insensitive.</span></span> <span data-ttu-id="3f256-899">Например, `HTTPS` и `Https` являются допустимыми.</span><span class="sxs-lookup"><span data-stu-id="3f256-899">For example, `HTTPS` and `Https` are valid.</span></span>
* <span data-ttu-id="3f256-900">Параметр `Url` является обязательным для каждой конечной точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-900">The `Url` parameter is required for each endpoint.</span></span> <span data-ttu-id="3f256-901">Формат этого параметра такой же, как для параметра конфигурации `Urls` верхнего уровня, только он ограничен одиночным значением.</span><span class="sxs-lookup"><span data-stu-id="3f256-901">The format for this parameter is the same as the top-level `Urls` configuration parameter except that it's limited to a single value.</span></span>
* <span data-ttu-id="3f256-902">Эти конечные точки заменяют конечные точки, определенные в конфигурации `Urls` верхнего уровня, а не дополняют их.</span><span class="sxs-lookup"><span data-stu-id="3f256-902">These endpoints replace those defined in the top-level `Urls` configuration rather than adding to them.</span></span> <span data-ttu-id="3f256-903">Конечные точки, определенные в коде через `Listen`, объединяются с конечными точками, определенными в разделе конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-903">Endpoints defined in code via `Listen` are cumulative with the endpoints defined in the configuration section.</span></span>
* <span data-ttu-id="3f256-904">Раздел `Certificate` является необязательным.</span><span class="sxs-lookup"><span data-stu-id="3f256-904">The `Certificate` section is optional.</span></span> <span data-ttu-id="3f256-905">Если раздел `Certificate` не указан, используются значения по умолчанию, определенные в предыдущих сценариях.</span><span class="sxs-lookup"><span data-stu-id="3f256-905">If the `Certificate` section isn't specified, the defaults defined in earlier scenarios are used.</span></span> <span data-ttu-id="3f256-906">Если значений по умолчанию нет, сервер выдает исключение и не запускается.</span><span class="sxs-lookup"><span data-stu-id="3f256-906">If no defaults are available, the server throws an exception and fails to start.</span></span>
* <span data-ttu-id="3f256-907">Раздел `Certificate` поддерживает сертификаты **Path**&ndash;**Password** и **Subject**&ndash;**Store**.</span><span class="sxs-lookup"><span data-stu-id="3f256-907">The `Certificate` section supports both **Path**&ndash;**Password** and **Subject**&ndash;**Store** certificates.</span></span>
* <span data-ttu-id="3f256-908">Таким образом, можно определить любое количество конечных точек, если это не приводит к конфликту портов.</span><span class="sxs-lookup"><span data-stu-id="3f256-908">Any number of endpoints may be defined in this way so long as they don't cause port conflicts.</span></span>
* <span data-ttu-id="3f256-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` возвращает `KestrelConfigurationLoader` с методом `.Endpoint(string name, listenOptions => { })`, который может использоваться в качестве дополнения для параметров настроенной конечной точки:</span><span class="sxs-lookup"><span data-stu-id="3f256-909">`options.Configure(context.Configuration.GetSection("{SECTION}"))` returns a `KestrelConfigurationLoader` with an `.Endpoint(string name, listenOptions => { })` method that can be used to supplement a configured endpoint's settings:</span></span>

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

<span data-ttu-id="3f256-910">Можно обратиться напрямую к `KestrelServerOptions.ConfigurationLoader`, чтобы и далее выполнять итерацию с существующим загрузчиком, например, предоставленным <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span><span class="sxs-lookup"><span data-stu-id="3f256-910">`KestrelServerOptions.ConfigurationLoader` can be directly accessed to continue iterating on the existing loader, such as the one provided by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>.</span></span>

* <span data-ttu-id="3f256-911">Раздел конфигурации для каждой конечной точки доступен в параметрах в методе `Endpoint`, чтобы можно было прочитать пользовательские параметры.</span><span class="sxs-lookup"><span data-stu-id="3f256-911">The configuration section for each endpoint is available on the options in the `Endpoint` method so that custom settings may be read.</span></span>
* <span data-ttu-id="3f256-912">Можно загрузить несколько конфигураций, снова вызвав `options.Configure(context.Configuration.GetSection("{SECTION}"))` с другим разделом.</span><span class="sxs-lookup"><span data-stu-id="3f256-912">Multiple configurations may be loaded by calling `options.Configure(context.Configuration.GetSection("{SECTION}"))` again with another section.</span></span> <span data-ttu-id="3f256-913">Используется только последняя конфигурация, если явным образом не вызвать `Load` в предыдущих экземплярах.</span><span class="sxs-lookup"><span data-stu-id="3f256-913">Only the last configuration is used, unless `Load` is explicitly called on prior instances.</span></span> <span data-ttu-id="3f256-914">Метапакет не вызывает `Load`, чтобы можно было заменить его раздел конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-914">The metapackage doesn't call `Load` so that its default configuration section may be replaced.</span></span>
* <span data-ttu-id="3f256-915">`KestrelConfigurationLoader` отражает семейство API `Listen` из `KestrelServerOptions` как перегрузки `Endpoint`, чтобы можно было настроить конечные точки кода и конфигурации в одном месте.</span><span class="sxs-lookup"><span data-stu-id="3f256-915">`KestrelConfigurationLoader` mirrors the `Listen` family of APIs from `KestrelServerOptions` as `Endpoint` overloads, so code and config endpoints may be configured in the same place.</span></span> <span data-ttu-id="3f256-916">Эти перегрузки не используют имена и используют только параметры по умолчанию из конфигурации.</span><span class="sxs-lookup"><span data-stu-id="3f256-916">These overloads don't use names and only consume default settings from configuration.</span></span>

<span data-ttu-id="3f256-917">*Изменение значений по умолчанию в коде*</span><span class="sxs-lookup"><span data-stu-id="3f256-917">*Change the defaults in code*</span></span>

<span data-ttu-id="3f256-918">Можно использовать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults` для изменения параметров по умолчанию для `ListenOptions` и `HttpsConnectionAdapterOptions`, включая переопределение сертификата по умолчанию, указанного в предыдущем сценарии.</span><span class="sxs-lookup"><span data-stu-id="3f256-918">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` can be used to change default settings for `ListenOptions` and `HttpsConnectionAdapterOptions`, including overriding the default certificate specified in the prior scenario.</span></span> <span data-ttu-id="3f256-919">Необходимо вызвать `ConfigureEndpointDefaults` и `ConfigureHttpsDefaults`, прежде чем настраивать конечные точки.</span><span class="sxs-lookup"><span data-stu-id="3f256-919">`ConfigureEndpointDefaults` and `ConfigureHttpsDefaults` should be called before any endpoints are configured.</span></span>

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

<span data-ttu-id="3f256-920">*Поддержка Kestrel для SNI*</span><span class="sxs-lookup"><span data-stu-id="3f256-920">*Kestrel support for SNI*</span></span>

<span data-ttu-id="3f256-921">Можно использовать [указание имени сервера (SNI)](https://tools.ietf.org/html/rfc6066#section-3) для размещения нескольких доменов в одном IP-адресе и порте.</span><span class="sxs-lookup"><span data-stu-id="3f256-921">[Server Name Indication (SNI)](https://tools.ietf.org/html/rfc6066#section-3) can be used to host multiple domains on the same IP address and port.</span></span> <span data-ttu-id="3f256-922">Для использования SNI клиент отправляет имя узла для безопасного сеанса серверу во время подтверждения TLS, чтобы сервер предоставил правильный сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-922">For SNI to function, the client sends the host name for the secure session to the server during the TLS handshake so that the server can provide the correct certificate.</span></span> <span data-ttu-id="3f256-923">Клиент использует предоставленный сертификат для зашифрованного соединения с сервером во время безопасного сеанса, который следует после подтверждения TLS.</span><span class="sxs-lookup"><span data-stu-id="3f256-923">The client uses the furnished certificate for encrypted communication with the server during the secure session that follows the TLS handshake.</span></span>

<span data-ttu-id="3f256-924">Kestrel поддерживает SNI через обратный вызов `ServerCertificateSelector`.</span><span class="sxs-lookup"><span data-stu-id="3f256-924">Kestrel supports SNI via the `ServerCertificateSelector` callback.</span></span> <span data-ttu-id="3f256-925">Функция обратного вызова используется один раз за подключение, чтобы приложение проверило имя узла и выбрало соответствующий сертификат.</span><span class="sxs-lookup"><span data-stu-id="3f256-925">The callback is invoked once per connection to allow the app to inspect the host name and select the appropriate certificate.</span></span>

<span data-ttu-id="3f256-926">Поддержка SNI требует:</span><span class="sxs-lookup"><span data-stu-id="3f256-926">SNI support requires:</span></span>

* <span data-ttu-id="3f256-927">Запуск на целевой платформе `netcoreapp2.1` или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="3f256-927">Running on target framework `netcoreapp2.1` or later.</span></span> <span data-ttu-id="3f256-928">В `net461` или более поздней версии обратный вызов выполняется, но `name` всегда имеет значение `null`.</span><span class="sxs-lookup"><span data-stu-id="3f256-928">On `net461` or later, the callback is invoked but the `name` is always `null`.</span></span> <span data-ttu-id="3f256-929">`name` также имеет значение `null`, если клиент не предоставляет параметр имени узла при подтверждении TLS.</span><span class="sxs-lookup"><span data-stu-id="3f256-929">The `name` is also `null` if the client doesn't provide the host name parameter in the TLS handshake.</span></span>
* <span data-ttu-id="3f256-930">Все веб-сайты выполняются на одном и том же экземпляре Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-930">All websites run on the same Kestrel instance.</span></span> <span data-ttu-id="3f256-931">Kestrel не поддерживает совместное использование IP-адреса и порта на нескольких экземплярах без обратного прокси-сервера.</span><span class="sxs-lookup"><span data-stu-id="3f256-931">Kestrel doesn't support sharing an IP address and port across multiple instances without a reverse proxy.</span></span>

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

### <a name="connection-logging"></a><span data-ttu-id="3f256-932">Ведение журнала подключения</span><span class="sxs-lookup"><span data-stu-id="3f256-932">Connection logging</span></span>

<span data-ttu-id="3f256-933">Вызовите <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*>, чтобы выдать журналы уровня отладки для обмена данными на уровне байтов в рамках подключения.</span><span class="sxs-lookup"><span data-stu-id="3f256-933">Call <xref:Microsoft.AspNetCore.Hosting.ListenOptionsConnectionLoggingExtensions.UseConnectionLogging*> to emit Debug level logs for byte-level communication on a connection.</span></span> <span data-ttu-id="3f256-934">Ведение журнала подключения полезно для устранения неполадок, связанных с низкоуровневым взаимодействием, например при TLS-шифровании и работе за прокси-серверами.</span><span class="sxs-lookup"><span data-stu-id="3f256-934">Connection logging is helpful for troubleshooting problems in low-level communication, such as during TLS encryption and behind proxies.</span></span> <span data-ttu-id="3f256-935">Если `UseConnectionLogging` поместить перед `UseHttps`, в журнале регистрируется зашифрованный трафик.</span><span class="sxs-lookup"><span data-stu-id="3f256-935">If `UseConnectionLogging` is placed before `UseHttps`, encrypted traffic is logged.</span></span> <span data-ttu-id="3f256-936">Если `UseConnectionLogging` поместить после `UseHttps`, в журнале регистрируется расшифрованный трафик.</span><span class="sxs-lookup"><span data-stu-id="3f256-936">If `UseConnectionLogging` is placed after `UseHttps`, decrypted traffic is logged.</span></span>

```csharp
webBuilder.ConfigureKestrel(serverOptions =>
{
    serverOptions.Listen(IPAddress.Any, 8000, listenOptions =>
    {
        listenOptions.UseConnectionLogging();
    });
});
```

### <a name="bind-to-a-tcp-socket"></a><span data-ttu-id="3f256-937">Привязка к TCP-сокету</span><span class="sxs-lookup"><span data-stu-id="3f256-937">Bind to a TCP socket</span></span>

<span data-ttu-id="3f256-938">Метод <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> выполняет привязку к TCP-сокету, а лямбда-выражение параметров позволяет настроить конфигурацию сертификата X.509:</span><span class="sxs-lookup"><span data-stu-id="3f256-938">The <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.Listen*> method binds to a TCP socket, and an options lambda permits X.509 certificate configuration:</span></span>

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

<span data-ttu-id="3f256-939">В этом примере настраивается HTTPS для конечной точки с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span><span class="sxs-lookup"><span data-stu-id="3f256-939">The example configures HTTPS for an endpoint with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.ListenOptions>.</span></span> <span data-ttu-id="3f256-940">С помощью этого API можно настроить и другие параметры Kestrel для отдельных конечных точек.</span><span class="sxs-lookup"><span data-stu-id="3f256-940">Use the same API to configure other Kestrel settings for specific endpoints.</span></span>

[!INCLUDE [How to make an X.509 cert](~/includes/make-x509-cert.md)]

### <a name="bind-to-a-unix-socket"></a><span data-ttu-id="3f256-941">Привязка к сокету UNIX</span><span class="sxs-lookup"><span data-stu-id="3f256-941">Bind to a Unix socket</span></span>

<span data-ttu-id="3f256-942">Вы можете прослушивать сокет UNIX с помощью <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*>, чтобы улучшить производительность Nginx, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="3f256-942">Listen on a Unix socket with <xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerOptions.ListenUnixSocket*> for improved performance with Nginx, as shown in this example:</span></span>

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

### <a name="port-0"></a><span data-ttu-id="3f256-943">Порт 0</span><span class="sxs-lookup"><span data-stu-id="3f256-943">Port 0</span></span>

<span data-ttu-id="3f256-944">Если указать номер порта `0`, Kestrel динамически привязывается к доступному порту.</span><span class="sxs-lookup"><span data-stu-id="3f256-944">When the port number `0` is specified, Kestrel dynamically binds to an available port.</span></span> <span data-ttu-id="3f256-945">Следующий пример показывает, как определить, к какому порту фактически привязан Kestrel во время выполнения:</span><span class="sxs-lookup"><span data-stu-id="3f256-945">The following example shows how to determine which port Kestrel actually bound at runtime:</span></span>

[!code-csharp[](kestrel/samples/2.x/KestrelSample/Startup.cs?name=snippet_Configure&highlight=3-4,15-21)]

<span data-ttu-id="3f256-946">Когда приложение выполняется, в выходных данных в окне консоли указывается динамический порт, по которому можно связаться с приложением:</span><span class="sxs-lookup"><span data-stu-id="3f256-946">When the app is run, the console window output indicates the dynamic port where the app can be reached:</span></span>

```console
Listening on the following addresses: http://127.0.0.1:48508
```

### <a name="limitations"></a><span data-ttu-id="3f256-947">Ограничения</span><span class="sxs-lookup"><span data-stu-id="3f256-947">Limitations</span></span>

<span data-ttu-id="3f256-948">Настройте конечные точки с помощью следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="3f256-948">Configure endpoints with the following approaches:</span></span>

* <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*>
* <span data-ttu-id="3f256-949">Аргументы командной строки `--urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-949">`--urls` command-line argument</span></span>
* <span data-ttu-id="3f256-950">Ключ конфигурации узла `urls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-950">`urls` host configuration key</span></span>
* <span data-ttu-id="3f256-951">Переменная среды `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="3f256-951">`ASPNETCORE_URLS` environment variable</span></span>

<span data-ttu-id="3f256-952">Эти методы удобны, если нужно, чтобы код работал с серверами, отличными от Kestrel.</span><span class="sxs-lookup"><span data-stu-id="3f256-952">These methods are useful for making code work with servers other than Kestrel.</span></span> <span data-ttu-id="3f256-953">Не забывайте о следующих ограничениях.</span><span class="sxs-lookup"><span data-stu-id="3f256-953">However, be aware of the following limitations:</span></span>

* <span data-ttu-id="3f256-954">С этими подходами нельзя использовать HTTPS, если в конфигурации конечной точки HTTPS не предоставлен сертификат по умолчанию (например, с помощью конфигурации `KestrelServerOptions` или файла конфигурации, как показано выше в этом разделе).</span><span class="sxs-lookup"><span data-stu-id="3f256-954">HTTPS can't be used with these approaches unless a default certificate is provided in the HTTPS endpoint configuration (for example, using `KestrelServerOptions` configuration or a configuration file as shown earlier in this topic).</span></span>
* <span data-ttu-id="3f256-955">Если подходы `Listen` и `UseUrls` используются одновременно, конечные точки `Listen` переопределяют конечные точки `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-955">When both the `Listen` and `UseUrls` approaches are used simultaneously, the `Listen` endpoints override the `UseUrls` endpoints.</span></span>

### <a name="iis-endpoint-configuration"></a><span data-ttu-id="3f256-956">Конфигурация конечной точки IIS</span><span class="sxs-lookup"><span data-stu-id="3f256-956">IIS endpoint configuration</span></span>

<span data-ttu-id="3f256-957">При использовании служб IIS привязки URL-адресов для IIS переопределяют привязки, заданные `Listen` или `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-957">When using IIS, the URL bindings for IIS override bindings are set by either `Listen` or `UseUrls`.</span></span> <span data-ttu-id="3f256-958">Дополнительные сведения см. в статье [Модуль ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="3f256-958">For more information, see the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module) topic.</span></span>

## <a name="transport-configuration"></a><span data-ttu-id="3f256-959">Конфигурация транспорта</span><span class="sxs-lookup"><span data-stu-id="3f256-959">Transport configuration</span></span>

<span data-ttu-id="3f256-960">После выпуска ASP.NET Core 2.1 транспорт Kestrel по умолчанию основан не на Libuv, а на управляемых сокетах.</span><span class="sxs-lookup"><span data-stu-id="3f256-960">With the release of ASP.NET Core 2.1, Kestrel's default transport is no longer based on Libuv but instead based on managed sockets.</span></span> <span data-ttu-id="3f256-961">Это критическое изменение для приложений ASP.NET Core 2.0, которые обновляются до версии 2.1, если они вызывают <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> и зависят от одного из следующих пакетов:</span><span class="sxs-lookup"><span data-stu-id="3f256-961">This is a breaking change for ASP.NET Core 2.0 apps upgrading to 2.1 that call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*> and depend on either of the following packages:</span></span>

* <span data-ttu-id="3f256-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (прямая ссылка на пакет)</span><span class="sxs-lookup"><span data-stu-id="3f256-962">[Microsoft.AspNetCore.Server.Kestrel](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel/) (direct package reference)</span></span>
* [<span data-ttu-id="3f256-963">Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="3f256-963">Microsoft.AspNetCore.App</span></span>](https://www.nuget.org/packages/Microsoft.AspNetCore.App/)

<span data-ttu-id="3f256-964">Для проектов, где требуется использовать Libuv:</span><span class="sxs-lookup"><span data-stu-id="3f256-964">For projects that require the use of Libuv:</span></span>

* <span data-ttu-id="3f256-965">Добавляют зависимость для пакета [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) к файлу проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="3f256-965">Add a dependency for the [Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv](https://www.nuget.org/packages/Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv/) package to the app's project file:</span></span>

  ```xml
  <PackageReference Include="Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv"
                    Version="{VERSION}" />
  ```

* <span data-ttu-id="3f256-966">Вызов <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span><span class="sxs-lookup"><span data-stu-id="3f256-966">Call <xref:Microsoft.AspNetCore.Hosting.WebHostBuilderLibuvExtensions.UseLibuv*>:</span></span>

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

### <a name="url-prefixes"></a><span data-ttu-id="3f256-967">Префиксы URL-адресов</span><span class="sxs-lookup"><span data-stu-id="3f256-967">URL prefixes</span></span>

<span data-ttu-id="3f256-968">Если вы используете `UseUrls`, аргумент командной строки `--urls`, ключ конфигурации узла `urls` или переменную среды `ASPNETCORE_URLS`, префиксы URL-адресов могут иметь любой из указанных ниже форматов.</span><span class="sxs-lookup"><span data-stu-id="3f256-968">When using `UseUrls`, `--urls` command-line argument, `urls` host configuration key, or `ASPNETCORE_URLS` environment variable, the URL prefixes can be in any of the following formats.</span></span>

<span data-ttu-id="3f256-969">Допустимы только префиксы URL-адресов HTTP.</span><span class="sxs-lookup"><span data-stu-id="3f256-969">Only HTTP URL prefixes are valid.</span></span> <span data-ttu-id="3f256-970">Kestrel не поддерживает HTTP при настройке привязок URL-адресов с помощью `UseUrls`.</span><span class="sxs-lookup"><span data-stu-id="3f256-970">Kestrel doesn't support HTTPS when configuring URL bindings using `UseUrls`.</span></span>

* <span data-ttu-id="3f256-971">IPv4-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-971">IPv4 address with port number</span></span>

  ```
  http://65.55.39.10:80/
  ```

  <span data-ttu-id="3f256-972">`0.0.0.0` является особым случаем, соответствующим привязке ко всем IPv4-адресам.</span><span class="sxs-lookup"><span data-stu-id="3f256-972">`0.0.0.0` is a special case that binds to all IPv4 addresses.</span></span>

* <span data-ttu-id="3f256-973">IPv6-адрес с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-973">IPv6 address with port number</span></span>

  ```
  http://[0:0:0:0:0:ffff:4137:270a]:80/
  ```

  <span data-ttu-id="3f256-974">`[::]` является IPv6-аналогом IPv4-адреса `0.0.0.0`.</span><span class="sxs-lookup"><span data-stu-id="3f256-974">`[::]` is the IPv6 equivalent of IPv4 `0.0.0.0`.</span></span>

* <span data-ttu-id="3f256-975">Имя узла с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-975">Host name with port number</span></span>

  ```
  http://contoso.com:80/
  http://*:80/
  ```

  <span data-ttu-id="3f256-976">Имена узлов, `*` и `+`, не являются особыми.</span><span class="sxs-lookup"><span data-stu-id="3f256-976">Host names, `*`, and `+`, aren't special.</span></span> <span data-ttu-id="3f256-977">Все, что не распознается как допустимый IP-адрес или `localhost`, привязывается ко всем IP-адресам IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f256-977">Anything not recognized as a valid IP address or `localhost` binds to all IPv4 and IPv6 IPs.</span></span> <span data-ttu-id="3f256-978">Чтобы привязать разные имена узлов к разным приложениям ASP.NET Core по одному порту, используйте [HTTP.sys](xref:fundamentals/servers/httpsys) или обратный прокси-сервер, такой как IIS, Nginx или Apache.</span><span class="sxs-lookup"><span data-stu-id="3f256-978">To bind different host names to different ASP.NET Core apps on the same port, use [HTTP.sys](xref:fundamentals/servers/httpsys) or a reverse proxy server, such as IIS, Nginx, or Apache.</span></span>

  > [!WARNING]
  > <span data-ttu-id="3f256-979">Для размещения в конфигурации обратного прокси-сервера требуется [фильтрация узлов](#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="3f256-979">Hosting in a reverse proxy configuration requires [host filtering](#host-filtering).</span></span>

* <span data-ttu-id="3f256-980">Имя узла `localhost` с номером порта или IP-адрес замыкания на себя с номером порта</span><span class="sxs-lookup"><span data-stu-id="3f256-980">Host `localhost` name with port number or loopback IP with port number</span></span>

  ```
  http://localhost:5000/
  http://127.0.0.1:5000/
  http://[::1]:5000/
  ```

  <span data-ttu-id="3f256-981">Когда указан `localhost`, Kestrel пытается привязаться к обоим интерфейсам замыкания на себя IPv4 и IPv6.</span><span class="sxs-lookup"><span data-stu-id="3f256-981">When `localhost` is specified, Kestrel attempts to bind to both IPv4 and IPv6 loopback interfaces.</span></span> <span data-ttu-id="3f256-982">Если запрошенный порт уже используется другой службой в одном из интерфейсов замыкания на себя, Kestrel не запускается.</span><span class="sxs-lookup"><span data-stu-id="3f256-982">If the requested port is in use by another service on either loopback interface, Kestrel fails to start.</span></span> <span data-ttu-id="3f256-983">Если один из интерфейсов замыкания на себя недоступен по любой другой причине (чаще всего, это отсутствие поддержки IPv6), Kestrel заносит в журнал предупреждение.</span><span class="sxs-lookup"><span data-stu-id="3f256-983">If either loopback interface is unavailable for any other reason (most commonly because IPv6 isn't supported), Kestrel logs a warning.</span></span>

## <a name="host-filtering"></a><span data-ttu-id="3f256-984">Фильтрация узлов</span><span class="sxs-lookup"><span data-stu-id="3f256-984">Host filtering</span></span>

<span data-ttu-id="3f256-985">Хотя Kestrel поддерживает конфигурации с использованием префиксов, такие как `http://example.com:5000`, Kestrel не учитывает имя узла.</span><span class="sxs-lookup"><span data-stu-id="3f256-985">While Kestrel supports configuration based on prefixes such as `http://example.com:5000`, Kestrel largely ignores the host name.</span></span> <span data-ttu-id="3f256-986">Узел `localhost` является особым случаем, используемым для привязки к адресам замыкания на себя.</span><span class="sxs-lookup"><span data-stu-id="3f256-986">Host `localhost` is a special case used for binding to loopback addresses.</span></span> <span data-ttu-id="3f256-987">Любой узел, отличный от явного IP-адреса, привязывается ко всем общедоступным IP-адресам.</span><span class="sxs-lookup"><span data-stu-id="3f256-987">Any host other than an explicit IP address binds to all public IP addresses.</span></span> <span data-ttu-id="3f256-988">Заголовки `Host` не проверяются.</span><span class="sxs-lookup"><span data-stu-id="3f256-988">`Host` headers aren't validated.</span></span>

<span data-ttu-id="3f256-989">В качестве обходного решения используйте ПО промежуточного слоя фильтрации узлов.</span><span class="sxs-lookup"><span data-stu-id="3f256-989">As a workaround, use Host Filtering Middleware.</span></span> <span data-ttu-id="3f256-990">ПО промежуточного слоя фильтрации узла предоставляется пакетом [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering), который входит в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или 2.2).</span><span class="sxs-lookup"><span data-stu-id="3f256-990">Host Filtering Middleware is provided by the [Microsoft.AspNetCore.HostFiltering](https://www.nuget.org/packages/Microsoft.AspNetCore.HostFiltering) package, which is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or 2.2).</span></span> <span data-ttu-id="3f256-991">ПО промежуточного слоя добавляется в <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, который вызывает <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span><span class="sxs-lookup"><span data-stu-id="3f256-991">The middleware is added by <xref:Microsoft.AspNetCore.WebHost.CreateDefaultBuilder*>, which calls <xref:Microsoft.AspNetCore.Builder.HostFilteringServicesExtensions.AddHostFiltering*>:</span></span>

[!code-csharp[](kestrel/samples-snapshot/2.x/KestrelSample/Program.cs?name=snippet_Program&highlight=9)]

<span data-ttu-id="3f256-992">ПО промежуточного слоя фильтрации узлов отключено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="3f256-992">Host Filtering Middleware is disabled by default.</span></span> <span data-ttu-id="3f256-993">Чтобы включить ПО промежуточного слоя, определите ключ `AllowedHosts` в *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span><span class="sxs-lookup"><span data-stu-id="3f256-993">To enable the middleware, define an `AllowedHosts` key in *appsettings.json*/*appsettings.\<EnvironmentName>.json*.</span></span> <span data-ttu-id="3f256-994">Значение представляет собой разделенный точками с запятой список имен узлов без номеров портов:</span><span class="sxs-lookup"><span data-stu-id="3f256-994">The value is a semicolon-delimited list of host names without port numbers:</span></span>

<span data-ttu-id="3f256-995">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="3f256-995">*appsettings.json*:</span></span>

```json
{
  "AllowedHosts": "example.com;localhost"
}
```

> [!NOTE]
> <span data-ttu-id="3f256-996">[ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer) имеет также параметр <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts>.</span><span class="sxs-lookup"><span data-stu-id="3f256-996">[Forwarded Headers Middleware](xref:host-and-deploy/proxy-load-balancer) also has an <xref:Microsoft.AspNetCore.Builder.ForwardedHeadersOptions.AllowedHosts> option.</span></span> <span data-ttu-id="3f256-997">ПО промежуточного слоя перенаправления заголовков и ПО промежуточного слоя фильтрации узлов обладают сходными возможностями для различных сценариев.</span><span class="sxs-lookup"><span data-stu-id="3f256-997">Forwarded Headers Middleware and Host Filtering Middleware have similar functionality for different scenarios.</span></span> <span data-ttu-id="3f256-998">Параметр `AllowedHosts` с ПО промежуточного слоя перенаправления заголовков подходит для случаев, когда заголовок `Host` не сохраняется при переадресации запросов с помощью обратного прокси-сервера или подсистемы балансировки нагрузки.</span><span class="sxs-lookup"><span data-stu-id="3f256-998">Setting `AllowedHosts` with Forwarded Headers Middleware is appropriate when the `Host` header isn't preserved while forwarding requests with a reverse proxy server or load balancer.</span></span> <span data-ttu-id="3f256-999">Параметр `AllowedHosts` с ПО промежуточного слоя фильтрации узлов подходит для случаев, когда Kestrel используется в качестве общедоступного пограничного сервера или если заголовок `Host` пересылается напрямую.</span><span class="sxs-lookup"><span data-stu-id="3f256-999">Setting `AllowedHosts` with Host Filtering Middleware is appropriate when Kestrel is used as a public-facing edge server or when the `Host` header is directly forwarded.</span></span>
>
> <span data-ttu-id="3f256-1000">Дополнительные сведения о ПО промежуточного слоя перенаправления заголовков см. в статье <xref:host-and-deploy/proxy-load-balancer>.</span><span class="sxs-lookup"><span data-stu-id="3f256-1000">For more information on Forwarded Headers Middleware, see <xref:host-and-deploy/proxy-load-balancer>.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3f256-1001">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3f256-1001">Additional resources</span></span>

* <xref:test/troubleshoot>
* <xref:security/enforcing-ssl>
* <xref:host-and-deploy/proxy-load-balancer>
* [<span data-ttu-id="3f256-1002">RFC 7230: синтаксис и маршрутизация сообщений (раздел 5.4: узел)</span><span class="sxs-lookup"><span data-stu-id="3f256-1002">RFC 7230: Message Syntax and Routing (Section 5.4: Host)</span></span>](https://tools.ietf.org/html/rfc7230#section-5.4)
