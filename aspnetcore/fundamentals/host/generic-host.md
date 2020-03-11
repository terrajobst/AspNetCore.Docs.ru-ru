---
title: Универсальный узел .NET
author: rick-anderson
description: Сведения об универсальном узле .NET Core, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/02/2019
uid: fundamentals/host/generic-host
ms.openlocfilehash: 6a0ef02db883db3bc91722786cd042ccec092735
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78647578"
---
# <a name="net-generic-host"></a><span data-ttu-id="1d9c7-103">Универсальный узел .NET</span><span class="sxs-lookup"><span data-stu-id="1d9c7-103">.NET Generic Host</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="1d9c7-104">В этой статье описывается универсальный узел .NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>) и приводятся рекомендации о том, как его использовать.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-104">This article introduces the .NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>) and provides guidance on how to use it.</span></span>

## <a name="whats-a-host"></a><span data-ttu-id="1d9c7-105">Что такое узел?</span><span class="sxs-lookup"><span data-stu-id="1d9c7-105">What's a host?</span></span>

<span data-ttu-id="1d9c7-106">*Узел* — это объект, который инкапсулирует все ресурсы приложения, такие как:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-106">A *host* is an object that encapsulates an app's resources, such as:</span></span>

* <span data-ttu-id="1d9c7-107">Внедрение зависимостей</span><span class="sxs-lookup"><span data-stu-id="1d9c7-107">Dependency injection (DI)</span></span>
* <span data-ttu-id="1d9c7-108">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="1d9c7-108">Logging</span></span>
* <span data-ttu-id="1d9c7-109">Параметр Configuration</span><span class="sxs-lookup"><span data-stu-id="1d9c7-109">Configuration</span></span>
* <span data-ttu-id="1d9c7-110">Реализации `IHostedService`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-110">`IHostedService` implementations</span></span>

