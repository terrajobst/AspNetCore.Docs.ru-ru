---
title: Универсальный узел .NET
author: tdykstra
description: Сведения об универсальном узле .NET Core, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 10/07/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 1582955cd18e6739111af05c9a892cd5cb4e270d
ms.sourcegitcommit: 3d082bd46e9e00a3297ea0314582b1ed2abfa830
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/07/2019
ms.locfileid: "72007237"
---
# <a name="net-generic-host"></a><span data-ttu-id="7570d-103">Универсальный узел .NET</span><span class="sxs-lookup"><span data-stu-id="7570d-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7570d-104">В этой статье описывается универсальный узел .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) и приводятся рекомендации о том, как его использовать.</span><span class="sxs-lookup"><span data-stu-id="7570d-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="7570d-105">Что такое узел?</span><span class="sxs-lookup"><span data-stu-id="7570d-105">What's a host?</span></span>

<span data-ttu-id="7570d-106">*Узел* — это объект, который инкапсулирует все ресурсы приложения, такие как:</span><span class="sxs-lookup"><span data-stu-id="7570d-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="7570d-107">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="7570d-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="7570d-108">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="7570d-108">Logging</span></span>
* <span data-ttu-id="7570d-109">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="7570d-109">Configuration</span></span>
* <span data-ttu-id="7570d-110">Реализации `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="7570d-110">`IHostedService` implementations</span></span>

<span data-ttu-id="7570d-111">После запуска узла он вызывает `IHostedService.StartAsync` в каждой реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, которую найдет в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7570d-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="7570d-112">В веб-приложении одна из реализаций `IHostedService` является веб-службой, которая запускает [реализацию сервера HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="7570d-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="7570d-113">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="7570d-114">В версиях ASP.NET Core до 3.0 [веб-узел](xref:fundamentals/host/web-host) используется для рабочих нагрузок HTTP.</span><span class="sxs-lookup"><span data-stu-id="7570d-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="7570d-115">Веб-узел больше не рекомендуется для веб-приложений и доступен только для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="7570d-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="7570d-116">Создание узла</span><span class="sxs-lookup"><span data-stu-id="7570d-116">Set up a host</span></span>

<span data-ttu-id="7570d-117">Узел обычно настраивается, собирается и выполняется кодом в классе `Program`.</span><span class="sxs-lookup"><span data-stu-id="7570d-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="7570d-118">Метод `Main`:</span><span class="sxs-lookup"><span data-stu-id="7570d-118">The `Main` method:</span></span>

* <span data-ttu-id="7570d-119">Вызывает метод `CreateHostBuilder` для создания и настройки объекта построителя.</span><span class="sxs-lookup"><span data-stu-id="7570d-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="7570d-120">Вызывает методы `Build` и `Run` в объекте построителя.</span><span class="sxs-lookup"><span data-stu-id="7570d-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="7570d-121">Вот код *Program.cs* для рабочей нагрузки, отличной от HTTP, с одной реализацией `IHostedService`, добавленной в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7570d-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureServices((hostContext, services) =>
            {
               services.AddHostedService<Worker>();
            });
}
```

<span data-ttu-id="7570d-122">При использовании рабочей нагрузки HTTP метод `Main` остается прежним, но `CreateHostBuilder` вызывает `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="7570d-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="7570d-123">Если приложение использует Entity Framework Core, не изменяйте имя или сигнатуру метода `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7570d-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="7570d-124">[Инструменты Entity Framework Core](/ef/core/miscellaneous/cli/) ищут метод `CreateHostBuilder`, который настраивает узел без необходимости запускать приложение.</span><span class="sxs-lookup"><span data-stu-id="7570d-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="7570d-125">Подробные сведения см. в статье [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation) (Создание экземпляра DbContext во время разработки).</span><span class="sxs-lookup"><span data-stu-id="7570d-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="7570d-126">Параметры построителя по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7570d-126">Default builder settings</span></span> 

<span data-ttu-id="7570d-127">Метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="7570d-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="7570d-128">В качестве [корневого каталога содержимого](xref:fundamentals/index#content-root) задает путь, возвращенный методом <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="7570d-129">Загружает конфигурацию узла из:</span><span class="sxs-lookup"><span data-stu-id="7570d-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="7570d-130">Переменные среды с префиксом "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="7570d-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="7570d-131">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="7570d-131">Command-line arguments.</span></span>
* <span data-ttu-id="7570d-132">Загружает конфигурацию приложения из:</span><span class="sxs-lookup"><span data-stu-id="7570d-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="7570d-133">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="7570d-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="7570d-134">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="7570d-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="7570d-135">[диспетчер секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development`;</span><span class="sxs-lookup"><span data-stu-id="7570d-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="7570d-136">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="7570d-136">Environment variables.</span></span>
  * <span data-ttu-id="7570d-137">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="7570d-137">Command-line arguments.</span></span>
