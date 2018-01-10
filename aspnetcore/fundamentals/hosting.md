---
title: "Размещение в ASP.NET Core"
author: guardrex
description: "Дополнительные сведения о веб-узла в ASP.NET Core, который отвечает за управление запуском и временем существования приложения."
keywords: "ASP.NET Core веб-узла, IWebHost, WebHostBuilder, IHostingEnvironment, IApplicationLifetime"
ms.author: riande
manager: wpickett
ms.date: 09/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/hosting
ms.openlocfilehash: 054b60206cafc3d6dd5775436995638d7f5700cf
ms.sourcegitcommit: 2d23ea501e0213bbacf65298acf1c8bd17209540
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="d89ba-104">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d89ba-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="d89ba-105">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="d89ba-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d89ba-106">Настройка приложений ASP.NET Core и запустите *узла*.</span><span class="sxs-lookup"><span data-stu-id="d89ba-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d89ba-107">Основное приложение отвечает за управление запуском и временем существования приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d89ba-108">Как минимум узел настраивает сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="d89ba-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="d89ba-109">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="d89ba-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d89ba-111">Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d89ba-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d89ba-112">Это обычно выполняется в точке входа приложения, `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="d89ba-112">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d89ba-113">В шаблоны проектов `Main` находится в папке *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d89ba-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d89ba-114">Типичный *Program.cs* вызовы [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) начата Настройка узла:</span><span class="sxs-lookup"><span data-stu-id="d89ba-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="d89ba-115">`CreateDefaultBuilder`выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="d89ba-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d89ba-116">Настраивает [Kestrel](servers/kestrel.md) и веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="d89ba-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="d89ba-117">Параметры по умолчанию Kestrel см. в разделе [Kestrel параметры раздела Kestrel реализация веб-сервера в ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="d89ba-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="d89ba-118">Задает содержимое корневого пути, возвращенных [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d89ba-118">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d89ba-119">Необязательная конфигурация загружает из:</span><span class="sxs-lookup"><span data-stu-id="d89ba-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="d89ba-120">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="d89ba-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="d89ba-121">*appSettings. {Среды} .json*.</span><span class="sxs-lookup"><span data-stu-id="d89ba-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d89ba-122">[Секреты пользователя](xref:security/app-secrets) при запуске приложения `Development` среде.</span><span class="sxs-lookup"><span data-stu-id="d89ba-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="d89ba-123">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="d89ba-123">Environment variables.</span></span>
  * <span data-ttu-id="d89ba-124">Аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="d89ba-124">Command-line arguments.</span></span>
* <span data-ttu-id="d89ba-125">Настраивает [входа](xref:fundamentals/logging/index) для вывода на консоль и отладки.</span><span class="sxs-lookup"><span data-stu-id="d89ba-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d89ba-126">Ведение журнала включает [фильтрации журнала](xref:fundamentals/logging/index#log-filtering) правила, указанные в разделе конфигурации ведения журнала *appsettings.json* или *appsettings. {} Среда} .json* файл.</span><span class="sxs-lookup"><span data-stu-id="d89ba-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d89ba-127">При работе под IIS позволяет [интеграции IIS](xref:publishing/iis).</span><span class="sxs-lookup"><span data-stu-id="d89ba-127">When running behind IIS, enables [IIS integration](xref:publishing/iis).</span></span> <span data-ttu-id="d89ba-128">Настраивает сервер должен прослушивать при использовании порта и пути к базовой папке [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d89ba-128">Configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d89ba-129">Модуль создает обратного прокси-сервера служб IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d89ba-129">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d89ba-130">Кроме того, настраивает приложение для [перехватить ошибки запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d89ba-130">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d89ba-131">Параметры по умолчанию служб IIS см. в разделе [IIS параметры раздела узла ASP.NET Core в Windows с помощью IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="d89ba-131">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="d89ba-132">*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d89ba-132">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d89ba-133">При запуске приложения из корневой папки проекта в корневой папки проекта используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="d89ba-133">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d89ba-134">Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d89ba-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d89ba-135">Дополнительные сведения о настройке приложения см. в разделе [конфигурации в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d89ba-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="d89ba-136">В качестве альтернативы с помощью статического `CreateDefaultBuilder` метод, создание узла из [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживаемых подход с ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="d89ba-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d89ba-137">Дополнительные сведения см. в разделе вкладке 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d89ba-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-139">Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d89ba-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d89ba-140">Создание ведущего приложения обычно выполняется в точке входа приложения, `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="d89ba-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d89ba-141">В шаблоны проектов `Main` находится в папке *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="d89ba-141">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="d89ba-142">`WebHostBuilder`требуется [сервера, который реализует IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d89ba-142">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="d89ba-143">Встроенные серверы являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 был вызван HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d89ba-143">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d89ba-144">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) указывает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d89ba-144">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d89ba-145">*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d89ba-145">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d89ba-146">Для получения содержимого корневого каталога по умолчанию `UseContentRoot` по [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d89ba-146">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d89ba-147">При запуске приложения из корневой папки проекта в корневой папки проекта используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="d89ba-147">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d89ba-148">Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d89ba-148">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d89ba-149">Чтобы использовать в качестве обратного прокси-сервера IIS, вызовите [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) как часть построения узла.</span><span class="sxs-lookup"><span data-stu-id="d89ba-149">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d89ba-150">`UseIISIntegration`неправильно настраивает *сервера*, таких как [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="d89ba-150">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d89ba-151">`UseIISIntegration`настраивает сервер должен прослушивать при использовании порта и пути к базовой папке [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) создание обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="d89ba-151">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d89ba-152">Для использования IIS с ASP.NET Core `UseKestrel` и `UseIISIntegration` должен быть указан.</span><span class="sxs-lookup"><span data-stu-id="d89ba-152">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d89ba-153">`UseIISIntegration`активируется, только при запуске IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d89ba-153">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d89ba-154">Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](xref:fundamentals/servers/aspnet-core-module) и [ссылки на конфигурации ASP.NET Core модуль](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d89ba-154">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="d89ba-155">Минимальную реализацию, которая настраивает узел (и приложения ASP.NET Core) включает в себя указание сервера и настройки конвейера запросов приложения:</span><span class="sxs-lookup"><span data-stu-id="d89ba-155">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="d89ba-156">При настройке узла, [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) методы могут быть предоставлены.</span><span class="sxs-lookup"><span data-stu-id="d89ba-156">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d89ba-157">Если `Startup` класса указан, то оно должно определять `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="d89ba-157">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d89ba-158">Дополнительные сведения см. в разделе [запуск приложения в ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="d89ba-158">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="d89ba-159">Несколько вызовов `ConfigureServices` append друг с другом.</span><span class="sxs-lookup"><span data-stu-id="d89ba-159">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d89ba-160">Несколько вызовов `Configure` или `UseStartup` на `WebHostBuilder` заменить предыдущие параметры.</span><span class="sxs-lookup"><span data-stu-id="d89ba-160">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d89ba-161">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="d89ba-161">Host configuration values</span></span>

<span data-ttu-id="d89ba-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) зависит от следующих подходов для задания значений конфигурации узла:</span><span class="sxs-lookup"><span data-stu-id="d89ba-162">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d89ba-163">Построитель конфигурации узла, включая переменные среды с форматом `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-163">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d89ba-164">Например, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-164">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="d89ba-165">Явные методы, такие как `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-165">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="d89ba-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="d89ba-166">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d89ba-167">При установке значения с `UseSetting`, имеет значение как строка независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="d89ba-167">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d89ba-168">Узел использует какой бы параметр задает значение последнего.</span><span class="sxs-lookup"><span data-stu-id="d89ba-168">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d89ba-169">Дополнительные сведения см. в разделе [переопределение конфигурации](#overriding-configuration) в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d89ba-169">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="d89ba-170">Запись ошибки при загрузке</span><span class="sxs-lookup"><span data-stu-id="d89ba-170">Capture Startup Errors</span></span>

<span data-ttu-id="d89ba-171">Этот параметр управляет отслеживания ошибок при запуске.</span><span class="sxs-lookup"><span data-stu-id="d89ba-171">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d89ba-172">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d89ba-172">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d89ba-173">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d89ba-173">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d89ba-174">**По умолчанию**: по умолчанию используется значение `false` Если приложение запускается с Kestrel за IIS, где значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-174">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d89ba-175">**Задать с помощью**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d89ba-175">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d89ba-176">**Переменная среды**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d89ba-176">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d89ba-177">Когда `false`, ошибки во время запуска результат в узле выхода.</span><span class="sxs-lookup"><span data-stu-id="d89ba-177">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d89ba-178">Когда `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="d89ba-178">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-179">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-179">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-180">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-180">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="d89ba-181">Содержимое корневого</span><span class="sxs-lookup"><span data-stu-id="d89ba-181">Content Root</span></span>

<span data-ttu-id="d89ba-182">Этот параметр определяет, где ASP.NET Core начинает поиск файлов содержимого, такие как представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d89ba-182">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d89ba-183">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d89ba-183">**Key**: contentRoot</span></span>  
<span data-ttu-id="d89ba-184">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="d89ba-184">**Type**: *string*</span></span>  
<span data-ttu-id="d89ba-185">**По умолчанию**: по умолчанию для папки, в которой хранится сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-185">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d89ba-186">**Задать с помощью**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d89ba-186">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d89ba-187">**Переменная среды**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d89ba-187">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d89ba-188">Содержимое корневого также используется как базовый путь для [параметр корневого веб-каталога](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d89ba-188">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d89ba-189">Если путь не существует, узлу не удается запустить.</span><span class="sxs-lookup"><span data-stu-id="d89ba-189">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="d89ba-192">Подробные сведения об ошибках</span><span class="sxs-lookup"><span data-stu-id="d89ba-192">Detailed Errors</span></span>

<span data-ttu-id="d89ba-193">Определяет ли подробные ошибки должны быть зафиксированы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-193">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d89ba-194">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d89ba-194">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d89ba-195">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d89ba-195">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d89ba-196">**По умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="d89ba-196">**Default**: false</span></span>  
<span data-ttu-id="d89ba-197">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d89ba-197">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d89ba-198">**Переменная среды**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d89ba-198">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d89ba-199">При включении (или когда <a href="#environment">среды</a> равно `Development`), приложение записывает подробные сведения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="d89ba-199">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-200">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-200">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-201">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-201">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="d89ba-202">Среда</span><span class="sxs-lookup"><span data-stu-id="d89ba-202">Environment</span></span>

<span data-ttu-id="d89ba-203">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-203">Sets the app's environment.</span></span>

<span data-ttu-id="d89ba-204">**Ключ**: среды</span><span class="sxs-lookup"><span data-stu-id="d89ba-204">**Key**: environment</span></span>  
<span data-ttu-id="d89ba-205">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="d89ba-205">**Type**: *string*</span></span>  
<span data-ttu-id="d89ba-206">**По умолчанию**: рабочей среде</span><span class="sxs-lookup"><span data-stu-id="d89ba-206">**Default**: Production</span></span>  
<span data-ttu-id="d89ba-207">**Задать с помощью**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d89ba-207">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d89ba-208">**Переменная среды**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d89ba-208">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d89ba-209">Среды может быть присвоено любое значение.</span><span class="sxs-lookup"><span data-stu-id="d89ba-209">The environment can be set to any value.</span></span> <span data-ttu-id="d89ba-210">Значения, определяемые платформой `Development`, `Staging`, и `Production`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-210">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d89ba-211">Значения не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="d89ba-211">Values aren't case sensitive.</span></span> <span data-ttu-id="d89ba-212">По умолчанию *среды* считывается из `ASPNETCORE_ENVIRONMENT` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="d89ba-212">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d89ba-213">При использовании [Visual Studio](https://www.visualstudio.com/), переменные среды может быть задан в *launchSettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="d89ba-213">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d89ba-214">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d89ba-214">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-215">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-215">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-216">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-216">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d89ba-217">Размещение загрузки сборок</span><span class="sxs-lookup"><span data-stu-id="d89ba-217">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d89ba-218">Задает размещения сборок при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-218">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d89ba-219">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d89ba-219">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d89ba-220">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="d89ba-220">**Type**: *string*</span></span>  
<span data-ttu-id="d89ba-221">**По умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="d89ba-221">**Default**: Empty string</span></span>  
<span data-ttu-id="d89ba-222">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d89ba-222">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d89ba-223">**Переменная среды**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d89ba-223">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d89ba-224">Разделенный точками с запятой строка размещения запуска сборок для загрузки при запуске.</span><span class="sxs-lookup"><span data-stu-id="d89ba-224">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="d89ba-225">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d89ba-225">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="d89ba-226">Несмотря на то, что значения конфигурации по умолчанию устанавливается равным пустой строке, размещения сборок запуска всегда включать сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-226">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d89ba-227">Когда указаны размещения сборок при запуске, они добавляются в сборку приложения для загрузки, когда приложение создает его общие службы во время запуска.</span><span class="sxs-lookup"><span data-stu-id="d89ba-227">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-228">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-228">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-229">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-229">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-230">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d89ba-230">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d89ba-231">Предпочтение размещение URL-адреса</span><span class="sxs-lookup"><span data-stu-id="d89ba-231">Prefer Hosting URLs</span></span>

<span data-ttu-id="d89ba-232">Указывает ли узел должен прослушивать URL-адресов настроены `WebHostBuilder` вместо настроенных с `IServer` реализации.</span><span class="sxs-lookup"><span data-stu-id="d89ba-232">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d89ba-233">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d89ba-233">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d89ba-234">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d89ba-234">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d89ba-235">**По умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="d89ba-235">**Default**: true</span></span>  
<span data-ttu-id="d89ba-236">**Задать с помощью**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d89ba-236">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d89ba-237">**Переменная среды**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d89ba-237">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="d89ba-238">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d89ba-238">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-239">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-239">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-240">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-240">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-241">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d89ba-241">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d89ba-242">Запретить размещение запуска</span><span class="sxs-lookup"><span data-stu-id="d89ba-242">Prevent Hosting Startup</span></span>

<span data-ttu-id="d89ba-243">Предотвращает автоматическая загрузка размещение запуска сборок, включая размещение сборок запуска задаются сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-243">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d89ba-244">В разделе [Добавление компонентов приложения из внешней сборки с помощью IHostingStartup](xref:hosting/ihostingstartup) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="d89ba-244">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="d89ba-245">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d89ba-245">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d89ba-246">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d89ba-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d89ba-247">**По умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="d89ba-247">**Default**: false</span></span>  
<span data-ttu-id="d89ba-248">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d89ba-248">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d89ba-249">**Переменная среды**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d89ba-249">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="d89ba-250">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d89ba-250">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-251">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-251">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-252">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-252">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-253">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d89ba-253">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="d89ba-254">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="d89ba-254">Server URLs</span></span>

<span data-ttu-id="d89ba-255">Указывает IP-адреса или адреса узлов с портами и протоколы, которые сервер должен прослушивать запросы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-255">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d89ba-256">**Ключ**: URL-адреса</span><span class="sxs-lookup"><span data-stu-id="d89ba-256">**Key**: urls</span></span>  
<span data-ttu-id="d89ba-257">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="d89ba-257">**Type**: *string*</span></span>  
<span data-ttu-id="d89ba-258">**По умолчанию**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="d89ba-258">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d89ba-259">**Задать с помощью**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d89ba-259">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d89ba-260">**Переменная среды**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d89ba-260">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d89ba-261">Значение, разделенных точкой с запятой (;) префиксов список URL-адрес которого сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="d89ba-261">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d89ba-262">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-262">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d89ba-263">Используйте "\*» для указания, что сервер должен прослушивать запросы по любой IP-адрес или имя узла, используя указанный порт и протокол (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d89ba-263">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d89ba-264">Протокол (`http://` или `https://`) должен быть включен, все URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="d89ba-264">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d89ba-265">Поддерживаемые форматы различаться для разных серверов.</span><span class="sxs-lookup"><span data-stu-id="d89ba-265">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-266">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-266">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="d89ba-267">Kestrel имеет собственную конфигурацию конечной точки API.</span><span class="sxs-lookup"><span data-stu-id="d89ba-267">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d89ba-268">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="d89ba-268">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-269">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-269">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="d89ba-270">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="d89ba-270">Shutdown Timeout</span></span>

<span data-ttu-id="d89ba-271">Указывает время ожидания для завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="d89ba-271">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="d89ba-272">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d89ba-272">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d89ba-273">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="d89ba-273">**Type**: *int*</span></span>  
<span data-ttu-id="d89ba-274">**По умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="d89ba-274">**Default**: 5</span></span>  
<span data-ttu-id="d89ba-275">**Задать с помощью**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d89ba-275">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d89ba-276">**Переменная среды**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d89ba-276">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d89ba-277">Несмотря на то, что ключ принимает *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` принимает метод расширения `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-277">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="d89ba-278">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d89ba-278">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-279">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-279">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-280">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-280">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-281">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d89ba-281">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="d89ba-282">При запуске сборки</span><span class="sxs-lookup"><span data-stu-id="d89ba-282">Startup Assembly</span></span>

<span data-ttu-id="d89ba-283">Определяет сборку для поиска `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="d89ba-283">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d89ba-284">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d89ba-284">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d89ba-285">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="d89ba-285">**Type**: *string*</span></span>  
<span data-ttu-id="d89ba-286">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="d89ba-286">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d89ba-287">**Задать с помощью**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d89ba-287">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d89ba-288">**Переменная среды**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d89ba-288">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d89ba-289">Сборки по имени (`string`) или тип (`TStartup`) может ссылаться.</span><span class="sxs-lookup"><span data-stu-id="d89ba-289">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d89ba-290">При наличии нескольких `UseStartup` методы вызываются, приоритет имеет последняя.</span><span class="sxs-lookup"><span data-stu-id="d89ba-290">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-291">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-291">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
    ...
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
    ...
```

---

### <a name="web-root"></a><span data-ttu-id="d89ba-293">Персональный компьютер</span><span class="sxs-lookup"><span data-stu-id="d89ba-293">Web Root</span></span>

<span data-ttu-id="d89ba-294">Задает относительный путь для приложения статических ресурсах.</span><span class="sxs-lookup"><span data-stu-id="d89ba-294">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d89ba-295">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="d89ba-295">**Key**: webroot</span></span>  
<span data-ttu-id="d89ba-296">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="d89ba-296">**Type**: *string*</span></span>  
<span data-ttu-id="d89ba-297">**По умолчанию**: Если не указан, значение по умолчанию — "(Content Root)/wwwroot», если путь существует.</span><span class="sxs-lookup"><span data-stu-id="d89ba-297">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d89ba-298">Если путь не существует, то используется поставщик холостой файла.</span><span class="sxs-lookup"><span data-stu-id="d89ba-298">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d89ba-299">**Задать с помощью**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d89ba-299">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d89ba-300">**Переменная среды**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d89ba-300">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-301">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-301">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-302">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-302">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="d89ba-303">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="d89ba-303">Overriding configuration</span></span>

<span data-ttu-id="d89ba-304">Используйте [конфигурации](xref:fundamentals/configuration/index) для настройки узла.</span><span class="sxs-lookup"><span data-stu-id="d89ba-304">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="d89ba-305">В следующем примере конфигурации узла при необходимости указывается в *hosting.json* файла.</span><span class="sxs-lookup"><span data-stu-id="d89ba-305">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="d89ba-306">Загрузить любую конфигурацию из *hosting.json* файл может быть переопределена аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="d89ba-306">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d89ba-307">Конфигурации построения (в `config`) используется для настройки узла с `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-307">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d89ba-309">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d89ba-309">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d89ba-310">Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:</span><span class="sxs-lookup"><span data-stu-id="d89ba-310">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        BuildWebHost(args).Run();
    }

    public static IWebHost BuildWebHost(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-312">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="d89ba-312">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d89ba-313">Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:</span><span class="sxs-lookup"><span data-stu-id="d89ba-313">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        var config = new ConfigurationBuilder()
            .SetBasePath(Directory.GetCurrentDirectory())
            .AddJsonFile("hosting.json", optional: true)
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

---

> [!NOTE]
> <span data-ttu-id="d89ba-314">[UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) метода расширения не может в настоящее время синтаксического анализа раздела конфигурации, возвращенных `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-314">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d89ba-315">`GetSection` Метод фильтрует ключи в запрошенный раздел конфигурации, но оставляет имя раздела по ключам (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d89ba-315">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d89ba-316">`UseConfiguration` Метод ожидает ключи для сопоставления `WebHostBuilder` ключи (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d89ba-316">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d89ba-317">Имя раздела по ключам присутствия запрещает настраивать узла значения этого раздела.</span><span class="sxs-lookup"><span data-stu-id="d89ba-317">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d89ba-318">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="d89ba-318">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d89ba-319">Дополнительные сведения и способы решения проблем см. в разделе [передачи раздела конфигурации в WebHostBuilder.UseConfiguration использует полное ключи](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="d89ba-319">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="d89ba-320">Для указания выполнения на определенный URL-адрес узла, требуемого значения могут передаваться в командной строке при выполнении `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-320">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="d89ba-321">Аргумент командной строки переопределяет `urls` значение из *hosting.json* файла, а сервер прослушивает порт 8080:</span><span class="sxs-lookup"><span data-stu-id="d89ba-321">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="d89ba-322">Запуск узла</span><span class="sxs-lookup"><span data-stu-id="d89ba-322">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d89ba-323">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-323">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d89ba-324">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="d89ba-324">**Run**</span></span>

<span data-ttu-id="d89ba-325">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:</span><span class="sxs-lookup"><span data-stu-id="d89ba-325">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d89ba-326">**Start**</span><span class="sxs-lookup"><span data-stu-id="d89ba-326">**Start**</span></span>

<span data-ttu-id="d89ba-327">Запустить узел в без блокирования путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="d89ba-327">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d89ba-328">Если передается список URL-адресов `Start` метод, он прослушивает URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="d89ba-328">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d89ba-329">Приложение может инициализации и запуска нового узла с помощью предварительно настроенного значения по умолчанию `CreateDefaultBuilder` с помощью статических удобных метода.</span><span class="sxs-lookup"><span data-stu-id="d89ba-329">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d89ba-330">Эти методы требуют запуска серверу без вывода на консоль с [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) ожидания break (Ctrl-C или SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="d89ba-330">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d89ba-331">**Начала (приложение RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="d89ba-331">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d89ba-332">Начать с `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-332">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d89ba-333">Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="d89ba-333">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d89ba-334">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d89ba-334">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d89ba-335">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="d89ba-335">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d89ba-336">**Запуск (строковый URL-адрес, RequestDelegate приложения)**</span><span class="sxs-lookup"><span data-stu-id="d89ba-336">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d89ba-337">Запуск с URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-337">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d89ba-338">Дает тот же результат, как **начала (приложение RequestDelegate)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-338">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d89ba-339">**Запуск (действие<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d89ba-339">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d89ba-340">Использовать экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для использования маршрутизации по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="d89ba-340">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d89ba-341">В примере можно используйте следующие запросы браузера:</span><span class="sxs-lookup"><span data-stu-id="d89ba-341">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d89ba-342">Запрос</span><span class="sxs-lookup"><span data-stu-id="d89ba-342">Request</span></span>                                    | <span data-ttu-id="d89ba-343">Ответ</span><span class="sxs-lookup"><span data-stu-id="d89ba-343">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d89ba-344">Привет, Мартин!</span><span class="sxs-lookup"><span data-stu-id="d89ba-344">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d89ba-345">Буэнос dias Catrina!</span><span class="sxs-lookup"><span data-stu-id="d89ba-345">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d89ba-346">Вызывает исключение со строкой «ooops!»</span><span class="sxs-lookup"><span data-stu-id="d89ba-346">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d89ba-347">Вызывает исключение со строкой «Uh ой!»</span><span class="sxs-lookup"><span data-stu-id="d89ba-347">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d89ba-348">Sante Kevin!</span><span class="sxs-lookup"><span data-stu-id="d89ba-348">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d89ba-349">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="d89ba-349">Hello World!</span></span>                             |

<span data-ttu-id="d89ba-350">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d89ba-350">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d89ba-351">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="d89ba-351">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d89ba-352">**Запуск (URL-адрес, действие строки<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d89ba-352">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d89ba-353">Использовать URL-адреса и имени экземпляра `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-353">Use a URL and an instance of `IRouteBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d89ba-354">Дает тот же результат, как **запуска (действие<IRouteBuilder> routeBuilder)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-354">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d89ba-355">**StartWith (действие<IApplicationBuilder> приложения)**</span><span class="sxs-lookup"><span data-stu-id="d89ba-355">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d89ba-356">Укажите делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-356">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d89ba-357">Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="d89ba-357">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d89ba-358">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d89ba-358">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d89ba-359">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="d89ba-359">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d89ba-360">**StartWith (URL-адрес, действие строки<IApplicationBuilder> приложения)**</span><span class="sxs-lookup"><span data-stu-id="d89ba-360">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d89ba-361">Укажите URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-361">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d89ba-362">Дает тот же результат, как **StartWith (действие<IApplicationBuilder> приложения)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-362">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d89ba-363">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d89ba-363">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d89ba-364">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="d89ba-364">**Run**</span></span>

<span data-ttu-id="d89ba-365">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:</span><span class="sxs-lookup"><span data-stu-id="d89ba-365">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d89ba-366">**Start**</span><span class="sxs-lookup"><span data-stu-id="d89ba-366">**Start**</span></span>

<span data-ttu-id="d89ba-367">Запустить узел в без блокирования путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="d89ba-367">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d89ba-368">Если передается список URL-адресов `Start` метод, он прослушивает URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="d89ba-368">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d89ba-369">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d89ba-369">IHostingEnvironment interface</span></span>

<span data-ttu-id="d89ba-370">[IHostingEnvironment интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде размещения веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-370">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d89ba-371">Используйте [внедрение конструктора](xref:fundamentals/dependency-injection) для получения `IHostingEnvironment` для использования его свойства и методы расширения:</span><span class="sxs-lookup"><span data-stu-id="d89ba-371">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d89ba-372">Объект [подход на основе соглашения о](xref:fundamentals/environments#startup-conventions) можно использовать для настройки приложения во время запуска, в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="d89ba-372">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d89ba-373">Кроме того, вставка `IHostingEnvironment` в `Startup` конструктора для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-373">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d89ba-374">В дополнение к `IsDevelopment` метод расширения `IHostingEnvironment` предлагает `IsStaging`, `IsProduction`, и `IsEnvironment(string environmentName)` методы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-374">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d89ba-375">В разделе [работа с несколькими средами](xref:fundamentals/environments) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-375">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="d89ba-376">`IHostingEnvironment` Службы также могут быть добавлены непосредственно в `Configure` метода настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="d89ba-376">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d89ba-377">`IHostingEnvironment`может быть введено в `Invoke` метод при создании пользовательских [по промежуточного слоя](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="d89ba-377">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d89ba-378">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d89ba-378">IApplicationLifetime interface</span></span>

<span data-ttu-id="d89ba-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет после запуска и завершения действия.</span><span class="sxs-lookup"><span data-stu-id="d89ba-379">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d89ba-380">Три свойства для интерфейса являются токенов отмены, используемый для регистрации `Action` методы, которые определяют события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-380">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="d89ba-381">Имеется также `StopApplication` метод.</span><span class="sxs-lookup"><span data-stu-id="d89ba-381">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="d89ba-382">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="d89ba-382">Cancellation Token</span></span>    | <span data-ttu-id="d89ba-383">Запустить &#8230;</span><span class="sxs-lookup"><span data-stu-id="d89ba-383">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="d89ba-384">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="d89ba-384">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="d89ba-385">Узел выполняет правильное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-385">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d89ba-386">По-прежнему может обрабатывать запросы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-386">Requests may still be processing.</span></span> <span data-ttu-id="d89ba-387">Завершение работы блокируется до завершения этого события.</span><span class="sxs-lookup"><span data-stu-id="d89ba-387">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="d89ba-388">Узел завершает нормальное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="d89ba-388">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d89ba-389">Все запросы будут обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="d89ba-389">All requests should be processed.</span></span> <span data-ttu-id="d89ba-390">Завершение работы блокируется до завершения этого события.</span><span class="sxs-lookup"><span data-stu-id="d89ba-390">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="d89ba-391">Метод</span><span class="sxs-lookup"><span data-stu-id="d89ba-391">Method</span></span>            | <span data-ttu-id="d89ba-392">Действие</span><span class="sxs-lookup"><span data-stu-id="d89ba-392">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="d89ba-393">Завершение запросов текущего приложения.</span><span class="sxs-lookup"><span data-stu-id="d89ba-393">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d89ba-394">Устранение неполадок System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d89ba-394">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d89ba-395">**Применяется к только базовые ASP.NET 2.0**</span><span class="sxs-lookup"><span data-stu-id="d89ba-395">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="d89ba-396">Узел может быть построена, внедряя `IStartup` непосредственно в контейнер внедрения зависимостей, а не вызовом `UseStartup` или `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d89ba-396">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d89ba-397">Если узел построен таким образом, может возникнуть следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="d89ba-397">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d89ba-398">Это происходит потому, что [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) необходим для проверки на наличие `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-398">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d89ba-399">Если приложение вручную внедряет `IStartup` в контейнер внедрения зависимостей, добавьте следующий вызов `WebHostBuilder` с указанным именем сборки:</span><span class="sxs-lookup"><span data-stu-id="d89ba-399">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="d89ba-400">Кроме того, добавить фиктивное `Configure` для `WebHostBuilder`, который устанавливает `applicationName`(`ApplicationKey`) автоматически:</span><span class="sxs-lookup"><span data-stu-id="d89ba-400">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="d89ba-401">**Примечание**: это необходимо только необходимые с выпуском ASP.NET Core 2.0 и только если приложение не вызывает `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d89ba-401">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="d89ba-402">Дополнительные сведения см. в разделе [объявления: Microsoft.Extensions.PlatformAbstractions был удален (комментарий)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и [StartupInjection пример](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d89ba-402">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d89ba-403">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d89ba-403">Additional resources</span></span>

* [<span data-ttu-id="d89ba-404">Публикация в Windows с помощью служб IIS</span><span class="sxs-lookup"><span data-stu-id="d89ba-404">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="d89ba-405">Публикация в Linux с помощью Nginx</span><span class="sxs-lookup"><span data-stu-id="d89ba-405">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="d89ba-406">Публикация в Linux с помощью Apache</span><span class="sxs-lookup"><span data-stu-id="d89ba-406">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="d89ba-407">Узел в службе Windows</span><span class="sxs-lookup"><span data-stu-id="d89ba-407">Host in a Windows Service</span></span>](xref:hosting/windows-service)
