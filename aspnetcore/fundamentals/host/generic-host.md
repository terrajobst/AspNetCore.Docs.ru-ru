---
title: Универсальный узел .NET
author: rick-anderson
description: Сведения об универсальном узле .NET Core, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/23/2020
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0f8f03dabf65f2cbfe4c41d36b02a25d7902cefb
ms.sourcegitcommit: 91dc1dd3d055b4c7d7298420927b3fd161067c64
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/24/2020
ms.locfileid: "80219224"
---
# <a name="net-generic-host"></a><span data-ttu-id="0681f-103">Универсальный узел .NET</span><span class="sxs-lookup"><span data-stu-id="0681f-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-5.0"

<span data-ttu-id="0681f-104">В этой статье описывается универсальный узел .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) и приводятся рекомендации о том, как его использовать.</span><span class="sxs-lookup"><span data-stu-id="0681f-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="0681f-105">Что такое узел?</span><span class="sxs-lookup"><span data-stu-id="0681f-105">What's a host?</span></span>

<span data-ttu-id="0681f-106">*Узел* — это объект, который инкапсулирует все ресурсы приложения, такие как:</span><span class="sxs-lookup"><span data-stu-id="0681f-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="0681f-107">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="0681f-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="0681f-108">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="0681f-108">Logging</span></span>
* <span data-ttu-id="0681f-109">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="0681f-109">Configuration</span></span>
* <span data-ttu-id="0681f-110">Реализации `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="0681f-110">`IHostedService` implementations</span></span>

<span data-ttu-id="0681f-111">После запуска узла он вызывает `IHostedService.StartAsync` в каждой реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, которую найдет в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="0681f-112">В веб-приложении одна из реализаций `IHostedService` является веб-службой, которая запускает [реализацию сервера HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="0681f-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="0681f-113">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="0681f-114">В версиях ASP.NET Core до 3.0 [веб-узел](xref:fundamentals/host/web-host) используется для рабочих нагрузок HTTP.</span><span class="sxs-lookup"><span data-stu-id="0681f-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="0681f-115">Веб-узел больше не рекомендуется для веб-приложений и доступен только для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="0681f-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0681f-116">Создание узла</span><span class="sxs-lookup"><span data-stu-id="0681f-116">Set up a host</span></span>

<span data-ttu-id="0681f-117">Узел обычно настраивается, собирается и выполняется кодом в классе `Program`.</span><span class="sxs-lookup"><span data-stu-id="0681f-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="0681f-118">Метод `Main`:</span><span class="sxs-lookup"><span data-stu-id="0681f-118">The `Main` method:</span></span>

* <span data-ttu-id="0681f-119">Вызывает метод `CreateHostBuilder` для создания и настройки объекта построителя.</span><span class="sxs-lookup"><span data-stu-id="0681f-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="0681f-120">Вызывает методы `Build` и `Run` в объекте построителя.</span><span class="sxs-lookup"><span data-stu-id="0681f-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="0681f-121">Вот код *Program.cs* для рабочей нагрузки, отличной от HTTP, с одной реализацией `IHostedService`, добавленной в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="0681f-122">При использовании рабочей нагрузки HTTP метод `Main` остается прежним, но `CreateHostBuilder` вызывает `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="0681f-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="0681f-123">Если приложение использует Entity Framework Core, не изменяйте имя или сигнатуру метода `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="0681f-124">[Инструменты Entity Framework Core](/ef/core/miscellaneous/cli/) ищут метод `CreateHostBuilder`, который настраивает узел без необходимости запускать приложение.</span><span class="sxs-lookup"><span data-stu-id="0681f-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="0681f-125">Подробные сведения см. в статье [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation) (Создание экземпляра DbContext во время разработки).</span><span class="sxs-lookup"><span data-stu-id="0681f-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="0681f-126">Параметры построителя по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0681f-126">Default builder settings</span></span>

<span data-ttu-id="0681f-127">Метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="0681f-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="0681f-128">В качестве [корневого каталога содержимого](xref:fundamentals/index#content-root) задает путь, возвращенный методом <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="0681f-129">Загружает конфигурацию узла из:</span><span class="sxs-lookup"><span data-stu-id="0681f-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="0681f-130">Переменные среды с префиксом `DOTNET_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-130">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="0681f-131">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="0681f-131">Command-line arguments.</span></span>
* <span data-ttu-id="0681f-132">Загружает конфигурацию приложения из:</span><span class="sxs-lookup"><span data-stu-id="0681f-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="0681f-133">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="0681f-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="0681f-134">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="0681f-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0681f-135">[диспетчер секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development`;</span><span class="sxs-lookup"><span data-stu-id="0681f-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="0681f-136">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-136">Environment variables.</span></span>
  * <span data-ttu-id="0681f-137">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="0681f-137">Command-line arguments.</span></span>
