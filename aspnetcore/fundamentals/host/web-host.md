---
title: Веб-узел ASP.NET Core
author: guardrex
description: Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования.
ms.author: riande
ms.custom: mvc
ms.date: 06/19/2018
uid: fundamentals/host/web-host
ms.openlocfilehash: 98070f49c98919e7ebff41ecc69c953249977dcc
ms.sourcegitcommit: e22097b84d26a812cd1380a6b2d12c93e522c125
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36314153"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="22180-103">Веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="22180-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="22180-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="22180-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="22180-105">Приложения ASP.NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="22180-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="22180-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="22180-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="22180-107">Узел настраивает как минимум сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="22180-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="22180-108">В этой статье рассматривается веб-узел ASP.NET ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), который используется для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="22180-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="22180-109">Сведения об универсальном узле .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="22180-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="22180-110">Создание узла</span><span class="sxs-lookup"><span data-stu-id="22180-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="22180-112">Создайте узел с помощью экземпляра [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="22180-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="22180-113">Обычно это делается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="22180-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="22180-114">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="22180-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="22180-115">Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.</span><span class="sxs-lookup"><span data-stu-id="22180-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="22180-116">Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.</span><span class="sxs-lookup"><span data-stu-id="22180-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="22180-117">Настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает сервер с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="22180-118">Параметры Kestrel по умолчанию см. в разделе [Параметры Kestrel](xref:fundamentals/servers/kestrel#kestrel-options) статьи "Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="22180-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="22180-119">В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="22180-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="22180-120">Загружает [конфигурацию узла](#host-configuration-values) из:</span><span class="sxs-lookup"><span data-stu-id="22180-120">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="22180-121">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="22180-121">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="22180-122">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="22180-122">Command-line arguments.</span></span>
* <span data-ttu-id="22180-123">Загружает конфигурацию приложения из:</span><span class="sxs-lookup"><span data-stu-id="22180-123">Loads app configuration from:</span></span>
  * <span data-ttu-id="22180-124">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="22180-124">*appsettings.json*.</span></span>
  * <span data-ttu-id="22180-125">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="22180-125">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="22180-126">[секреты пользователя](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок;</span><span class="sxs-lookup"><span data-stu-id="22180-126">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="22180-127">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="22180-127">Environment variables.</span></span>
  * <span data-ttu-id="22180-128">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="22180-128">Command-line arguments.</span></span>
* <span data-ttu-id="22180-129">Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="22180-129">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="22180-130">Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="22180-130">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="22180-131">При выполнении за службами IIS обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="22180-131">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="22180-132">Настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="22180-132">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="22180-133">Модуль создает обратный прокси-сервер между службами IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22180-133">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="22180-134">Кроме того, настраивает [перехват приложением ошибок запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="22180-134">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="22180-135">Параметры служб IIS по умолчанию см. в разделе [Параметры служб IIS](xref:host-and-deploy/iis/index#iis-options) статьи "Размещение ASP.NET Core в Windows со службами IIS".</span><span class="sxs-lookup"><span data-stu-id="22180-135">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="22180-136">Устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="22180-136">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="22180-137">Дополнительные сведения см. в разделе [Проверка области](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="22180-137">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="22180-138">Настройки, определенные `CreateDefaultBuilder`, можно переопределить и усилить с помощью [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) и других методов и методов расширения [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="22180-138">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="22180-139">Ниже приведены некоторые примеры:</span><span class="sxs-lookup"><span data-stu-id="22180-139">A few examples follow:</span></span>

* <span data-ttu-id="22180-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) используется для указания дополнительного объекта `IConfiguration` для приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-140">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="22180-141">Следующий вызов `ConfigureAppConfiguration` добавляет делегат, чтобы включить конфигурацию приложения в файл *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="22180-141">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="22180-142">`ConfigureAppConfiguration` можно вызывать несколько раз.</span><span class="sxs-lookup"><span data-stu-id="22180-142">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="22180-143">Обратите внимание, что эта конфигурация не распространяется на узел (например, URL-адреса серверов или среду).</span><span class="sxs-lookup"><span data-stu-id="22180-143">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="22180-144">См. раздел [Значения конфигурации узла](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="22180-144">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="22180-145">Следующий вызов `ConfigureLogging` добавляет делегата, чтобы настроить минимальный уровень ведения журнала ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) для [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="22180-145">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="22180-146">Этот параметр переопределяет параметры в *appsettings.Development.json* (`LogLevel.Debug`) и *appsettings.Production.json* (`LogLevel.Error`), заданные `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="22180-146">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="22180-147">`ConfigureLogging` можно вызывать несколько раз.</span><span class="sxs-lookup"><span data-stu-id="22180-147">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

* <span data-ttu-id="22180-148">Следующий вызов [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) переопределяет значение по умолчанию [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), равное 30 000 000 байтов, установленное при настройке Kestrel методом `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="22180-148">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
        ...
    ```

<span data-ttu-id="22180-149">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="22180-149">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="22180-150">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="22180-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="22180-151">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="22180-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="22180-152">Дополнительные сведения о конфигурации приложения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="22180-152">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="22180-153">Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="22180-153">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="22180-154">Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="22180-154">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-155">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-155">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="22180-156">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="22180-156">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="22180-157">Узел обычно создается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="22180-157">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="22180-158">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="22180-158">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="22180-159">Для `WebHostBuilder` требуется [сервер, реализующий интерфейс IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="22180-159">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="22180-160">Встроенными серверами являются [Kestrel](xref:fundamentals/servers/kestrel) и [HTTP.sys](xref:fundamentals/servers/httpsys) (до выхода ASP.NET Core 2.0 сервер HTTP.sys назывался [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="22180-160">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="22180-161">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) задает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="22180-161">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="22180-162">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="22180-162">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="22180-163">Для получения корня содержимого по умолчанию для `UseContentRoot` используется метод [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="22180-163">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="22180-164">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="22180-164">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="22180-165">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="22180-165">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="22180-166">Чтобы использовать службы IIS в качестве обратного прокси-сервера, вызовите метод [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) в процессе создания узла.</span><span class="sxs-lookup"><span data-stu-id="22180-166">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="22180-167">Метод `UseIISIntegration` не настраивает *сервер*, как это делает метод [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="22180-167">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="22180-168">`UseIISIntegration` настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для создания обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="22180-168">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="22180-169">Чтобы использовать службы IIS с ASP.NET Core, нужно указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="22180-169">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="22180-170">`UseIISIntegration` активирует только при выполнении за службами IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="22180-170">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="22180-171">Дополнительные сведения см. в разделах [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="22180-171">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="22180-172">Минимальная реализация для настройки узла (и приложения ASP.NET Core) включает в себя указание сервера и настройку конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-172">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

---

<span data-ttu-id="22180-173">При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="22180-173">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="22180-174">Если используется класс `Startup`, в нем должен быть определен метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="22180-174">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="22180-175">Дополнительные сведения см. в разделе [Запуск приложения в ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="22180-175">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="22180-176">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="22180-176">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="22180-177">При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.</span><span class="sxs-lookup"><span data-stu-id="22180-177">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="22180-178">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="22180-178">Host configuration values</span></span>

<span data-ttu-id="22180-179">Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="22180-179">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="22180-180">конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`,</span><span class="sxs-lookup"><span data-stu-id="22180-180">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="22180-181">Например, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="22180-181">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="22180-182">Расширения, такие как [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) и [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (см. раздел [Переопределение конфигурации](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="22180-182">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="22180-183">метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="22180-183">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="22180-184">Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="22180-184">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="22180-185">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="22180-185">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="22180-186">Дополнительные сведения см. в подразделе [Переопределение конфигурации](#override-configuration) следующего раздела.</span><span class="sxs-lookup"><span data-stu-id="22180-186">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="22180-187">Перехват ошибок при загрузке</span><span class="sxs-lookup"><span data-stu-id="22180-187">Capture Startup Errors</span></span>

<span data-ttu-id="22180-188">Этот параметр управляет перехватом ошибок при загрузке.</span><span class="sxs-lookup"><span data-stu-id="22180-188">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="22180-189">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="22180-189">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="22180-190">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="22180-190">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="22180-191">**Значение по умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="22180-191">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="22180-192">**Задается с помощью**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="22180-192">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="22180-193">**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="22180-193">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="22180-194">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="22180-194">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="22180-195">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="22180-195">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-196">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-196">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-197">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-197">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="22180-198">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="22180-198">Content Root</span></span>

<span data-ttu-id="22180-199">Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC.</span><span class="sxs-lookup"><span data-stu-id="22180-199">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="22180-200">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="22180-200">**Key**: contentRoot</span></span>  
<span data-ttu-id="22180-201">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="22180-201">**Type**: *string*</span></span>  
<span data-ttu-id="22180-202">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-202">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="22180-203">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="22180-203">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="22180-204">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="22180-204">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="22180-205">Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root).</span><span class="sxs-lookup"><span data-stu-id="22180-205">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="22180-206">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="22180-206">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-207">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-207">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-208">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-208">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="22180-209">Подробные сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="22180-209">Detailed Errors</span></span>

<span data-ttu-id="22180-210">Определяет, следует ли перехватывать подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="22180-210">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="22180-211">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="22180-211">**Key**: detailedErrors</span></span>  
<span data-ttu-id="22180-212">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="22180-212">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="22180-213">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="22180-213">**Default**: false</span></span>  
<span data-ttu-id="22180-214">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="22180-214">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="22180-215">**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="22180-215">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="22180-216">Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.</span><span class="sxs-lookup"><span data-stu-id="22180-216">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-217">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-217">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-218">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-218">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="22180-219">Среда</span><span class="sxs-lookup"><span data-stu-id="22180-219">Environment</span></span>

<span data-ttu-id="22180-220">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-220">Sets the app's environment.</span></span>

<span data-ttu-id="22180-221">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="22180-221">**Key**: environment</span></span>  
<span data-ttu-id="22180-222">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="22180-222">**Type**: *string*</span></span>  
<span data-ttu-id="22180-223">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="22180-223">**Default**: Production</span></span>  
<span data-ttu-id="22180-224">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="22180-224">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="22180-225">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="22180-225">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="22180-226">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="22180-226">The environment can be set to any value.</span></span> <span data-ttu-id="22180-227">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="22180-227">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="22180-228">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="22180-228">Values aren't case sensitive.</span></span> <span data-ttu-id="22180-229">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="22180-229">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="22180-230">При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="22180-230">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="22180-231">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="22180-231">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-232">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-232">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-233">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-233">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="22180-234">Начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="22180-234">Hosting Startup Assemblies</span></span>

<span data-ttu-id="22180-235">Задает начальные сборки размещения для приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-235">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="22180-236">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="22180-236">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="22180-237">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="22180-237">**Type**: *string*</span></span>  
<span data-ttu-id="22180-238">**Значение по умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="22180-238">**Default**: Empty string</span></span>  
<span data-ttu-id="22180-239">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="22180-239">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="22180-240">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="22180-240">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="22180-241">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="22180-241">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="22180-242">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="22180-242">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="22180-243">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-243">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="22180-244">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="22180-244">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-245">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-245">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-246">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-246">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22180-247">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="22180-247">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="22180-248">Предпочитать URL-адреса размещения</span><span class="sxs-lookup"><span data-stu-id="22180-248">Prefer Hosting URLs</span></span>

<span data-ttu-id="22180-249">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="22180-249">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="22180-250">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="22180-250">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="22180-251">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="22180-251">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="22180-252">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="22180-252">**Default**: true</span></span>  
<span data-ttu-id="22180-253">**Задается с помощью**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="22180-253">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="22180-254">**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="22180-254">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="22180-255">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="22180-255">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-256">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-256">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-257">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-257">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22180-258">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="22180-258">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="22180-259">Запретить запуск размещения</span><span class="sxs-lookup"><span data-stu-id="22180-259">Prevent Hosting Startup</span></span>

<span data-ttu-id="22180-260">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-260">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="22180-261">Дополнительные сведения см. в разделе [Усовершенствование приложения из внешней сборки с помощью IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="22180-261">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="22180-262">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="22180-262">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="22180-263">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="22180-263">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="22180-264">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="22180-264">**Default**: false</span></span>  
<span data-ttu-id="22180-265">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="22180-265">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="22180-266">**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="22180-266">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="22180-267">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="22180-267">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-268">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-268">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22180-270">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="22180-270">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="22180-271">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="22180-271">Server URLs</span></span>

<span data-ttu-id="22180-272">Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="22180-272">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="22180-273">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="22180-273">**Key**: urls</span></span>  
<span data-ttu-id="22180-274">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="22180-274">**Type**: *string*</span></span>  
<span data-ttu-id="22180-275">**По умолчанию**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="22180-275">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="22180-276">**Задается с помощью**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="22180-276">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="22180-277">**Переменная среды**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="22180-277">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="22180-278">Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="22180-278">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="22180-279">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="22180-279">For example, `http://localhost:123`.</span></span> <span data-ttu-id="22180-280">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="22180-280">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="22180-281">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="22180-281">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="22180-282">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="22180-282">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-283">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-283">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="22180-284">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="22180-284">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="22180-285">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="22180-285">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-286">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-286">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="22180-287">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="22180-287">Shutdown Timeout</span></span>

<span data-ttu-id="22180-288">Определяет, как долго необходимо ожидать завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="22180-288">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="22180-289">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="22180-289">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="22180-290">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="22180-290">**Type**: *int*</span></span>  
<span data-ttu-id="22180-291">**Значение по умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="22180-291">**Default**: 5</span></span>  
<span data-ttu-id="22180-292">**Задается с помощью**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="22180-292">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="22180-293">**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="22180-293">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="22180-294">Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) принимает [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="22180-294">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="22180-295">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="22180-295">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="22180-296">Во время ожидания размещение:</span><span class="sxs-lookup"><span data-stu-id="22180-296">During the timeout period, hosting:</span></span>

* <span data-ttu-id="22180-297">Активирует [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="22180-297">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="22180-298">Пытается остановить размещенные службы, записывая в журнал все ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="22180-298">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="22180-299">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-299">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="22180-300">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="22180-300">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="22180-301">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="22180-301">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-302">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-302">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-303">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-303">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22180-304">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="22180-304">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="22180-305">Стартовая сборка</span><span class="sxs-lookup"><span data-stu-id="22180-305">Startup Assembly</span></span>

<span data-ttu-id="22180-306">Определяет сборку, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="22180-306">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="22180-307">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="22180-307">**Key**: startupAssembly</span></span>  
<span data-ttu-id="22180-308">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="22180-308">**Type**: *string*</span></span>  
<span data-ttu-id="22180-309">**Значение по умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="22180-309">**Default**: The app's assembly</span></span>  
<span data-ttu-id="22180-310">**Задается с помощью**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="22180-310">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="22180-311">**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="22180-311">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="22180-312">На сборку можно ссылаться по имени (`string`) или типу (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="22180-312">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="22180-313">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="22180-313">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-314">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-314">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-315">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-315">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="22180-316">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="22180-316">Web Root</span></span>

<span data-ttu-id="22180-317">Задает относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-317">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="22180-318">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="22180-318">**Key**: webroot</span></span>  
<span data-ttu-id="22180-319">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="22180-319">**Type**: *string*</span></span>  
<span data-ttu-id="22180-320">**Значение по умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии.</span><span class="sxs-lookup"><span data-stu-id="22180-320">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="22180-321">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="22180-321">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="22180-322">**Задается с помощью**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="22180-322">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="22180-323">**Переменная среды**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="22180-323">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-324">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-324">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-325">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-325">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="22180-326">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="22180-326">Override configuration</span></span>

<span data-ttu-id="22180-327">Используйте [конфигурацию](xref:fundamentals/configuration/index) для настройки веб-узла.</span><span class="sxs-lookup"><span data-stu-id="22180-327">Use [Configuration](xref:fundamentals/configuration/index) to configure the web host.</span></span> <span data-ttu-id="22180-328">В приведенном ниже примере необязательная конфигурация узла задается в файле *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="22180-328">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="22180-329">Любую конфигурацию, загружаемую из файла *hostsettings.json*, можно переопределить с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="22180-329">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="22180-330">Встроенная конфигурация (в `config`) используется для настройки узла с помощью [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="22180-330">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="22180-331">Конфигурация `IWebHostBuilder` добавляется в конфигурацию приложения, но не наоборот &mdash; `ConfigureAppConfiguration` не влияет на конфигурацию `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="22180-331">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="22180-333">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hostsettings.json*, а затем с помощью аргументов командной строки:</span><span class="sxs-lookup"><span data-stu-id="22180-333">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="22180-334">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="22180-334">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-335">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-335">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22180-336">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hostsettings.json*, а затем с помощью аргументов командной строки:</span><span class="sxs-lookup"><span data-stu-id="22180-336">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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

<span data-ttu-id="22180-337">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="22180-337">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

---

> [!NOTE]
> <span data-ttu-id="22180-338">Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="22180-338">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="22180-339">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="22180-339">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="22180-340">Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="22180-340">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="22180-341">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="22180-341">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="22180-342">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="22180-342">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="22180-343">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="22180-343">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="22180-344">`UseConfiguration` копирует ключи только из предоставленного объекта `IConfiguration` для конфигурации конструктора узла.</span><span class="sxs-lookup"><span data-stu-id="22180-344">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="22180-345">Поэтому указание `reloadOnChange: true` для файлов JSON, XML и INI ни на что не влияет.</span><span class="sxs-lookup"><span data-stu-id="22180-345">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="22180-346">Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение из командной строки при выполнении команды [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="22180-346">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="22180-347">Аргумент командной строки переопределяет значение `urls` из файла *hostsettings.json*, и сервер будет ожидать передачи данных через порт 8080:</span><span class="sxs-lookup"><span data-stu-id="22180-347">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="22180-348">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="22180-348">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="22180-349">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="22180-349">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="22180-350">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="22180-350">**Run**</span></span>

<span data-ttu-id="22180-351">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="22180-351">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="22180-352">**Start**</span><span class="sxs-lookup"><span data-stu-id="22180-352">**Start**</span></span>

<span data-ttu-id="22180-353">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="22180-353">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="22180-354">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="22180-354">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="22180-355">Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода.</span><span class="sxs-lookup"><span data-stu-id="22180-355">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="22180-356">Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="22180-356">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="22180-357">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="22180-357">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="22180-358">Выполните запуск с помощью `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="22180-358">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="22180-359">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="22180-359">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="22180-360">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="22180-360">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="22180-361">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="22180-361">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="22180-362">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="22180-362">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="22180-363">Выполните запуск с помощью URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="22180-363">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="22180-364">Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="22180-364">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="22180-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="22180-365">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="22180-366">Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="22180-366">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="22180-367">В этом примере используйте следующие запросы в браузере:</span><span class="sxs-lookup"><span data-stu-id="22180-367">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="22180-368">Запрос</span><span class="sxs-lookup"><span data-stu-id="22180-368">Request</span></span>                                    | <span data-ttu-id="22180-369">Ответ</span><span class="sxs-lookup"><span data-stu-id="22180-369">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="22180-370">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="22180-370">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="22180-371">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="22180-371">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="22180-372">Вызывает исключение со строкой "ooops!"</span><span class="sxs-lookup"><span data-stu-id="22180-372">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="22180-373">Вызывает исключение со строкой "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="22180-373">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="22180-374">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="22180-374">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="22180-375">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="22180-375">Hello World!</span></span>                             |

<span data-ttu-id="22180-376">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="22180-376">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="22180-377">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="22180-377">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="22180-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="22180-378">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="22180-379">Используйте URL-адрес и экземпляр `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="22180-379">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="22180-380">Результат будет тем же, что и при использовании **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="22180-380">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="22180-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="22180-381">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="22180-382">Предоставьте делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="22180-382">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="22180-383">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="22180-383">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="22180-384">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="22180-384">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="22180-385">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="22180-385">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="22180-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="22180-386">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="22180-387">Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="22180-387">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="22180-388">Результат будет тем же, что и при использовании **StartWith(Action&lt;IApplicationBuilder&gt; app)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="22180-388">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="22180-389">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="22180-389">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="22180-390">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="22180-390">**Run**</span></span>

<span data-ttu-id="22180-391">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="22180-391">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="22180-392">**Start**</span><span class="sxs-lookup"><span data-stu-id="22180-392">**Start**</span></span>

<span data-ttu-id="22180-393">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="22180-393">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="22180-394">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="22180-394">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

---

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="22180-395">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="22180-395">IHostingEnvironment interface</span></span>

<span data-ttu-id="22180-396">[Интерфейс IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-396">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="22180-397">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="22180-397">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="22180-398">Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="22180-398">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="22180-399">Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="22180-399">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="22180-400">Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="22180-400">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="22180-401">Подробные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="22180-401">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="22180-402">Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="22180-402">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="22180-403">`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="22180-403">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="22180-404">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="22180-404">IApplicationLifetime interface</span></span>

<span data-ttu-id="22180-405">[Интерфейс IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="22180-405">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="22180-406">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="22180-406">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="22180-407">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="22180-407">Cancellation Token</span></span>    | <span data-ttu-id="22180-408">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="22180-408">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="22180-409">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="22180-409">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="22180-410">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="22180-410">The host has fully started.</span></span> |
| [<span data-ttu-id="22180-411">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="22180-411">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="22180-412">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="22180-412">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="22180-413">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="22180-413">All requests should be processed.</span></span> <span data-ttu-id="22180-414">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="22180-414">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="22180-415">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="22180-415">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="22180-416">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="22180-416">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="22180-417">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="22180-417">Requests may still be processing.</span></span> <span data-ttu-id="22180-418">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="22180-418">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="22180-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="22180-419">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="22180-420">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="22180-420">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="22180-421">Проверка области</span><span class="sxs-lookup"><span data-stu-id="22180-421">Scope validation</span></span>

<span data-ttu-id="22180-422">В ASP.NET Core 2.0 или более поздней версии [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="22180-422">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="22180-423">Если для `ValidateScopes` установлено значение `true`, поставщик службы по умолчанию проверяет, что:</span><span class="sxs-lookup"><span data-stu-id="22180-423">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="22180-424">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="22180-424">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="22180-425">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="22180-425">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="22180-426">Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="22180-426">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="22180-427">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="22180-427">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="22180-428">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="22180-428">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="22180-429">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="22180-429">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="22180-430">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="22180-430">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="22180-431">Чтобы всегда проверять области, в том числе в рабочей среде, настройте [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) с [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) в конструкторе узлов:</span><span class="sxs-lookup"><span data-stu-id="22180-431">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="22180-432">Устранение неполадок, связанных с исключениями System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="22180-432">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="22180-433">**Относится только к ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="22180-433">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="22180-434">Узел может создаваться путем внедрения интерфейса `IStartup` непосредственно в контейнер внедрения зависимостей, а не путем вызова `UseStartup` или `Configure`:</span><span class="sxs-lookup"><span data-stu-id="22180-434">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="22180-435">Если узел создается таким способом, может произойти следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="22180-435">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="22180-436">Причина в том, что [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) требуется для проверки `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="22180-436">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="22180-437">Если в приложении интерфейс `IStartup` вручную внедряется в контейнер внедрения зависимостей, добавьте следующий вызов в `WebHostBuilder` с указанием имени сборки:</span><span class="sxs-lookup"><span data-stu-id="22180-437">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="22180-438">Кроме того, в `WebHostBuilder` можно добавить метод-заглушку `Configure`, который автоматически задает `applicationName`(`ApplicationKey`):</span><span class="sxs-lookup"><span data-stu-id="22180-438">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="22180-439">**Примечание**. Это необходимо только в выпуске ASP.NET Core 2.0 и только в случае, если приложение не вызывает метод `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="22180-439">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="22180-440">Дополнительные сведения см. в [комментарии к объявлению об удалении пакета Microsoft.Extensions.PlatformAbstractions](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и в [примере использования StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="22180-440">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="22180-441">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="22180-441">Additional resources</span></span>

* [<span data-ttu-id="22180-442">Размещение в Windows с помощью IIS</span><span class="sxs-lookup"><span data-stu-id="22180-442">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="22180-443">Размещение в Linux с использованием Nginx</span><span class="sxs-lookup"><span data-stu-id="22180-443">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="22180-444">Размещение в Linux с использованием Apache</span><span class="sxs-lookup"><span data-stu-id="22180-444">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="22180-445">Размещение в службе Windows</span><span class="sxs-lookup"><span data-stu-id="22180-445">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
