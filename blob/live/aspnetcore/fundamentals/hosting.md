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
ms.openlocfilehash: 14e48adf5671a41ad6e135caeb4a87fdf7292aa6
ms.sourcegitcommit: 5834afb87e4262b9b88e60e3fe6c735e61a1e08d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/20/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="6611f-104">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="6611f-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="6611f-105">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="6611f-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6611f-106">Настройка приложений ASP.NET Core и запустите *узла*.</span><span class="sxs-lookup"><span data-stu-id="6611f-106">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="6611f-107">Основное приложение отвечает за управление запуском и временем существования приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-107">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="6611f-108">Как минимум узел настраивает сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="6611f-108">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="6611f-109">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="6611f-109">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6611f-111">Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="6611f-111">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="6611f-112">Это обычно выполняется в точке входа приложения, `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="6611f-112">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="6611f-113">В шаблоны проектов `Main` находится в папке *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="6611f-113">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="6611f-114">Типичный *Program.cs* вызовы [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) начата Настройка узла:</span><span class="sxs-lookup"><span data-stu-id="6611f-114">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="6611f-115">`CreateDefaultBuilder`выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="6611f-115">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="6611f-116">Настраивает [Kestrel](servers/kestrel.md) и веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="6611f-116">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="6611f-117">Параметры по умолчанию Kestrel см. в разделе [Kestrel параметры раздела Kestrel реализация веб-сервера в ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="6611f-117">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="6611f-118">Задает содержимое корневого [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="6611f-118">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="6611f-119">Необязательная конфигурация загружает из:</span><span class="sxs-lookup"><span data-stu-id="6611f-119">Loads optional configuration from:</span></span>
  * <span data-ttu-id="6611f-120">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="6611f-120">*appsettings.json*.</span></span>
  * <span data-ttu-id="6611f-121">*appSettings. {Среды} .json*.</span><span class="sxs-lookup"><span data-stu-id="6611f-121">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="6611f-122">[Секреты пользователя](xref:security/app-secrets) при запуске приложения `Development` среде.</span><span class="sxs-lookup"><span data-stu-id="6611f-122">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="6611f-123">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="6611f-123">Environment variables.</span></span>
  * <span data-ttu-id="6611f-124">Аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="6611f-124">Command-line arguments.</span></span>