* <span data-ttu-id="0681f-138">Добавляет следующие [регистраторы](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="0681f-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="0681f-139">Консоль</span><span class="sxs-lookup"><span data-stu-id="0681f-139">Console</span></span>
  * <span data-ttu-id="0681f-140">Отладка</span><span class="sxs-lookup"><span data-stu-id="0681f-140">Debug</span></span>
  * <span data-ttu-id="0681f-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="0681f-141">EventSource</span></span>
  * <span data-ttu-id="0681f-142">Журнал событий (только при запуске в Windows)</span><span class="sxs-lookup"><span data-stu-id="0681f-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="0681f-143">Включает [проверку области](xref:fundamentals/dependency-injection#scope-validation) и [проверку зависимостей](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild), если это среда разработки.</span><span class="sxs-lookup"><span data-stu-id="0681f-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="0681f-144">Метод `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="0681f-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="0681f-145">Загружает конфигурацию узла из переменных среды с префиксом `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-145">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="0681f-146">Задает сервер [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает его с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="0681f-147">Параметры сервера Kestrel по умолчанию см. в разделе <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="0681f-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="0681f-148">Добавляет [ПО промежуточного слоя фильтрации узлов](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="0681f-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="0681f-149">Добавляет [Параметры ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer#forwarded-headers), если `ASPNETCORE_FORWARDEDHEADERS_ENABLED` равно `true`.</span><span class="sxs-lookup"><span data-stu-id="0681f-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="0681f-150">Обеспечивает интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="0681f-150">Enables IIS integration.</span></span> <span data-ttu-id="0681f-151">Параметры IIS по умолчанию см. в разделе <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="0681f-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="0681f-152">Разделы [Параметры для всех типов приложений](#settings-for-all-app-types) и [Параметры для веб-приложений](#settings-for-web-apps) далее в этой статье описывают, как переопределить параметры построителя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0681f-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="0681f-153">Платформенные службы</span><span class="sxs-lookup"><span data-stu-id="0681f-153">Framework-provided services</span></span>

<span data-ttu-id="0681f-154">Следующие службы регистрируются автоматически.</span><span class="sxs-lookup"><span data-stu-id="0681f-154">The following services are registered automatically:</span></span>

* [<span data-ttu-id="0681f-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="0681f-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="0681f-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="0681f-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="0681f-158">Дополнительные сведения о службах, предоставляемых платформой, см. в разделе <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="0681f-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="0681f-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="0681f-160">Внедрите <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (прежнее название — `IApplicationLifetime`) в любой класс для выполнения задач после запуска и корректного завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="0681f-161">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов обработчика событий запуска и завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="0681f-162">Этот интерфейс также включает метод `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="0681f-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="0681f-163">Ниже приведен пример реализации `IHostedService`, которая регистрирует события `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="0681f-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="0681f-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-164">IHostLifetime</span></span>

<span data-ttu-id="0681f-165">Реализация <xref:Microsoft.Extensions.Hosting.IHostLifetime> контролирует, когда узел запускается и останавливается.</span><span class="sxs-lookup"><span data-stu-id="0681f-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="0681f-166">Используется последняя зарегистрированная реализация.</span><span class="sxs-lookup"><span data-stu-id="0681f-166">The last implementation registered is used.</span></span>

<span data-ttu-id="0681f-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` — это реализация `IHostLifetime` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0681f-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="0681f-168">`ConsoleLifetime`.</span><span class="sxs-lookup"><span data-stu-id="0681f-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="0681f-169">Прослушивает сигналы <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-169">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="0681f-170">разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="0681f-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="0681f-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="0681f-171">IHostEnvironment</span></span>

<span data-ttu-id="0681f-172">Внедряет службу <xref:Microsoft.Extensions.Hosting.IHostEnvironment> в класс, чтобы получить сведения о следующих параметрах.</span><span class="sxs-lookup"><span data-stu-id="0681f-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="0681f-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="0681f-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="0681f-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="0681f-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="0681f-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="0681f-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="0681f-176">Веб-приложения реализуют интерфейс `IWebHostEnvironment`, который наследует `IHostEnvironment` и добавляет [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="0681f-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="0681f-177">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="0681f-177">Host configuration</span></span>

<span data-ttu-id="0681f-178">Конфигурация узла используется для свойств реализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="0681f-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="0681f-179">Конфигурация узла доступна из [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) внутри <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="0681f-180">После `ConfigureAppConfiguration``HostBuilderContext.Configuration` заменяется конфигурацией приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="0681f-181">Чтобы добавить конфигурацию узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="0681f-182">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="0681f-183">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="0681f-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="0681f-184">Поставщик переменных среды с префиксом `DOTNET_` и аргументы командной строки включены в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-184">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0681f-185">Для веб-приложений добавляется поставщик переменных среды с префиксом `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="0681f-186">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="0681f-187">Например, значение переменной среды для `ASPNETCORE_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="0681f-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="0681f-188">В следующем примере создается конфигурация узла:</span><span class="sxs-lookup"><span data-stu-id="0681f-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="0681f-189">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="0681f-189">App configuration</span></span>

<span data-ttu-id="0681f-190">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="0681f-191">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="0681f-192">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="0681f-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="0681f-193">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и как служба из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="0681f-194">Конфигурация узла также добавляется к конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="0681f-195">Дополнительные сведения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0681f-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="0681f-196">Параметры для всех типов приложений</span><span class="sxs-lookup"><span data-stu-id="0681f-196">Settings for all app types</span></span>

<span data-ttu-id="0681f-197">В этом разделе перечислены параметры узла, которые применяются к рабочим нагрузкам HTTP и остальным.</span><span class="sxs-lookup"><span data-stu-id="0681f-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="0681f-198">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="0681f-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="0681f-199">ApplicationName</span></span>

<span data-ttu-id="0681f-200">Свойство [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="0681f-201">**Ключ**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="0681f-201">**Key**: `applicationName`</span></span>  
<span data-ttu-id="0681f-202">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-202">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-203">**По умолчанию**: Имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="0681f-204">**Переменная среды**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="0681f-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="0681f-205">Чтобы задать это значение, используйте переменную среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="0681f-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="0681f-206">ContentRootPath</span></span>

<span data-ttu-id="0681f-207">Свойство [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) определяет, где узел начинает поиск файлов содержимого.</span><span class="sxs-lookup"><span data-stu-id="0681f-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="0681f-208">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="0681f-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="0681f-209">**Ключ**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="0681f-209">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="0681f-210">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-210">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-211">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="0681f-212">**Переменная среды**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0681f-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="0681f-213">Чтобы задать это значение, используйте переменную среды или вызов `UseContentRoot` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0681f-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="0681f-214">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="0681f-214">For more information, see:</span></span>

* [<span data-ttu-id="0681f-215">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="0681f-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="0681f-216">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="0681f-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="0681f-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="0681f-217">EnvironmentName</span></span>

<span data-ttu-id="0681f-218">Свойству [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) может быть присвоено любое значение.</span><span class="sxs-lookup"><span data-stu-id="0681f-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="0681f-219">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="0681f-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0681f-220">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="0681f-220">Values aren't case-sensitive.</span></span>

<span data-ttu-id="0681f-221">**Ключ**: `environment`</span><span class="sxs-lookup"><span data-stu-id="0681f-221">**Key**: `environment`</span></span>  
<span data-ttu-id="0681f-222">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-222">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-223">**По умолчанию**: `Production`</span><span class="sxs-lookup"><span data-stu-id="0681f-223">**Default**: `Production`</span></span>  
<span data-ttu-id="0681f-224">**Переменная среды**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0681f-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="0681f-225">Чтобы задать это значение, используйте переменную среды или вызов `UseEnvironment` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0681f-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="0681f-226">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="0681f-226">ShutdownTimeout</span></span>

<span data-ttu-id="0681f-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="0681f-228">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="0681f-228">The default value is five seconds.</span></span>  <span data-ttu-id="0681f-229">Во время ожидания узел:</span><span class="sxs-lookup"><span data-stu-id="0681f-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="0681f-230">Активирует [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="0681f-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="0681f-231">Пытается остановить размещенные службы, записывая в журнал ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="0681f-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="0681f-232">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="0681f-233">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="0681f-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="0681f-234">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="0681f-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="0681f-235">**Ключ**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="0681f-235">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="0681f-236">**Тип**: `int`</span><span class="sxs-lookup"><span data-stu-id="0681f-236">**Type**: `int`</span></span>  
<span data-ttu-id="0681f-237">**По умолчанию**: 5 секунд</span><span class="sxs-lookup"><span data-stu-id="0681f-237">**Default**: 5 seconds</span></span>  
<span data-ttu-id="0681f-238">**Переменная среды**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0681f-238">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0681f-239">Чтобы задать это значение, используйте переменную среды или настройте `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="0681f-239">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="0681f-240">В следующем примере устанавливается время ожидания в 20 секунд:</span><span class="sxs-lookup"><span data-stu-id="0681f-240">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

### <a name="disable-app-configuration-reload-on-change"></a><span data-ttu-id="0681f-241">Отключение перезагрузки конфигурации приложения при изменении</span><span class="sxs-lookup"><span data-stu-id="0681f-241">Disable app configuration reload on change</span></span>

<span data-ttu-id="0681f-242">[По умолчанию](xref:fundamentals/configuration/index#default) при изменении файла выполняется перезагрузка *appsettings.json* и *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="0681f-242">By [default](xref:fundamentals/configuration/index#default), *appsettings.json* and *appsettings.{Environment}.json* are reloaded when the file changes.</span></span> <span data-ttu-id="0681f-243">Чтобы отключить эту функцию перезагрузки в ASP.NET Core 5.0, предварительная версия 3 или более поздняя, присвойте ключу `hostBuilder:reloadConfigOnChange` значение `false`.</span><span class="sxs-lookup"><span data-stu-id="0681f-243">To disable this reload behavior in ASP.NET Core 5.0 Preview 3 or later, set the `hostBuilder:reloadConfigOnChange` key to `false`.</span></span>

<span data-ttu-id="0681f-244">**Ключ**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="0681f-244">**Key**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="0681f-245">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-245">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-246">**По умолчанию**: `true`</span><span class="sxs-lookup"><span data-stu-id="0681f-246">**Default**: `true`</span></span>  
<span data-ttu-id="0681f-247">**Аргумент командной строки**: `hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="0681f-247">**Command-line argument**: `hostBuilder:reloadConfigOnChange`</span></span>  
<span data-ttu-id="0681f-248">**Переменная среды**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span><span class="sxs-lookup"><span data-stu-id="0681f-248">**Environment variable**: `<PREFIX_>hostBuilder:reloadConfigOnChange`</span></span>

> [!WARNING]
> <span data-ttu-id="0681f-249">Двоеточие (`:`) не работает в качестве разделителя с иерархическими ключами переменных среды на всех платформах.</span><span class="sxs-lookup"><span data-stu-id="0681f-249">The colon (`:`) separator doesn't work with environment variable hierarchical keys on all platforms.</span></span> <span data-ttu-id="0681f-250">Дополнительную информацию см. в разделе [Переменные среды](xref:fundamentals/configuration/index#environment-variables).</span><span class="sxs-lookup"><span data-stu-id="0681f-250">For more information, see [Environment variables](xref:fundamentals/configuration/index#environment-variables).</span></span>

## <a name="settings-for-web-apps"></a><span data-ttu-id="0681f-251">Параметры для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="0681f-251">Settings for web apps</span></span>

<span data-ttu-id="0681f-252">Некоторые параметры узла применяются только к рабочим нагрузкам HTTP.</span><span class="sxs-lookup"><span data-stu-id="0681f-252">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="0681f-253">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-253">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="0681f-254">Методы расширения в `IWebHostBuilder` доступны для этих параметров.</span><span class="sxs-lookup"><span data-stu-id="0681f-254">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="0681f-255">В примерах кода, которые показывают, как вызывать методы расширения, предполагается, что `webBuilder` является экземпляром `IWebHostBuilder`, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0681f-255">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="0681f-256">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0681f-256">CaptureStartupErrors</span></span>

<span data-ttu-id="0681f-257">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-257">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0681f-258">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="0681f-258">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="0681f-259">**Ключ**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0681f-259">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="0681f-260">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-260">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-261">**По умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="0681f-261">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0681f-262">**Переменная среды**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0681f-262">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0681f-263">Чтобы задать это значение, используйте конфигурацию или вызов `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="0681f-263">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="0681f-264">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="0681f-264">DetailedErrors</span></span>

<span data-ttu-id="0681f-265">Если этот параметр включен или если среда имеет значение `Development`, приложение перехватывает подробные ошибки.</span><span class="sxs-lookup"><span data-stu-id="0681f-265">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="0681f-266">**Ключ**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="0681f-266">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="0681f-267">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-267">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-268">**По умолчанию**: `false`</span><span class="sxs-lookup"><span data-stu-id="0681f-268">**Default**: `false`</span></span>  
<span data-ttu-id="0681f-269">**Переменная среды**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0681f-269">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="0681f-270">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-270">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="0681f-271">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0681f-271">HostingStartupAssemblies</span></span>

<span data-ttu-id="0681f-272">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="0681f-272">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="0681f-273">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-273">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0681f-274">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="0681f-274">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="0681f-275">**Ключ**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="0681f-275">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="0681f-276">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-276">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-277">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="0681f-277">**Default**: Empty string</span></span>  
<span data-ttu-id="0681f-278">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0681f-278">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0681f-279">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-279">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="0681f-280">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="0681f-280">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="0681f-281">Разделенная точками с запятой строка начальных сборок размещения, которые необходимо исключить при запуске.</span><span class="sxs-lookup"><span data-stu-id="0681f-281">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="0681f-282">**Ключ**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="0681f-282">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="0681f-283">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-283">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-284">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="0681f-284">**Default**: Empty string</span></span>  
<span data-ttu-id="0681f-285">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0681f-285">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="0681f-286">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-286">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="0681f-287">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="0681f-287">HTTPS_Port</span></span>

<span data-ttu-id="0681f-288">Порт перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0681f-288">The HTTPS redirect port.</span></span> <span data-ttu-id="0681f-289">Используется при [принудительном применении HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="0681f-289">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="0681f-290">**Ключ**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="0681f-290">**Key**: `https_port`</span></span>  
<span data-ttu-id="0681f-291">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-291">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-292">**По умолчанию**: значение по умолчанию не задано.</span><span class="sxs-lookup"><span data-stu-id="0681f-292">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="0681f-293">**Переменная среды**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="0681f-293">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="0681f-294">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-294">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="0681f-295">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0681f-295">PreferHostingUrls</span></span>

<span data-ttu-id="0681f-296">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `IWebHostBuilder`, вместо URL-адресов, настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="0681f-296">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0681f-297">**Ключ**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0681f-297">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="0681f-298">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-298">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-299">**По умолчанию**: `true`</span><span class="sxs-lookup"><span data-stu-id="0681f-299">**Default**: `true`</span></span>  
<span data-ttu-id="0681f-300">**Переменная среды**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0681f-300">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="0681f-301">Чтобы задать это значение, используйте переменную среды или вызов `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="0681f-301">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="0681f-302">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0681f-302">PreventHostingStartup</span></span>

<span data-ttu-id="0681f-303">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-303">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0681f-304">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0681f-304">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="0681f-305">**Ключ**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="0681f-305">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="0681f-306">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-306">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-307">**По умолчанию**: `false`</span><span class="sxs-lookup"><span data-stu-id="0681f-307">**Default**: `false`</span></span>  
<span data-ttu-id="0681f-308">**Переменная среды**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0681f-308">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="0681f-309">Чтобы задать это значение, используйте переменную среды или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-309">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="0681f-310">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="0681f-310">StartupAssembly</span></span>

<span data-ttu-id="0681f-311">Сборка, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0681f-311">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0681f-312">**Ключ**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="0681f-312">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="0681f-313">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-313">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-314">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="0681f-314">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0681f-315">**Переменная среды**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0681f-315">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0681f-316">Чтобы задать это значение, используйте переменную среды или вызов `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="0681f-316">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="0681f-317">`UseStartup` может использовать имя сборки (`string`) или тип (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="0681f-317">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="0681f-318">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="0681f-318">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="0681f-319">URL-адреса</span><span class="sxs-lookup"><span data-stu-id="0681f-319">URLs</span></span>

<span data-ttu-id="0681f-320">Разделенный точками с запятой список IP-адресов или адресов узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="0681f-320">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="0681f-321">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="0681f-321">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0681f-322">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="0681f-322">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0681f-323">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="0681f-323">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0681f-324">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="0681f-324">Supported formats vary among servers.</span></span>

<span data-ttu-id="0681f-325">**Ключ**: `urls`</span><span class="sxs-lookup"><span data-stu-id="0681f-325">**Key**: `urls`</span></span>  
<span data-ttu-id="0681f-326">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-326">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-327">**Значения по умолчанию**: `http://localhost:5000` и `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="0681f-327">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="0681f-328">**Переменная среды**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="0681f-328">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="0681f-329">Чтобы задать это значение, используйте переменную среды или вызов `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="0681f-329">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="0681f-330">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="0681f-330">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0681f-331">Для получения дополнительной информации см. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0681f-331">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="0681f-332">WebRoot</span><span class="sxs-lookup"><span data-stu-id="0681f-332">WebRoot</span></span>

<span data-ttu-id="0681f-333">Относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-333">The relative path to the app's static assets.</span></span>

<span data-ttu-id="0681f-334">**Ключ**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="0681f-334">**Key**: `webroot`</span></span>  
<span data-ttu-id="0681f-335">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-335">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-336">**По умолчанию**: Значение по умолчанию — `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="0681f-336">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="0681f-337">Наличие пути *{корневой_каталог_содержимого}/wwwroot* обязательно.</span><span class="sxs-lookup"><span data-stu-id="0681f-337">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="0681f-338">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="0681f-338">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="0681f-339">**Переменная среды**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0681f-339">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="0681f-340">Чтобы задать это значение, используйте переменную среды или вызов `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="0681f-340">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="0681f-341">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="0681f-341">For more information, see:</span></span>

* [<span data-ttu-id="0681f-342">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="0681f-342">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="0681f-343">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="0681f-343">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="0681f-344">Управление временем существования узла</span><span class="sxs-lookup"><span data-stu-id="0681f-344">Manage the host lifetime</span></span>

<span data-ttu-id="0681f-345">Вызывает методы в реализации <xref:Microsoft.Extensions.Hosting.IHost> для запуска и остановки приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-345">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="0681f-346">Эти методы влияют на все реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, зарегистрированные в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="0681f-346">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="0681f-347">Выполнить</span><span class="sxs-lookup"><span data-stu-id="0681f-347">Run</span></span>

<span data-ttu-id="0681f-348">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="0681f-348"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="0681f-349">RunAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-349">RunAsync</span></span>

<span data-ttu-id="0681f-350">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-350"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="0681f-351">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-351">RunConsoleAsync</span></span>

<span data-ttu-id="0681f-352">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-352"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="0681f-353">Начало</span><span class="sxs-lookup"><span data-stu-id="0681f-353">Start</span></span>

<span data-ttu-id="0681f-354">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="0681f-354"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="0681f-355">StartAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-355">StartAsync</span></span>

<span data-ttu-id="0681f-356">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает узел и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-356"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="0681f-357">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале `StartAsync`, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="0681f-357"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="0681f-358">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="0681f-358">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="0681f-359">StopAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-359">StopAsync</span></span>

<span data-ttu-id="0681f-360">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="0681f-360"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="0681f-361">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="0681f-361">WaitForShutdown</span></span>

<span data-ttu-id="0681f-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> блокирует вызывающий поток до завершения работы, активированного IHostLifetime, например через <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="0681f-362"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="0681f-363">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-363">WaitForShutdownAsync</span></span>

<span data-ttu-id="0681f-364">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-364"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="0681f-365">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="0681f-365">External control</span></span>

<span data-ttu-id="0681f-366">Прямое управление временем существования узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="0681f-366">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

::: moniker range=">= aspnetcore-3.0 <= aspnetcore-3.1"

<span data-ttu-id="0681f-367">В этой статье описывается универсальный узел .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) и приводятся рекомендации о том, как его использовать.</span><span class="sxs-lookup"><span data-stu-id="0681f-367">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="0681f-368">Что такое узел?</span><span class="sxs-lookup"><span data-stu-id="0681f-368">What's a host?</span></span>

<span data-ttu-id="0681f-369">*Узел* — это объект, который инкапсулирует все ресурсы приложения, такие как:</span><span class="sxs-lookup"><span data-stu-id="0681f-369">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="0681f-370">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="0681f-370">Dependency injection (DI)</span></span>
* <span data-ttu-id="0681f-371">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="0681f-371">Logging</span></span>
* <span data-ttu-id="0681f-372">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="0681f-372">Configuration</span></span>
* <span data-ttu-id="0681f-373">Реализации `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="0681f-373">`IHostedService` implementations</span></span>

<span data-ttu-id="0681f-374">После запуска узла он вызывает `IHostedService.StartAsync` в каждой реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, которую найдет в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-374">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="0681f-375">В веб-приложении одна из реализаций `IHostedService` является веб-службой, которая запускает [реализацию сервера HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="0681f-375">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="0681f-376">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-376">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="0681f-377">В версиях ASP.NET Core до 3.0 [веб-узел](xref:fundamentals/host/web-host) используется для рабочих нагрузок HTTP.</span><span class="sxs-lookup"><span data-stu-id="0681f-377">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="0681f-378">Веб-узел больше не рекомендуется для веб-приложений и доступен только для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="0681f-378">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0681f-379">Создание узла</span><span class="sxs-lookup"><span data-stu-id="0681f-379">Set up a host</span></span>

<span data-ttu-id="0681f-380">Узел обычно настраивается, собирается и выполняется кодом в классе `Program`.</span><span class="sxs-lookup"><span data-stu-id="0681f-380">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="0681f-381">Метод `Main`:</span><span class="sxs-lookup"><span data-stu-id="0681f-381">The `Main` method:</span></span>

* <span data-ttu-id="0681f-382">Вызывает метод `CreateHostBuilder` для создания и настройки объекта построителя.</span><span class="sxs-lookup"><span data-stu-id="0681f-382">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="0681f-383">Вызывает методы `Build` и `Run` в объекте построителя.</span><span class="sxs-lookup"><span data-stu-id="0681f-383">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="0681f-384">Вот код *Program.cs* для рабочей нагрузки, отличной от HTTP, с одной реализацией `IHostedService`, добавленной в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-384">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="0681f-385">При использовании рабочей нагрузки HTTP метод `Main` остается прежним, но `CreateHostBuilder` вызывает `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="0681f-385">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="0681f-386">Если приложение использует Entity Framework Core, не изменяйте имя или сигнатуру метода `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-386">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="0681f-387">[Инструменты Entity Framework Core](/ef/core/miscellaneous/cli/) ищут метод `CreateHostBuilder`, который настраивает узел без необходимости запускать приложение.</span><span class="sxs-lookup"><span data-stu-id="0681f-387">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="0681f-388">Подробные сведения см. в статье [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation) (Создание экземпляра DbContext во время разработки).</span><span class="sxs-lookup"><span data-stu-id="0681f-388">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="0681f-389">Параметры построителя по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0681f-389">Default builder settings</span></span>

<span data-ttu-id="0681f-390">Метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="0681f-390">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="0681f-391">В качестве [корневого каталога содержимого](xref:fundamentals/index#content-root) задает путь, возвращенный методом <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-391">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="0681f-392">Загружает конфигурацию узла из:</span><span class="sxs-lookup"><span data-stu-id="0681f-392">Loads host configuration from:</span></span>
  * <span data-ttu-id="0681f-393">Переменные среды с префиксом `DOTNET_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-393">Environment variables prefixed with `DOTNET_`.</span></span>
  * <span data-ttu-id="0681f-394">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="0681f-394">Command-line arguments.</span></span>
* <span data-ttu-id="0681f-395">Загружает конфигурацию приложения из:</span><span class="sxs-lookup"><span data-stu-id="0681f-395">Loads app configuration from:</span></span>
  * <span data-ttu-id="0681f-396">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="0681f-396">*appsettings.json*.</span></span>
  * <span data-ttu-id="0681f-397">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="0681f-397">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="0681f-398">[диспетчер секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development`;</span><span class="sxs-lookup"><span data-stu-id="0681f-398">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="0681f-399">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-399">Environment variables.</span></span>
  * <span data-ttu-id="0681f-400">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="0681f-400">Command-line arguments.</span></span>
* <span data-ttu-id="0681f-401">Добавляет следующие [регистраторы](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="0681f-401">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="0681f-402">Консоль</span><span class="sxs-lookup"><span data-stu-id="0681f-402">Console</span></span>
  * <span data-ttu-id="0681f-403">Отладка</span><span class="sxs-lookup"><span data-stu-id="0681f-403">Debug</span></span>
  * <span data-ttu-id="0681f-404">EventSource</span><span class="sxs-lookup"><span data-stu-id="0681f-404">EventSource</span></span>
  * <span data-ttu-id="0681f-405">Журнал событий (только при запуске в Windows)</span><span class="sxs-lookup"><span data-stu-id="0681f-405">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="0681f-406">Включает [проверку области](xref:fundamentals/dependency-injection#scope-validation) и [проверку зависимостей](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild), если это среда разработки.</span><span class="sxs-lookup"><span data-stu-id="0681f-406">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="0681f-407">Метод `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="0681f-407">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="0681f-408">Загружает конфигурацию узла из переменных среды с префиксом `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-408">Loads host configuration from environment variables prefixed with `ASPNETCORE_`.</span></span>
* <span data-ttu-id="0681f-409">Задает сервер [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает его с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-409">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="0681f-410">Параметры сервера Kestrel по умолчанию см. в разделе <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="0681f-410">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="0681f-411">Добавляет [ПО промежуточного слоя фильтрации узлов](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="0681f-411">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="0681f-412">Добавляет [Параметры ПО промежуточного слоя перенаправления заголовков](xref:host-and-deploy/proxy-load-balancer#forwarded-headers), если `ASPNETCORE_FORWARDEDHEADERS_ENABLED` равно `true`.</span><span class="sxs-lookup"><span data-stu-id="0681f-412">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if `ASPNETCORE_FORWARDEDHEADERS_ENABLED` equals `true`.</span></span>
* <span data-ttu-id="0681f-413">Обеспечивает интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="0681f-413">Enables IIS integration.</span></span> <span data-ttu-id="0681f-414">Параметры IIS по умолчанию см. в разделе <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="0681f-414">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="0681f-415">Разделы [Параметры для всех типов приложений](#settings-for-all-app-types) и [Параметры для веб-приложений](#settings-for-web-apps) далее в этой статье описывают, как переопределить параметры построителя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0681f-415">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="0681f-416">Платформенные службы</span><span class="sxs-lookup"><span data-stu-id="0681f-416">Framework-provided services</span></span>

<span data-ttu-id="0681f-417">Следующие службы регистрируются автоматически.</span><span class="sxs-lookup"><span data-stu-id="0681f-417">The following services are registered automatically:</span></span>

* [<span data-ttu-id="0681f-418">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-418">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="0681f-419">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-419">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="0681f-420">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="0681f-420">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="0681f-421">Дополнительные сведения о службах, предоставляемых платформой, см. в разделе <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="0681f-421">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="0681f-422">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-422">IHostApplicationLifetime</span></span>

<span data-ttu-id="0681f-423">Внедрите <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (прежнее название — `IApplicationLifetime`) в любой класс для выполнения задач после запуска и корректного завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-423">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="0681f-424">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов обработчика событий запуска и завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-424">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="0681f-425">Этот интерфейс также включает метод `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="0681f-425">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="0681f-426">Ниже приведен пример реализации `IHostedService`, которая регистрирует события `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="0681f-426">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="0681f-427">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-427">IHostLifetime</span></span>

<span data-ttu-id="0681f-428">Реализация <xref:Microsoft.Extensions.Hosting.IHostLifetime> контролирует, когда узел запускается и останавливается.</span><span class="sxs-lookup"><span data-stu-id="0681f-428">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="0681f-429">Используется последняя зарегистрированная реализация.</span><span class="sxs-lookup"><span data-stu-id="0681f-429">The last implementation registered is used.</span></span>

<span data-ttu-id="0681f-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` — это реализация `IHostLifetime` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0681f-430">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="0681f-431">`ConsoleLifetime`.</span><span class="sxs-lookup"><span data-stu-id="0681f-431">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="0681f-432">Прослушивает сигналы <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-432">Listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="0681f-433">разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="0681f-433">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="0681f-434">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="0681f-434">IHostEnvironment</span></span>

<span data-ttu-id="0681f-435">Внедряет службу <xref:Microsoft.Extensions.Hosting.IHostEnvironment> в класс, чтобы получить сведения о следующих параметрах.</span><span class="sxs-lookup"><span data-stu-id="0681f-435">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following settings:</span></span>

* [<span data-ttu-id="0681f-436">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="0681f-436">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="0681f-437">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="0681f-437">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="0681f-438">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="0681f-438">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="0681f-439">Веб-приложения реализуют интерфейс `IWebHostEnvironment`, который наследует `IHostEnvironment` и добавляет [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="0681f-439">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="0681f-440">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="0681f-440">Host configuration</span></span>

<span data-ttu-id="0681f-441">Конфигурация узла используется для свойств реализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="0681f-441">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="0681f-442">Конфигурация узла доступна из [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) внутри <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-442">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="0681f-443">После `ConfigureAppConfiguration``HostBuilderContext.Configuration` заменяется конфигурацией приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-443">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="0681f-444">Чтобы добавить конфигурацию узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-444">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="0681f-445">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-445">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="0681f-446">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="0681f-446">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="0681f-447">Поставщик переменных среды с префиксом `DOTNET_` и аргументы командной строки включены в `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-447">The environment variable provider with prefix `DOTNET_` and command-line arguments are included by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="0681f-448">Для веб-приложений добавляется поставщик переменных среды с префиксом `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-448">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="0681f-449">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-449">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="0681f-450">Например, значение переменной среды для `ASPNETCORE_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="0681f-450">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="0681f-451">В следующем примере создается конфигурация узла:</span><span class="sxs-lookup"><span data-stu-id="0681f-451">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="0681f-452">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="0681f-452">App configuration</span></span>

<span data-ttu-id="0681f-453">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="0681f-453">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="0681f-454">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-454">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="0681f-455">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="0681f-455">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="0681f-456">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и как служба из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-456">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="0681f-457">Конфигурация узла также добавляется к конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-457">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="0681f-458">Дополнительные сведения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0681f-458">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="0681f-459">Параметры для всех типов приложений</span><span class="sxs-lookup"><span data-stu-id="0681f-459">Settings for all app types</span></span>

<span data-ttu-id="0681f-460">В этом разделе перечислены параметры узла, которые применяются к рабочим нагрузкам HTTP и остальным.</span><span class="sxs-lookup"><span data-stu-id="0681f-460">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="0681f-461">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-461">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="0681f-462">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="0681f-462">ApplicationName</span></span>

<span data-ttu-id="0681f-463">Свойство [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-463">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="0681f-464">**Ключ**: `applicationName`</span><span class="sxs-lookup"><span data-stu-id="0681f-464">**Key**: `applicationName`</span></span>  
<span data-ttu-id="0681f-465">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-465">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-466">**По умолчанию**: Имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-466">**Default**: The name of the assembly that contains the app's entry point.</span></span>  
<span data-ttu-id="0681f-467">**Переменная среды**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="0681f-467">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="0681f-468">Чтобы задать это значение, используйте переменную среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-468">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="0681f-469">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="0681f-469">ContentRootPath</span></span>

<span data-ttu-id="0681f-470">Свойство [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) определяет, где узел начинает поиск файлов содержимого.</span><span class="sxs-lookup"><span data-stu-id="0681f-470">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="0681f-471">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="0681f-471">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="0681f-472">**Ключ**: `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="0681f-472">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="0681f-473">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-473">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-474">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-474">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="0681f-475">**Переменная среды**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="0681f-475">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="0681f-476">Чтобы задать это значение, используйте переменную среды или вызов `UseContentRoot` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0681f-476">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="0681f-477">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="0681f-477">For more information, see:</span></span>

* [<span data-ttu-id="0681f-478">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="0681f-478">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="0681f-479">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="0681f-479">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="0681f-480">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="0681f-480">EnvironmentName</span></span>

<span data-ttu-id="0681f-481">Свойству [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) может быть присвоено любое значение.</span><span class="sxs-lookup"><span data-stu-id="0681f-481">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="0681f-482">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="0681f-482">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0681f-483">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="0681f-483">Values aren't case-sensitive.</span></span>

<span data-ttu-id="0681f-484">**Ключ**: `environment`</span><span class="sxs-lookup"><span data-stu-id="0681f-484">**Key**: `environment`</span></span>  
<span data-ttu-id="0681f-485">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-485">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-486">**По умолчанию**: `Production`</span><span class="sxs-lookup"><span data-stu-id="0681f-486">**Default**: `Production`</span></span>  
<span data-ttu-id="0681f-487">**Переменная среды**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="0681f-487">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="0681f-488">Чтобы задать это значение, используйте переменную среды или вызов `UseEnvironment` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="0681f-488">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="0681f-489">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="0681f-489">ShutdownTimeout</span></span>

<span data-ttu-id="0681f-490">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-490">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="0681f-491">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="0681f-491">The default value is five seconds.</span></span>  <span data-ttu-id="0681f-492">Во время ожидания узел:</span><span class="sxs-lookup"><span data-stu-id="0681f-492">During the timeout period, the host:</span></span>

* <span data-ttu-id="0681f-493">Активирует [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="0681f-493">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="0681f-494">Пытается остановить размещенные службы, записывая в журнал ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="0681f-494">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="0681f-495">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-495">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="0681f-496">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="0681f-496">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="0681f-497">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="0681f-497">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="0681f-498">**Ключ**: `shutdownTimeoutSeconds`</span><span class="sxs-lookup"><span data-stu-id="0681f-498">**Key**: `shutdownTimeoutSeconds`</span></span>  
<span data-ttu-id="0681f-499">**Тип**: `int`</span><span class="sxs-lookup"><span data-stu-id="0681f-499">**Type**: `int`</span></span>  
<span data-ttu-id="0681f-500">**По умолчанию**: 5 секунд</span><span class="sxs-lookup"><span data-stu-id="0681f-500">**Default**: 5 seconds</span></span>  
<span data-ttu-id="0681f-501">**Переменная среды**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="0681f-501">**Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="0681f-502">Чтобы задать это значение, используйте переменную среды или настройте `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="0681f-502">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="0681f-503">В следующем примере устанавливается время ожидания в 20 секунд:</span><span class="sxs-lookup"><span data-stu-id="0681f-503">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="0681f-504">Параметры для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="0681f-504">Settings for web apps</span></span>

<span data-ttu-id="0681f-505">Некоторые параметры узла применяются только к рабочим нагрузкам HTTP.</span><span class="sxs-lookup"><span data-stu-id="0681f-505">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="0681f-506">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-506">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="0681f-507">Методы расширения в `IWebHostBuilder` доступны для этих параметров.</span><span class="sxs-lookup"><span data-stu-id="0681f-507">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="0681f-508">В примерах кода, которые показывают, как вызывать методы расширения, предполагается, что `webBuilder` является экземпляром `IWebHostBuilder`, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="0681f-508">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="0681f-509">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="0681f-509">CaptureStartupErrors</span></span>

<span data-ttu-id="0681f-510">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-510">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="0681f-511">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="0681f-511">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="0681f-512">**Ключ**: `captureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="0681f-512">**Key**: `captureStartupErrors`</span></span>  
<span data-ttu-id="0681f-513">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-513">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-514">**По умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="0681f-514">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="0681f-515">**Переменная среды**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="0681f-515">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="0681f-516">Чтобы задать это значение, используйте конфигурацию или вызов `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="0681f-516">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="0681f-517">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="0681f-517">DetailedErrors</span></span>

<span data-ttu-id="0681f-518">Если этот параметр включен или если среда имеет значение `Development`, приложение перехватывает подробные ошибки.</span><span class="sxs-lookup"><span data-stu-id="0681f-518">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="0681f-519">**Ключ**: `detailedErrors`</span><span class="sxs-lookup"><span data-stu-id="0681f-519">**Key**: `detailedErrors`</span></span>  
<span data-ttu-id="0681f-520">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-520">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-521">**По умолчанию**: `false`</span><span class="sxs-lookup"><span data-stu-id="0681f-521">**Default**: `false`</span></span>  
<span data-ttu-id="0681f-522">**Переменная среды**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="0681f-522">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="0681f-523">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-523">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="0681f-524">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="0681f-524">HostingStartupAssemblies</span></span>

<span data-ttu-id="0681f-525">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="0681f-525">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="0681f-526">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-526">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="0681f-527">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="0681f-527">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="0681f-528">**Ключ**: `hostingStartupAssemblies`</span><span class="sxs-lookup"><span data-stu-id="0681f-528">**Key**: `hostingStartupAssemblies`</span></span>  
<span data-ttu-id="0681f-529">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-529">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-530">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="0681f-530">**Default**: Empty string</span></span>  
<span data-ttu-id="0681f-531">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0681f-531">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="0681f-532">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-532">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="0681f-533">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="0681f-533">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="0681f-534">Разделенная точками с запятой строка начальных сборок размещения, которые необходимо исключить при запуске.</span><span class="sxs-lookup"><span data-stu-id="0681f-534">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="0681f-535">**Ключ**: `hostingStartupExcludeAssemblies`</span><span class="sxs-lookup"><span data-stu-id="0681f-535">**Key**: `hostingStartupExcludeAssemblies`</span></span>  
<span data-ttu-id="0681f-536">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-536">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-537">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="0681f-537">**Default**: Empty string</span></span>  
<span data-ttu-id="0681f-538">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="0681f-538">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="0681f-539">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-539">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="0681f-540">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="0681f-540">HTTPS_Port</span></span>

<span data-ttu-id="0681f-541">Порт перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="0681f-541">The HTTPS redirect port.</span></span> <span data-ttu-id="0681f-542">Используется при [принудительном применении HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="0681f-542">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="0681f-543">**Ключ**: `https_port`</span><span class="sxs-lookup"><span data-stu-id="0681f-543">**Key**: `https_port`</span></span>  
<span data-ttu-id="0681f-544">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-544">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-545">**По умолчанию**: значение по умолчанию не задано.</span><span class="sxs-lookup"><span data-stu-id="0681f-545">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="0681f-546">**Переменная среды**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="0681f-546">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="0681f-547">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-547">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="0681f-548">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="0681f-548">PreferHostingUrls</span></span>

<span data-ttu-id="0681f-549">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `IWebHostBuilder`, вместо URL-адресов, настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="0681f-549">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those URLs configured with the `IServer` implementation.</span></span>

<span data-ttu-id="0681f-550">**Ключ**: `preferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="0681f-550">**Key**: `preferHostingUrls`</span></span>  
<span data-ttu-id="0681f-551">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-551">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-552">**По умолчанию**: `true`</span><span class="sxs-lookup"><span data-stu-id="0681f-552">**Default**: `true`</span></span>  
<span data-ttu-id="0681f-553">**Переменная среды**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="0681f-553">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="0681f-554">Чтобы задать это значение, используйте переменную среды или вызов `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="0681f-554">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="0681f-555">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="0681f-555">PreventHostingStartup</span></span>

<span data-ttu-id="0681f-556">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-556">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="0681f-557">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0681f-557">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="0681f-558">**Ключ**: `preventHostingStartup`</span><span class="sxs-lookup"><span data-stu-id="0681f-558">**Key**: `preventHostingStartup`</span></span>  
<span data-ttu-id="0681f-559">**Тип**: `bool` (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="0681f-559">**Type**: `bool` (`true` or `1`)</span></span>  
<span data-ttu-id="0681f-560">**По умолчанию**: `false`</span><span class="sxs-lookup"><span data-stu-id="0681f-560">**Default**: `false`</span></span>  
<span data-ttu-id="0681f-561">**Переменная среды**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="0681f-561">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="0681f-562">Чтобы задать это значение, используйте переменную среды или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="0681f-562">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="0681f-563">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="0681f-563">StartupAssembly</span></span>

<span data-ttu-id="0681f-564">Сборка, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0681f-564">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="0681f-565">**Ключ**: `startupAssembly`</span><span class="sxs-lookup"><span data-stu-id="0681f-565">**Key**: `startupAssembly`</span></span>  
<span data-ttu-id="0681f-566">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-566">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-567">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="0681f-567">**Default**: The app's assembly</span></span>  
<span data-ttu-id="0681f-568">**Переменная среды**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="0681f-568">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="0681f-569">Чтобы задать это значение, используйте переменную среды или вызов `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="0681f-569">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="0681f-570">`UseStartup` может использовать имя сборки (`string`) или тип (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="0681f-570">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="0681f-571">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="0681f-571">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="0681f-572">URL-адреса</span><span class="sxs-lookup"><span data-stu-id="0681f-572">URLs</span></span>

<span data-ttu-id="0681f-573">Разделенный точками с запятой список IP-адресов или адресов узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="0681f-573">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="0681f-574">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="0681f-574">For example, `http://localhost:123`.</span></span> <span data-ttu-id="0681f-575">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="0681f-575">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="0681f-576">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="0681f-576">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="0681f-577">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="0681f-577">Supported formats vary among servers.</span></span>

<span data-ttu-id="0681f-578">**Ключ**: `urls`</span><span class="sxs-lookup"><span data-stu-id="0681f-578">**Key**: `urls`</span></span>  
<span data-ttu-id="0681f-579">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-579">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-580">**Значения по умолчанию**: `http://localhost:5000` и `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="0681f-580">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="0681f-581">**Переменная среды**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="0681f-581">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="0681f-582">Чтобы задать это значение, используйте переменную среды или вызов `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="0681f-582">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="0681f-583">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="0681f-583">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="0681f-584">Для получения дополнительной информации см. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="0681f-584">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="0681f-585">WebRoot</span><span class="sxs-lookup"><span data-stu-id="0681f-585">WebRoot</span></span>

<span data-ttu-id="0681f-586">Относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-586">The relative path to the app's static assets.</span></span>

<span data-ttu-id="0681f-587">**Ключ**: `webroot`</span><span class="sxs-lookup"><span data-stu-id="0681f-587">**Key**: `webroot`</span></span>  
<span data-ttu-id="0681f-588">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-588">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-589">**По умолчанию**: Значение по умолчанию — `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="0681f-589">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="0681f-590">Наличие пути *{корневой_каталог_содержимого}/wwwroot* обязательно.</span><span class="sxs-lookup"><span data-stu-id="0681f-590">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="0681f-591">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="0681f-591">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="0681f-592">**Переменная среды**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="0681f-592">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="0681f-593">Чтобы задать это значение, используйте переменную среды или вызов `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="0681f-593">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="0681f-594">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="0681f-594">For more information, see:</span></span>

* [<span data-ttu-id="0681f-595">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="0681f-595">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="0681f-596">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="0681f-596">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="0681f-597">Управление временем существования узла</span><span class="sxs-lookup"><span data-stu-id="0681f-597">Manage the host lifetime</span></span>

<span data-ttu-id="0681f-598">Вызывает методы в реализации <xref:Microsoft.Extensions.Hosting.IHost> для запуска и остановки приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-598">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="0681f-599">Эти методы влияют на все реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, зарегистрированные в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="0681f-599">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="0681f-600">Выполнить</span><span class="sxs-lookup"><span data-stu-id="0681f-600">Run</span></span>

<span data-ttu-id="0681f-601">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="0681f-601"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="0681f-602">RunAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-602">RunAsync</span></span>

<span data-ttu-id="0681f-603">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-603"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="0681f-604">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-604">RunConsoleAsync</span></span>

<span data-ttu-id="0681f-605">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-605"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="0681f-606">Начало</span><span class="sxs-lookup"><span data-stu-id="0681f-606">Start</span></span>

<span data-ttu-id="0681f-607">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="0681f-607"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="0681f-608">StartAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-608">StartAsync</span></span>

<span data-ttu-id="0681f-609">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает узел и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-609"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="0681f-610">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале `StartAsync`, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="0681f-610"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="0681f-611">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="0681f-611">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="0681f-612">StopAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-612">StopAsync</span></span>

<span data-ttu-id="0681f-613">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="0681f-613"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="0681f-614">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="0681f-614">WaitForShutdown</span></span>

<span data-ttu-id="0681f-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> блокирует вызывающий поток до завершения работы, активированного IHostLifetime, например через <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="0681f-615"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="0681f-616">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-616">WaitForShutdownAsync</span></span>

<span data-ttu-id="0681f-617">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-617"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="0681f-618">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="0681f-618">External control</span></span>

<span data-ttu-id="0681f-619">Прямое управление временем существования узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="0681f-619">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="0681f-620">Приложения ASP.NET Core настраивают и запускают узел.</span><span class="sxs-lookup"><span data-stu-id="0681f-620">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="0681f-621">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="0681f-621">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="0681f-622">В этой статье рассматривается универсальный узел ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), используемый для приложений, которые не обрабатывают запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="0681f-622">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="0681f-623">Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-623">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="0681f-624">Обмен сообщениями, фоновые задачи и другие рабочие нагрузки, не связанные с HTTP, используют перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведение журналов.</span><span class="sxs-lookup"><span data-stu-id="0681f-624">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="0681f-625">Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением.</span><span class="sxs-lookup"><span data-stu-id="0681f-625">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="0681f-626">Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="0681f-626">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="0681f-627">Универсальный узел заменит веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".</span><span class="sxs-lookup"><span data-stu-id="0681f-627">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="0681f-628">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="0681f-628">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="0681f-629">При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*.</span><span class="sxs-lookup"><span data-stu-id="0681f-629">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="0681f-630">Не выполняйте пример в `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="0681f-630">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="0681f-631">Настройка консоли в Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="0681f-631">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="0681f-632">Откройте файл *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="0681f-632">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="0681f-633">В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**.</span><span class="sxs-lookup"><span data-stu-id="0681f-633">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="0681f-634">Измените значение на `externalTerminal` или `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="0681f-634">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="0681f-635">Вступление</span><span class="sxs-lookup"><span data-stu-id="0681f-635">Introduction</span></span>

<span data-ttu-id="0681f-636">Библиотека универсального узла находится в пространстве имен <xref:Microsoft.Extensions.Hosting> и включена в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="0681f-636">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="0681f-637">Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="0681f-637">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="0681f-638"><xref:Microsoft.Extensions.Hosting.IHostedService> — это точка входа для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="0681f-638"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="0681f-639">Каждая реализация <xref:Microsoft.Extensions.Hosting.IHostedService> выполняется в порядке [регистрации служб в ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="0681f-639">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="0681f-640">Метод <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> вызывается в каждом <xref:Microsoft.Extensions.Hosting.IHostedService> при запуске узла, а метод <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.</span><span class="sxs-lookup"><span data-stu-id="0681f-640"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="0681f-641">Создание узла</span><span class="sxs-lookup"><span data-stu-id="0681f-641">Set up a host</span></span>

<span data-ttu-id="0681f-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:</span><span class="sxs-lookup"><span data-stu-id="0681f-642"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="0681f-643">Параметры</span><span class="sxs-lookup"><span data-stu-id="0681f-643">Options</span></span>

<span data-ttu-id="0681f-644"><xref:Microsoft.Extensions.Hosting.HostOptions> настраивает параметры для <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="0681f-644"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="0681f-645">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="0681f-645">Shutdown timeout</span></span>

<span data-ttu-id="0681f-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-646"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="0681f-647">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="0681f-647">The default value is five seconds.</span></span>

<span data-ttu-id="0681f-648">Следующий пример конфигурации в `Program.Main` увеличивает значение времени ожидания завершения работы по умолчанию с 5 до 20 секунд.</span><span class="sxs-lookup"><span data-stu-id="0681f-648">The following option configuration in `Program.Main` increases the default five-second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="0681f-649">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0681f-649">Default services</span></span>

<span data-ttu-id="0681f-650">Во время инициализации узла регистрируются следующие службы:</span><span class="sxs-lookup"><span data-stu-id="0681f-650">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="0681f-651">[Окружение](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="0681f-651">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="0681f-652">[Конфигурация](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="0681f-652">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="0681f-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="0681f-653"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="0681f-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="0681f-654"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="0681f-655">[Параметры](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="0681f-655">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="0681f-656">[Ведение журнала](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="0681f-656">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="0681f-657">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="0681f-657">Host configuration</span></span>

<span data-ttu-id="0681f-658">Существуют следующие подходы для создания конфигурации узла:</span><span class="sxs-lookup"><span data-stu-id="0681f-658">Host configuration is created by:</span></span>

* <span data-ttu-id="0681f-659">вызов методов расширения в <xref:Microsoft.Extensions.Hosting.IHostBuilder> для задания [корневой папки содержимого](#content-root) и [среды](#environment);</span><span class="sxs-lookup"><span data-stu-id="0681f-659">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="0681f-660">чтение конфигурации из поставщиков конфигурации в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-660">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="0681f-661">Методы расширения</span><span class="sxs-lookup"><span data-stu-id="0681f-661">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="0681f-662">Ключ приложения (имя)</span><span class="sxs-lookup"><span data-stu-id="0681f-662">Application key (name)</span></span>

<span data-ttu-id="0681f-663">Свойство [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-663">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="0681f-664">Чтобы явно задать значение, используйте [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey).</span><span class="sxs-lookup"><span data-stu-id="0681f-664">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="0681f-665">**Ключ:** `applicationName`</span><span class="sxs-lookup"><span data-stu-id="0681f-665">**Key**: `applicationName`</span></span>  
<span data-ttu-id="0681f-666">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-666">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-667">**По умолчанию**: имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-667">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="0681f-668">**Задается с помощью**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="0681f-668">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="0681f-669">**Переменная среды**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>`[необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="0681f-669">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="0681f-670">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="0681f-670">Content root</span></span>

<span data-ttu-id="0681f-671">Этот параметр определяет, где узел начинает искать файлы содержимого.</span><span class="sxs-lookup"><span data-stu-id="0681f-671">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="0681f-672">**Ключ:** `contentRoot`</span><span class="sxs-lookup"><span data-stu-id="0681f-672">**Key**: `contentRoot`</span></span>  
<span data-ttu-id="0681f-673">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-673">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-674">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-674">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="0681f-675">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="0681f-675">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="0681f-676">**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>`[необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="0681f-676">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="0681f-677">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="0681f-677">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="0681f-678">Дополнительные сведения можно найти в разделе [Корневой каталог содержимого](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="0681f-678">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="0681f-679">Среда</span><span class="sxs-lookup"><span data-stu-id="0681f-679">Environment</span></span>

<span data-ttu-id="0681f-680">Задает [среду](xref:fundamentals/environments) приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-680">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="0681f-681">**Ключ:** `environment`</span><span class="sxs-lookup"><span data-stu-id="0681f-681">**Key**: `environment`</span></span>  
<span data-ttu-id="0681f-682">**Тип**: `string`</span><span class="sxs-lookup"><span data-stu-id="0681f-682">**Type**: `string`</span></span>  
<span data-ttu-id="0681f-683">**По умолчанию**: `Production`</span><span class="sxs-lookup"><span data-stu-id="0681f-683">**Default**: `Production`</span></span>  
<span data-ttu-id="0681f-684">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="0681f-684">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="0681f-685">**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>`[необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="0681f-685">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="0681f-686">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="0681f-686">The environment can be set to any value.</span></span> <span data-ttu-id="0681f-687">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="0681f-687">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="0681f-688">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="0681f-688">Values aren't case-sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="0681f-689">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="0681f-689">ConfigureHostConfiguration</span></span>

<span data-ttu-id="0681f-690">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-690"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="0681f-691">Конфигурация узла применяется для инициализации <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> в целях использования в процессе сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-691">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="0681f-692">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-692"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="0681f-693">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="0681f-693">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="0681f-694">Поставщики не включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0681f-694">No providers are included by default.</span></span> <span data-ttu-id="0681f-695">Необходимо явно указать поставщики конфигурации, требуемые приложению в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, в том числе:</span><span class="sxs-lookup"><span data-stu-id="0681f-695">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="0681f-696">конфигурация файла (например, из файла *hostsettings.json*);</span><span class="sxs-lookup"><span data-stu-id="0681f-696">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="0681f-697">конфигурация переменной среды;</span><span class="sxs-lookup"><span data-stu-id="0681f-697">Environment variable configuration.</span></span>
* <span data-ttu-id="0681f-698">конфигурация аргумента командной строки;</span><span class="sxs-lookup"><span data-stu-id="0681f-698">Command-line argument configuration.</span></span>
* <span data-ttu-id="0681f-699">другие необходимые поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0681f-699">Any other required configuration providers.</span></span>

<span data-ttu-id="0681f-700">Конфигурация файла узла включается путем указания основного пути приложения с помощью `SetBasePath` и последующего вызова одного из [поставщиков конфигурации файла](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="0681f-700">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="0681f-701">В примере приложения используется JSON-файл *hostsettings.json* и вызывается метод <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> для использования параметров конфигурации узла файла.</span><span class="sxs-lookup"><span data-stu-id="0681f-701">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="0681f-702">Чтобы добавить [конфигурацию переменной среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider) узла, вызовите метод <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в построителе узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-702">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="0681f-703">`AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс.</span><span class="sxs-lookup"><span data-stu-id="0681f-703">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="0681f-704">В примере приложения используется префикс `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="0681f-704">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="0681f-705">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="0681f-705">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="0681f-706">После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="0681f-706">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="0681f-707">Во время разработки с использованием [Visual Studio](https://visualstudio.microsoft.com) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="0681f-707">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="0681f-708">В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки.</span><span class="sxs-lookup"><span data-stu-id="0681f-708">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="0681f-709">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0681f-709">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="0681f-710">[Конфигурация командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) добавляется путем вызова метода <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-710">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="0681f-711">Конфигурация командной строки добавляется в последнюю очередь, чтобы разрешить аргументам командной строки переопределять конфигурацию, предоставленную предыдущими поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="0681f-711">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="0681f-712">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0681f-712">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="0681f-713">Дополнительную конфигурацию можно указать с помощью ключей [applicationName](#application-key-name) и [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="0681f-713">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="0681f-714">Пример конфигурации `HostBuilder` с использованием <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="0681f-714">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="0681f-715">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="0681f-715">ConfigureAppConfiguration</span></span>

<span data-ttu-id="0681f-716">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в реализации <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0681f-716">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="0681f-717">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-717"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="0681f-718">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-718"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="0681f-719">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="0681f-719">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="0681f-720">Конфигурация, созданная с помощью <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и в <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-720">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="0681f-721">Конфигурация приложения автоматически получает конфигурацию узла, предоставленную с помощью [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="0681f-721">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="0681f-722">Пример конфигурации приложения с использованием <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="0681f-722">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="0681f-723">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="0681f-723">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="0681f-724">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="0681f-724">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="0681f-725">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="0681f-725">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="0681f-726">Чтобы переместить файлы параметров в выходной каталог, укажите файлы параметров как [элементы проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="0681f-726">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="0681f-727">Пример приложения перемещает свои файлы параметров приложения JSON и *hostsettings.json* со следующим элементом `<Content>`:</span><span class="sxs-lookup"><span data-stu-id="0681f-727">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="0681f-728">Для таких методов расширения конфигурации, как <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> и <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>, необходимы дополнительные пакеты NuGet, такие как [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) и [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="0681f-728">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="0681f-729">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), эти пакеты нужно добавить в проект в дополнение к основному пакету [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration).</span><span class="sxs-lookup"><span data-stu-id="0681f-729">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="0681f-730">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="0681f-730">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="0681f-731">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="0681f-731">ConfigureServices</span></span>

<span data-ttu-id="0681f-732">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-732"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="0681f-733">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-733"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="0681f-734">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="0681f-734">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="0681f-735">Для получения дополнительной информации см. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="0681f-735">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="0681f-736">[Пример приложения](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="0681f-736">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="0681f-737">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="0681f-737">ConfigureLogging</span></span>

<span data-ttu-id="0681f-738">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> добавляет делегат для настройки указанного интерфейса <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0681f-738"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="0681f-739">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-739"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="0681f-740">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-740">UseConsoleLifetime</span></span>

<span data-ttu-id="0681f-741">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> прослушивает сигналы <kbd>CTRL</kbd>+<kbd>C</kbd>//SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-741"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="0681f-742">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="0681f-742"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="0681f-743">Класс `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` предварительно зарегистрирован и является реализацией времени жизни по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0681f-743">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="0681f-744">Используется то время жизни, которое зарегистрировано последним.</span><span class="sxs-lookup"><span data-stu-id="0681f-744">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="0681f-745">Конфигурация контейнера</span><span class="sxs-lookup"><span data-stu-id="0681f-745">Container configuration</span></span>

<span data-ttu-id="0681f-746">Для поддержки подключения к другим контейнерам узел может принимать <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="0681f-746">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="0681f-747">Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="0681f-747">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="0681f-748">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-748">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="0681f-749">Пользовательская конфигурация контейнера управляется методом <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-749">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="0681f-750">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-750"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="0681f-751">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="0681f-751"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="0681f-752">Создание контейнера службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="0681f-752">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="0681f-753">Настройка фабрики контейнера службы:</span><span class="sxs-lookup"><span data-stu-id="0681f-753">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="0681f-754">Использование фабрики и настройка пользовательского контейнера служб для приложения:</span><span class="sxs-lookup"><span data-stu-id="0681f-754">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="0681f-755">Расширение среды</span><span class="sxs-lookup"><span data-stu-id="0681f-755">Extensibility</span></span>

<span data-ttu-id="0681f-756">Расширяемость узла выполняется с помощью методов расширения класса <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="0681f-756">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="0681f-757">В следующем примере показано, как метод расширения расширяет реализацию класса <xref:Microsoft.Extensions.Hosting.IHostBuilder> с помощью класса [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks), пример которого приведен в статье <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="0681f-757">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="0681f-758">Приложение устанавливает метод расширения `UseHostedService`, чтобы зарегистрировать размещенную службу, переданную в `T`:</span><span class="sxs-lookup"><span data-stu-id="0681f-758">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="0681f-759">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="0681f-759">Manage the host</span></span>

<span data-ttu-id="0681f-760">Реализация <xref:Microsoft.Extensions.Hosting.IHost> отвечает за запуск и остановку реализаций <xref:Microsoft.Extensions.Hosting.IHostedService>, которые зарегистрированы в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="0681f-760">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="0681f-761">Выполнить</span><span class="sxs-lookup"><span data-stu-id="0681f-761">Run</span></span>

<span data-ttu-id="0681f-762">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:</span><span class="sxs-lookup"><span data-stu-id="0681f-762"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="0681f-763">RunAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-763">RunAsync</span></span>

<span data-ttu-id="0681f-764">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершение работы:</span><span class="sxs-lookup"><span data-stu-id="0681f-764"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="0681f-765">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-765">RunConsoleAsync</span></span>

<span data-ttu-id="0681f-766">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-766"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="0681f-767">Start и StopAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-767">Start and StopAsync</span></span>

<span data-ttu-id="0681f-768">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="0681f-768"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="0681f-769">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="0681f-769"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="0681f-770">StartAsync и StopAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-770">StartAsync and StopAsync</span></span>

<span data-ttu-id="0681f-771">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="0681f-771"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="0681f-772">Метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> останавливает приложение.</span><span class="sxs-lookup"><span data-stu-id="0681f-772"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="0681f-773">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="0681f-773">WaitForShutdown</span></span>

<span data-ttu-id="0681f-774">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> запускается с помощью <xref:Microsoft.Extensions.Hosting.IHostLifetime>, например `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (прослушивает <kbd>CTRL</kbd>+<kbd>C</kbd>/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="0681f-774"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for <kbd>Ctrl</kbd>+<kbd>C</kbd>/SIGINT or SIGTERM).</span></span> <span data-ttu-id="0681f-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> вызывает <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-775"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="0681f-776">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="0681f-776">WaitForShutdownAsync</span></span>

<span data-ttu-id="0681f-777">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="0681f-777"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="0681f-778">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="0681f-778">External control</span></span>

<span data-ttu-id="0681f-779">Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="0681f-779">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="0681f-780">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="0681f-780"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="0681f-781">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="0681f-781">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="0681f-782">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="0681f-782">IHostingEnvironment interface</span></span>

<span data-ttu-id="0681f-783">Интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> предоставляет сведения о среде размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-783"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="0681f-784">Чтобы получить интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="0681f-784">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="0681f-785">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="0681f-785">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="0681f-786">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="0681f-786">IApplicationLifetime interface</span></span>

<span data-ttu-id="0681f-787">Интерфейс <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-787"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="0681f-788">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов <xref:System.Action>, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="0681f-788">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="0681f-789">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="0681f-789">Cancellation Token</span></span> | <span data-ttu-id="0681f-790">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="0681f-790">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="0681f-791">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="0681f-791">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="0681f-792">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-792">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="0681f-793">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="0681f-793">All requests should be processed.</span></span> <span data-ttu-id="0681f-794">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="0681f-794">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="0681f-795">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="0681f-795">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="0681f-796">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="0681f-796">Requests may still be processing.</span></span> <span data-ttu-id="0681f-797">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="0681f-797">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="0681f-798">Конструктор внедряет службу <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> в любой класс.</span><span class="sxs-lookup"><span data-stu-id="0681f-798">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="0681f-799">[Пример приложения](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию <xref:Microsoft.Extensions.Hosting.IHostedService>) для регистрации событий.</span><span class="sxs-lookup"><span data-stu-id="0681f-799">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="0681f-800">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="0681f-800">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="0681f-801">Метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="0681f-801"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="0681f-802">Следующий класс использует <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="0681f-802">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="0681f-803">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0681f-803">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
