---
title: Веб-узел ASP.NET Core
author: guardrex
description: Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования.
ms.author: riande
ms.custom: mvc
ms.date: 10/18/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: e19f12f69dfdd5653aea9c6be2b05f24009b875e
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477453"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="78a7d-103">Веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78a7d-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="78a7d-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="78a7d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="78a7d-105">Приложения ASP.NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="78a7d-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="78a7d-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="78a7d-107">Узел настраивает как минимум сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="78a7d-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="78a7d-108">В этой статье рассматривается веб-узел ASP.NET ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), который используется для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="78a7d-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="78a7d-109">Сведения об универсальном узле .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) см. в статье <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="78a7d-110">Создание узла</span><span class="sxs-lookup"><span data-stu-id="78a7d-110">Set up a host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="78a7d-111">Создайте узел с помощью экземпляра [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="78a7d-111">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="78a7d-112">Обычно это делается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="78a7d-113">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="78a7d-114">Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>();
}
```

<span data-ttu-id="78a7d-115">Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.</span><span class="sxs-lookup"><span data-stu-id="78a7d-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="78a7d-116">Настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает сервер с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-116">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="78a7d-117">Параметры Kestrel по умолчанию см. в разделе <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-117">For the Kestrel default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="78a7d-118">В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="78a7d-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="78a7d-119">Загружает [конфигурацию узла](#host-configuration-values) из:</span><span class="sxs-lookup"><span data-stu-id="78a7d-119">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="78a7d-120">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="78a7d-120">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="78a7d-121">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="78a7d-121">Command-line arguments.</span></span>
* <span data-ttu-id="78a7d-122">Загружает конфигурацию приложения в следующем порядке из:</span><span class="sxs-lookup"><span data-stu-id="78a7d-122">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="78a7d-123">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="78a7d-123">*appsettings.json*.</span></span>
  * <span data-ttu-id="78a7d-124">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="78a7d-124">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="78a7d-125">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="78a7d-125">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="78a7d-126">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="78a7d-126">Environment variables.</span></span>
  * <span data-ttu-id="78a7d-127">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="78a7d-127">Command-line arguments.</span></span>
* <span data-ttu-id="78a7d-128">Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="78a7d-128">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="78a7d-129">Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-129">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="78a7d-130">При выполнении за службами IIS обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="78a7d-130">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="78a7d-131">Настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="78a7d-131">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="78a7d-132">Модуль создает обратный прокси-сервер между службами IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="78a7d-132">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="78a7d-133">Кроме того, настраивает [перехват приложением ошибок запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="78a7d-133">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="78a7d-134">Параметры IIS по умолчанию см. в разделе <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-134">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="78a7d-135">Устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="78a7d-135">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="78a7d-136">Дополнительные сведения см. в разделе [Проверка области](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="78a7d-136">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="78a7d-137">Настройки, определенные `CreateDefaultBuilder`, можно переопределить и усилить с помощью [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) и других методов и методов расширения [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="78a7d-137">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="78a7d-138">Ниже приведены некоторые примеры:</span><span class="sxs-lookup"><span data-stu-id="78a7d-138">A few examples follow:</span></span>

* <span data-ttu-id="78a7d-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) используется для указания дополнительного объекта `IConfiguration` для приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-139">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="78a7d-140">Следующий вызов `ConfigureAppConfiguration` добавляет делегат, чтобы включить конфигурацию приложения в файл *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-140">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="78a7d-141">`ConfigureAppConfiguration` можно вызывать несколько раз.</span><span class="sxs-lookup"><span data-stu-id="78a7d-141">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="78a7d-142">Обратите внимание, что эта конфигурация не распространяется на узел (например, URL-адреса серверов или среду).</span><span class="sxs-lookup"><span data-stu-id="78a7d-142">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="78a7d-143">См. раздел [Значения конфигурации узла](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="78a7d-143">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="78a7d-144">Следующий вызов `ConfigureLogging` добавляет делегата, чтобы настроить минимальный уровень ведения журнала ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) для [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="78a7d-144">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="78a7d-145">Этот параметр переопределяет параметры в *appsettings.Development.json* (`LogLevel.Debug`) и *appsettings.Production.json* (`LogLevel.Error`), заданные `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-145">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="78a7d-146">`ConfigureLogging` можно вызывать несколько раз.</span><span class="sxs-lookup"><span data-stu-id="78a7d-146">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="78a7d-147">При следующем вызове к `ConfigureKestrel` переопределяется значение по умолчанию [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), равное 30 000 000 байтов, установленное при настройке Kestrel методом `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-147">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range="= aspnetcore-2.0 || aspnetcore-2.1"

