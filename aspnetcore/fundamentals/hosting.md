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
ms.openlocfilehash: 7deccf135ddd21729206ebed58ddc8aca52c1deb
ms.sourcegitcommit: 8f42ab93402c1b8044815e1e48d0bb84c81f8b59
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/29/2017
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="2a2ac-104">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="2a2ac-104">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="2a2ac-105">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="2a2ac-105">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="2a2ac-106">Приложения ASP.NET Core настраивают и запускают *хост*, который отвечает за запуск приложений и управление жизненным циклом.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-106">ASP.NET Core apps configure and launch a *host*, which is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="2a2ac-107">Как минимум узел настраивает сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="2a2ac-108">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="2a2ac-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2a2ac-110">Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="2a2ac-111">Это обычно выполняется в точке входа приложения, `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-111">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="2a2ac-112">В шаблоны проектов `Main` находится в папке *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="2a2ac-113">Типичный *Program.cs* вызовы [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) начата Настройка узла:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="2a2ac-114">`CreateDefaultBuilder`выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="2a2ac-115">Настраивает [Kestrel](servers/kestrel.md) и веб-сервер.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="2a2ac-116">Параметры по умолчанию Kestrel см. в разделе [Kestrel параметры раздела Kestrel реализация веб-сервера в ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="2a2ac-117">Задает содержимое корневого [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-117">Sets the content root to [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="2a2ac-118">Необязательная конфигурация загружает из:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="2a2ac-119">*appSettings.JSON*.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="2a2ac-120">*appSettings. {Среды} .json*.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="2a2ac-121">[Секреты пользователя](xref:security/app-secrets) при запуске приложения `Development` среде.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="2a2ac-122">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-122">Environment variables.</span></span>
  * <span data-ttu-id="2a2ac-123">Аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-123">Command-line arguments.</span></span>
* <span data-ttu-id="2a2ac-124">Настраивает [входа](xref:fundamentals/logging/index) для вывода на консоль и отладка с [фильтрации журнала](xref:fundamentals/logging/index#log-filtering) правила, указанные в разделе конфигурации ведения журнала *appsettings.json* или *appsettings. {Среды} .json* файл.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output with [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="2a2ac-125">При работе под IIS позволяет [интеграции IIS](xref:publishing/iis) путем настройки пути к базовой папке и порта сервера должен прослушивать при использовании [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-125">When running behind IIS, enables [IIS integration](xref:publishing/iis) by configuring the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="2a2ac-126">Модуль создает обратного прокси-сервера служб IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-126">The module creates a reverse-proxy between IIS and Kestrel.</span></span> <span data-ttu-id="2a2ac-127">Кроме того, настраивает приложение для [перехватить ошибки запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-127">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="2a2ac-128">Параметры по умолчанию служб IIS см. в разделе [IIS параметры раздела узла ASP.NET Core в Windows с помощью IIS](xref:publishing/iis#iis-options).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-128">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:publishing/iis#iis-options).</span></span>

<span data-ttu-id="2a2ac-129">*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-129">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="2a2ac-130">Корень содержимого по умолчанию — [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-130">The default content root is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span> <span data-ttu-id="2a2ac-131">Это приведет к с помощью корневую папку веб-проекта, который является корнем содержимого при запуске приложения из корневой папки (например, вызов [dotnet запуска](/dotnet/core/tools/dotnet-run) из папки проекта).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-131">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="2a2ac-132">Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-132">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="2a2ac-133">В разделе [конфигурации в ASP.NET Core](xref:fundamentals/configuration/index) Дополнительные сведения о настройке приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-133">See [Configuration in ASP.NET Core](xref:fundamentals/configuration/index) for more information on app configuration.</span></span>

> [!NOTE]
> <span data-ttu-id="2a2ac-134">В качестве альтернативы с помощью статического `CreateDefaultBuilder` метод, создание узла из [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживаемых подход с ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-134">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="2a2ac-135">См. на вкладке 1.x ASP.NET Core для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-135">See the ASP.NET Core 1.x tab for more information.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-136">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-136">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-137">Создание узла с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-137">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="2a2ac-138">Это обычно выполняется в точке входа приложения, `Main` метод.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-138">This is typically performed in your app's entry point, the `Main` method.</span></span> <span data-ttu-id="2a2ac-139">В шаблоны проектов `Main` находится в папке *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-139">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="2a2ac-140">Следующие *Program.cs* демонстрируется использование `WebHostBuilder` для создания узла:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-140">The following *Program.cs* demonstrates how to use `WebHostBuilder` to build the host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="2a2ac-141">`WebHostBuilder`требуется [сервера, который реализует IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="2a2ac-142">Встроенные серверы являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 был вызван HTTP.sys [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="2a2ac-143">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) указывает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="2a2ac-144">*Содержимое корневого* определяет, где узел ищет файлы содержимого, такие как файлы представления MVC.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="2a2ac-145">Указанный корневой содержимого по умолчанию для `UseContentRoot` — [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-145">The default content root supplied to `UseContentRoot` is [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="2a2ac-146">Это приведет к с помощью корневую папку веб-проекта, который является корнем содержимого при запуске приложения из корневой папки (например, вызов [dotnet запуска](/dotnet/core/tools/dotnet-run) из папки проекта).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-146">This results in using the web project's root folder as the content root when the app is started from the root folder (for example, calling [dotnet run](/dotnet/core/tools/dotnet-run) from the project folder).</span></span> <span data-ttu-id="2a2ac-147">Это значение по умолчанию, используемых в [Visual Studio](https://www.visualstudio.com/) и [dotnet новые шаблоны](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="2a2ac-148">Чтобы использовать в качестве обратного прокси-сервера IIS, вызовите [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) как часть построения узла.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="2a2ac-149">`UseIISIntegration`неправильно настраивает *сервера*, таких как [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="2a2ac-150">`UseIISIntegration`настраивает сервер должен прослушивать при использовании порта и пути к базовой папке [модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) создание обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-150">`UseIISIntegration` configures the base path and port the server should listen on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse-proxy between Kestrel and IIS.</span></span> <span data-ttu-id="2a2ac-151">Чтобы использовать IIS с ASP.NET Core, необходимо указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-151">To use IIS with ASP.NET Core, you must specify both `UseKestrel` and `UseIISIntegration`.</span></span> <span data-ttu-id="2a2ac-152">`UseIISIntegration`активируется, только при запуске IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="2a2ac-153">Дополнительные сведения см. в разделе [введение в ASP.NET Core модуля](xref:fundamentals/servers/aspnet-core-module) и [ссылки на конфигурации ASP.NET Core модуль](xref:hosting/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:hosting/aspnet-core-module).</span></span>

<span data-ttu-id="2a2ac-154">Минимальную реализацию, которая настраивает узел (и приложения ASP.NET Core) включает в себя указание сервера и настройки конвейера запросов приложения:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="2a2ac-155">При настройке узла, чтобы обеспечить [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) методы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-155">When setting up a host, you can provide [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods.</span></span> <span data-ttu-id="2a2ac-156">При указании `Startup` класса, оно должно определять `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-156">If you specify a `Startup` class, it must define a `Configure` method.</span></span> <span data-ttu-id="2a2ac-157">Дополнительные сведения см. в разделе [запуск приложения в ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="2a2ac-158">Несколько вызовов `ConfigureServices` append друг с другом.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="2a2ac-159">Несколько вызовов `Configure` или `UseStartup` на `WebHostBuilder` заменить предыдущие параметры.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="2a2ac-160">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="2a2ac-160">Host configuration values</span></span>

<span data-ttu-id="2a2ac-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) предоставляет методы для настройки большинства значений конфигурации, доступные для узла, которой можно также задать непосредственно с [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) provides methods for setting most of the available configuration values for the host, which can also be set directly with [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="2a2ac-162">При установке значения с `UseSetting`, значение устанавливается в виде строки (в кавычках) независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-162">When setting a value with `UseSetting`, the value is set as a string (in quotes) regardless of the type.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="2a2ac-163">Запись ошибки при загрузке</span><span class="sxs-lookup"><span data-stu-id="2a2ac-163">Capture Startup Errors</span></span>

<span data-ttu-id="2a2ac-164">Этот параметр управляет отслеживания ошибок при запуске.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-164">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="2a2ac-165">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="2a2ac-165">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="2a2ac-166">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="2a2ac-166">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2a2ac-167">**По умолчанию**: по умолчанию используется значение `false` Если приложение запускается с Kestrel за IIS, где значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-167">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="2a2ac-168">**Задать с помощью**:`CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-168">**Set using**: `CaptureStartupErrors`</span></span>

<span data-ttu-id="2a2ac-169">Когда `false`, ошибки во время запуска результат в узле выхода.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-169">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="2a2ac-170">Когда `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-170">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-171">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-171">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-172">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-172">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="2a2ac-173">Содержимое корневого</span><span class="sxs-lookup"><span data-stu-id="2a2ac-173">Content Root</span></span>

<span data-ttu-id="2a2ac-174">Этот параметр определяет, где ASP.NET Core начинает поиск файлов содержимого, такие как представления MVC.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-174">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="2a2ac-175">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="2a2ac-175">**Key**: contentRoot</span></span>  
<span data-ttu-id="2a2ac-176">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-176">**Type**: *string*</span></span>  
<span data-ttu-id="2a2ac-177">**По умолчанию**: по умолчанию для папки, в которой хранится сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-177">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="2a2ac-178">**Задать с помощью**:`UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-178">**Set using**: `UseContentRoot`</span></span>

<span data-ttu-id="2a2ac-179">Содержимое корневого также используется как базовый путь для [параметр корневого веб-каталога](#web-root).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-179">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="2a2ac-180">Если путь не существует, узлу не удается запустить.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-180">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-181">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-181">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-182">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-182">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="2a2ac-183">Подробные сведения об ошибках</span><span class="sxs-lookup"><span data-stu-id="2a2ac-183">Detailed Errors</span></span>

<span data-ttu-id="2a2ac-184">Определяет ли подробные ошибки должны быть зафиксированы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-184">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="2a2ac-185">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="2a2ac-185">**Key**: detailedErrors</span></span>  
<span data-ttu-id="2a2ac-186">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="2a2ac-186">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2a2ac-187">**По умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="2a2ac-187">**Default**: false</span></span>  
<span data-ttu-id="2a2ac-188">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-188">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="2a2ac-189">При включении (или когда <a href="#environment">среды</a> равно `Development`), приложение записывает подробные сведения об исключениях.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-189">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-190">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-190">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-191">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-191">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="2a2ac-192">Среда</span><span class="sxs-lookup"><span data-stu-id="2a2ac-192">Environment</span></span>

<span data-ttu-id="2a2ac-193">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-193">Sets the app's environment.</span></span>

<span data-ttu-id="2a2ac-194">**Ключ**: среды</span><span class="sxs-lookup"><span data-stu-id="2a2ac-194">**Key**: environment</span></span>  
<span data-ttu-id="2a2ac-195">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-195">**Type**: *string*</span></span>  
<span data-ttu-id="2a2ac-196">**По умолчанию**: рабочей среде</span><span class="sxs-lookup"><span data-stu-id="2a2ac-196">**Default**: Production</span></span>  
<span data-ttu-id="2a2ac-197">**Задать с помощью**:`UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-197">**Set using**: `UseEnvironment`</span></span>

<span data-ttu-id="2a2ac-198">Можно задать *среды* любое значение.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-198">You can set the *Environment* to any value.</span></span> <span data-ttu-id="2a2ac-199">Значения, определяемые платформой `Development`, `Staging`, и `Production`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-199">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="2a2ac-200">Значения не учитывается регистр.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-200">Values aren't case sensitive.</span></span> <span data-ttu-id="2a2ac-201">По умолчанию *среды* считывается из `ASPNETCORE_ENVIRONMENT` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-201">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="2a2ac-202">При использовании [Visual Studio](https://www.visualstudio.com/), переменные среды может быть задан в *launchSettings.json* файла.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-202">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="2a2ac-203">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-203">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-204">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-204">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-205">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-205">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="2a2ac-206">Размещение загрузки сборок</span><span class="sxs-lookup"><span data-stu-id="2a2ac-206">Hosting Startup Assemblies</span></span>

<span data-ttu-id="2a2ac-207">Задает размещения сборок при запуске приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-207">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="2a2ac-208">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="2a2ac-208">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="2a2ac-209">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-209">**Type**: *string*</span></span>  
<span data-ttu-id="2a2ac-210">**По умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="2a2ac-210">**Default**: Empty string</span></span>  
<span data-ttu-id="2a2ac-211">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-211">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="2a2ac-212">Разделенный точками с запятой строка размещения запуска сборок для загрузки при запуске.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-212">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="2a2ac-213">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-213">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="2a2ac-214">Несмотря на то, что значения конфигурации по умолчанию устанавливается равным пустой строке, размещения сборок запуска всегда включать сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-214">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="2a2ac-215">При предоставлении услуг размещения сборок при запуске, они добавляются в сборку приложения для загрузки, когда приложение создает его общие службы во время запуска.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-215">When you provide hosting startup assemblies, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-218">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-218">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="2a2ac-219">Предпочтение размещение URL-адреса</span><span class="sxs-lookup"><span data-stu-id="2a2ac-219">Prefer Hosting URLs</span></span>

<span data-ttu-id="2a2ac-220">Указывает ли узел должен прослушивать URL-адресов настроены `WebHostBuilder` вместо настроенных с `IServer` реализации.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-220">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="2a2ac-221">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="2a2ac-221">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="2a2ac-222">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="2a2ac-222">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2a2ac-223">**По умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="2a2ac-223">**Default**: true</span></span>  
<span data-ttu-id="2a2ac-224">**Задать с помощью**:`PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-224">**Set using**: `PreferHostingUrls`</span></span>

<span data-ttu-id="2a2ac-225">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-225">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-226">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-226">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-227">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-227">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-228">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-228">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="2a2ac-229">Запретить размещение запуска</span><span class="sxs-lookup"><span data-stu-id="2a2ac-229">Prevent Hosting Startup</span></span>

<span data-ttu-id="2a2ac-230">Предотвращает автоматическая загрузка размещение загрузки сборок, включая сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-230">Prevents the automatic loading of hosting startup assemblies, including the app's assembly.</span></span>

<span data-ttu-id="2a2ac-231">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="2a2ac-231">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="2a2ac-232">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="2a2ac-232">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="2a2ac-233">**По умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="2a2ac-233">**Default**: false</span></span>  
<span data-ttu-id="2a2ac-234">**Задать с помощью**:`UseSetting`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-234">**Set using**: `UseSetting`</span></span>

<span data-ttu-id="2a2ac-235">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-235">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-236">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-236">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-237">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-237">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-238">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-238">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="2a2ac-239">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="2a2ac-239">Server URLs</span></span>

<span data-ttu-id="2a2ac-240">Указывает IP-адреса или адреса узлов с портами и протоколы, которые сервер должен прослушивать запросы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-240">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="2a2ac-241">**Ключ**: URL-адреса</span><span class="sxs-lookup"><span data-stu-id="2a2ac-241">**Key**: urls</span></span>  
<span data-ttu-id="2a2ac-242">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-242">**Type**: *string*</span></span>  
<span data-ttu-id="2a2ac-243">**По умолчанию**: http://localhost: 5000</span><span class="sxs-lookup"><span data-stu-id="2a2ac-243">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="2a2ac-244">**Задать с помощью**:`UseUrls`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-244">**Set using**: `UseUrls`</span></span>

<span data-ttu-id="2a2ac-245">Значение, разделенных точкой с запятой (;) префиксов список URL-адрес которого сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-245">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="2a2ac-246">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-246">For example, `http://localhost:123`.</span></span> <span data-ttu-id="2a2ac-247">Используйте "\*» для указания, что сервер должен прослушивать запросы по любой IP-адрес или имя узла, используя указанный порт и протокол (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-247">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="2a2ac-248">Протокол (`http://` или `https://`) должен быть включен, все URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-248">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="2a2ac-249">Поддерживаемые форматы различаться для разных серверов.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-249">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="2a2ac-251">Kestrel имеет собственную конфигурацию конечной точки API.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-251">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="2a2ac-252">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-252">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="2a2ac-254">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="2a2ac-254">Shutdown Timeout</span></span>

<span data-ttu-id="2a2ac-255">Указывает время ожидания для завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-255">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="2a2ac-256">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="2a2ac-256">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="2a2ac-257">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-257">**Type**: *int*</span></span>  
<span data-ttu-id="2a2ac-258">**По умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="2a2ac-258">**Default**: 5</span></span>  
<span data-ttu-id="2a2ac-259">**Задать с помощью**:`UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-259">**Set using**: `UseShutdownTimeout`</span></span>

<span data-ttu-id="2a2ac-260">Несмотря на то, что ключ принимает *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), `UseShutdownTimeout` принимает метод расширения `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-260">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="2a2ac-261">Этот компонент впервые появился в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-261">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-262">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-262">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-263">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-263">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-264">Эта функция недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-264">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="2a2ac-265">При запуске сборки</span><span class="sxs-lookup"><span data-stu-id="2a2ac-265">Startup Assembly</span></span>

<span data-ttu-id="2a2ac-266">Определяет сборку для поиска `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-266">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="2a2ac-267">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="2a2ac-267">**Key**: startupAssembly</span></span>  
<span data-ttu-id="2a2ac-268">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-268">**Type**: *string*</span></span>  
<span data-ttu-id="2a2ac-269">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="2a2ac-269">**Default**: The app's assembly</span></span>  
<span data-ttu-id="2a2ac-270">**Задать с помощью**:`UseStartup`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-270">**Set using**: `UseStartup`</span></span>

<span data-ttu-id="2a2ac-271">Сборку можно ссылаться по имени (`string`) или тип (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-271">You can reference the assembly by name (`string`) or type (`TStartup`).</span></span> <span data-ttu-id="2a2ac-272">При наличии нескольких `UseStartup` методы вызываются, приоритет имеет последняя.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-272">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-273">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-273">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-274">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-274">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="2a2ac-275">Персональный компьютер</span><span class="sxs-lookup"><span data-stu-id="2a2ac-275">Web Root</span></span>

<span data-ttu-id="2a2ac-276">Задает относительный путь для приложения статических ресурсах.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-276">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="2a2ac-277">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="2a2ac-277">**Key**: webroot</span></span>  
<span data-ttu-id="2a2ac-278">**Тип**: *строка*</span><span class="sxs-lookup"><span data-stu-id="2a2ac-278">**Type**: *string*</span></span>  
<span data-ttu-id="2a2ac-279">**По умолчанию**: Если не указан, значение по умолчанию — "(Content Root)/wwwroot», если путь существует.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-279">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="2a2ac-280">Если путь не существует, то используется поставщик холостой файла.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-280">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="2a2ac-281">**Задать с помощью**:`UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="2a2ac-281">**Set using**: `UseWebRoot`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-282">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-282">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-283">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-283">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="2a2ac-284">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="2a2ac-284">Overriding configuration</span></span>

<span data-ttu-id="2a2ac-285">Используйте [конфигурации](xref:fundamentals/configuration/index) для настройки узла.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-285">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="2a2ac-286">В следующем примере конфигурации узла при необходимости указывается в *hosting.json* файла.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-286">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="2a2ac-287">Загрузить любую конфигурацию из *hosting.json* файл может быть переопределена аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-287">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="2a2ac-288">Конфигурации построения (в `config`) используется для настройки узла с `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-288">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-289">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-289">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2a2ac-290">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-290">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="2a2ac-291">Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-291">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-292">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-292">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-293">*Hosting.JSON*:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-293">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="2a2ac-294">Переопределение конфигурации, обеспечиваемой `UseUrls` с *hosting.json* конфигурации первый аргументом командной строки конфигурации второй:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-294">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="2a2ac-295">`UseConfiguration` Метода расширения не может в настоящее время синтаксического анализа раздела конфигурации, возвращенных `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-295">The `UseConfiguration` extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="2a2ac-296">`GetSection` Метод фильтрует ключи в запрошенный раздел конфигурации, но оставляет имя раздела по ключам (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-296">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="2a2ac-297">`UseConfiguration` Метод ожидает ключи для сопоставления `WebHostBuilder` ключи (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-297">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="2a2ac-298">Имя раздела по ключам присутствия запрещает настраивать узла значения этого раздела.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-298">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="2a2ac-299">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-299">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="2a2ac-300">Дополнительные сведения и способы решения проблем см. в разделе [передачи раздела конфигурации в WebHostBuilder.UseConfiguration использует полное ключи](https://github.com/aspnet/Hosting/issues/839).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-300">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="2a2ac-301">Для указания выполнения на определенный URL-адрес узла, можно передать в нужное значение из командной строки при выполнении `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-301">To specify the host run on a particular URL, you could pass in the desired value from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="2a2ac-302">Аргумент командной строки переопределяет `urls` значение из *hosting.json* файла, а сервер прослушивает порт 8080:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-302">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="ordering-importance"></a><span data-ttu-id="2a2ac-303">Упорядочение важности</span><span class="sxs-lookup"><span data-stu-id="2a2ac-303">Ordering importance</span></span>

<span data-ttu-id="2a2ac-304">Некоторые из `WebHostBuilder` параметры сначала считываются из переменных среды, если задать.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-304">Some of the `WebHostBuilder` settings are first read from environment variables, if set.</span></span> <span data-ttu-id="2a2ac-305">Эти переменные среды используйте формат `ASPNETCORE_{configurationKey}`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-305">These environment variables use the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="2a2ac-306">Чтобы задать URL-адреса, которые прослушивает сервер по умолчанию, задайте `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-306">To set the URLs that the server listens on by default, you set `ASPNETCORE_URLS`.</span></span>

<span data-ttu-id="2a2ac-307">Эти значения переменной среды можно переопределить, указав конфигурацию (с помощью `UseConfiguration`) или явно задать значение (с помощью `UseSetting` или одного из методов явного расширения, такие как `UseUrls`).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-307">You can override any of these environment variable values by specifying configuration (using `UseConfiguration`) or by setting the value explicitly (using `UseSetting` or one of the explicit extension methods, such as `UseUrls`).</span></span> <span data-ttu-id="2a2ac-308">Узел использует какой бы параметр задает значение последнего.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-308">The host uses whichever option sets the value last.</span></span> <span data-ttu-id="2a2ac-309">Если требуется программно присвоено одно значение по умолчанию URL-адрес, но позволить приложению переопределить с помощью конфигурации, можно использовать настройки командной строки после настройки URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-309">If you want to programmatically set the default URL to one value but allow it to be overridden with configuration, you can use command-line configuration after setting the URL.</span></span> <span data-ttu-id="2a2ac-310">В разделе [переопределение конфигурации](#overriding-configuration).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-310">See [Overriding configuration](#overriding-configuration).</span></span>

## <a name="starting-the-host"></a><span data-ttu-id="2a2ac-311">Запуск узла</span><span class="sxs-lookup"><span data-stu-id="2a2ac-311">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="2a2ac-312">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-312">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="2a2ac-313">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-313">**Run**</span></span>

<span data-ttu-id="2a2ac-314">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-314">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="2a2ac-315">**Start**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-315">**Start**</span></span>

<span data-ttu-id="2a2ac-316">Узел без блокирования запускается путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-316">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="2a2ac-317">Если передать список URL-адресов для `Start` метод, он прослушивает URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-317">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="2a2ac-318">Можно инициализировать и начать новый узел с помощью предварительно настроенного значения по умолчанию `CreateDefaultBuilder` с помощью статических удобных метода.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-318">You can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="2a2ac-319">Эти методы требуют запуска серверу без вывода на консоль с [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) ожидания break (Ctrl-C или SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="2a2ac-319">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="2a2ac-320">**Начала (приложение RequestDelegate)**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-320">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="2a2ac-321">Начать с `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-321">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="2a2ac-322">Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="2a2ac-322">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="2a2ac-323">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-323">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="2a2ac-324">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-324">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="2a2ac-325">**Запуск (строковый URL-адрес, RequestDelegate приложения)**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-325">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="2a2ac-326">Запуск с URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-326">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="2a2ac-327">Дает тот же результат, как **начала (приложение RequestDelegate)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-327">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="2a2ac-328">**Запуск (действие<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-328">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="2a2ac-329">Использовать экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для использования маршрутизации по промежуточного слоя:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-329">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="2a2ac-330">В примере можно используйте следующие запросы браузера:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-330">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="2a2ac-331">Запрос</span><span class="sxs-lookup"><span data-stu-id="2a2ac-331">Request</span></span>                                    | <span data-ttu-id="2a2ac-332">Ответ</span><span class="sxs-lookup"><span data-stu-id="2a2ac-332">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="2a2ac-333">Привет, Мартин!</span><span class="sxs-lookup"><span data-stu-id="2a2ac-333">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="2a2ac-334">Буэнос dias Catrina!</span><span class="sxs-lookup"><span data-stu-id="2a2ac-334">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="2a2ac-335">Вызывает исключение со строкой «ooops!»</span><span class="sxs-lookup"><span data-stu-id="2a2ac-335">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="2a2ac-336">Вызывает исключение со строкой «Uh ой!»</span><span class="sxs-lookup"><span data-stu-id="2a2ac-336">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="2a2ac-337">Sante Kevin!</span><span class="sxs-lookup"><span data-stu-id="2a2ac-337">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="2a2ac-338">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="2a2ac-338">Hello World!</span></span>                             |

<span data-ttu-id="2a2ac-339">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-339">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="2a2ac-340">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-340">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="2a2ac-341">**Запуск (URL-адрес, действие строки<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-341">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="2a2ac-342">Использовать URL-адреса и имени экземпляра `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-342">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="2a2ac-343">Дает тот же результат, как **запуска (действие<IRouteBuilder> routeBuilder)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-343">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="2a2ac-344">**StartWith (действие<IApplicationBuilder> приложения)**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-344">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="2a2ac-345">Укажите делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-345">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="2a2ac-346">Сделать запрос в браузере для `http://localhost:5000` для получения ответа «Hello World!»</span><span class="sxs-lookup"><span data-stu-id="2a2ac-346">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="2a2ac-347">`WaitForShutdown`блоки, пока не будет выполнена break (Ctrl-C или SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-347">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="2a2ac-348">Приложение отображается `Console.WriteLine` сообщение и ожидает keypress для выхода.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-348">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="2a2ac-349">**StartWith (URL-адрес, действие строки<IApplicationBuilder> приложения)**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-349">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="2a2ac-350">Укажите URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-350">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="2a2ac-351">Дает тот же результат, как **StartWith (действие<IApplicationBuilder> приложения)**, за исключением приложение реагирует на `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-351">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="2a2ac-352">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="2a2ac-352">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="2a2ac-353">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-353">**Run**</span></span>

<span data-ttu-id="2a2ac-354">`Run` Метод запускает веб-приложения и блокирует вызывающий поток до завершения работы узла:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-354">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="2a2ac-355">**Start**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-355">**Start**</span></span>

<span data-ttu-id="2a2ac-356">Узел без блокирования запускается путем вызова его `Start` метод:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-356">You can run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="2a2ac-357">Если передать список URL-адресов для `Start` метод, он прослушивает URL-адреса:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-357">If you pass a list of URLs to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="2a2ac-358">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="2a2ac-358">IHostingEnvironment interface</span></span>

<span data-ttu-id="2a2ac-359">[IHostingEnvironment интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде размещения веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-359">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="2a2ac-360">Можно использовать [внедрение конструктора](xref:fundamentals/dependency-injection) для получения `IHostingEnvironment` для использования его свойства и методы расширения:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-360">You can use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="2a2ac-361">Можно использовать [подход на основе соглашения о](xref:fundamentals/environments#startup-conventions) нужно настроить приложение во время запуска, в зависимости от среды.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-361">You can use a [convention-based approach](xref:fundamentals/environments#startup-conventions) to configure your app at startup based on the environment.</span></span> <span data-ttu-id="2a2ac-362">Кроме того, можно ввести `IHostingEnvironment` в `Startup` конструктора для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-362">Alternatively, you can inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="2a2ac-363">В дополнение к `IsDevelopment` метод расширения `IHostingEnvironment` предлагает `IsStaging`, `IsProduction`, и `IsEnvironment(string environmentName)` методы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-363">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="2a2ac-364">В разделе [работа с несколькими средами](xref:fundamentals/environments) подробные сведения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-364">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="2a2ac-365">`IHostingEnvironment` Службы также могут быть добавлены непосредственно в `Configure` метода настройки конвейер обработки:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-365">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up your processing pipeline:</span></span>

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

<span data-ttu-id="2a2ac-366">Можно ввести `IHostingEnvironment` в `Invoke` метод при создании пользовательских [по промежуточного слоя](xref:fundamentals/middleware#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="2a2ac-366">You can inject `IHostingEnvironment` into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="2a2ac-367">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="2a2ac-367">IApplicationLifetime interface</span></span>

<span data-ttu-id="2a2ac-368">[IApplicationLifetime интерфейс](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) дает возможность выполнять действия, выполняемые после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-368">The [IApplicationLifetime interface](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows you to perform post-startup and shutdown activities.</span></span> <span data-ttu-id="2a2ac-369">Три свойства для интерфейса являются токенов отмены, которые можно зарегистрировать `Action` методы для определения событий запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-369">Three properties on the interface are cancellation tokens that you can register with `Action` methods to define startup and shutdown events.</span></span> <span data-ttu-id="2a2ac-370">Имеется также `StopApplication` метод.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-370">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="2a2ac-371">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="2a2ac-371">Cancellation Token</span></span>    | <span data-ttu-id="2a2ac-372">Запустить &#8230;</span><span class="sxs-lookup"><span data-stu-id="2a2ac-372">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="2a2ac-373">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-373">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="2a2ac-374">Узел выполняет правильное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-374">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="2a2ac-375">По-прежнему может обрабатывать запросы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-375">Requests may still be processing.</span></span> <span data-ttu-id="2a2ac-376">Завершение работы блокируется до завершения этого события.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-376">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="2a2ac-377">Узел завершает нормальное завершение работы.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-377">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="2a2ac-378">Все запросы должны обрабатываться полностью.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-378">All requests should be completely processed.</span></span> <span data-ttu-id="2a2ac-379">Завершение работы блокируется до завершения этого события.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-379">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="2a2ac-380">Метод</span><span class="sxs-lookup"><span data-stu-id="2a2ac-380">Method</span></span>            | <span data-ttu-id="2a2ac-381">Действие</span><span class="sxs-lookup"><span data-stu-id="2a2ac-381">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="2a2ac-382">Завершение запросов текущего приложения.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-382">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="2a2ac-383">Устранение неполадок System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="2a2ac-383">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="2a2ac-384">**Применяется к только базовые ASP.NET 2.0**</span><span class="sxs-lookup"><span data-stu-id="2a2ac-384">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="2a2ac-385">При создании узла, внедряя `IStartup` непосредственно в контейнер внедрения зависимостей, а не вызовом `UseStartup` или `Configure`, может возникнуть следующая ошибка: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-385">If you build the host by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`, you may encounter the following error: `Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided`.</span></span>

<span data-ttu-id="2a2ac-386">Это происходит потому, что [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) необходим для проверки на наличие `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-386">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="2a2ac-387">Если вы вручную ввести `IStartup` в контейнер внедрения зависимостей, добавьте следующий вызов для вашей `WebHostBuilder` с указанным именем сборки:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-387">If you manually inject `IStartup` into the dependency injection container, add the following call to your `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="2a2ac-388">Кроме того, добавить фиктивное `Configure` для вашей `WebHostBuilder`, который устанавливает `applicationName`(`ApplicationKey`) автоматически:</span><span class="sxs-lookup"><span data-stu-id="2a2ac-388">Alternatively, add a dummy `Configure` to your `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="2a2ac-389">**Примечание**: это необходимо только обязательные с выпуском ASP.NET Core 2.0 и только если вы не вызываете `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="2a2ac-389">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when you don't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="2a2ac-390">Дополнительные сведения см. в разделе [объявления: Microsoft.Extensions.PlatformAbstractions был удален (комментарий)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и [StartupInjection пример](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="2a2ac-390">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2a2ac-391">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2a2ac-391">Additional resources</span></span>

* [<span data-ttu-id="2a2ac-392">Публикация в Windows с помощью служб IIS</span><span class="sxs-lookup"><span data-stu-id="2a2ac-392">Publish to Windows using IIS</span></span>](../publishing/iis.md)
* [<span data-ttu-id="2a2ac-393">Публикация в Linux с помощью Nginx</span><span class="sxs-lookup"><span data-stu-id="2a2ac-393">Publish to Linux using Nginx</span></span>](../publishing/linuxproduction.md)
* [<span data-ttu-id="2a2ac-394">Публикация в Linux с помощью Apache</span><span class="sxs-lookup"><span data-stu-id="2a2ac-394">Publish to Linux using Apache</span></span>](../publishing/apache-proxy.md)
* [<span data-ttu-id="2a2ac-395">Узел в службе Windows</span><span class="sxs-lookup"><span data-stu-id="2a2ac-395">Host in a Windows Service</span></span>](xref:hosting/windows-service)