<span data-ttu-id="1d9c7-111">После запуска узла он вызывает `IHostedService.StartAsync` в каждой реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, которую найдет в контейнере внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-111">When a host starts, it calls `IHostedService.StartAsync` on each implementation of <xref:Microsoft.Extensions.Hosting.IHostedService> that it finds in the DI container.</span></span> <span data-ttu-id="1d9c7-112">В веб-приложении одна из реализаций `IHostedService` является веб-службой, которая запускает [реализацию сервера HTTP](xref:fundamentals/index#servers).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-112">In a web app, one of the `IHostedService` implementations is a web service that starts an [HTTP server implementation](xref:fundamentals/index#servers).</span></span>

<span data-ttu-id="1d9c7-113">Основной причиной включения всех взаимозависимых ресурсов приложения в один объект является управление жизненным циклом: контроль запуска и корректного завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-113">The main reason for including all of the app's interdependent resources in one object is lifetime management: control over app startup and graceful shutdown.</span></span>

<span data-ttu-id="1d9c7-114">В версиях ASP.NET Core до 3.0 [веб-узел](xref:fundamentals/host/web-host) используется для рабочих нагрузок HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-114">In versions of ASP.NET Core earlier than 3.0, the [Web Host](xref:fundamentals/host/web-host) is used for HTTP workloads.</span></span> <span data-ttu-id="1d9c7-115">Веб-узел больше не рекомендуется для веб-приложений и доступен только для обратной совместимости.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-115">The Web Host is no longer recommended for web apps and remains available only for backward compatibility.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="1d9c7-116">Создание узла</span><span class="sxs-lookup"><span data-stu-id="1d9c7-116">Set up a host</span></span>

<span data-ttu-id="1d9c7-117">Узел обычно настраивается, собирается и выполняется кодом в классе `Program`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-117">The host is typically configured, built, and run by code in the `Program` class.</span></span> <span data-ttu-id="1d9c7-118">Метод `Main`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-118">The `Main` method:</span></span>

* <span data-ttu-id="1d9c7-119">Вызывает метод `CreateHostBuilder` для создания и настройки объекта построителя.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-119">Calls a `CreateHostBuilder` method to create and configure a builder object.</span></span>
* <span data-ttu-id="1d9c7-120">Вызывает методы `Build` и `Run` в объекте построителя.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-120">Calls `Build` and `Run` methods on the builder object.</span></span>

<span data-ttu-id="1d9c7-121">Вот код *Program.cs* для рабочей нагрузки, отличной от HTTP, с одной реализацией `IHostedService`, добавленной в контейнер внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-121">Here's *Program.cs* code for a non-HTTP workload, with a single `IHostedService` implementation added to the DI container.</span></span> 

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

<span data-ttu-id="1d9c7-122">При использовании рабочей нагрузки HTTP метод `Main` остается прежним, но `CreateHostBuilder` вызывает `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-122">For an HTTP workload, the `Main` method is the same but `CreateHostBuilder` calls `ConfigureWebHostDefaults`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="1d9c7-123">Если приложение использует Entity Framework Core, не изменяйте имя или сигнатуру метода `CreateHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-123">If the app uses Entity Framework Core, don't change the name or signature of the `CreateHostBuilder` method.</span></span> <span data-ttu-id="1d9c7-124">[Инструменты Entity Framework Core](/ef/core/miscellaneous/cli/) ищут метод `CreateHostBuilder`, который настраивает узел без необходимости запускать приложение.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-124">The [Entity Framework Core tools](/ef/core/miscellaneous/cli/) expect to find a `CreateHostBuilder` method that configures the host without running the app.</span></span> <span data-ttu-id="1d9c7-125">Подробные сведения см. в статье [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation) (Создание экземпляра DbContext во время разработки).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-125">For more information, see [Design-time DbContext Creation](/ef/core/miscellaneous/cli/dbcontext-creation).</span></span>

## <a name="default-builder-settings"></a><span data-ttu-id="1d9c7-126">Параметры построителя по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1d9c7-126">Default builder settings</span></span>

<span data-ttu-id="1d9c7-127">Метод <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*>:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-127">The <xref:Microsoft.Extensions.Hosting.Host.CreateDefaultBuilder*> method:</span></span>

* <span data-ttu-id="1d9c7-128">В качестве [корневого каталога содержимого](xref:fundamentals/index#content-root) задает путь, возвращенный методом <xref:System.IO.Directory.GetCurrentDirectory*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-128">Sets the [content root](xref:fundamentals/index#content-root) to the path returned by <xref:System.IO.Directory.GetCurrentDirectory*>.</span></span>
* <span data-ttu-id="1d9c7-129">Загружает конфигурацию узла из:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-129">Loads host configuration from:</span></span>
  * <span data-ttu-id="1d9c7-130">Переменные среды с префиксом "DOTNET_".</span><span class="sxs-lookup"><span data-stu-id="1d9c7-130">Environment variables prefixed with "DOTNET_".</span></span>
  * <span data-ttu-id="1d9c7-131">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-131">Command-line arguments.</span></span>
* <span data-ttu-id="1d9c7-132">Загружает конфигурацию приложения из:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-132">Loads app configuration from:</span></span>
  * <span data-ttu-id="1d9c7-133">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="1d9c7-133">*appsettings.json*.</span></span>
  * <span data-ttu-id="1d9c7-134">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="1d9c7-134">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="1d9c7-135">[диспетчер секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development`;</span><span class="sxs-lookup"><span data-stu-id="1d9c7-135">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="1d9c7-136">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-136">Environment variables.</span></span>
  * <span data-ttu-id="1d9c7-137">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-137">Command-line arguments.</span></span>
* <span data-ttu-id="1d9c7-138">Добавляет следующие [регистраторы](xref:fundamentals/logging/index):</span><span class="sxs-lookup"><span data-stu-id="1d9c7-138">Adds the following [logging](xref:fundamentals/logging/index) providers:</span></span>
  * <span data-ttu-id="1d9c7-139">Консоль</span><span class="sxs-lookup"><span data-stu-id="1d9c7-139">Console</span></span>
  * <span data-ttu-id="1d9c7-140">Отладка</span><span class="sxs-lookup"><span data-stu-id="1d9c7-140">Debug</span></span>
  * <span data-ttu-id="1d9c7-141">EventSource</span><span class="sxs-lookup"><span data-stu-id="1d9c7-141">EventSource</span></span>
  * <span data-ttu-id="1d9c7-142">Журнал событий (только при запуске в Windows)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-142">EventLog (only when running on Windows)</span></span>
* <span data-ttu-id="1d9c7-143">Включает [проверку области](xref:fundamentals/dependency-injection#scope-validation) и [проверку зависимостей](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild), если это среда разработки.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-143">Enables [scope validation](xref:fundamentals/dependency-injection#scope-validation) and [dependency validation](xref:Microsoft.Extensions.DependencyInjection.ServiceProviderOptions.ValidateOnBuild) when the environment is Development.</span></span>

<span data-ttu-id="1d9c7-144">Метод `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-144">The `ConfigureWebHostDefaults` method:</span></span>

* <span data-ttu-id="1d9c7-145">Загружает конфигурацию узла из переменных среды с префиксом "ASPNETCORE_".</span><span class="sxs-lookup"><span data-stu-id="1d9c7-145">Loads host configuration from environment variables prefixed with "ASPNETCORE_".</span></span>
* <span data-ttu-id="1d9c7-146">Задает сервер [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает его с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-146">Sets [Kestrel](xref:fundamentals/servers/kestrel) server as the web server and configures it using the app's hosting configuration providers.</span></span> <span data-ttu-id="1d9c7-147">Параметры сервера Kestrel по умолчанию см. в разделе <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-147">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="1d9c7-148">Добавляет [ПО промежуточного слоя фильтрации узлов](xref:fundamentals/servers/kestrel#host-filtering).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-148">Adds [Host Filtering middleware](xref:fundamentals/servers/kestrel#host-filtering).</span></span>
* <span data-ttu-id="1d9c7-149">Добавляет [ПО промежуточного слоя переадресованных заголовков](xref:host-and-deploy/proxy-load-balancer#forwarded-headers), если ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-149">Adds [Forwarded Headers middleware](xref:host-and-deploy/proxy-load-balancer#forwarded-headers) if ASPNETCORE_FORWARDEDHEADERS_ENABLED=true.</span></span>
* <span data-ttu-id="1d9c7-150">Обеспечивает интеграцию служб IIS.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-150">Enables IIS integration.</span></span> <span data-ttu-id="1d9c7-151">Параметры IIS по умолчанию см. в разделе <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-151">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>

<span data-ttu-id="1d9c7-152">Разделы [Параметры для всех типов приложений](#settings-for-all-app-types) и [Параметры для веб-приложений](#settings-for-web-apps) далее в этой статье описывают, как переопределить параметры построителя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-152">The [Settings for all app types](#settings-for-all-app-types) and [Settings for web apps](#settings-for-web-apps) sections later in this article show how to override default builder settings.</span></span>

## <a name="framework-provided-services"></a><span data-ttu-id="1d9c7-153">Платформенные службы</span><span class="sxs-lookup"><span data-stu-id="1d9c7-153">Framework-provided services</span></span>

<span data-ttu-id="1d9c7-154">Службы, которые регистрируются автоматически, включают следующее:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-154">Services that are registered automatically include the following:</span></span>

* [<span data-ttu-id="1d9c7-155">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="1d9c7-155">IHostApplicationLifetime</span></span>](#ihostapplicationlifetime)
* [<span data-ttu-id="1d9c7-156">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="1d9c7-156">IHostLifetime</span></span>](#ihostlifetime)
* [<span data-ttu-id="1d9c7-157">IHostEnvironment / IWebHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="1d9c7-157">IHostEnvironment / IWebHostEnvironment</span></span>](#ihostenvironment)

<span data-ttu-id="1d9c7-158">Дополнительные сведения о службах, предоставляемых платформой, см. в разделе <xref:fundamentals/dependency-injection#framework-provided-services>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-158">For more information on framework-provided services, see <xref:fundamentals/dependency-injection#framework-provided-services>.</span></span>

## <a name="ihostapplicationlifetime"></a><span data-ttu-id="1d9c7-159">IHostApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="1d9c7-159">IHostApplicationLifetime</span></span>

<span data-ttu-id="1d9c7-160">Внедрите <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (прежнее название — `IApplicationLifetime`) в любой класс для выполнения задач после запуска и корректного завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-160">Inject the <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime> (formerly `IApplicationLifetime`) service into any class to handle post-startup and graceful shutdown tasks.</span></span> <span data-ttu-id="1d9c7-161">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов обработчика событий запуска и завершения работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-161">Three properties on the interface are cancellation tokens used to register app start and app stop event handler methods.</span></span> <span data-ttu-id="1d9c7-162">Этот интерфейс также включает метод `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-162">The interface also includes a `StopApplication` method.</span></span>

<span data-ttu-id="1d9c7-163">Ниже приведен пример реализации `IHostedService`, которая регистрирует события `IHostApplicationLifetime`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-163">The following example is an `IHostedService` implementation that registers `IHostApplicationLifetime` events:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/LifetimeEventsHostedService.cs?name=snippet_LifetimeEvents)]

## <a name="ihostlifetime"></a><span data-ttu-id="1d9c7-164">IHostLifetime</span><span class="sxs-lookup"><span data-stu-id="1d9c7-164">IHostLifetime</span></span>

<span data-ttu-id="1d9c7-165">Реализация <xref:Microsoft.Extensions.Hosting.IHostLifetime> контролирует, когда узел запускается и останавливается.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-165">The <xref:Microsoft.Extensions.Hosting.IHostLifetime> implementation controls when the host starts and when it stops.</span></span> <span data-ttu-id="1d9c7-166">Используется последняя зарегистрированная реализация.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-166">The last implementation registered is used.</span></span>

<span data-ttu-id="1d9c7-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` — это реализация `IHostLifetime` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-167">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is the default `IHostLifetime` implementation.</span></span> <span data-ttu-id="1d9c7-168">`ConsoleLifetime`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-168">`ConsoleLifetime`:</span></span>

* <span data-ttu-id="1d9c7-169">Прослушивает сигналы CTRL+C/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-169">Listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IHostApplicationLifetime.StopApplication*> to start the shutdown process.</span></span>
* <span data-ttu-id="1d9c7-170">разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-170">Unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span>

## <a name="ihostenvironment"></a><span data-ttu-id="1d9c7-171">IHostEnvironment</span><span class="sxs-lookup"><span data-stu-id="1d9c7-171">IHostEnvironment</span></span>

<span data-ttu-id="1d9c7-172">Внедряет службу <xref:Microsoft.Extensions.Hosting.IHostEnvironment> в класс, чтобы получить следующие сведения:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-172">Inject the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> service into a class to get information about the following:</span></span>

* [<span data-ttu-id="1d9c7-173">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="1d9c7-173">ApplicationName</span></span>](#applicationname)
* [<span data-ttu-id="1d9c7-174">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="1d9c7-174">EnvironmentName</span></span>](#environmentname)
* [<span data-ttu-id="1d9c7-175">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="1d9c7-175">ContentRootPath</span></span>](#contentrootpath)

<span data-ttu-id="1d9c7-176">Веб-приложения реализуют интерфейс `IWebHostEnvironment`, который наследует `IHostEnvironment` и добавляет [WebRootPath](#webroot).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-176">Web apps implement the `IWebHostEnvironment` interface, which inherits `IHostEnvironment` and adds the [WebRootPath](#webroot).</span></span>

## <a name="host-configuration"></a><span data-ttu-id="1d9c7-177">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="1d9c7-177">Host configuration</span></span>

<span data-ttu-id="1d9c7-178">Конфигурация узла используется для свойств реализации <xref:Microsoft.Extensions.Hosting.IHostEnvironment>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-178">Host configuration is used for the properties of the <xref:Microsoft.Extensions.Hosting.IHostEnvironment> implementation.</span></span>

<span data-ttu-id="1d9c7-179">Конфигурация узла доступна из [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) внутри <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-179">Host configuration is available from [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration) inside <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>.</span></span> <span data-ttu-id="1d9c7-180">После `ConfigureAppConfiguration``HostBuilderContext.Configuration` заменяется конфигурацией приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-180">After `ConfigureAppConfiguration`, `HostBuilderContext.Configuration` is replaced with the app config.</span></span>

<span data-ttu-id="1d9c7-181">Чтобы добавить конфигурацию узла, вызовите <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-181">To add host configuration, call <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="1d9c7-182">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-182">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="1d9c7-183">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-183">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="1d9c7-184">Поставщик переменных среды с префиксом `DOTNET_` и аргументы командной строки включены в CreateDefaultBuilder.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-184">The environment variable provider with prefix `DOTNET_` and command line args are included by CreateDefaultBuilder.</span></span> <span data-ttu-id="1d9c7-185">Для веб-приложений добавляется поставщик переменных среды с префиксом `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-185">For web apps, the environment variable provider with prefix `ASPNETCORE_` is added.</span></span> <span data-ttu-id="1d9c7-186">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-186">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="1d9c7-187">Например, значение переменной среды для `ASPNETCORE_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-187">For example, the environment variable value for `ASPNETCORE_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="1d9c7-188">В следующем примере создается конфигурация узла:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-188">The following example creates host configuration:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostConfig)]

## <a name="app-configuration"></a><span data-ttu-id="1d9c7-189">Конфигурация приложения</span><span class="sxs-lookup"><span data-stu-id="1d9c7-189">App configuration</span></span>

<span data-ttu-id="1d9c7-190">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-190">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on `IHostBuilder`.</span></span> <span data-ttu-id="1d9c7-191">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-191">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="1d9c7-192">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-192">The app uses whichever option sets a value last on a given key.</span></span> 

<span data-ttu-id="1d9c7-193">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и как служба из внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-193">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and as a service from DI.</span></span> <span data-ttu-id="1d9c7-194">Конфигурация узла также добавляется к конфигурации приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-194">The host configuration is also added to the app configuration.</span></span>

<span data-ttu-id="1d9c7-195">Дополнительные сведения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-195">For more information, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index#configureappconfiguration).</span></span>

## <a name="settings-for-all-app-types"></a><span data-ttu-id="1d9c7-196">Параметры для всех типов приложений</span><span class="sxs-lookup"><span data-stu-id="1d9c7-196">Settings for all app types</span></span>

<span data-ttu-id="1d9c7-197">В этом разделе перечислены параметры узла, которые применяются к рабочим нагрузкам HTTP и остальным.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-197">This section lists host settings that apply to both HTTP and non-HTTP workloads.</span></span> <span data-ttu-id="1d9c7-198">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-198">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<!-- In the following sections, two spaces at end of line are used to force line breaks in the rendered page. -->

### <a name="applicationname"></a><span data-ttu-id="1d9c7-199">ApplicationName</span><span class="sxs-lookup"><span data-stu-id="1d9c7-199">ApplicationName</span></span>

<span data-ttu-id="1d9c7-200">Свойство [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-200">The [IHostEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span>

<span data-ttu-id="1d9c7-201">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="1d9c7-201">**Key**: applicationName</span></span>  
<span data-ttu-id="1d9c7-202">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-202">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-203">**По умолчанию**: Имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-203">**Default**: The name of the assembly that contains the app's entry point.</span></span>
<span data-ttu-id="1d9c7-204">**Переменная среды**: `<PREFIX_>APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-204">**Environment variable**: `<PREFIX_>APPLICATIONNAME`</span></span>

<span data-ttu-id="1d9c7-205">Чтобы задать это значение, используйте переменную среды.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-205">To set this value, use the environment variable.</span></span> 

### <a name="contentrootpath"></a><span data-ttu-id="1d9c7-206">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="1d9c7-206">ContentRootPath</span></span>

<span data-ttu-id="1d9c7-207">Свойство [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) определяет, где узел начинает поиск файлов содержимого.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-207">The [IHostEnvironment.ContentRootPath](xref:Microsoft.Extensions.Hosting.IHostEnvironment.ContentRootPath*) property determines where the host begins searching for content files.</span></span> <span data-ttu-id="1d9c7-208">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-208">If the path doesn't exist, the host fails to start.</span></span>

<span data-ttu-id="1d9c7-209">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1d9c7-209">**Key**: contentRoot</span></span>  
<span data-ttu-id="1d9c7-210">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-210">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-211">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-211">**Default**: The folder where the app assembly resides.</span></span>  
<span data-ttu-id="1d9c7-212">**Переменная среды**: `<PREFIX_>CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-212">**Environment variable**: `<PREFIX_>CONTENTROOT`</span></span>

<span data-ttu-id="1d9c7-213">Чтобы задать это значение, используйте переменную среды или вызов `UseContentRoot` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-213">To set this value, use the environment variable or call `UseContentRoot` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\content-root")
    //...
```

<span data-ttu-id="1d9c7-214">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="1d9c7-214">For more information, see:</span></span>

* [<span data-ttu-id="1d9c7-215">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="1d9c7-215">Fundamentals: Content root</span></span>](xref:fundamentals/index#content-root)
* [<span data-ttu-id="1d9c7-216">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="1d9c7-216">WebRoot</span></span>](#webroot)

### <a name="environmentname"></a><span data-ttu-id="1d9c7-217">EnvironmentName</span><span class="sxs-lookup"><span data-stu-id="1d9c7-217">EnvironmentName</span></span>

<span data-ttu-id="1d9c7-218">Свойству [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) может быть присвоено любое значение.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-218">The [IHostEnvironment.EnvironmentName](xref:Microsoft.Extensions.Hosting.IHostEnvironment.EnvironmentName*) property can be set to any value.</span></span> <span data-ttu-id="1d9c7-219">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-219">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1d9c7-220">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-220">Values aren't case sensitive.</span></span>

<span data-ttu-id="1d9c7-221">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="1d9c7-221">**Key**: environment</span></span>  
<span data-ttu-id="1d9c7-222">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-222">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-223">**По умолчанию**: Рабочие</span><span class="sxs-lookup"><span data-stu-id="1d9c7-223">**Default**: Production</span></span>  
<span data-ttu-id="1d9c7-224">**Переменная среды**: `<PREFIX_>ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-224">**Environment variable**: `<PREFIX_>ENVIRONMENT`</span></span>

<span data-ttu-id="1d9c7-225">Чтобы задать это значение, используйте переменную среды или вызов `UseEnvironment` в `IHostBuilder`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-225">To set this value, use the environment variable or call `UseEnvironment` on `IHostBuilder`:</span></span>

```csharp
Host.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    //...
```

### <a name="shutdowntimeout"></a><span data-ttu-id="1d9c7-226">ShutdownTimeout</span><span class="sxs-lookup"><span data-stu-id="1d9c7-226">ShutdownTimeout</span></span>

<span data-ttu-id="1d9c7-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-227">[HostOptions.ShutdownTimeout](xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*) sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="1d9c7-228">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-228">The default value is five seconds.</span></span>  <span data-ttu-id="1d9c7-229">Во время ожидания узел:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-229">During the timeout period, the host:</span></span>

* <span data-ttu-id="1d9c7-230">Активирует [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-230">Triggers [IHostApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.ihostapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="1d9c7-231">Пытается остановить размещенные службы, записывая в журнал ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-231">Attempts to stop hosted services, logging errors for services that fail to stop.</span></span>

<span data-ttu-id="1d9c7-232">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-232">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="1d9c7-233">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-233">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="1d9c7-234">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-234">If services require additional time to stop, increase the timeout.</span></span>

<span data-ttu-id="1d9c7-235">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="1d9c7-235">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="1d9c7-236">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-236">**Type**: *int*</span></span>  
<span data-ttu-id="1d9c7-237">**По умолчанию**: 5 секунд **Переменная среды**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-237">**Default**: 5 seconds **Environment variable**: `<PREFIX_>SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="1d9c7-238">Чтобы задать это значение, используйте переменную среды или настройте `HostOptions`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-238">To set this value, use the environment variable or configure `HostOptions`.</span></span> <span data-ttu-id="1d9c7-239">В следующем примере устанавливается время ожидания в 20 секунд:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-239">The following example sets the timeout to 20 seconds:</span></span>

[!code-csharp[](generic-host/samples-snapshot/3.x/Program.cs?name=snippet_HostOptions)]

## <a name="settings-for-web-apps"></a><span data-ttu-id="1d9c7-240">Параметры для веб-приложений</span><span class="sxs-lookup"><span data-stu-id="1d9c7-240">Settings for web apps</span></span>

<span data-ttu-id="1d9c7-241">Некоторые параметры узла применяются только к рабочим нагрузкам HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-241">Some host settings apply only to HTTP workloads.</span></span> <span data-ttu-id="1d9c7-242">По умолчанию переменные среды, используемые для настройки этих параметров, могут иметь префикс `DOTNET_` или `ASPNETCORE_`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-242">By default, environment variables used to configure these settings can have a `DOTNET_` or `ASPNETCORE_` prefix.</span></span>

<span data-ttu-id="1d9c7-243">Методы расширения в `IWebHostBuilder` доступны для этих параметров.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-243">Extension methods on `IWebHostBuilder` are available for these settings.</span></span> <span data-ttu-id="1d9c7-244">В примерах кода, которые показывают, как вызывать методы расширения, предполагается, что `webBuilder` является экземпляром `IWebHostBuilder`, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-244">Code samples that show how to call the extension methods assume `webBuilder` is an instance of `IWebHostBuilder`, as in the following example:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.CaptureStartupErrors(true);
            webBuilder.UseStartup<Startup>();
        });
```

### <a name="capturestartuperrors"></a><span data-ttu-id="1d9c7-245">CaptureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1d9c7-245">CaptureStartupErrors</span></span>

<span data-ttu-id="1d9c7-246">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-246">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="1d9c7-247">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-247">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

<span data-ttu-id="1d9c7-248">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="1d9c7-248">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="1d9c7-249">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1d9c7-250">**По умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-250">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="1d9c7-251">**Переменная среды**: `<PREFIX_>CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-251">**Environment variable**: `<PREFIX_>CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="1d9c7-252">Чтобы задать это значение, используйте конфигурацию или вызов `CaptureStartupErrors`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-252">To set this value, use configuration or call `CaptureStartupErrors`:</span></span>

```csharp
webBuilder.CaptureStartupErrors(true);
```

### <a name="detailederrors"></a><span data-ttu-id="1d9c7-253">DetailedErrors</span><span class="sxs-lookup"><span data-stu-id="1d9c7-253">DetailedErrors</span></span>

<span data-ttu-id="1d9c7-254">Если этот параметр включен или если среда имеет значение `Development`, приложение перехватывает подробные ошибки.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-254">When enabled, or when the environment is `Development`, the app captures detailed errors.</span></span>

<span data-ttu-id="1d9c7-255">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="1d9c7-255">**Key**: detailedErrors</span></span>  
<span data-ttu-id="1d9c7-256">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-256">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1d9c7-257">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="1d9c7-257">**Default**: false</span></span>  
<span data-ttu-id="1d9c7-258">**Переменная среды**: `<PREFIX_>_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-258">**Environment variable**: `<PREFIX_>_DETAILEDERRORS`</span></span>

<span data-ttu-id="1d9c7-259">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-259">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.DetailedErrorsKey, "true");
```

### <a name="hostingstartupassemblies"></a><span data-ttu-id="1d9c7-260">HostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1d9c7-260">HostingStartupAssemblies</span></span>

<span data-ttu-id="1d9c7-261">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-261">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="1d9c7-262">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-262">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="1d9c7-263">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-263">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

<span data-ttu-id="1d9c7-264">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="1d9c7-264">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="1d9c7-265">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-265">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-266">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="1d9c7-266">**Default**: Empty string</span></span>  
<span data-ttu-id="1d9c7-267">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-267">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="1d9c7-268">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-268">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2");
```

### <a name="hostingstartupexcludeassemblies"></a><span data-ttu-id="1d9c7-269">HostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="1d9c7-269">HostingStartupExcludeAssemblies</span></span>

<span data-ttu-id="1d9c7-270">Разделенная точками с запятой строка начальных сборок размещения, которые необходимо исключить при запуске.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-270">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="1d9c7-271">**Ключ**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="1d9c7-271">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="1d9c7-272">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-272">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-273">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="1d9c7-273">**Default**: Empty string</span></span>  
<span data-ttu-id="1d9c7-274">**Переменная среды**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-274">**Environment variable**: `<PREFIX_>_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

<span data-ttu-id="1d9c7-275">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-275">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2");
```

### <a name="https_port"></a><span data-ttu-id="1d9c7-276">HTTPS_Port</span><span class="sxs-lookup"><span data-stu-id="1d9c7-276">HTTPS_Port</span></span>

<span data-ttu-id="1d9c7-277">Порт перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-277">The HTTPS redirect port.</span></span> <span data-ttu-id="1d9c7-278">Используется при [принудительном применении HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-278">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="1d9c7-279">**Ключ**: https_port</span><span class="sxs-lookup"><span data-stu-id="1d9c7-279">**Key**: https_port</span></span>  
<span data-ttu-id="1d9c7-280">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-280">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-281">**По умолчанию**: значение по умолчанию не задано.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-281">**Default**: A default value isn't set.</span></span>  
<span data-ttu-id="1d9c7-282">**Переменная среды**: `<PREFIX_>HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-282">**Environment variable**: `<PREFIX_>HTTPS_PORT`</span></span>

<span data-ttu-id="1d9c7-283">Чтобы задать это значение, используйте конфигурацию или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-283">To set this value, use configuration or call `UseSetting`:</span></span>

```csharp
webBuilder.UseSetting("https_port", "8080");
```

### <a name="preferhostingurls"></a><span data-ttu-id="1d9c7-284">PreferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1d9c7-284">PreferHostingUrls</span></span>

<span data-ttu-id="1d9c7-285">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `IWebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-285">Indicates whether the host should listen on the URLs configured with the `IWebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="1d9c7-286">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="1d9c7-286">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="1d9c7-287">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-287">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1d9c7-288">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="1d9c7-288">**Default**: true</span></span>  
<span data-ttu-id="1d9c7-289">**Переменная среды**: `<PREFIX_>_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-289">**Environment variable**: `<PREFIX_>_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="1d9c7-290">Чтобы задать это значение, используйте переменную среды или вызов `PreferHostingUrls`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-290">To set this value, use the environment variable or call `PreferHostingUrls`:</span></span>

```csharp
webBuilder.PreferHostingUrls(false);
```

### <a name="preventhostingstartup"></a><span data-ttu-id="1d9c7-291">PreventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1d9c7-291">PreventHostingStartup</span></span>

<span data-ttu-id="1d9c7-292">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-292">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="1d9c7-293">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-293">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="1d9c7-294">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="1d9c7-294">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="1d9c7-295">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-295">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="1d9c7-296">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="1d9c7-296">**Default**: false</span></span>  
<span data-ttu-id="1d9c7-297">**Переменная среды**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-297">**Environment variable**: `<PREFIX_>_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="1d9c7-298">Чтобы задать это значение, используйте переменную среды или вызов `UseSetting`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-298">To set this value, use the environment variable or call `UseSetting` :</span></span>

```csharp
webBuilder.UseSetting(WebHostDefaults.PreventHostingStartupKey, "true");
```

### <a name="startupassembly"></a><span data-ttu-id="1d9c7-299">StartupAssembly</span><span class="sxs-lookup"><span data-stu-id="1d9c7-299">StartupAssembly</span></span>

<span data-ttu-id="1d9c7-300">Сборка, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-300">The assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="1d9c7-301">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="1d9c7-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="1d9c7-302">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-302">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-303">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="1d9c7-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="1d9c7-304">**Переменная среды**: `<PREFIX_>STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-304">**Environment variable**: `<PREFIX_>STARTUPASSEMBLY`</span></span>

<span data-ttu-id="1d9c7-305">Чтобы задать это значение, используйте переменную среды или вызов `UseStartup`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-305">To set this value, use the environment variable or call `UseStartup`.</span></span> <span data-ttu-id="1d9c7-306">`UseStartup` может использовать имя сборки (`string`) или тип (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-306">`UseStartup` can take an assembly name (`string`) or a type (`TStartup`).</span></span> <span data-ttu-id="1d9c7-307">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
webBuilder.UseStartup("StartupAssemblyName");
```

```csharp
webBuilder.UseStartup<Startup>();
```

### <a name="urls"></a><span data-ttu-id="1d9c7-308">URL-адреса</span><span class="sxs-lookup"><span data-stu-id="1d9c7-308">URLs</span></span>

<span data-ttu-id="1d9c7-309">Разделенный точками с запятой список IP-адресов или адресов узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-309">A semicolon-delimited list of IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span> <span data-ttu-id="1d9c7-310">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-310">For example, `http://localhost:123`.</span></span> <span data-ttu-id="1d9c7-311">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-311">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="1d9c7-312">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-312">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="1d9c7-313">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-313">Supported formats vary among servers.</span></span>

<span data-ttu-id="1d9c7-314">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="1d9c7-314">**Key**: urls</span></span>  
<span data-ttu-id="1d9c7-315">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-315">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-316">**Значения по умолчанию**: `http://localhost:5000` и `https://localhost:5001`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-316">**Default**: `http://localhost:5000` and `https://localhost:5001`</span></span>  
<span data-ttu-id="1d9c7-317">**Переменная среды**: `<PREFIX_>URLS`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-317">**Environment variable**: `<PREFIX_>URLS`</span></span>

<span data-ttu-id="1d9c7-318">Чтобы задать это значение, используйте переменную среды или вызов `UseUrls`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-318">To set this value, use the environment variable or call `UseUrls`:</span></span>

```csharp
webBuilder.UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002");
```

<span data-ttu-id="1d9c7-319">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-319">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="1d9c7-320">Для получения дополнительной информации см. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-320">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="webroot"></a><span data-ttu-id="1d9c7-321">WebRoot</span><span class="sxs-lookup"><span data-stu-id="1d9c7-321">WebRoot</span></span>

<span data-ttu-id="1d9c7-322">Относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-322">The relative path to the app's static assets.</span></span>

<span data-ttu-id="1d9c7-323">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="1d9c7-323">**Key**: webroot</span></span>  
<span data-ttu-id="1d9c7-324">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-324">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-325">**По умолчанию**: Значение по умолчанию — `wwwroot`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-325">**Default**: The default is `wwwroot`.</span></span> <span data-ttu-id="1d9c7-326">Наличие пути *{корневой_каталог_содержимого}/wwwroot* обязательно.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-326">The path to *{content root}/wwwroot* must exist.</span></span> <span data-ttu-id="1d9c7-327">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-327">If the path doesn't exist, a no-op file provider is used.</span></span>  
<span data-ttu-id="1d9c7-328">**Переменная среды**: `<PREFIX_>WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-328">**Environment variable**: `<PREFIX_>WEBROOT`</span></span>

<span data-ttu-id="1d9c7-329">Чтобы задать это значение, используйте переменную среды или вызов `UseWebRoot`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-329">To set this value, use the environment variable or call `UseWebRoot`:</span></span>

```csharp
webBuilder.UseWebRoot("public");
```

<span data-ttu-id="1d9c7-330">Дополнительные сведения можно найти в разделе</span><span class="sxs-lookup"><span data-stu-id="1d9c7-330">For more information, see:</span></span>

* [<span data-ttu-id="1d9c7-331">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="1d9c7-331">Fundamentals: Web root</span></span>](xref:fundamentals/index#web-root)
* [<span data-ttu-id="1d9c7-332">ContentRootPath</span><span class="sxs-lookup"><span data-stu-id="1d9c7-332">ContentRootPath</span></span>](#contentrootpath)

## <a name="manage-the-host-lifetime"></a><span data-ttu-id="1d9c7-333">Управление временем существования узла</span><span class="sxs-lookup"><span data-stu-id="1d9c7-333">Manage the host lifetime</span></span>

<span data-ttu-id="1d9c7-334">Вызывает методы в реализации <xref:Microsoft.Extensions.Hosting.IHost> для запуска и остановки приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-334">Call methods on the built <xref:Microsoft.Extensions.Hosting.IHost> implementation to start and stop the app.</span></span> <span data-ttu-id="1d9c7-335">Эти методы влияют на все реализации <xref:Microsoft.Extensions.Hosting.IHostedService>, зарегистрированные в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-335">These methods affect all  <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="1d9c7-336">Выполнить</span><span class="sxs-lookup"><span data-stu-id="1d9c7-336">Run</span></span>

<span data-ttu-id="1d9c7-337">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-337"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down.</span></span>

### <a name="runasync"></a><span data-ttu-id="1d9c7-338">RunAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-338">RunAsync</span></span>

<span data-ttu-id="1d9c7-339">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-339"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span>

### <a name="runconsoleasync"></a><span data-ttu-id="1d9c7-340">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-340">RunConsoleAsync</span></span>

<span data-ttu-id="1d9c7-341">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы CTRL + C/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-341"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

### <a name="start"></a><span data-ttu-id="1d9c7-342">Начало</span><span class="sxs-lookup"><span data-stu-id="1d9c7-342">Start</span></span>

<span data-ttu-id="1d9c7-343">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-343"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

### <a name="startasync"></a><span data-ttu-id="1d9c7-344">StartAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-344">StartAsync</span></span>

<span data-ttu-id="1d9c7-345">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает узел и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершении работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-345"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the host and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered.</span></span> 

<span data-ttu-id="1d9c7-346">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале `StartAsync`, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-346"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of `StartAsync`, which waits until it's complete before continuing.</span></span> <span data-ttu-id="1d9c7-347">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-347">This can be used to delay startup until signaled by an external event.</span></span>

### <a name="stopasync"></a><span data-ttu-id="1d9c7-348">StopAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-348">StopAsync</span></span>

<span data-ttu-id="1d9c7-349">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-349"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

### <a name="waitforshutdown"></a><span data-ttu-id="1d9c7-350">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="1d9c7-350">WaitForShutdown</span></span>

<span data-ttu-id="1d9c7-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> блокирует вызывающий поток до завершения работы, активированного IHostLifetime, например, через CTRL + C/SIGINT или SIGTERM.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-351"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> blocks the calling thread until shutdown is triggered by the IHostLifetime, such as via Ctrl+C/SIGINT or SIGTERM.</span></span>

### <a name="waitforshutdownasync"></a><span data-ttu-id="1d9c7-352">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-352">WaitForShutdownAsync</span></span>

<span data-ttu-id="1d9c7-353">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-353"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

### <a name="external-control"></a><span data-ttu-id="1d9c7-354">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="1d9c7-354">External control</span></span>

<span data-ttu-id="1d9c7-355">Прямое управление временем существования узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-355">Direct control of the host lifetime can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="1d9c7-356">Приложения ASP.NET Core настраивают и запускают узел.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-356">ASP.NET Core apps configure and launch a host.</span></span> <span data-ttu-id="1d9c7-357">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-357">The host is responsible for app startup and lifetime management.</span></span>

<span data-ttu-id="1d9c7-358">В этой статье рассматривается универсальный узел ASP.NET Core (<xref:Microsoft.Extensions.Hosting.HostBuilder>), используемый для приложений, которые не обрабатывают запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-358">This article covers the ASP.NET Core Generic Host (<xref:Microsoft.Extensions.Hosting.HostBuilder>), which is used for apps that don't process HTTP requests.</span></span>

<span data-ttu-id="1d9c7-359">Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-359">The purpose of Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="1d9c7-360">Обмен сообщениями, фоновые задачи и другие рабочие нагрузки, не связанные с HTTP, используют перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведение журналов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-360">Messaging, background tasks, and other non-HTTP workloads based on Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="1d9c7-361">Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-361">Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="1d9c7-362">Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-362">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="1d9c7-363">Универсальный узел заменит веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".</span><span class="sxs-lookup"><span data-stu-id="1d9c7-363">Generic Host will replace Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="1d9c7-364">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="1d9c7-364">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="1d9c7-365">При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-365">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="1d9c7-366">Не выполняйте пример в `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-366">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="1d9c7-367">Настройка консоли в Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-367">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="1d9c7-368">Откройте файл *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-368">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="1d9c7-369">В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-369">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="1d9c7-370">Измените значение на `externalTerminal` или `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-370">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="1d9c7-371">Вступление</span><span class="sxs-lookup"><span data-stu-id="1d9c7-371">Introduction</span></span>

<span data-ttu-id="1d9c7-372">Библиотека универсального узла находится в пространстве имен <xref:Microsoft.Extensions.Hosting> и включена в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-372">The Generic Host library is available in the <xref:Microsoft.Extensions.Hosting> namespace and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="1d9c7-373">Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-373">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="1d9c7-374"><xref:Microsoft.Extensions.Hosting.IHostedService> — это точка входа для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-374"><xref:Microsoft.Extensions.Hosting.IHostedService> is the entry point to code execution.</span></span> <span data-ttu-id="1d9c7-375">Каждая реализация <xref:Microsoft.Extensions.Hosting.IHostedService> выполняется в порядке [регистрации служб в ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-375">Each <xref:Microsoft.Extensions.Hosting.IHostedService> implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="1d9c7-376">Метод <xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> вызывается в каждом <xref:Microsoft.Extensions.Hosting.IHostedService> при запуске узла, а метод <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-376"><xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*> is called on each <xref:Microsoft.Extensions.Hosting.IHostedService> when the host starts, and <xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*> is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="1d9c7-377">Создание узла</span><span class="sxs-lookup"><span data-stu-id="1d9c7-377">Set up a host</span></span>

<span data-ttu-id="1d9c7-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-378"><xref:Microsoft.Extensions.Hosting.IHostBuilder> is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="options"></a><span data-ttu-id="1d9c7-379">Параметры</span><span class="sxs-lookup"><span data-stu-id="1d9c7-379">Options</span></span>

<span data-ttu-id="1d9c7-380"><xref:Microsoft.Extensions.Hosting.HostOptions> настраивает параметры для <xref:Microsoft.Extensions.Hosting.IHost>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-380"><xref:Microsoft.Extensions.Hosting.HostOptions> configure options for the <xref:Microsoft.Extensions.Hosting.IHost>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="1d9c7-381">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="1d9c7-381">Shutdown timeout</span></span>

<span data-ttu-id="1d9c7-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> задает время ожидания для <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-382"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> sets the timeout for <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span> <span data-ttu-id="1d9c7-383">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-383">The default value is five seconds.</span></span>

<span data-ttu-id="1d9c7-384">Следующий пример конфигурации в `Program.Main` увеличивает значение времени ожидания завершения работы по умолчанию с пяти до 20 секунд:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-384">The following option configuration in `Program.Main` increases the default five second shutdown timeout to 20 seconds:</span></span>

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

## <a name="default-services"></a><span data-ttu-id="1d9c7-385">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="1d9c7-385">Default services</span></span>

<span data-ttu-id="1d9c7-386">Во время инициализации узла регистрируются следующие службы:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-386">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="1d9c7-387">[Окружение](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-387">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="1d9c7-388">[Конфигурация](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-388">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="1d9c7-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-389"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (`Microsoft.Extensions.Hosting.Internal.ApplicationLifetime`)</span></span>
* <span data-ttu-id="1d9c7-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-390"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime`)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="1d9c7-391">[Параметры](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-391">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="1d9c7-392">[Ведение журнала](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-392">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="1d9c7-393">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="1d9c7-393">Host configuration</span></span>

<span data-ttu-id="1d9c7-394">Существуют следующие подходы для создания конфигурации узла:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-394">Host configuration is created by:</span></span>

* <span data-ttu-id="1d9c7-395">вызов методов расширения в <xref:Microsoft.Extensions.Hosting.IHostBuilder> для задания [корневой папки содержимого](#content-root) и [среды](#environment);</span><span class="sxs-lookup"><span data-stu-id="1d9c7-395">Calling extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder> to set the [content root](#content-root) and [environment](#environment).</span></span>
* <span data-ttu-id="1d9c7-396">чтение конфигурации из поставщиков конфигурации в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-396">Reading configuration from configuration providers in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>.</span></span>

### <a name="extension-methods"></a><span data-ttu-id="1d9c7-397">Методы расширения</span><span class="sxs-lookup"><span data-stu-id="1d9c7-397">Extension methods</span></span>

### <a name="application-key-name"></a><span data-ttu-id="1d9c7-398">Ключ приложения (имя)</span><span class="sxs-lookup"><span data-stu-id="1d9c7-398">Application key (name)</span></span>

<span data-ttu-id="1d9c7-399">Свойство [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-399">The [IHostingEnvironment.ApplicationName](xref:Microsoft.Extensions.Hosting.IHostingEnvironment.ApplicationName*) property is set from host configuration during host construction.</span></span> <span data-ttu-id="1d9c7-400">Чтобы явно задать значение, используйте [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-400">To set the value explicitly, use the [HostDefaults.ApplicationKey](xref:Microsoft.Extensions.Hosting.HostDefaults.ApplicationKey):</span></span>

<span data-ttu-id="1d9c7-401">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="1d9c7-401">**Key**: applicationName</span></span>  
<span data-ttu-id="1d9c7-402">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-402">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-403">**По умолчанию**: имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-403">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="1d9c7-404">**Задается с помощью**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-404">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="1d9c7-405">**Переменная среды**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>`[необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="1d9c7-405">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

### <a name="content-root"></a><span data-ttu-id="1d9c7-406">Корневой каталог содержимого</span><span class="sxs-lookup"><span data-stu-id="1d9c7-406">Content root</span></span>

<span data-ttu-id="1d9c7-407">Этот параметр определяет, где узел начинает искать файлы содержимого.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-407">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="1d9c7-408">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="1d9c7-408">**Key**: contentRoot</span></span>  
<span data-ttu-id="1d9c7-409">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-409">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-410">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-410">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="1d9c7-411">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-411">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="1d9c7-412">**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>`[необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="1d9c7-412">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="1d9c7-413">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-413">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

<span data-ttu-id="1d9c7-414">Дополнительные сведения можно найти в разделе [Корневой каталог содержимого](xref:fundamentals/index#content-root).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-414">For more information, see [Fundamentals: Content root](xref:fundamentals/index#content-root).</span></span>

### <a name="environment"></a><span data-ttu-id="1d9c7-415">Среда</span><span class="sxs-lookup"><span data-stu-id="1d9c7-415">Environment</span></span>

<span data-ttu-id="1d9c7-416">Задает [среду](xref:fundamentals/environments) приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-416">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="1d9c7-417">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="1d9c7-417">**Key**: environment</span></span>  
<span data-ttu-id="1d9c7-418">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="1d9c7-418">**Type**: *string*</span></span>  
<span data-ttu-id="1d9c7-419">**По умолчанию**: Рабочие</span><span class="sxs-lookup"><span data-stu-id="1d9c7-419">**Default**: Production</span></span>  
<span data-ttu-id="1d9c7-420">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="1d9c7-420">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="1d9c7-421">**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>`[необязательно и определяется пользователем](#configurehostconfiguration))</span><span class="sxs-lookup"><span data-stu-id="1d9c7-421">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configurehostconfiguration))</span></span>

<span data-ttu-id="1d9c7-422">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-422">The environment can be set to any value.</span></span> <span data-ttu-id="1d9c7-423">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-423">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="1d9c7-424">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-424">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

### <a name="configurehostconfiguration"></a><span data-ttu-id="1d9c7-425">ConfigureHostConfiguration</span><span class="sxs-lookup"><span data-stu-id="1d9c7-425">ConfigureHostConfiguration</span></span>

<span data-ttu-id="1d9c7-426">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-426"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the host.</span></span> <span data-ttu-id="1d9c7-427">Конфигурация узла применяется для инициализации <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> в целях использования в процессе сборки приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-427">The host configuration is used to initialize the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> for use in the app's build process.</span></span>

<span data-ttu-id="1d9c7-428">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-428"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="1d9c7-429">Узел использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-429">The host uses whichever option sets a value last on a given key.</span></span>

<span data-ttu-id="1d9c7-430">Поставщики не включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-430">No providers are included by default.</span></span> <span data-ttu-id="1d9c7-431">Необходимо явно указать поставщики конфигурации, требуемые приложению в <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, в том числе:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-431">You must explicitly specify whatever configuration providers the app requires in <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>, including:</span></span>

* <span data-ttu-id="1d9c7-432">конфигурация файла (например, из файла *hostsettings.json*);</span><span class="sxs-lookup"><span data-stu-id="1d9c7-432">File configuration (for example, from a *hostsettings.json* file).</span></span>
* <span data-ttu-id="1d9c7-433">конфигурация переменной среды;</span><span class="sxs-lookup"><span data-stu-id="1d9c7-433">Environment variable configuration.</span></span>
* <span data-ttu-id="1d9c7-434">конфигурация аргумента командной строки;</span><span class="sxs-lookup"><span data-stu-id="1d9c7-434">Command-line argument configuration.</span></span>
* <span data-ttu-id="1d9c7-435">другие необходимые поставщики конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-435">Any other required configuration providers.</span></span>

<span data-ttu-id="1d9c7-436">Конфигурация файла узла включается путем указания основного пути приложения с помощью `SetBasePath` и последующего вызова одного из [поставщиков конфигурации файла](xref:fundamentals/configuration/index#file-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-436">File configuration of the host is enabled by specifying the app's base path with `SetBasePath` followed by a call to one of the [file configuration providers](xref:fundamentals/configuration/index#file-configuration-provider).</span></span> <span data-ttu-id="1d9c7-437">В примере приложения используется JSON-файл *hostsettings.json* и вызывается метод <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> для использования параметров конфигурации узла файла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-437">The sample app uses a JSON file, *hostsettings.json*, and calls <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> to consume the file's host configuration settings.</span></span>

<span data-ttu-id="1d9c7-438">Чтобы добавить [конфигурацию переменной среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider) узла, вызовите метод <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> в построителе узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-438">To add [environment variable configuration](xref:fundamentals/configuration/index#environment-variables-configuration-provider) of the host, call <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> on the host builder.</span></span> <span data-ttu-id="1d9c7-439">`AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-439">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="1d9c7-440">В примере приложения используется префикс `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-440">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="1d9c7-441">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-441">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="1d9c7-442">После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-442">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="1d9c7-443">Во время разработки с использованием [Visual Studio](https://visualstudio.microsoft.com) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-443">During development when using [Visual Studio](https://visualstudio.microsoft.com) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="1d9c7-444">В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-444">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="1d9c7-445">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-445">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="1d9c7-446">[Конфигурация командной строки](xref:fundamentals/configuration/index#command-line-configuration-provider) добавляется путем вызова метода <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-446">[Command-line configuration](xref:fundamentals/configuration/index#command-line-configuration-provider) is added by calling <xref:Microsoft.Extensions.Configuration.CommandLineConfigurationExtensions.AddCommandLine*>.</span></span> <span data-ttu-id="1d9c7-447">Конфигурация командной строки добавляется в последнюю очередь, чтобы разрешить аргументам командной строки переопределять конфигурацию, предоставленную предыдущими поставщиками конфигурации.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-447">Command-line configuration is added last to permit command-line arguments to override configuration provided by the earlier configuration providers.</span></span>

<span data-ttu-id="1d9c7-448">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-448">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="1d9c7-449">Дополнительную конфигурацию можно указать с помощью ключей [applicationName](#application-key-name) и [contentRoot](#content-root).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-449">Additional configuration can be provided with the [applicationName](#application-key-name) and [contentRoot](#content-root) keys.</span></span>

<span data-ttu-id="1d9c7-450">Пример конфигурации `HostBuilder` с использованием <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-450">Example `HostBuilder` configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureHostConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

## <a name="configureappconfiguration"></a><span data-ttu-id="1d9c7-451">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="1d9c7-451">ConfigureAppConfiguration</span></span>

<span data-ttu-id="1d9c7-452">Конфигурация приложения создается путем вызова метода <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> в реализации <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-452">App configuration is created by calling <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> on the <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation.</span></span> <span data-ttu-id="1d9c7-453">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> использует <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder>, чтобы создать экземпляр <xref:Microsoft.Extensions.Configuration.IConfiguration> для приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-453"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> uses an <xref:Microsoft.Extensions.Configuration.IConfigurationBuilder> to create an <xref:Microsoft.Extensions.Configuration.IConfiguration> for the app.</span></span> <span data-ttu-id="1d9c7-454">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-454"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> can be called multiple times with additive results.</span></span> <span data-ttu-id="1d9c7-455">Приложение использует значение, заданное последним для данного ключа.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-455">The app uses whichever option sets a value last on a given key.</span></span> <span data-ttu-id="1d9c7-456">Конфигурация, созданная с помощью <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>, доступна в свойствах [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) для последующих операций и в <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-456">The configuration created by <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*> is available at [HostBuilderContext.Configuration](xref:Microsoft.Extensions.Hosting.HostBuilderContext.Configuration*) for subsequent operations and in <xref:Microsoft.Extensions.Hosting.IHost.Services*>.</span></span>

<span data-ttu-id="1d9c7-457">Конфигурация приложения автоматически получает конфигурацию узла, предоставленную с помощью [ConfigureHostConfiguration](#configurehostconfiguration).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-457">App configuration automatically receives host configuration provided by [ConfigureHostConfiguration](#configurehostconfiguration).</span></span>

<span data-ttu-id="1d9c7-458">Пример конфигурации приложения с использованием <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-458">Example app configuration using <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureAppConfiguration*>:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="1d9c7-459">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-459">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="1d9c7-460">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-460">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="1d9c7-461">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-461">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

<span data-ttu-id="1d9c7-462">Чтобы переместить файлы параметров в выходной каталог, укажите файлы параметров как [элементы проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-462">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="1d9c7-463">Пример приложения перемещает свои файлы параметров приложения JSON и *hostsettings.json* со следующим элементом `<Content>`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-463">The sample app moves its JSON app settings files and *hostsettings.json* with the following `<Content>` item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" Exclude="bin\**\*;obj\**\*" 
      CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="1d9c7-464">Для таких методов расширения конфигурации, как <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> и <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*>, необходимы дополнительные пакеты NuGet, такие как [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) и [ Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-464">Configuration extension methods, such as <xref:Microsoft.Extensions.Configuration.JsonConfigurationExtensions.AddJsonFile*> and <xref:Microsoft.Extensions.Configuration.EnvironmentVariablesExtensions.AddEnvironmentVariables*> require additional NuGet packages, such as [Microsoft.Extensions.Configuration.Json](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.Json) and [Microsoft.Extensions.Configuration.EnvironmentVariables](https://www.nuget.org/packages/Microsoft.Extensions.Configuration.EnvironmentVariables).</span></span> <span data-ttu-id="1d9c7-465">Если приложение не использует [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app), эти пакеты нужно добавить в проект в дополнение к основному пакету [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-465">Unless the app uses the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app), these packages must be added to the project in addition to the core [Microsoft.Extensions.Configuration](https://www.nuget.org/packages/Microsoft.Extensions.Configuration) package.</span></span> <span data-ttu-id="1d9c7-466">Для получения дополнительной информации см. <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-466">For more information, see <xref:fundamentals/configuration/index>.</span></span>

## <a name="configureservices"></a><span data-ttu-id="1d9c7-467">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="1d9c7-467">ConfigureServices</span></span>

<span data-ttu-id="1d9c7-468">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-468"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="1d9c7-469">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-469"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureServices*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="1d9c7-470">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-470">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="1d9c7-471">Для получения дополнительной информации см. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-471">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="1d9c7-472">[Пример приложения](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="1d9c7-472">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="1d9c7-473">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="1d9c7-473">ConfigureLogging</span></span>

<span data-ttu-id="1d9c7-474">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> добавляет делегат для настройки указанного интерфейса <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-474"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> adds a delegate for configuring the provided <xref:Microsoft.Extensions.Logging.ILoggingBuilder>.</span></span> <span data-ttu-id="1d9c7-475">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-475"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.ConfigureLogging*> may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="1d9c7-476">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="1d9c7-476">UseConsoleLifetime</span></span>

<span data-ttu-id="1d9c7-477">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> прослушивает сигналы CTRL + C/SIGINT или SIGTERM и вызывает метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-477"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> listens for Ctrl+C/SIGINT or SIGTERM and calls <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to start the shutdown process.</span></span> <span data-ttu-id="1d9c7-478">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-478"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.UseConsoleLifetime*> unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="1d9c7-479">Класс `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` предварительно зарегистрирован и является реализацией времени жизни по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-479">`Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="1d9c7-480">Используется то время жизни, которое зарегистрировано последним.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-480">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="1d9c7-481">Конфигурация контейнера</span><span class="sxs-lookup"><span data-stu-id="1d9c7-481">Container configuration</span></span>

<span data-ttu-id="1d9c7-482">Для поддержки подключения к другим контейнерам узел может принимать <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-482">To support plugging in other containers, the host can accept an <xref:Microsoft.Extensions.DependencyInjection.IServiceProviderFactory%601>.</span></span> <span data-ttu-id="1d9c7-483">Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-483">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="1d9c7-484">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-484">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](xref:Microsoft.Extensions.Hosting.HostBuilder.UseServiceProviderFactory*) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="1d9c7-485">Пользовательская конфигурация контейнера управляется методом <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-485">Custom container configuration is managed by the <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> method.</span></span> <span data-ttu-id="1d9c7-486">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-486"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="1d9c7-487">Метод <xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-487"><xref:Microsoft.Extensions.Hosting.HostBuilder.ConfigureContainer*> can be called multiple times with additive results.</span></span>

<span data-ttu-id="1d9c7-488">Создание контейнера службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-488">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="1d9c7-489">Настройка фабрики контейнера службы:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-489">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="1d9c7-490">Использование фабрики и настройка пользовательского контейнера служб для приложения:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-490">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="1d9c7-491">Расширение среды</span><span class="sxs-lookup"><span data-stu-id="1d9c7-491">Extensibility</span></span>

<span data-ttu-id="1d9c7-492">Расширяемость узла выполняется с помощью методов расширения класса <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-492">Host extensibility is performed with extension methods on <xref:Microsoft.Extensions.Hosting.IHostBuilder>.</span></span> <span data-ttu-id="1d9c7-493">В следующем примере показано, как метод расширения расширяет реализацию класса <xref:Microsoft.Extensions.Hosting.IHostBuilder> с помощью класса [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks), пример которого приведен в статье <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-493">The following example shows how an extension method extends an <xref:Microsoft.Extensions.Hosting.IHostBuilder> implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="1d9c7-494">Приложение устанавливает метод расширения `UseHostedService`, чтобы зарегистрировать размещенную службу, переданную в `T`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-494">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="1d9c7-495">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="1d9c7-495">Manage the host</span></span>

<span data-ttu-id="1d9c7-496">Реализация <xref:Microsoft.Extensions.Hosting.IHost> отвечает за запуск и остановку реализаций <xref:Microsoft.Extensions.Hosting.IHostedService>, которые зарегистрированы в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-496">The <xref:Microsoft.Extensions.Hosting.IHost> implementation is responsible for starting and stopping the <xref:Microsoft.Extensions.Hosting.IHostedService> implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="1d9c7-497">Выполнить</span><span class="sxs-lookup"><span data-stu-id="1d9c7-497">Run</span></span>

<span data-ttu-id="1d9c7-498">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-498"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Run*> runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="1d9c7-499">RunAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-499">RunAsync</span></span>

<span data-ttu-id="1d9c7-500">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> запускает приложение и возвращает объект <xref:System.Threading.Tasks.Task>, который завершается при активации токена отмены или завершение работы:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-500"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.RunAsync*> runs the app and returns a <xref:System.Threading.Tasks.Task> that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="1d9c7-501">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-501">RunConsoleAsync</span></span>

<span data-ttu-id="1d9c7-502">Метод <xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> включает поддержку консоли, собирает и запускает узел и ожидает сигналы CTRL + C/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-502"><xref:Microsoft.Extensions.Hosting.HostingHostBuilderExtensions.RunConsoleAsync*> enables console support, builds and starts the host, and waits for Ctrl+C/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="1d9c7-503">Start и StopAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-503">Start and StopAsync</span></span>

<span data-ttu-id="1d9c7-504">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-504"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.Start*> starts the host synchronously.</span></span>

<span data-ttu-id="1d9c7-505">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-505"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.StopAsync*> attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="1d9c7-506">StartAsync и StopAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-506">StartAsync and StopAsync</span></span>

<span data-ttu-id="1d9c7-507">Метод <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-507"><xref:Microsoft.Extensions.Hosting.IHost.StartAsync*> starts the app.</span></span>

<span data-ttu-id="1d9c7-508">Метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> останавливает приложение.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-508"><xref:Microsoft.Extensions.Hosting.IHost.StopAsync*> stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="1d9c7-509">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="1d9c7-509">WaitForShutdown</span></span>

<span data-ttu-id="1d9c7-510">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> запускается с помощью <xref:Microsoft.Extensions.Hosting.IHostLifetime>, например `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (прослушивает CTRL + C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="1d9c7-510"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> is triggered via the <xref:Microsoft.Extensions.Hosting.IHostLifetime>, such as `Microsoft.Extensions.Hosting.Internal.ConsoleLifetime` (listens for Ctrl+C/SIGINT or SIGTERM).</span></span> <span data-ttu-id="1d9c7-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> вызывает <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-511"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdown*> calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="1d9c7-512">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="1d9c7-512">WaitForShutdownAsync</span></span>

<span data-ttu-id="1d9c7-513">Метод <xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> возвращает объект <xref:System.Threading.Tasks.Task>, который завершается после активации завершения работы через токен, и вызывает метод <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-513"><xref:Microsoft.Extensions.Hosting.HostingAbstractionsHostExtensions.WaitForShutdownAsync*> returns a <xref:System.Threading.Tasks.Task> that completes when shutdown is triggered via the given token and calls <xref:Microsoft.Extensions.Hosting.IHost.StopAsync*>.</span></span>

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

### <a name="external-control"></a><span data-ttu-id="1d9c7-514">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="1d9c7-514">External control</span></span>

<span data-ttu-id="1d9c7-515">Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-515">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="1d9c7-516">Метод <xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> вызывается в начале <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-516"><xref:Microsoft.Extensions.Hosting.IHostLifetime.WaitForStartAsync*> is called at the start of <xref:Microsoft.Extensions.Hosting.IHost.StartAsync*>, which waits until it's complete before continuing.</span></span> <span data-ttu-id="1d9c7-517">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-517">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="1d9c7-518">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="1d9c7-518">IHostingEnvironment interface</span></span>

<span data-ttu-id="1d9c7-519">Интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> предоставляет сведения о среде размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-519"><xref:Microsoft.Extensions.Hosting.IHostingEnvironment> provides information about the app's hosting environment.</span></span> <span data-ttu-id="1d9c7-520">Чтобы получить интерфейс <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="1d9c7-520">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the <xref:Microsoft.Extensions.Hosting.IHostingEnvironment> in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="1d9c7-521">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-521">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="1d9c7-522">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="1d9c7-522">IApplicationLifetime interface</span></span>

<span data-ttu-id="1d9c7-523">Интерфейс <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-523"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="1d9c7-524">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов <xref:System.Action>, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-524">Three properties on the interface are cancellation tokens used to register <xref:System.Action> methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="1d9c7-525">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="1d9c7-525">Cancellation Token</span></span> | <span data-ttu-id="1d9c7-526">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="1d9c7-526">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStarted*> | <span data-ttu-id="1d9c7-527">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-527">The host has fully started.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopped*> | <span data-ttu-id="1d9c7-528">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-528">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="1d9c7-529">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-529">All requests should be processed.</span></span> <span data-ttu-id="1d9c7-530">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-530">Shutdown blocks until this event completes.</span></span> |
| <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.ApplicationStopping*> | <span data-ttu-id="1d9c7-531">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-531">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="1d9c7-532">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-532">Requests may still be processing.</span></span> <span data-ttu-id="1d9c7-533">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-533">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="1d9c7-534">Конструктор внедряет службу <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> в любой класс.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-534">Constructor-inject the <xref:Microsoft.Extensions.Hosting.IApplicationLifetime> service into any class.</span></span> <span data-ttu-id="1d9c7-535">[Пример приложения](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию <xref:Microsoft.Extensions.Hosting.IHostedService>) для регистрации событий.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-535">The [sample app](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an <xref:Microsoft.Extensions.Hosting.IHostedService> implementation) to register the events.</span></span>

<span data-ttu-id="1d9c7-536">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-536">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="1d9c7-537">Метод <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="1d9c7-537"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> requests termination of the app.</span></span> <span data-ttu-id="1d9c7-538">Следующий класс использует <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="1d9c7-538">The following class uses <xref:Microsoft.Extensions.Hosting.IApplicationLifetime.StopApplication*> to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="1d9c7-539">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1d9c7-539">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