* <span data-ttu-id="78a7d-148">Следующий вызов [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) переопределяет значение по умолчанию [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), равное 30 000 000 байтов, установленное при настройке Kestrel методом `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="78a7d-149">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="78a7d-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="78a7d-150">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="78a7d-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="78a7d-151">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="78a7d-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="78a7d-152">Дополнительные сведения о конфигурации приложения см. в разделе <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-152">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="78a7d-153">Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="78a7d-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="78a7d-154">Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="78a7d-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="78a7d-155">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="78a7d-155">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="78a7d-156">Узел обычно создается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-156">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="78a7d-157">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-157">In the project templates, `Main` is located in *Program.cs*:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args) =>
        WebHost.CreateDefaultBuilder(args)
            .UseStartup<Startup>()
            .Build();
}
```

<span data-ttu-id="78a7d-158">Для `WebHostBuilder` требуется [сервер, реализующий интерфейс IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="78a7d-158">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="78a7d-159">Встроенными серверами являются [Kestrel](xref:fundamentals/servers/kestrel) и [HTTP.sys](xref:fundamentals/servers/httpsys) (до выхода ASP.NET Core 2.0 сервер HTTP.sys назывался [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="78a7d-159">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="78a7d-160">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) задает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="78a7d-160">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="78a7d-161">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="78a7d-161">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="78a7d-162">Для получения корня содержимого по умолчанию для `UseContentRoot` используется метод [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="78a7d-162">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="78a7d-163">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="78a7d-163">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="78a7d-164">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="78a7d-164">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="78a7d-165">Чтобы использовать службы IIS в качестве обратного прокси-сервера, вызовите метод [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) в процессе создания узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-165">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="78a7d-166">Метод `UseIISIntegration` не настраивает *сервер*, как это делает метод [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="78a7d-166">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="78a7d-167">`UseIISIntegration` настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для создания обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="78a7d-167">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="78a7d-168">Чтобы использовать службы IIS с ASP.NET Core, нужно указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-168">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="78a7d-169">`UseIISIntegration` активирует только при выполнении за службами IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="78a7d-169">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="78a7d-170">Дополнительные сведения см. в разделах <xref:fundamentals/servers/aspnet-core-module> и <xref:host-and-deploy/aspnet-core-module>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-170">For more information, see <xref:fundamentals/servers/aspnet-core-module> and <xref:host-and-deploy/aspnet-core-module>.</span></span>

<span data-ttu-id="78a7d-171">Минимальная реализация для настройки узла (и приложения ASP.NET Core) включает в себя указание сервера и настройку конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-171">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

```csharp
var host = new WebHostBuilder()
    .UseKestrel()
    .Configure(app =>
    {
        app.Run(context => context.Response.WriteAsync("Hello World!"));
    })
    .Build();

host.Run();
```

::: moniker-end

<span data-ttu-id="78a7d-172">При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="78a7d-172">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="78a7d-173">Если используется класс `Startup`, в нем должен быть определен метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-173">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="78a7d-174">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-174">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="78a7d-175">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="78a7d-175">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="78a7d-176">При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.</span><span class="sxs-lookup"><span data-stu-id="78a7d-176">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="78a7d-177">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="78a7d-177">Host configuration values</span></span>

<span data-ttu-id="78a7d-178">Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="78a7d-178">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="78a7d-179">конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`,</span><span class="sxs-lookup"><span data-stu-id="78a7d-179">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="78a7d-180">Например, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-180">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="78a7d-181">Расширения, такие как [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) и [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (см. раздел [Переопределение конфигурации](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="78a7d-181">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="78a7d-182">метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="78a7d-182">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="78a7d-183">Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="78a7d-183">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="78a7d-184">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="78a7d-184">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="78a7d-185">Дополнительные сведения см. в подразделе [Переопределение конфигурации](#override-configuration) следующего раздела.</span><span class="sxs-lookup"><span data-stu-id="78a7d-185">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="78a7d-186">Ключ приложения (имя)</span><span class="sxs-lookup"><span data-stu-id="78a7d-186">Application Key (Name)</span></span>

<span data-ttu-id="78a7d-187">Свойство [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) задается автоматически при вызове [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) или [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-187">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="78a7d-188">Значение присваивается имени сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-188">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="78a7d-189">Чтобы явно задать значение, используйте [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey).</span><span class="sxs-lookup"><span data-stu-id="78a7d-189">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="78a7d-190">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="78a7d-190">**Key**: applicationName</span></span>  
<span data-ttu-id="78a7d-191">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-191">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-192">**По умолчанию**: имя сборки, содержащей точку входа приложения</span><span class="sxs-lookup"><span data-stu-id="78a7d-192">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="78a7d-193">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="78a7d-193">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="78a7d-194">**Переменная среды**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="78a7d-194">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

::: moniker range=">= aspnetcore-2.1"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

::: moniker-end

::: moniker range="< aspnetcore-2.1"

```csharp
var host = new WebHostBuilder()
    .UseSetting("applicationName", "CustomApplicationName")
```

::: moniker-end

### <a name="capture-startup-errors"></a><span data-ttu-id="78a7d-195">Перехват ошибок при загрузке</span><span class="sxs-lookup"><span data-stu-id="78a7d-195">Capture Startup Errors</span></span>

<span data-ttu-id="78a7d-196">Этот параметр управляет перехватом ошибок при загрузке.</span><span class="sxs-lookup"><span data-stu-id="78a7d-196">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="78a7d-197">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="78a7d-197">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="78a7d-198">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="78a7d-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="78a7d-199">**Значение по умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-199">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="78a7d-200">**Задается с помощью**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="78a7d-200">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="78a7d-201">**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="78a7d-201">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="78a7d-202">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-202">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="78a7d-203">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="78a7d-203">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

::: moniker-end

### <a name="content-root"></a><span data-ttu-id="78a7d-204">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="78a7d-204">Content Root</span></span>

<span data-ttu-id="78a7d-205">Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC.</span><span class="sxs-lookup"><span data-stu-id="78a7d-205">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="78a7d-206">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="78a7d-206">**Key**: contentRoot</span></span>  
<span data-ttu-id="78a7d-207">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-207">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-208">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-208">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="78a7d-209">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="78a7d-209">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="78a7d-210">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="78a7d-210">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="78a7d-211">Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root).</span><span class="sxs-lookup"><span data-stu-id="78a7d-211">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="78a7d-212">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="78a7d-212">If the path doesn't exist, the host fails to start.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

::: moniker-end

### <a name="detailed-errors"></a><span data-ttu-id="78a7d-213">Подробные сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="78a7d-213">Detailed Errors</span></span>

<span data-ttu-id="78a7d-214">Определяет, следует ли перехватывать подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="78a7d-214">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="78a7d-215">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="78a7d-215">**Key**: detailedErrors</span></span>  
<span data-ttu-id="78a7d-216">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="78a7d-216">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="78a7d-217">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="78a7d-217">**Default**: false</span></span>  
<span data-ttu-id="78a7d-218">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="78a7d-218">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="78a7d-219">**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="78a7d-219">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="78a7d-220">Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-220">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

::: moniker-end

### <a name="environment"></a><span data-ttu-id="78a7d-221">Среда</span><span class="sxs-lookup"><span data-stu-id="78a7d-221">Environment</span></span>

<span data-ttu-id="78a7d-222">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-222">Sets the app's environment.</span></span>

<span data-ttu-id="78a7d-223">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="78a7d-223">**Key**: environment</span></span>  
<span data-ttu-id="78a7d-224">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-224">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-225">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="78a7d-225">**Default**: Production</span></span>  
<span data-ttu-id="78a7d-226">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="78a7d-226">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="78a7d-227">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="78a7d-227">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="78a7d-228">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="78a7d-228">The environment can be set to any value.</span></span> <span data-ttu-id="78a7d-229">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-229">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="78a7d-230">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="78a7d-230">Values aren't case sensitive.</span></span> <span data-ttu-id="78a7d-231">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-231">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="78a7d-232">При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-232">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="78a7d-233">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-233">For more information, see <xref:fundamentals/environments>.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="78a7d-234">Начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="78a7d-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="78a7d-235">Задает начальные сборки размещения для приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="78a7d-236">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="78a7d-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="78a7d-237">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-237">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-238">**Значение по умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="78a7d-238">**Default**: Empty string</span></span>  
<span data-ttu-id="78a7d-239">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="78a7d-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="78a7d-240">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="78a7d-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="78a7d-241">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="78a7d-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="78a7d-242">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-242">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="78a7d-243">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="78a7d-243">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="https-port"></a><span data-ttu-id="78a7d-244">HTTPS-порт</span><span class="sxs-lookup"><span data-stu-id="78a7d-244">HTTPS Port</span></span>

<span data-ttu-id="78a7d-245">Задайте порт перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="78a7d-245">Set the HTTPS redirect port.</span></span> <span data-ttu-id="78a7d-246">Используется при [принудительном применении HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="78a7d-246">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="78a7d-247">**Ключ**: https_port **Тип**: *строка*
**Значение по умолчанию**: не задано.</span><span class="sxs-lookup"><span data-stu-id="78a7d-247">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="78a7d-248">**Настраивается с помощью**:`UseSetting`
**Переменная среды**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="78a7d-248">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="78a7d-249">Исключаемые начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="78a7d-249">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="78a7d-250">Разделенная точками с запятой строка начальных сборок размещения, которые необходимо исключить при запуске.</span><span class="sxs-lookup"><span data-stu-id="78a7d-250">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="78a7d-251">**Ключ**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="78a7d-251">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="78a7d-252">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-252">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-253">**Значение по умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="78a7d-253">**Default**: Empty string</span></span>  
<span data-ttu-id="78a7d-254">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="78a7d-254">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="78a7d-255">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="78a7d-255">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prefer-hosting-urls"></a><span data-ttu-id="78a7d-256">Предпочитать URL-адреса размещения</span><span class="sxs-lookup"><span data-stu-id="78a7d-256">Prefer Hosting URLs</span></span>

<span data-ttu-id="78a7d-257">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-257">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="78a7d-258">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="78a7d-258">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="78a7d-259">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="78a7d-259">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="78a7d-260">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="78a7d-260">**Default**: true</span></span>  
<span data-ttu-id="78a7d-261">**Задается с помощью**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="78a7d-261">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="78a7d-262">**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="78a7d-262">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="prevent-hosting-startup"></a><span data-ttu-id="78a7d-263">Запретить запуск размещения</span><span class="sxs-lookup"><span data-stu-id="78a7d-263">Prevent Hosting Startup</span></span>

<span data-ttu-id="78a7d-264">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-264">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="78a7d-265">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-265">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="78a7d-266">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="78a7d-266">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="78a7d-267">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="78a7d-267">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="78a7d-268">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="78a7d-268">**Default**: false</span></span>  
<span data-ttu-id="78a7d-269">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="78a7d-269">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="78a7d-270">**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="78a7d-270">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

::: moniker-end

### <a name="server-urls"></a><span data-ttu-id="78a7d-271">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="78a7d-271">Server URLs</span></span>

<span data-ttu-id="78a7d-272">Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="78a7d-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="78a7d-273">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="78a7d-273">**Key**: urls</span></span>  
<span data-ttu-id="78a7d-274">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-274">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-275">**По умолчанию**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="78a7d-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="78a7d-276">**Задается с помощью**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="78a7d-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="78a7d-277">**Переменная среды**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="78a7d-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="78a7d-278">Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="78a7d-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="78a7d-279">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="78a7d-280">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="78a7d-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="78a7d-281">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="78a7d-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="78a7d-282">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="78a7d-282">Supported formats vary between servers.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="78a7d-283">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="78a7d-283">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="78a7d-284">Для получения дополнительной информации см. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-284">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

### <a name="shutdown-timeout"></a><span data-ttu-id="78a7d-285">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="78a7d-285">Shutdown Timeout</span></span>

<span data-ttu-id="78a7d-286">Определяет, как долго необходимо ожидать завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-286">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="78a7d-287">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="78a7d-287">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="78a7d-288">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="78a7d-288">**Type**: *int*</span></span>  
<span data-ttu-id="78a7d-289">**Значение по умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="78a7d-289">**Default**: 5</span></span>  
<span data-ttu-id="78a7d-290">**Задается с помощью**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="78a7d-290">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="78a7d-291">**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="78a7d-291">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="78a7d-292">Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) принимает [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="78a7d-292">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="78a7d-293">Во время ожидания размещение:</span><span class="sxs-lookup"><span data-stu-id="78a7d-293">During the timeout period, hosting:</span></span>

* <span data-ttu-id="78a7d-294">Активирует [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="78a7d-294">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="78a7d-295">Пытается остановить размещенные службы, записывая в журнал все ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="78a7d-295">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="78a7d-296">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-296">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="78a7d-297">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="78a7d-297">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="78a7d-298">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="78a7d-298">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

::: moniker-end

### <a name="startup-assembly"></a><span data-ttu-id="78a7d-299">Стартовая сборка</span><span class="sxs-lookup"><span data-stu-id="78a7d-299">Startup Assembly</span></span>

<span data-ttu-id="78a7d-300">Определяет сборку, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-300">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="78a7d-301">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="78a7d-301">**Key**: startupAssembly</span></span>  
<span data-ttu-id="78a7d-302">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-302">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-303">**Значение по умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="78a7d-303">**Default**: The app's assembly</span></span>  
<span data-ttu-id="78a7d-304">**Задается с помощью**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="78a7d-304">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="78a7d-305">**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="78a7d-305">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="78a7d-306">На сборку можно ссылаться по имени (`string`) или типу (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="78a7d-306">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="78a7d-307">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="78a7d-307">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

::: moniker-end

### <a name="web-root"></a><span data-ttu-id="78a7d-308">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="78a7d-308">Web Root</span></span>

<span data-ttu-id="78a7d-309">Задает относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-309">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="78a7d-310">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="78a7d-310">**Key**: webroot</span></span>  
<span data-ttu-id="78a7d-311">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="78a7d-311">**Type**: *string*</span></span>  
<span data-ttu-id="78a7d-312">**Значение по умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии.</span><span class="sxs-lookup"><span data-stu-id="78a7d-312">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="78a7d-313">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="78a7d-313">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="78a7d-314">**Задается с помощью**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="78a7d-314">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="78a7d-315">**Переменная среды**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="78a7d-315">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

::: moniker range=">= aspnetcore-2.0"

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

::: moniker-end

## <a name="override-configuration"></a><span data-ttu-id="78a7d-316">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="78a7d-316">Override configuration</span></span>

<span data-ttu-id="78a7d-317">Используйте [конфигурацию](xref:fundamentals/configuration/index) для настройки веб-узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-317">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="78a7d-318">В приведенном ниже примере необязательная конфигурация узла задается в файле *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="78a7d-318">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="78a7d-319">Любую конфигурацию, загружаемую из файла *hostsettings.json*, можно переопределить с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="78a7d-319">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="78a7d-320">Встроенная конфигурация (в `config`) используется для настройки узла с помощью [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="78a7d-320">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="78a7d-321">Конфигурация `IWebHostBuilder` добавляется в конфигурацию приложения, но не наоборот &mdash; `ConfigureAppConfiguration` не влияет на конфигурацию `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-321">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="78a7d-322">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hostsettings.json*, а затем с помощью аргументов командной строки:</span><span class="sxs-lookup"><span data-stu-id="78a7d-322">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateWebHostBuilder(args).Build().Run();
    }

    public static IWebHostBuilder CreateWebHostBuilder(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        return WebHost.CreateDefaultBuilder(args)
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();
    }
}
```

<span data-ttu-id="78a7d-323">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="78a7d-323">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="78a7d-324">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hostsettings.json*, а затем с помощью аргументов командной строки:</span><span class="sxs-lookup"><span data-stu-id="78a7d-324">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hostsettings.json", optional: true)
            .AddCommandLine(args)
            .Build();

        var host = new WebHostBuilder()
            .UseUrls("http://*:5000")
            .UseConfiguration(config)
            .UseKestrel()
            .Configure(app =>
            {
                app.Run(context => 
                    context.Response.WriteAsync("Hello, World!"));
            })
            .Build();

        host.Run();
    }
}
```

<span data-ttu-id="78a7d-325">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="78a7d-325">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="78a7d-326">Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="78a7d-326">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="78a7d-327">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="78a7d-327">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="78a7d-328">Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="78a7d-328">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="78a7d-329">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="78a7d-329">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="78a7d-330">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="78a7d-330">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="78a7d-331">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="78a7d-331">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="78a7d-332">`UseConfiguration` копирует ключи только из предоставленного объекта `IConfiguration` для конфигурации конструктора узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-332">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="78a7d-333">Поэтому указание `reloadOnChange: true` для файлов JSON, XML и INI ни на что не влияет.</span><span class="sxs-lookup"><span data-stu-id="78a7d-333">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="78a7d-334">Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение из командной строки при выполнении команды [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="78a7d-334">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="78a7d-335">Аргумент командной строки переопределяет значение `urls` из файла *hostsettings.json*, и сервер будет ожидать передачи данных через порт 8080:</span><span class="sxs-lookup"><span data-stu-id="78a7d-335">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="78a7d-336">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="78a7d-336">Manage the host</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="78a7d-337">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="78a7d-337">**Run**</span></span>

<span data-ttu-id="78a7d-338">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="78a7d-338">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="78a7d-339">**Запуск**</span><span class="sxs-lookup"><span data-stu-id="78a7d-339">**Start**</span></span>

<span data-ttu-id="78a7d-340">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-340">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="78a7d-341">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="78a7d-341">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

<span data-ttu-id="78a7d-342">Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода.</span><span class="sxs-lookup"><span data-stu-id="78a7d-342">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="78a7d-343">Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="78a7d-343">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="78a7d-344">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="78a7d-344">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="78a7d-345">Выполните запуск с помощью `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-345">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="78a7d-346">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="78a7d-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="78a7d-347">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="78a7d-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="78a7d-348">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="78a7d-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="78a7d-349">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="78a7d-349">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="78a7d-350">Выполните запуск с помощью URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-350">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="78a7d-351">Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-351">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="78a7d-352">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="78a7d-352">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="78a7d-353">Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="78a7d-353">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

```csharp
using (var host = WebHost.Start(router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="78a7d-354">В этом примере используйте следующие запросы в браузере:</span><span class="sxs-lookup"><span data-stu-id="78a7d-354">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="78a7d-355">Запрос</span><span class="sxs-lookup"><span data-stu-id="78a7d-355">Request</span></span>                                    | <span data-ttu-id="78a7d-356">Ответ</span><span class="sxs-lookup"><span data-stu-id="78a7d-356">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="78a7d-357">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="78a7d-357">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="78a7d-358">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="78a7d-358">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="78a7d-359">Вызывает исключение со строкой "ooops!"</span><span class="sxs-lookup"><span data-stu-id="78a7d-359">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="78a7d-360">Вызывает исключение со строкой "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="78a7d-360">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="78a7d-361">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="78a7d-361">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="78a7d-362">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="78a7d-362">Hello World!</span></span>                             |

<span data-ttu-id="78a7d-363">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="78a7d-363">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="78a7d-364">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="78a7d-364">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="78a7d-365">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="78a7d-365">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="78a7d-366">Используйте URL-адрес и экземпляр `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-366">Use a URL and an instance of `IRouteBuilder`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", router => router
    .MapGet("hello/{name}", (req, res, data) => 
        res.WriteAsync($"Hello, {data.Values["name"]}!"))
    .MapGet("buenosdias/{name}", (req, res, data) => 
        res.WriteAsync($"Buenos dias, {data.Values["name"]}!"))
    .MapGet("throw/{message?}", (req, res, data) => 
        throw new Exception((string)data.Values["message"] ?? "Uh oh!"))
    .MapGet("{greeting}/{name}", (req, res, data) => 
        res.WriteAsync($"{data.Values["greeting"]}, {data.Values["name"]}!"))
    .MapGet("", (req, res, data) => res.WriteAsync("Hello, World!"))))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="78a7d-367">Результат будет тем же, что и при использовании **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-367">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="78a7d-368">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="78a7d-368">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="78a7d-369">Предоставьте делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-369">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith(app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="78a7d-370">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="78a7d-370">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="78a7d-371">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="78a7d-371">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="78a7d-372">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="78a7d-372">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="78a7d-373">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="78a7d-373">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="78a7d-374">Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-374">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

```csharp
using (var host = WebHost.StartWith("http://localhost:8080", app => 
    app.Use(next => 
    {
        return async context => 
        {
            await context.Response.WriteAsync("Hello World!");
        };
    })))
{
    Console.WriteLine("Use Ctrl-C to shut down the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="78a7d-375">Результат будет тем же, что и при использовании **StartWith(Action&lt;IApplicationBuilder&gt; app)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-375">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.0"

<span data-ttu-id="78a7d-376">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="78a7d-376">**Run**</span></span>

<span data-ttu-id="78a7d-377">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="78a7d-377">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="78a7d-378">**Запуск**</span><span class="sxs-lookup"><span data-stu-id="78a7d-378">**Start**</span></span>

<span data-ttu-id="78a7d-379">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-379">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="78a7d-380">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="78a7d-380">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

```csharp
var urls = new List<string>()
{
    "http://*:5000",
    "http://localhost:5001"
};

var host = new WebHostBuilder()
    .UseKestrel()
    .UseStartup<Startup>()
    .Start(urls.ToArray());

using (host)
{
    Console.ReadLine();
}
```

::: moniker-end

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="78a7d-381">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="78a7d-381">IHostingEnvironment interface</span></span>

<span data-ttu-id="78a7d-382">[Интерфейс IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-382">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="78a7d-383">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="78a7d-383">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

```csharp
public class CustomFileReader
{
    private readonly IHostingEnvironment _env;

    public CustomFileReader(IHostingEnvironment env)
    {
        _env = env;
    }

    public string ReadFile(string filePath)
    {
        var fileProvider = _env.WebRootFileProvider;
        // Process the file here
    }
}
```

<span data-ttu-id="78a7d-384">Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="78a7d-384">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="78a7d-385">Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-385">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

```csharp
public class Startup
{
    public Startup(IHostingEnvironment env)
    {
        HostingEnvironment = env;
    }

    public IHostingEnvironment HostingEnvironment { get; }

    public void ConfigureServices(IServiceCollection services)
    {
        if (HostingEnvironment.IsDevelopment())
        {
            // Development configuration
        }
        else
        {
            // Staging/Production configuration
        }

        var contentRootPath = HostingEnvironment.ContentRootPath;
    }
}
```

> [!NOTE]
> <span data-ttu-id="78a7d-386">Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-386">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="78a7d-387">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="78a7d-387">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="78a7d-388">Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="78a7d-388">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the developer exception page
        app.UseDeveloperExceptionPage();
    }
    else
    {
        // In Staging/Production, route exceptions to /error
        app.UseExceptionHandler("/error");
    }

    var contentRootPath = env.ContentRootPath;
}
```

<span data-ttu-id="78a7d-389">`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware/index#write-middleware):</span><span class="sxs-lookup"><span data-stu-id="78a7d-389">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#write-middleware):</span></span>

```csharp
public async Task Invoke(HttpContext context, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // Configure middleware for Development
    }
    else
    {
        // Configure middleware for Staging/Production
    }

    var contentRootPath = env.ContentRootPath;
}
```

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="78a7d-390">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="78a7d-390">IApplicationLifetime interface</span></span>

<span data-ttu-id="78a7d-391">[Интерфейс IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="78a7d-391">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="78a7d-392">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="78a7d-392">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="78a7d-393">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="78a7d-393">Cancellation Token</span></span>    | <span data-ttu-id="78a7d-394">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="78a7d-394">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="78a7d-395">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="78a7d-395">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="78a7d-396">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="78a7d-396">The host has fully started.</span></span> |
| [<span data-ttu-id="78a7d-397">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="78a7d-397">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="78a7d-398">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-398">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="78a7d-399">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="78a7d-399">All requests should be processed.</span></span> <span data-ttu-id="78a7d-400">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="78a7d-400">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="78a7d-401">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="78a7d-401">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="78a7d-402">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="78a7d-402">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="78a7d-403">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="78a7d-403">Requests may still be processing.</span></span> <span data-ttu-id="78a7d-404">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="78a7d-404">Shutdown blocks until this event completes.</span></span> |

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app, IApplicationLifetime appLifetime)
    {
        appLifetime.ApplicationStarted.Register(OnStarted);
        appLifetime.ApplicationStopping.Register(OnStopping);
        appLifetime.ApplicationStopped.Register(OnStopped);

        Console.CancelKeyPress += (sender, eventArgs) =>
        {
            appLifetime.StopApplication();
            // Don't terminate the process immediately, wait for the Main thread to exit gracefully.
            eventArgs.Cancel = true;
        };
    }

    private void OnStarted()
    {
        // Perform post-startup activities here
    }

    private void OnStopping()
    {
        // Perform on-stopping activities here
    }

    private void OnStopped()
    {
        // Perform post-stopped activities here
    }
}
```

<span data-ttu-id="78a7d-405">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-405">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="78a7d-406">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-406">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="78a7d-407">Проверка области</span><span class="sxs-lookup"><span data-stu-id="78a7d-407">Scope validation</span></span>

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="78a7d-408">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="78a7d-408">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

::: moniker-end

<span data-ttu-id="78a7d-409">Если для `ValidateScopes` установлено значение `true`, поставщик службы по умолчанию проверяет, что:</span><span class="sxs-lookup"><span data-stu-id="78a7d-409">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="78a7d-410">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="78a7d-410">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="78a7d-411">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="78a7d-411">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="78a7d-412">Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="78a7d-412">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="78a7d-413">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="78a7d-413">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="78a7d-414">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="78a7d-414">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="78a7d-415">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="78a7d-415">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="78a7d-416">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-416">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="78a7d-417">Чтобы всегда проверять области, в том числе в рабочей среде, настройте [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) с [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) в конструкторе узлов:</span><span class="sxs-lookup"><span data-stu-id="78a7d-417">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

::: moniker range="= aspnetcore-2.0"

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="78a7d-418">Устранение неполадок, связанных с исключениями System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="78a7d-418">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="78a7d-419">**Следующее относится только к приложениям ASP.NET Core 2.0, если приложение не вызывает метод `UseStartup` или `Configure`.**</span><span class="sxs-lookup"><span data-stu-id="78a7d-419">**The following only applies to ASP.NET Core 2.0 apps when the app doesn't call `UseStartup` or `Configure`.**</span></span>

<span data-ttu-id="78a7d-420">Узел может создаваться путем внедрения интерфейса `IStartup` непосредственно в контейнер внедрения зависимостей, а не путем вызова `UseStartup` или `Configure`:</span><span class="sxs-lookup"><span data-stu-id="78a7d-420">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="78a7d-421">Если узел создается таким способом, может произойти следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="78a7d-421">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="78a7d-422">Причина в том, что имя приложения (имя текущей сборки) требуется для проверки `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="78a7d-422">This occurs because the app name (the name of the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="78a7d-423">Если в приложении интерфейс `IStartup` вручную внедряется в контейнер внедрения зависимостей, добавьте следующий вызов в `WebHostBuilder` с указанием имени сборки:</span><span class="sxs-lookup"><span data-stu-id="78a7d-423">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "AssemblyName")
```

<span data-ttu-id="78a7d-424">Кроме того, в `WebHostBuilder` можно добавить метод-заглушку `Configure`, который автоматически задает имя приложения.</span><span class="sxs-lookup"><span data-stu-id="78a7d-424">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the app name automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
```

<span data-ttu-id="78a7d-425">Дополнительные сведения см. в [комментарии к объявлению об удалении пакета Microsoft.Extensions.PlatformAbstractions](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и в [примере использования StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="78a7d-425">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="78a7d-426">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78a7d-426">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