* <span data-ttu-id="7570d-138">Добавляет следующие [регистраторы](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="7570d-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="7570d-139">Консоль</span><span class="sxs-lookup"><span data-stu-id="7570d-139">Console</span></span>
  * <span data-ttu-id="7570d-140">Отладка</span><span class="sxs-lookup"><span data-stu-id="7570d-140">Debug</span></span>
  * <span data-ttu-id="7570d-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="7570d-141">EventSource</span></span>
  * <span data-ttu-id="7570d-142">Журнал событий (только при запуске в Windows)</span><span class="sxs-lookup"><span data-stu-id="7570d-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="7570d-143">Включает [проверку области](xref:fundamentals/dependency-injection#scope-validation) и [проверку зависимостей](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild), если это среда разработки.</span><span class="sxs-lookup"><span data-stu-id="7570d-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="7570d-144">Метод `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="7570d-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="7570d-145">Загружает конфигурацию узла из переменных среды с префиксом "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="7570d-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="7570d-146">Задает сервер [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает его с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="7570d-147">Параметры сервера Kestrel по умолчанию см. в разделе <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="7570d-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="7570d-148">Добавляет [ПО промежуточного слоя фильтрации узлов](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="7570d-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="7570d-149">Добавляет [ПО промежуточного слоя переадресованных заголовков](xref:host-and-deploy/proxy-load-balancer#forwarded-headers), если ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span><span class="sxs-lookup"><span data-stu-id="7570d-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="7570d-150">Обеспечивает интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="7570d-150">Enables IIS integration.</span></span> <span data-ttu-id="7570d-151">Параметры IIS по умолчанию см. в разделе <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="7570d-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="7570d-152">Разделы [Параметры для всех типов приложений](#settings-for-all-app-types) и [Параметры для веб-приложений](#settings-for-web-apps) далее в этой статье описывают, как переопределить параметры построителя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7570d-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="7570d-153">Платформенные службы</span><span class="sxs-lookup"><span data-stu-id="7570d-153">Framework-provided services</span></span>

<span data-ttu-id="7570d-154">Службы, которые регистрируются автоматически, включают следующее:</span><span class="sxs-lookup"><span data-stu-id="7570d-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="7570d-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7570d-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="7570d-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="7570d-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="7570d-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="7570d-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="7570d-158">Дополнительные сведения о службах, предоставляемых платформой, см. в разделе <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="7570d-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="7570d-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7570d-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="7570d-160">Внедрите <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (прежнее название — `IApplicationLifetime`) в любой класс для выполнения задач после запуска и корректного завершения работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="7570d-161">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов обработчика событий запуска и завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="7570d-162">Этот интерфейс также включает метод `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="7570d-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="7570d-163">Ниже приведен пример реализации `IHostedService`, которая регистрирует события `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="7570d-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="7570d-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="7570d-164">IHostLifetime</span></span>

<span data-ttu-id="7570d-165">Реализация <xref:Microsoft.Extensions.Hosting.IHostLifetime> контролирует, когда узел запускается и останавливается.</span><span class="sxs-lookup"><span data-stu-id="7570d-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="7570d-166">Используется последняя зарегистрированная реализация.</span><span class="sxs-lookup"><span data-stu-id="7570d-166">The last implementation registered is used.</span></span>

<span data-ttu-id="7570d-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> — это реализация `IHostLifetime` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7570d-167"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="7570d-168">`ConsoleLifetime`.</span><span class="sxs-lookup"><span data-stu-id="7570d-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="7570d-169">прослушивает сигналы CTRL + C/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-169">listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="7570d-170">разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="7570d-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="7570d-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="7570d-171">IHostEnvironment</span></span>

<span data-ttu-id="7570d-172">Внедряет службу <xref:Microsoft.Extensions.Hosting.IHostEnvironment> в класс, чтобы получить следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="7570d-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="7570d-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="7570d-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="7570d-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="7570d-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="7570d-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="7570d-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="7570d-176">Веб-приложения реализуют интерфейс `IWebHostEnvironment`, который наследует `IHostEnvironment` и добавляет:</span><span class="sxs-lookup"><span data-stu-id="7570d-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds:</span></span>

* [<span data-ttu-id="7570d-177">WebRootPath</span><span class="sxs-lookup"><span data-stu-id="7570d-177">WebRootPath</span></span>](#webroot)

## <a name="host-configuration"></a><span data-ttu-id="7570d-178">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="7570d-178">Host configuration</span></span>

<span data-ttu-id="7570d-179">Конфигурация узла используется для свойств реализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="7570d-179">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="7570d-180">Конфигурация узла доступна из [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) внутри <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-180">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="7570d-181">После `ConfigureAppConfiguration` `HostBuilderContext.Configuration` заменяется конфигурацией приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-181">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="7570d-182">Чтобы добавить конфигурацию узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7570d-182">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="7570d-183">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-183">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7570d-184">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="7570d-184">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="7570d-185">Поставщик переменных среды с префиксом `DOTNET_` и аргументы командной строки включены в CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="7570d-185">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="7570d-186">Для веб-приложений добавляется поставщик переменных среды с префиксом `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="7570d-186">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="7570d-187">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="7570d-187">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="7570d-188">Например, значение переменной среды для `ASPNETCORE_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="7570d-188">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="7570d-189">В следующем примере создается конфигурация узла:</span><span class="sxs-lookup"><span data-stu-id="7570d-189">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="7570d-190">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="7570d-190">App configuration</span></span>

<span data-ttu-id="7570d-191">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="7570d-191">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="7570d-192">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-192">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="7570d-193">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="7570d-193">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="7570d-194">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и как служба из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7570d-194">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="7570d-195">Конфигурация узла также добавляется к конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-195">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="7570d-196">Дополнительные сведения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="7570d-196">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="7570d-197">Параметры для всех типов приложений</span><span class="sxs-lookup"><span data-stu-id="7570d-197">Settings for all app types</span></span>

<span data-ttu-id="7570d-198">В этом разделе перечислены параметры узла, которые применяются к рабочим нагрузкам HTTP и остальным.</span><span class="sxs-lookup"><span data-stu-id="7570d-198">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="7570d-199">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="7570d-199">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="7570d-200">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="7570d-200">ApplicationName</span></span>

<span data-ttu-id="7570d-201">Свойство [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-201">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="7570d-202">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="7570d-202">**Key**: applicationName</span></span>  
<span data-ttu-id="7570d-203">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-203">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-204">**По умолчанию**: Имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-204">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="7570d-205">**Переменная среды**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="7570d-205">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="7570d-206">Чтобы задать это значение, используйте переменную среды.</span><span class="sxs-lookup"><span data-stu-id="7570d-206">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="7570d-207">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="7570d-207">ContentRootPath</span></span>

<span data-ttu-id="7570d-208">Свойство [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) определяет, где узел начинает поиск файлов содержимого.</span><span class="sxs-lookup"><span data-stu-id="7570d-208">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="7570d-209">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="7570d-209">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="7570d-210">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="7570d-210">**Key**: contentRoot</span></span>  
<span data-ttu-id="7570d-211">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-211">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-212">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-212">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="7570d-213">**Переменная среды**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="7570d-213">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="7570d-214">Чтобы задать это значение, используйте переменную среды или вызов `UseContentRoot` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="7570d-214">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="7570d-215">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="7570d-215">For more information, see:</span></span>

* [<span data-ttu-id="7570d-216">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="7570d-216">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="7570d-217">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="7570d-217">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="7570d-218">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="7570d-218">EnvironmentName</span></span>

<span data-ttu-id="7570d-219">Свойству [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) может быть присвоено любое значение.</span><span class="sxs-lookup"><span data-stu-id="7570d-219">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="7570d-220">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="7570d-220">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7570d-221">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="7570d-221">Values aren't case sensitive.</span></span>

<span data-ttu-id="7570d-222">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="7570d-222">**Key**: environment</span></span>  
<span data-ttu-id="7570d-223">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-223">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-224">**По умолчанию**: Рабочие</span><span class="sxs-lookup"><span data-stu-id="7570d-224">**Default**: Production</span></span>  
<span data-ttu-id="7570d-225">**Переменная среды**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="7570d-225">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="7570d-226">Чтобы задать это значение, используйте переменную среды или вызов `UseEnvironment` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="7570d-226">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="7570d-227">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="7570d-227">ShutdownTimeout</span></span>

<span data-ttu-id="7570d-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-228">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="7570d-229">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="7570d-229">The default value is five seconds.</span></span>  <span data-ttu-id="7570d-230">Во время ожидания узел:</span><span class="sxs-lookup"><span data-stu-id="7570d-230">During the timeout period, the host:</span></span>

* <span data-ttu-id="7570d-231">Активирует [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="7570d-231">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="7570d-232">Пытается остановить размещенные службы, записывая в журнал ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="7570d-232">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="7570d-233">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-233">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="7570d-234">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="7570d-234">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="7570d-235">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="7570d-235">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="7570d-236">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="7570d-236">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="7570d-237">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="7570d-237">**Type**: *int*</span></span>  
<span data-ttu-id="7570d-238">**По умолчанию**: 5 секунд **Переменная среды**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="7570d-238">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="7570d-239">Чтобы задать это значение, используйте переменную среды или настройте `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="7570d-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="7570d-240">В следующем примере устанавливается время ожидания в 20 секунд:</span><span class="sxs-lookup"><span data-stu-id="7570d-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="7570d-241">Параметры для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="7570d-241">Settings for web apps</span></span>

<span data-ttu-id="7570d-242">Некоторые параметры узла применяются только к рабочим нагрузкам HTTP.</span><span class="sxs-lookup"><span data-stu-id="7570d-242">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="7570d-243">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="7570d-243">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="7570d-244">Методы расширения в `IWebHostBuilder` доступны для этих параметров.</span><span class="sxs-lookup"><span data-stu-id="7570d-244">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="7570d-245">В примерах кода, которые показывают, как вызывать методы расширения, предполагается, что `webBuilder` является экземпляром `IWebHostBuilder`, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="7570d-245">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="7570d-246">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="7570d-246">CaptureStartupErrors</span></span>

<span data-ttu-id="7570d-247">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-247">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="7570d-248">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="7570d-248">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="7570d-249">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="7570d-249">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="7570d-250">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="7570d-250">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7570d-251">**По умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="7570d-251">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="7570d-252">**Переменная среды**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="7570d-252">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="7570d-253">Чтобы задать это значение, используйте конфигурацию или вызов `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="7570d-253">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="7570d-254">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="7570d-254">DetailedErrors</span></span>

<span data-ttu-id="7570d-255">Если этот параметр включен или если среда имеет значение `Development`, приложение перехватывает подробные ошибки.</span><span class="sxs-lookup"><span data-stu-id="7570d-255">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="7570d-256">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="7570d-256">**Key**: detailedErrors</span></span>  
<span data-ttu-id="7570d-257">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="7570d-257">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7570d-258">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="7570d-258">**Default**: false</span></span>  
<span data-ttu-id="7570d-259">**Переменная среды**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="7570d-259">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="7570d-260">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7570d-260">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="7570d-261">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="7570d-261">HostingStartupAssemblies</span></span>

<span data-ttu-id="7570d-262">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="7570d-262">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="7570d-263">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-263">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="7570d-264">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="7570d-264">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="7570d-265">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="7570d-265">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="7570d-266">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-266">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-267">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="7570d-267">**Default**: Empty string</span></span>  
<span data-ttu-id="7570d-268">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="7570d-268">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="7570d-269">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7570d-269">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="7570d-270">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="7570d-270">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="7570d-271">Разделенная точками с запятой строка начальных сборок размещения, которые необходимо исключить при запуске.</span><span class="sxs-lookup"><span data-stu-id="7570d-271">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="7570d-272">**Ключ**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="7570d-272">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="7570d-273">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-273">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-274">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="7570d-274">**Default**: Empty string</span></span>  
<span data-ttu-id="7570d-275">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="7570d-275">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="7570d-276">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7570d-276">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="7570d-277">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="7570d-277">HTTPS_Port</span></span>

<span data-ttu-id="7570d-278">Порт перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="7570d-278">The HTTPS redirect port.</span></span> <span data-ttu-id="7570d-279">Используется при [принудительном применении HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="7570d-279">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="7570d-280">**Ключ**: https_port</span><span class="sxs-lookup"><span data-stu-id="7570d-280">**Key**: https_port</span></span>  
<span data-ttu-id="7570d-281">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-281">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-282">**По умолчанию**: значение по умолчанию не задано.</span><span class="sxs-lookup"><span data-stu-id="7570d-282">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="7570d-283">**Переменная среды**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="7570d-283">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="7570d-284">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7570d-284">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="7570d-285">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="7570d-285">PreferHostingUrls</span></span>

<span data-ttu-id="7570d-286">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `IWebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="7570d-286">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="7570d-287">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="7570d-287">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="7570d-288">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="7570d-288">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7570d-289">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="7570d-289">**Default**: true</span></span>  
<span data-ttu-id="7570d-290">**Переменная среды**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="7570d-290">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="7570d-291">Чтобы задать это значение, используйте переменную среды или вызов `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="7570d-291">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="7570d-292">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7570d-292">PreventHostingStartup</span></span>

<span data-ttu-id="7570d-293">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-293">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="7570d-294">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="7570d-294">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="7570d-295">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="7570d-295">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="7570d-296">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="7570d-296">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="7570d-297">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="7570d-297">**Default**: false</span></span>  
<span data-ttu-id="7570d-298">**Переменная среды**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="7570d-298">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="7570d-299">Чтобы задать это значение, используйте переменную среды или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="7570d-299">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="7570d-300">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="7570d-300">StartupAssembly</span></span>

<span data-ttu-id="7570d-301">Сборка, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="7570d-301">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="7570d-302">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="7570d-302">**Key**: startupAssembly</span></span>  
<span data-ttu-id="7570d-303">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-303">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-304">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="7570d-304">**Default**: The app's assembly</span></span>  
<span data-ttu-id="7570d-305">**Переменная среды**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="7570d-305">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="7570d-306">Чтобы задать это значение, используйте переменную среды или вызов `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="7570d-306">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="7570d-307">`UseStartup` может использовать имя сборки (`string`) или тип (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="7570d-307">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="7570d-308">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="7570d-308">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="7570d-309">URL-адреса</span><span class="sxs-lookup"><span data-stu-id="7570d-309">URLs</span></span>

<span data-ttu-id="7570d-310">Разделенный точками с запятой список IP-адресов или адресов узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="7570d-310">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="7570d-311">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="7570d-311">For example, `http://localhost:123`.</span></span> <span data-ttu-id="7570d-312">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="7570d-312">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="7570d-313">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="7570d-313">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="7570d-314">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="7570d-314">Supported formats vary among servers.</span></span>

<span data-ttu-id="7570d-315">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="7570d-315">**Key**: urls</span></span>  
<span data-ttu-id="7570d-316">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-316">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-317">**Значения по умолчанию**: `http://localhost:5000` и `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="7570d-317">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="7570d-318">**Переменная среды**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="7570d-318">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="7570d-319">Чтобы задать это значение, используйте переменную среды или вызов `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="7570d-319">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="7570d-320">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="7570d-320">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="7570d-321">Дополнительные сведения можно найти по адресу: <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="7570d-321">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="7570d-322">WebRoot</span><span class="sxs-lookup"><span data-stu-id="7570d-322">WebRoot</span></span>

<span data-ttu-id="7570d-323">Относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-323">The relative path to the app's static assets.</span></span>

<span data-ttu-id="7570d-324">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="7570d-324">**Key**: webroot</span></span>  
<span data-ttu-id="7570d-325">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-325">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-326">**По умолчанию**: Значение по умолчанию — `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="7570d-326">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="7570d-327">Наличие пути *{корневой_каталог_содержимого}/wwwroot* обязательно.</span><span class="sxs-lookup"><span data-stu-id="7570d-327">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="7570d-328">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="7570d-328">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="7570d-329">**Переменная среды**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="7570d-329">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="7570d-330">Чтобы задать это значение, используйте переменную среды или вызов `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="7570d-330">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="7570d-331">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="7570d-331">For more information, see:</span></span>

* [<span data-ttu-id="7570d-332">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="7570d-332">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="7570d-333">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="7570d-333">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="7570d-334">Управление временем существования узла</span><span class="sxs-lookup"><span data-stu-id="7570d-334">Manage the host lifetime</span></span>

<span data-ttu-id="7570d-335">Вызывает методы в реализации <xref:Microsoft.Extensions.Hosting.IHost> для запуска и остановки приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-335">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="7570d-336">Эти методы влияют на все реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, зарегистрированные в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="7570d-336">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="7570d-337">Выполнить</span><span class="sxs-lookup"><span data-stu-id="7570d-337">Run</span></span>

<span data-ttu-id="7570d-338">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="7570d-338"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="7570d-339">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-339">RunAsync</span></span>

<span data-ttu-id="7570d-340">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-340"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="7570d-341">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-341">RunConsoleAsync</span></span>

<span data-ttu-id="7570d-342">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы CTRL + C/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-342"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="7570d-343">Начало</span><span class="sxs-lookup"><span data-stu-id="7570d-343">Start</span></span>

<span data-ttu-id="7570d-344">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="7570d-344"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="7570d-345">StartAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-345">StartAsync</span></span>

<span data-ttu-id="7570d-346">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает узел и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-346"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="7570d-347">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале `StartAsync`, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="7570d-347"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="7570d-348">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="7570d-348">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="7570d-349">StopAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-349">StopAsync</span></span>

<span data-ttu-id="7570d-350">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="7570d-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="7570d-351">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="7570d-351">WaitForShutdown</span></span>

<span data-ttu-id="7570d-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> блокирует вызывающий поток до завершения работы, активированного IHostLifetime, например, через CTRL + C/SIGINT или SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="7570d-352"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="7570d-353">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-353">WaitForShutdownAsync</span></span>

<span data-ttu-id="7570d-354">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="7570d-355">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="7570d-355">External control</span></span>

<span data-ttu-id="7570d-356">Прямое управление временем существования узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="7570d-356">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7570d-357">Приложения ASP.NET Core настраивают и запускают узел.</span><span class="sxs-lookup"><span data-stu-id="7570d-357">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="7570d-358">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="7570d-358">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="7570d-359">В этой статье рассматривается универсальный узел ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), используемый для приложений, которые не обрабатывают запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="7570d-359">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="7570d-360">Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-360">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="7570d-361">Обмен сообщениями, фоновые задачи и другие рабочие нагрузки, не связанные с HTTP, используют перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведение журналов.</span><span class="sxs-lookup"><span data-stu-id="7570d-361">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="7570d-362">Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением.</span><span class="sxs-lookup"><span data-stu-id="7570d-362">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="7570d-363">Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="7570d-363">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="7570d-364">Универсальный узел заменит веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".</span><span class="sxs-lookup"><span data-stu-id="7570d-364">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="7570d-365">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="7570d-365">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="7570d-366">При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*.</span><span class="sxs-lookup"><span data-stu-id="7570d-366">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="7570d-367">Не выполняйте пример в `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="7570d-367">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="7570d-368">Настройка консоли в Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="7570d-368">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="7570d-369">Откройте файл *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="7570d-369">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="7570d-370">В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**.</span><span class="sxs-lookup"><span data-stu-id="7570d-370">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="7570d-371">Измените значение на `externalTerminal` или `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="7570d-371">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="7570d-372">Вступление</span><span class="sxs-lookup"><span data-stu-id="7570d-372">Introduction</span></span>

<span data-ttu-id="7570d-373">Библиотека универсального узла находится в пространстве имен <xref:Microsoft.Extensions.Hosting> и включена в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="7570d-373">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="7570d-374">Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="7570d-374">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="7570d-375"><xref:Microsoft.Extensions.Hosting.IHostedService> — это точка входа для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="7570d-375"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="7570d-376">Каждая реализация <xref:Microsoft.Extensions.Hosting.IHostedService> выполняется в порядке [регистрации служб в ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="7570d-376">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="7570d-377">Метод <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> вызывается в каждом <xref:Microsoft.Extensions.Hosting.IHostedService> при запуске узла, а метод <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.</span><span class="sxs-lookup"><span data-stu-id="7570d-377"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="7570d-378">Создание узла</span><span class="sxs-lookup"><span data-stu-id="7570d-378">Set up a host</span></span>

<span data-ttu-id="7570d-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:</span><span class="sxs-lookup"><span data-stu-id="7570d-379"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="7570d-380">Параметры</span><span class="sxs-lookup"><span data-stu-id="7570d-380">Options</span></span>

<span data-ttu-id="7570d-381"><xref:Microsoft.Extensions.Hosting.HostOptions> настраивает параметры для <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="7570d-381"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="7570d-382">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="7570d-382">Shutdown timeout</span></span>

<span data-ttu-id="7570d-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-383"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="7570d-384">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="7570d-384">The default value is five seconds.</span></span>

<span data-ttu-id="7570d-385">Следующий пример конфигурации в `Program.Main` увеличивает значение времени ожидания завершения работы по умолчанию с пяти до 20 секунд:</span><span class="sxs-lookup"><span data-stu-id="7570d-385">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureServices((hostContext, services) =>
    {
        services.Configure<HostOptions>(option =>
        {
            option.ShutdownTimeout = System.TimeSpan.FromSeconds(20);
        });
    })
    .Build();
```

## <a name="default-services"></a><span data-ttu-id="7570d-386">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="7570d-386">Default services</span></span>

<span data-ttu-id="7570d-387">Во время инициализации узла регистрируются следующие службы:</span><span class="sxs-lookup"><span data-stu-id="7570d-387">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="7570d-388">[Окружение](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="7570d-388">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="7570d-389">[Конфигурация](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="7570d-389">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="7570d-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="7570d-390"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="7570d-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="7570d-391"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="7570d-392">[Параметры](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="7570d-392">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="7570d-393">[Ведение журнала](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="7570d-393">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="7570d-394">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="7570d-394">Host configuration</span></span>

<span data-ttu-id="7570d-395">Существуют следующие подходы для создания конфигурации узла:</span><span class="sxs-lookup"><span data-stu-id="7570d-395">Host configuration is created by:</span></span>

* <span data-ttu-id="7570d-396">вызов методов расширения в <xref:Microsoft.Extensions.Hosting.IHostBuilder> для задания [корневой папки содержимого](#content-root) и [среды](#environment);</span><span class="sxs-lookup"><span data-stu-id="7570d-396">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="7570d-397">чтение конфигурации из поставщиков конфигурации в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-397">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="7570d-398">Методы расширения</span><span class="sxs-lookup"><span data-stu-id="7570d-398">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="7570d-399">Ключ приложения (имя)</span><span class="sxs-lookup"><span data-stu-id="7570d-399">Application key (name)</span></span>

<span data-ttu-id="7570d-400">Свойство [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-400">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="7570d-401">Чтобы явно задать значение, используйте [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey).</span><span class="sxs-lookup"><span data-stu-id="7570d-401">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="7570d-402">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="7570d-402">**Key**: applicationName</span></span>  
<span data-ttu-id="7570d-403">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-403">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-404">**По умолчанию**: имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-404">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="7570d-405">**Задается с помощью**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="7570d-405">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="7570d-406">**Переменная среды**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="7570d-406">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="7570d-407">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="7570d-407">Content root</span></span>

<span data-ttu-id="7570d-408">Этот параметр определяет, где узел начинает искать файлы содержимого.</span><span class="sxs-lookup"><span data-stu-id="7570d-408">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="7570d-409">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="7570d-409">**Key**: contentRoot</span></span>  
<span data-ttu-id="7570d-410">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-410">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-411">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-411">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="7570d-412">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="7570d-412">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="7570d-413">**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="7570d-413">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="7570d-414">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="7570d-414">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="7570d-415">Дополнительные сведения можно найти в разделе [Корневой каталог содержимого](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="7570d-415">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="7570d-416">Среда</span><span class="sxs-lookup"><span data-stu-id="7570d-416">Environment</span></span>

<span data-ttu-id="7570d-417">Задает [среду](xref:fundamentals/environments) приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-417">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="7570d-418">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="7570d-418">**Key**: environment</span></span>  
<span data-ttu-id="7570d-419">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="7570d-419">**Type**: *string*</span></span>  
<span data-ttu-id="7570d-420">**По умолчанию**: Рабочие</span><span class="sxs-lookup"><span data-stu-id="7570d-420">**Default**: Production</span></span>  
<span data-ttu-id="7570d-421">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="7570d-421">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="7570d-422">**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="7570d-422">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="7570d-423">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="7570d-423">The environment can be set to any value.</span></span> <span data-ttu-id="7570d-424">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="7570d-424">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="7570d-425">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="7570d-425">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="7570d-426">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="7570d-426">ConfigureHostConfiguration</span></span>

<span data-ttu-id="7570d-427">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-427"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="7570d-428">Конфигурация узла применяется для инициализации <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> в целях использования в процессе сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-428">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="7570d-429">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-429"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="7570d-430">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="7570d-430">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="7570d-431">Поставщики не включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7570d-431">No providers are included by default.</span></span> <span data-ttu-id="7570d-432">Необходимо явно указать поставщики конфигурации, требуемые приложению в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, в том числе:</span><span class="sxs-lookup"><span data-stu-id="7570d-432">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="7570d-433">конфигурация файла (например, из файла *hostsettings.json*);</span><span class="sxs-lookup"><span data-stu-id="7570d-433">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="7570d-434">конфигурация переменной среды;</span><span class="sxs-lookup"><span data-stu-id="7570d-434">Environment variable configuration.</span></span>
* <span data-ttu-id="7570d-435">конфигурация аргумента командной строки;</span><span class="sxs-lookup"><span data-stu-id="7570d-435">Command-line argument configuration.</span></span>
* <span data-ttu-id="7570d-436">другие необходимые поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="7570d-436">Any other required configuration providers.</span></span>

<span data-ttu-id="7570d-437">Конфигурация файла узла включается путем указания основного пути приложения с помощью `SetBasePath` и последующего вызова одного из [поставщиков конфигурации файла](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="7570d-437">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="7570d-438">В примере приложения используется JSON-файл *hostsettings.json* и вызывается метод <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> для использования параметров конфигурации узла файла.</span><span class="sxs-lookup"><span data-stu-id="7570d-438">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="7570d-439">Чтобы добавить [конфигурацию переменной среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider) узла, вызовите метод <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в построителе узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-439">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="7570d-440">`AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс.</span><span class="sxs-lookup"><span data-stu-id="7570d-440">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="7570d-441">В примере приложения используется префикс `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="7570d-441">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="7570d-442">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="7570d-442">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="7570d-443">После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="7570d-443">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="7570d-444">Во время разработки с использованием [Visual Studio](https://visualstudio.microsoft.com) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="7570d-444">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="7570d-445">В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки.</span><span class="sxs-lookup"><span data-stu-id="7570d-445">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="7570d-446">Дополнительные сведения можно найти по адресу: <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7570d-446">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="7570d-447">[Конфигурация командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) добавляется путем вызова метода <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-447">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="7570d-448">Конфигурация командной строки добавляется в последнюю очередь, чтобы разрешить аргументам командной строки переопределять конфигурацию, предоставленную предыдущими поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="7570d-448">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="7570d-449">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7570d-449">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="7570d-450">Дополнительную конфигурацию можно указать с помощью ключей [applicationName](#application-key-name) и [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="7570d-450">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="7570d-451">Пример конфигурации `HostBuilder` с использованием <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="7570d-451">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="7570d-452">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="7570d-452">ConfigureAppConfiguration</span></span>

<span data-ttu-id="7570d-453">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в реализации <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7570d-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="7570d-454">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="7570d-455">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-455"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="7570d-456">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="7570d-456">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="7570d-457">Конфигурация, созданная с помощью <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и в <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-457">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="7570d-458">Конфигурация приложения автоматически получает конфигурацию узла, предоставленную с помощью [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="7570d-458">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="7570d-459">Пример конфигурации приложения с использованием <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="7570d-459">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="7570d-460">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="7570d-460">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="7570d-461">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="7570d-461">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="7570d-462">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="7570d-462">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="7570d-463">Чтобы переместить файлы параметров в выходной каталог, укажите файлы параметров как [элементы проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="7570d-463">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="7570d-464">Пример приложения перемещает свои файлы параметров приложения JSON и *hostsettings.json* со следующим элементом `<Content>`:</span><span class="sxs-lookup"><span data-stu-id="7570d-464">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="7570d-465">Для таких методов расширения конфигурации, как <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> и <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>, необходимы дополнительные пакеты NuGet, такие как [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) и [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="7570d-465">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="7570d-466">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), эти пакеты нужно добавить в проект в дополнение к основному пакету [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration).</span><span class="sxs-lookup"><span data-stu-id="7570d-466">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="7570d-467">Дополнительные сведения можно найти по адресу: <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="7570d-467">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="7570d-468">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="7570d-468">ConfigureServices</span></span>

<span data-ttu-id="7570d-469">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="7570d-470">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-470"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="7570d-471">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="7570d-471">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="7570d-472">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7570d-472">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="7570d-473">[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="7570d-473">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="7570d-474">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="7570d-474">ConfigureLogging</span></span>

<span data-ttu-id="7570d-475">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> добавляет делегат для настройки указанного интерфейса <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7570d-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="7570d-476">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-476"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="7570d-477">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="7570d-477">UseConsoleLifetime</span></span>

<span data-ttu-id="7570d-478">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> прослушивает сигналы CTRL + C/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="7570d-479">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="7570d-479"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="7570d-480">Класс <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> предварительно зарегистрирован и является реализацией времени жизни по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="7570d-480"><xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="7570d-481">Используется то время жизни, которое зарегистрировано последним.</span><span class="sxs-lookup"><span data-stu-id="7570d-481">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="7570d-482">Конфигурация контейнера</span><span class="sxs-lookup"><span data-stu-id="7570d-482">Container configuration</span></span>

<span data-ttu-id="7570d-483">Для поддержки подключения к другим контейнерам узел может принимать <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="7570d-483">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="7570d-484">Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7570d-484">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="7570d-485">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-485">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="7570d-486">Пользовательская конфигурация контейнера управляется методом <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-486">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="7570d-487">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="7570d-488">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="7570d-488"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="7570d-489">Создание контейнера службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="7570d-489">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="7570d-490">Настройка фабрики контейнера службы:</span><span class="sxs-lookup"><span data-stu-id="7570d-490">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="7570d-491">Использование фабрики и настройка пользовательского контейнера служб для приложения:</span><span class="sxs-lookup"><span data-stu-id="7570d-491">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="7570d-492">Расширение среды</span><span class="sxs-lookup"><span data-stu-id="7570d-492">Extensibility</span></span>

<span data-ttu-id="7570d-493">Расширяемость узла выполняется с помощью методов расширения класса <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="7570d-493">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="7570d-494">В следующем примере показано, как метод расширения расширяет реализацию класса <xref:Microsoft.Extensions.Hosting.IHostBuilder> с помощью класса [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks), пример которого приведен в статье <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="7570d-494">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="7570d-495">Приложение устанавливает метод расширения `UseHostedService`, чтобы зарегистрировать размещенную службу, переданную в `T`:</span><span class="sxs-lookup"><span data-stu-id="7570d-495">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

```csharp
using System;
using Microsoft.Extensions.DependencyInjection;
using Microsoft.Extensions.Hosting;

public static class Extensions
{
    public static IHostBuilder UseHostedService<T>(this IHostBuilder hostBuilder)
        where T : class, IHostedService, IDisposable
    {
        return hostBuilder.ConfigureServices(services =>
            services.AddHostedService<T>());
    }
}
```

## <a name="manage-the-host"></a><span data-ttu-id="7570d-496">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="7570d-496">Manage the host</span></span>

<span data-ttu-id="7570d-497">Реализация <xref:Microsoft.Extensions.Hosting.IHost> отвечает за запуск и остановку реализаций <xref:Microsoft.Extensions.Hosting.IHostedService>, которые зарегистрированы в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="7570d-497">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="7570d-498">Выполнить</span><span class="sxs-lookup"><span data-stu-id="7570d-498">Run</span></span>

<span data-ttu-id="7570d-499">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:</span><span class="sxs-lookup"><span data-stu-id="7570d-499"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        host.Run();
    }
}
```

### <a name="runasync"></a><span data-ttu-id="7570d-500">RunAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-500">RunAsync</span></span>

<span data-ttu-id="7570d-501">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершение работы:</span><span class="sxs-lookup"><span data-stu-id="7570d-501"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        await host.RunAsync();
    }
}
```

### <a name="runconsoleasync"></a><span data-ttu-id="7570d-502">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-502">RunConsoleAsync</span></span>

<span data-ttu-id="7570d-503">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы CTRL + C/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-503"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var hostBuilder = new HostBuilder();

        await hostBuilder.RunConsoleAsync();
    }
}
```

### <a name="start-and-stopasync"></a><span data-ttu-id="7570d-504">Start и StopAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-504">Start and StopAsync</span></span>

<span data-ttu-id="7570d-505">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="7570d-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="7570d-506">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="7570d-506"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            await host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

### <a name="startasync-and-stopasync"></a><span data-ttu-id="7570d-507">StartAsync и StopAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-507">StartAsync and StopAsync</span></span>

<span data-ttu-id="7570d-508">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="7570d-508"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="7570d-509">Метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> останавливает приложение.</span><span class="sxs-lookup"><span data-stu-id="7570d-509"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.StopAsync();
        }
    }
}
```

### <a name="waitforshutdown"></a><span data-ttu-id="7570d-510">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="7570d-510">WaitForShutdown</span></span>

<span data-ttu-id="7570d-511">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> запускается с помощью <xref:Microsoft.Extensions.Hosting.IHostLifetime>, например <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (прослушивает CTRL + C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="7570d-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as <xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime> (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="7570d-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> вызывает <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-512"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public void Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            host.Start();

            host.WaitForShutdown();
        }
    }
}
```

### <a name="waitforshutdownasync"></a><span data-ttu-id="7570d-513">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="7570d-513">WaitForShutdownAsync</span></span>

<span data-ttu-id="7570d-514">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="7570d-514"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

```csharp
public class Program
{
    public static async Task Main(string[] args)
    {
        var host = new HostBuilder()
            .Build();

        using (host)
        {
            await host.StartAsync();

            await host.WaitForShutdownAsync();
        }

    }
}
```

### <a name="external-control"></a><span data-ttu-id="7570d-515">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="7570d-515">External control</span></span>

<span data-ttu-id="7570d-516">Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="7570d-516">External control of the host can be achieved using methods that can be called externally:</span></span>

```csharp
public class Program
{
    private IHost _host;

    public Program()
    {
        _host = new HostBuilder()
            .Build();
    }

    public async Task StartAsync()
    {
        _host.StartAsync();
    }

    public async Task StopAsync()
    {
        using (_host)
        {
            await _host.StopAsync(TimeSpan.FromSeconds(5));
        }
    }
}
```

<span data-ttu-id="7570d-517">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="7570d-517"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="7570d-518">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="7570d-518">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="7570d-519">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="7570d-519">IHostingEnvironment interface</span></span>

<span data-ttu-id="7570d-520">Интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> предоставляет сведения о среде размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-520"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="7570d-521">Чтобы получить интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="7570d-521">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

```csharp
public class MyClass
{
    private readonly IHostingEnvironment _env;

    public MyClass(IHostingEnvironment env)
    {
        _env = env;
    }

    public void DoSomething()
    {
        var environmentName = _env.EnvironmentName;
    }
}
```

<span data-ttu-id="7570d-522">Дополнительные сведения можно найти по адресу: <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="7570d-522">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="7570d-523">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="7570d-523">IApplicationLifetime interface</span></span>

<span data-ttu-id="7570d-524">Интерфейс <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-524"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="7570d-525">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов <xref:System.Action>, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="7570d-525">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="7570d-526">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="7570d-526">Cancellation Token</span></span> | <span data-ttu-id="7570d-527">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="7570d-527">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="7570d-528">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="7570d-528">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="7570d-529">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-529">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="7570d-530">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="7570d-530">All requests should be processed.</span></span> <span data-ttu-id="7570d-531">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="7570d-531">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="7570d-532">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="7570d-532">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="7570d-533">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="7570d-533">Requests may still be processing.</span></span> <span data-ttu-id="7570d-534">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="7570d-534">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="7570d-535">Конструктор внедряет службу <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> в любой класс.</span><span class="sxs-lookup"><span data-stu-id="7570d-535">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="7570d-536">[Пример приложения](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию <xref:Microsoft.Extensions.Hosting.IHostedService>) для регистрации событий.</span><span class="sxs-lookup"><span data-stu-id="7570d-536">The [sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="7570d-537">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="7570d-537">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="7570d-538">Метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="7570d-538"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="7570d-539">Следующий класс использует <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="7570d-539">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

```csharp
public class MyClass
{
    private readonly IApplicationLifetime _appLifetime;

    public MyClass(IApplicationLifetime appLifetime)
    {
        _appLifetime = appLifetime;
    }

    public void Shutdown()
    {
        _appLifetime.StopApplication();
    }
}
```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="7570d-540">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7570d-540">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