* <span data-ttu-id="6611f-125">Настраивает [входа](xref:fundamentals/logging/index) для вывода на консоль и отладки.</span><span class="sxs-lookup"><span data-stu-id="6611f-125">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="6611f-126">Ведение журнала включает [фильтрации журнала](xref:fundamentals/logging/index#log-filtering) правила, указанные в разделе конфигурации ведения журнала *appsettings.json* или *appsettings. {} Среда} .json* файл.</span><span class="sxs-lookup"><span data-stu-id="6611f-126">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="6611f-127">При работе под IIS позволяет [интеграции IIS](xref:publishing/iis) путем настройки пути к базовой папке и порта сервера должен прослушивать при использовании [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6611f-127">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="6611f-128">Модуль создает обратного прокси-сервера служб IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6611f-128">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="6611f-129">Кроме того, настраивает приложение для [перехватить ошибки запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="6611f-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="6611f-130">Параметры по умолчанию служб IIS см. в разделе [IIS параметры раздела узла ASP.NET Core в Windows с помощью IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="6611f-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="6611f-131">*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC.</span><span class="sxs-lookup"><span data-stu-id="6611f-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="6611f-132">Корень содержимого по умолчанию — [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="6611f-132">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="6611f-133">Содержимое корневого по умолчанию (`Directory.GetCurrentDirectory`) приводит к с помощью корневую папку веб-проекта, который является корнем содержимого при запуске приложения из корневой папки (например, вызов [dotnet запуска](/dotnet/core/tools/dotnet-run) из папки проекта).</span><span class="sxs-lookup"><span data-stu-id="6611f-133">The default content root (`Directory.GetCurrentDirectory`) results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="6611f-134">Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="6611f-134">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="6611f-135">Дополнительные сведения о настройке приложения см. в разделе [конфигурации в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="6611f-135">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="6611f-136">В качестве альтернативы с помощью статического `CreateDefaultBuilder` метод, создание узла из [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживаемых подход с ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="6611f-136">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="6611f-137">Дополнительные сведения см. в разделе вкладке 1.x ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="6611f-137">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-138">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-138">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-139">Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="6611f-139">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="6611f-140">Создание ведущего приложения обычно выполняется в точке входа приложения, `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="6611f-140">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="6611f-141">В шаблоны проектов `Main` находится в папке *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="6611f-141">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="6611f-142">Типичный *Program.cs* вызовы [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) начата Настройка узла:</span><span class="sxs-lookup"><span data-stu-id="6611f-142">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="6611f-143">`WebHostBuilder`требуется [сервера, который реализует IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="6611f-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="6611f-144">Встроенные серверы являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 был вызван HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="6611f-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="6611f-145">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) указывает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="6611f-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="6611f-146">*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC.</span><span class="sxs-lookup"><span data-stu-id="6611f-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="6611f-147">Указанный корневой содержимого по умолчанию для `UseContentRoot` — [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="6611f-147">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="6611f-148">Это приведет к с помощью корневую папку веб-проекта, который является корнем содержимого при запуске приложения из корневой папки (например, вызов [dotnet запуска](/dotnet/core/tools/dotnet-run) из папки проекта).</span><span class="sxs-lookup"><span data-stu-id="6611f-148">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="6611f-149">Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="6611f-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="6611f-150">Чтобы использовать в качестве обратного прокси-сервера IIS, вызовите [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) как часть построения узла.</span><span class="sxs-lookup"><span data-stu-id="6611f-150">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="6611f-151">`UseIISIntegration`неправильно настраивает *сервера*, таких как [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="6611f-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="6611f-152">`UseIISIntegration`настраивает сервер должен прослушивать при использовании порта и пути к базовой папке [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) создание обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="6611f-152">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="6611f-153">Чтобы использовать IIS с ASP.NET Core, необходимо указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="6611f-153">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="6611f-154">`UseIISIntegration`активируется, только при запуске IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="6611f-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="6611f-155">Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](xref:fundamentals/servers/aspnet-core-module) и [ссылки на конфигурации ASP.NET Core модуль](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="6611f-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="6611f-156">Минимальную реализацию, которая настраивает узел (и приложения ASP.NET Core) включает в себя указание сервера и настройки конвейера запросов приложения:</span><span class="sxs-lookup"><span data-stu-id="6611f-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="6611f-157">При настройке узла, чтобы обеспечить [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) методы.</span><span class="sxs-lookup"><span data-stu-id="6611f-157">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="6611f-158">При указании `Startup` класса, оно должно определять `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="6611f-158">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="6611f-159">Дополнительные сведения см. в разделе [запуск приложения в ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="6611f-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="6611f-160">Несколько вызовов `ConfigureServices` append друг с другом.</span><span class="sxs-lookup"><span data-stu-id="6611f-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="6611f-161">Несколько вызовов `Configure` или `UseStartup` на `WebHostBuilder` заменить предыдущие параметры.</span><span class="sxs-lookup"><span data-stu-id="6611f-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="6611f-162">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="6611f-162">Host configuration values</span></span>

<span data-ttu-id="6611f-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) обеспечивает следующие подходы для установки большинства значений конфигурации, доступные для узла:</span><span class="sxs-lookup"><span data-stu-id="6611f-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides the following approaches for setting most of the available configuration values for the host:</span></span>

* <span data-ttu-id="6611f-164">Переменные среды в формате `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="6611f-164">Environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="6611f-165">Например, `ASPNETCORE_DETAILEDERRORS`.</span><span class="sxs-lookup"><span data-stu-id="6611f-165">For example, `ASPNETCORE_DETAILEDERRORS`.</span></span>
* <span data-ttu-id="6611f-166">Явные методы, такие как `CaptureStartupErrors`.</span><span class="sxs-lookup"><span data-stu-id="6611f-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="6611f-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="6611f-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="6611f-168">При установке значения с `UseSetting`, имеет значение как строка независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="6611f-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="6611f-169">Запись ошибки при загрузке</span><span class="sxs-lookup"><span data-stu-id="6611f-169">Capture Startup Errors</span></span>

<span data-ttu-id="6611f-170">Этот параметр управляет отслеживания ошибок при запуске.</span><span class="sxs-lookup"><span data-stu-id="6611f-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="6611f-171">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="6611f-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="6611f-172">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="6611f-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="6611f-173">**По умолчанию**: по умолчанию используется значение `false` Если приложение запускается с Kestrel за IIS, где значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="6611f-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="6611f-174">**Задать с помощью**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="6611f-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="6611f-175">**Переменная среды**:`ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="6611f-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="6611f-176">Когда `false`, ошибки во время запуска результат в узле выхода.</span><span class="sxs-lookup"><span data-stu-id="6611f-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="6611f-177">Когда `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="6611f-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="6611f-180">Содержимое корневого</span><span class="sxs-lookup"><span data-stu-id="6611f-180">Content Root</span></span>

<span data-ttu-id="6611f-181">Этот параметр определяет, где ASP.NET Core начинает поиск файлов содержимого, такие как представления MVC.</span><span class="sxs-lookup"><span data-stu-id="6611f-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="6611f-182">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="6611f-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="6611f-183">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="6611f-183">**Type**: *string*</span></span>  
<span data-ttu-id="6611f-184">**По умолчанию**: по умолчанию для папки, в которой хранится сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="6611f-185">**Задать с помощью**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="6611f-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="6611f-186">**Переменная среды**:`ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="6611f-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="6611f-187">Содержимое корневого также используется как базовый путь для [параметр корневого веб-каталога](#web-root).</span><span class="sxs-lookup"><span data-stu-id="6611f-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="6611f-188">Если путь не существует, узлу не удается запустить.</span><span class="sxs-lookup"><span data-stu-id="6611f-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="6611f-191">Подробные сведения об ошибках</span><span class="sxs-lookup"><span data-stu-id="6611f-191">Detailed Errors</span></span>

<span data-ttu-id="6611f-192">Определяет ли подробные ошибки должны быть зафиксированы.</span><span class="sxs-lookup"><span data-stu-id="6611f-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="6611f-193">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="6611f-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="6611f-194">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="6611f-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="6611f-195">**По умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="6611f-195">**Default**: false</span></span>  
<span data-ttu-id="6611f-196">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="6611f-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="6611f-197">**Переменная среды**:`ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="6611f-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="6611f-198">При включении (или когда <a href="#environment">среды</a> равно `Development`), приложение записывает подробные сведения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="6611f-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="6611f-201">Среда</span><span class="sxs-lookup"><span data-stu-id="6611f-201">Environment</span></span>

<span data-ttu-id="6611f-202">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-202">Sets the app's environment.</span></span>

<span data-ttu-id="6611f-203">**Ключ**: среды</span><span class="sxs-lookup"><span data-stu-id="6611f-203">**Key**: environment</span></span>  
<span data-ttu-id="6611f-204">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="6611f-204">**Type**: *string*</span></span>  
<span data-ttu-id="6611f-205">**По умолчанию**: рабочей среде</span><span class="sxs-lookup"><span data-stu-id="6611f-205">**Default**: Production</span></span>  
<span data-ttu-id="6611f-206">**Задать с помощью**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="6611f-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="6611f-207">**Переменная среды**:`ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="6611f-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="6611f-208">Можно задать *среды* любое значение.</span><span class="sxs-lookup"><span data-stu-id="6611f-208">You can set the *Environment* to any value.</span></span> <span data-ttu-id="6611f-209">Значения, определяемые платформой `Development`, `Staging`, и `Production`.</span><span class="sxs-lookup"><span data-stu-id="6611f-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="6611f-210">Значения не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="6611f-210">Values aren't case sensitive.</span></span> <span data-ttu-id="6611f-211">По умолчанию *среды* считывается из `ASPNETCORE_ENVIRONMENT` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6611f-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="6611f-212">При использовании [Visual Studio](https://www.visualstudio.com/), переменные среды может быть задан в *launchSettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="6611f-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="6611f-213">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6611f-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="6611f-216">Размещение загрузки сборок</span><span class="sxs-lookup"><span data-stu-id="6611f-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="6611f-217">Задает размещения сборок при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="6611f-218">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="6611f-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="6611f-219">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="6611f-219">**Type**: *string*</span></span>  
<span data-ttu-id="6611f-220">**По умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="6611f-220">**Default**: Empty string</span></span>  
<span data-ttu-id="6611f-221">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="6611f-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="6611f-222">**Переменная среды**:`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="6611f-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="6611f-223">Разделенный точками с запятой строка размещения запуска сборок для загрузки при запуске.</span><span class="sxs-lookup"><span data-stu-id="6611f-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="6611f-224">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6611f-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="6611f-225">Несмотря на то, что значения конфигурации по умолчанию устанавливается равным пустой строке, размещения сборок запуска всегда включать сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="6611f-226">При предоставлении услуг размещения сборок при запуске, они добавляются в сборку приложения для загрузки, когда приложение создает его общие службы во время запуска.</span><span class="sxs-lookup"><span data-stu-id="6611f-226">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-229">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6611f-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="6611f-230">Предпочтение размещение URL-адреса</span><span class="sxs-lookup"><span data-stu-id="6611f-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="6611f-231">Указывает ли узел должен прослушивать URL-адресов настроены `WebHostBuilder` вместо настроенных с `IServer` реализации.</span><span class="sxs-lookup"><span data-stu-id="6611f-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="6611f-232">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="6611f-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="6611f-233">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="6611f-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="6611f-234">**По умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="6611f-234">**Default**: true</span></span>  
<span data-ttu-id="6611f-235">**Задать с помощью**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="6611f-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="6611f-236">**Переменная среды**:`ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="6611f-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="6611f-237">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6611f-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-240">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6611f-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="6611f-241">Запретить размещение запуска</span><span class="sxs-lookup"><span data-stu-id="6611f-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="6611f-242">Предотвращает автоматическая загрузка размещение запуска сборок, включая размещение сборок запуска задаются сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="6611f-243">В разделе [Добавление компонентов приложения из внешней сборки с помощью IHostingStartup](xref:hosting/ihostingstartup) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="6611f-243">See [Add app features from an external assembly using IHostingStartup](xref:hosting/ihostingstartup) for more information.</span></span>

<span data-ttu-id="6611f-244">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="6611f-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="6611f-245">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="6611f-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="6611f-246">**По умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="6611f-246">**Default**: false</span></span>  
<span data-ttu-id="6611f-247">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="6611f-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="6611f-248">**Переменная среды**:`ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="6611f-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="6611f-249">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6611f-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-252">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6611f-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="6611f-253">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="6611f-253">Server URLs</span></span>

<span data-ttu-id="6611f-254">Указывает IP-адреса или адреса узлов с портами и протоколы, которые сервер должен прослушивать запросы.</span><span class="sxs-lookup"><span data-stu-id="6611f-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="6611f-255">**Ключ**: URL-адреса</span><span class="sxs-lookup"><span data-stu-id="6611f-255">**Key**: urls</span></span>  
<span data-ttu-id="6611f-256">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="6611f-256">**Type**: *string*</span></span>  
<span data-ttu-id="6611f-257">**По умолчанию**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="6611f-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="6611f-258">**Задать с помощью**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="6611f-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="6611f-259">**Переменная среды**:`ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="6611f-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="6611f-260">Значение, разделенных точкой с запятой (;) префиксов список URL-адрес которого сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="6611f-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="6611f-261">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="6611f-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="6611f-262">Используйте "\*» для указания, что сервер должен прослушивать запросы по любой IP-адрес или имя узла, используя указанный порт и протокол (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="6611f-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="6611f-263">Протокол (`http://` или `https://`) должен быть включен, все URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="6611f-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="6611f-264">Поддерживаемые форматы различаться для разных серверов.</span><span class="sxs-lookup"><span data-stu-id="6611f-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="6611f-266">Kestrel имеет собственную конфигурацию конечной точки API.</span><span class="sxs-lookup"><span data-stu-id="6611f-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="6611f-267">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="6611f-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="6611f-269">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="6611f-269">Shutdown Timeout</span></span>

<span data-ttu-id="6611f-270">Указывает время ожидания для завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="6611f-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="6611f-271">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="6611f-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="6611f-272">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="6611f-272">**Type**: *int*</span></span>  
<span data-ttu-id="6611f-273">**По умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="6611f-273">**Default**: 5</span></span>  
<span data-ttu-id="6611f-274">**Задать с помощью**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="6611f-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="6611f-275">**Переменная среды**:`ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="6611f-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="6611f-276">Несмотря на то, что ключ принимает *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` принимает метод расширения `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="6611f-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="6611f-277">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="6611f-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-280">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="6611f-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="6611f-281">При запуске сборки</span><span class="sxs-lookup"><span data-stu-id="6611f-281">Startup Assembly</span></span>

<span data-ttu-id="6611f-282">Определяет сборку для поиска `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="6611f-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="6611f-283">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="6611f-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="6611f-284">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="6611f-284">**Type**: *string*</span></span>  
<span data-ttu-id="6611f-285">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="6611f-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="6611f-286">**Задать с помощью**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="6611f-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="6611f-287">**Переменная среды**:`ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="6611f-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="6611f-288">Сборку можно ссылаться по имени (`string`) или тип (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="6611f-288">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="6611f-289">При наличии нескольких `UseStartup` методы вызываются, приоритет имеет последняя.</span><span class="sxs-lookup"><span data-stu-id="6611f-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="6611f-292">Персональный компьютер</span><span class="sxs-lookup"><span data-stu-id="6611f-292">Web Root</span></span>

<span data-ttu-id="6611f-293">Задает относительный путь для приложения статических ресурсах.</span><span class="sxs-lookup"><span data-stu-id="6611f-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="6611f-294">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="6611f-294">**Key**: webroot</span></span>  
<span data-ttu-id="6611f-295">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="6611f-295">**Type**: *string*</span></span>  
<span data-ttu-id="6611f-296">**По умолчанию**: Если не указан, значение по умолчанию — "(Content Root)/wwwroot», если путь существует.</span><span class="sxs-lookup"><span data-stu-id="6611f-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="6611f-297">Если путь не существует, то используется поставщик холостой файла.</span><span class="sxs-lookup"><span data-stu-id="6611f-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="6611f-298">**Задать с помощью**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="6611f-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="6611f-299">**Переменная среды**:`ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="6611f-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="6611f-302">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="6611f-302">Overriding configuration</span></span>

<span data-ttu-id="6611f-303">Используйте [конфигурации](xref:fundamentals/configuration/index) для настройки узла.</span><span class="sxs-lookup"><span data-stu-id="6611f-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="6611f-304">В следующем примере конфигурации узла при необходимости указывается в *hosting.json* файла.</span><span class="sxs-lookup"><span data-stu-id="6611f-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="6611f-305">Загрузить любую конфигурацию из *hosting.json* файл может быть переопределена аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="6611f-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="6611f-306">Конфигурации построения (в `config`) используется для настройки узла с `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="6611f-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6611f-308">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="6611f-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="6611f-309">Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:</span><span class="sxs-lookup"><span data-stu-id="6611f-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-311">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="6611f-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="6611f-312">Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:</span><span class="sxs-lookup"><span data-stu-id="6611f-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="6611f-313">`UseConfiguration` Метода расширения не может в настоящее время синтаксического анализа раздела конфигурации, возвращенных `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="6611f-313">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="6611f-314">`GetSection` Метод фильтрует ключи в запрошенный раздел конфигурации, но оставляет имя раздела по ключам (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="6611f-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="6611f-315">`UseConfiguration` Метод ожидает ключи для сопоставления `WebHostBuilder` ключи (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="6611f-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="6611f-316">Имя раздела по ключам присутствия запрещает настраивать узла значения этого раздела.</span><span class="sxs-lookup"><span data-stu-id="6611f-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="6611f-317">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="6611f-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="6611f-318">Дополнительные сведения и способы решения проблем см. в разделе [передачи раздела конфигурации в WebHostBuilder.UseConfiguration использует полное ключи](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="6611f-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="6611f-319">Для указания выполнения на определенный URL-адрес узла, можно передать в нужное значение из командной строки при выполнении `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="6611f-319">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="6611f-320">Аргумент командной строки переопределяет `urls` значение из *hosting.json* файла, а сервер прослушивает порт 8080:</span><span class="sxs-lookup"><span data-stu-id="6611f-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="6611f-321">Упорядочение важности</span><span class="sxs-lookup"><span data-stu-id="6611f-321">Ordering importance</span></span>

<span data-ttu-id="6611f-322">Некоторые из `WebHostBuilder` параметры сначала считываются из переменных среды, если задать.</span><span class="sxs-lookup"><span data-stu-id="6611f-322">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="6611f-323">Эти переменные среды используйте формат `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="6611f-323">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="6611f-324">Чтобы задать URL-адреса, которые прослушивает сервер по умолчанию, задайте `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="6611f-324">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="6611f-325">Эти значения переменной среды можно переопределить, указав конфигурацию (с помощью `UseConfiguration`) или явно задать значение (с помощью `UseSetting` или одного из методов явного расширения, такие как `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="6611f-325">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="6611f-326">Узел использует какой бы параметр задает значение последнего.</span><span class="sxs-lookup"><span data-stu-id="6611f-326">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="6611f-327">Если требуется программно присвоено одно значение по умолчанию URL-адрес, но позволить приложению переопределить с помощью конфигурации, можно использовать настройки командной строки после настройки URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="6611f-327">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="6611f-328">В разделе [переопределение конфигурации](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="6611f-328">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="6611f-329">Запуск узла</span><span class="sxs-lookup"><span data-stu-id="6611f-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="6611f-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="6611f-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="6611f-331">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="6611f-331">**Run**</span></span>

<span data-ttu-id="6611f-332">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:</span><span class="sxs-lookup"><span data-stu-id="6611f-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="6611f-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="6611f-333">**Start**</span></span>

<span data-ttu-id="6611f-334">Узел без блокирования запускается путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="6611f-334">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="6611f-335">Если передать список URL-адресов для `Start` метод, он прослушивает URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="6611f-335">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="6611f-336">Можно инициализировать и начать новый узел с помощью предварительно настроенного значения по умолчанию `CreateDefaultBuilder` с помощью статических удобных метода.</span><span class="sxs-lookup"><span data-stu-id="6611f-336">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="6611f-337">Эти методы требуют запуска серверу без вывода на консоль с [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) ожидания break (Ctrl-C или SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="6611f-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="6611f-338">**Начала (приложение RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="6611f-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="6611f-339">Начать с `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="6611f-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="6611f-340">Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="6611f-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="6611f-341">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="6611f-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="6611f-342">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="6611f-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="6611f-343">**Запуск (строковый URL-адрес, RequestDelegate приложения)**</span><span class="sxs-lookup"><span data-stu-id="6611f-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="6611f-344">Запуск с URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="6611f-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="6611f-345">Дает тот же результат, как **начала (приложение RequestDelegate)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="6611f-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="6611f-346">**Запуск (действие<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="6611f-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="6611f-347">Использовать экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для использования маршрутизации по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="6611f-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="6611f-348">В примере можно используйте следующие запросы браузера:</span><span class="sxs-lookup"><span data-stu-id="6611f-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="6611f-349">Запрос</span><span class="sxs-lookup"><span data-stu-id="6611f-349">Request</span></span>                                    | <span data-ttu-id="6611f-350">Ответ</span><span class="sxs-lookup"><span data-stu-id="6611f-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="6611f-351">Привет, Мартин!</span><span class="sxs-lookup"><span data-stu-id="6611f-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="6611f-352">Буэнос dias Catrina!</span><span class="sxs-lookup"><span data-stu-id="6611f-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="6611f-353">Вызывает исключение со строкой «ooops!»</span><span class="sxs-lookup"><span data-stu-id="6611f-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="6611f-354">Вызывает исключение со строкой «Uh ой!»</span><span class="sxs-lookup"><span data-stu-id="6611f-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="6611f-355">Sante Kevin!</span><span class="sxs-lookup"><span data-stu-id="6611f-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="6611f-356">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="6611f-356">Hello World!</span></span>                             |

<span data-ttu-id="6611f-357">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="6611f-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="6611f-358">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="6611f-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="6611f-359">**Запуск (URL-адрес, действие строки<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="6611f-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="6611f-360">Использовать URL-адреса и имени экземпляра `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="6611f-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="6611f-361">Дает тот же результат, как **запуска (действие<IRouteBuilder> routeBuilder)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="6611f-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="6611f-362">**StartWith (действие<IApplicationBuilder> приложения)**</span><span class="sxs-lookup"><span data-stu-id="6611f-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="6611f-363">Укажите делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="6611f-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="6611f-364">Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="6611f-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="6611f-365">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="6611f-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="6611f-366">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="6611f-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="6611f-367">**StartWith (URL-адрес, действие строки<IApplicationBuilder> приложения)**</span><span class="sxs-lookup"><span data-stu-id="6611f-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="6611f-368">Укажите URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="6611f-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="6611f-369">Дает тот же результат, как **StartWith (действие<IApplicationBuilder> приложения)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="6611f-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="6611f-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="6611f-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="6611f-371">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="6611f-371">**Run**</span></span>

<span data-ttu-id="6611f-372">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:</span><span class="sxs-lookup"><span data-stu-id="6611f-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="6611f-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="6611f-373">**Start**</span></span>

<span data-ttu-id="6611f-374">Узел без блокирования запускается путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="6611f-374">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="6611f-375">Если передать список URL-адресов для `Start` метод, он прослушивает URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="6611f-375">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="6611f-376">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="6611f-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="6611f-377">[IHostingEnvironment интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде размещения веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-377">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="6611f-378">Можно использовать [внедрение конструктора](xref:fundamentals/dependency-injection) для получения `IHostingEnvironment` для использования его свойства и методы расширения:</span><span class="sxs-lookup"><span data-stu-id="6611f-378">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="6611f-379">Можно использовать [подход на основе соглашения о](xref:fundamentals/environments#startup-conventions) нужно настроить приложение во время запуска, в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="6611f-379">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="6611f-380">Кроме того, можно ввести `IHostingEnvironment` в `Startup` конструктора для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="6611f-380">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="6611f-381">В дополнение к `IsDevelopment` метод расширения `IHostingEnvironment` предлагает `IsStaging`, `IsProduction`, и `IsEnvironment(string environmentName)` методы.</span><span class="sxs-lookup"><span data-stu-id="6611f-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="6611f-382">В разделе [работа с несколькими средами](xref:fundamentals/environments) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="6611f-382">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="6611f-383">`IHostingEnvironment` Службы также могут быть добавлены непосредственно в `Configure` метода настройки конвейер обработки:</span><span class="sxs-lookup"><span data-stu-id="6611f-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="6611f-384">Можно ввести `IHostingEnvironment` в `Invoke` метод при создании пользовательских [по промежуточного слоя](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="6611f-384">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="6611f-385">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="6611f-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="6611f-386">[IApplicationLifetime интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) дает возможность выполнять действия, выполняемые после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="6611f-386">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="6611f-387">Три свойства для интерфейса являются токенов отмены, которые можно зарегистрировать `Action` методы для определения событий запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="6611f-387">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="6611f-388">Имеется также `StopApplication` метод.</span><span class="sxs-lookup"><span data-stu-id="6611f-388">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="6611f-389">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="6611f-389">Cancellation Token</span></span>    | <span data-ttu-id="6611f-390">Запустить &#8230;</span><span class="sxs-lookup"><span data-stu-id="6611f-390">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="6611f-391">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="6611f-391">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="6611f-392">Узел выполняет правильное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="6611f-392">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="6611f-393">По-прежнему может обрабатывать запросы.</span><span class="sxs-lookup"><span data-stu-id="6611f-393">Requests may still be processing.</span></span> <span data-ttu-id="6611f-394">Завершение работы блокируется до завершения этого события.</span><span class="sxs-lookup"><span data-stu-id="6611f-394">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="6611f-395">Узел завершает нормальное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="6611f-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="6611f-396">Все запросы будут обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="6611f-396">All requests should be processed.</span></span> <span data-ttu-id="6611f-397">Завершение работы блокируется до завершения этого события.</span><span class="sxs-lookup"><span data-stu-id="6611f-397">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="6611f-398">Метод</span><span class="sxs-lookup"><span data-stu-id="6611f-398">Method</span></span>            | <span data-ttu-id="6611f-399">Действие</span><span class="sxs-lookup"><span data-stu-id="6611f-399">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="6611f-400">Завершение запросов текущего приложения.</span><span class="sxs-lookup"><span data-stu-id="6611f-400">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="6611f-401">Устранение неполадок System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="6611f-401">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="6611f-402">**Применяется к только базовые ASP.NET 2.0**</span><span class="sxs-lookup"><span data-stu-id="6611f-402">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="6611f-403">При создании узла, внедряя `IStartup` непосредственно в контейнер внедрения зависимостей, а не вызовом `UseStartup` или `Configure`, может возникнуть следующая ошибка: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="6611f-403">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="6611f-404">Это происходит потому, что [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) необходим для проверки на наличие `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="6611f-404">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="6611f-405">Если вы вручную ввести `IStartup` в контейнер внедрения зависимостей, добавьте следующий вызов для вашей `WebHostBuilder` с указанным именем сборки:</span><span class="sxs-lookup"><span data-stu-id="6611f-405">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="6611f-406">Кроме того, добавить фиктивное `Configure` для вашей `WebHostBuilder`, который устанавливает `applicationName`(`ApplicationKey`) автоматически:</span><span class="sxs-lookup"><span data-stu-id="6611f-406">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="6611f-407">**Примечание**: это необходимо только обязательные с выпуском ASP.NET Core 2.0 и только если вы не вызываете `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="6611f-407">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="6611f-408">Дополнительные сведения см. в разделе [объявления: Microsoft.Extensions.PlatformAbstractions был удален (комментарий)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и [StartupInjection пример](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="6611f-408">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6611f-409">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="6611f-409">Additional resources</span></span>

* [<span data-ttu-id="6611f-410">Публикация в Windows с помощью служб IIS</span><span class="sxs-lookup"><span data-stu-id="6611f-410">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="6611f-411">Публикация в Linux с помощью Nginx</span><span class="sxs-lookup"><span data-stu-id="6611f-411">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="6611f-412">Публикация в Linux с помощью Apache</span><span class="sxs-lookup"><span data-stu-id="6611f-412">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="6611f-413">Узел в службе Windows</span><span class="sxs-lookup"><span data-stu-id="6611f-413">Host in a Windows Service</span></span>](xref:hosting/windows-service)
