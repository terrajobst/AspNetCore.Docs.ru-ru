---
title: "Размещение в ASP.NET Core"
author: guardrex
description: "Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования."
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 004487aebe5262a515e2375c30ccd2a84844dff6
ms.sourcegitcommit: f2a11a89037471a77ad68a67533754b7bb8303e2
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/01/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="d5d1d-103">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d5d1d-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="d5d1d-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="d5d1d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d5d1d-105">Приложения ASP.NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="d5d1d-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d5d1d-107">Узел настраивает как минимум сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="d5d1d-108">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="d5d1d-108">Setting up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d5d1d-110">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d5d1d-111">Обычно это делается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d5d1d-112">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="d5d1d-113">Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="d5d1d-114">Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="d5d1d-115">Настраивает [Kestrel](servers/kestrel.md) в качестве веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="d5d1d-116">Параметры Kestrel по умолчанию см. в разделе [Параметры Kestrel](xref:fundamentals/servers/kestrel#kestrel-options) статьи "Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="d5d1d-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="d5d1d-117">В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="d5d1d-118">Загружает дополнительные параметры конфигурации из следующих файлов и элементов:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="d5d1d-119">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="d5d1d-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="d5d1d-120">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="d5d1d-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="d5d1d-121">[секреты пользователя](xref:security/app-secrets), когда приложение выполняется в среде `Development`;</span><span class="sxs-lookup"><span data-stu-id="d5d1d-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="d5d1d-122">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-122">Environment variables.</span></span>
  * <span data-ttu-id="d5d1d-123">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-123">Command-line arguments.</span></span>
* <span data-ttu-id="d5d1d-124">Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="d5d1d-125">Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="d5d1d-126">При выполнении за службами IIS обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="d5d1d-127">Настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="d5d1d-128">Модуль создает обратный прокси-сервер между службами IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="d5d1d-129">Кроме того, настраивает [перехват приложением ошибок запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="d5d1d-130">Параметры служб IIS по умолчанию см. в разделе [Параметры служб IIS](xref:host-and-deploy/iis/index#iis-options) статьи "Размещение ASP.NET Core в Windows со службами IIS".</span><span class="sxs-lookup"><span data-stu-id="d5d1d-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>

<span data-ttu-id="d5d1d-131">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-131">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d5d1d-132">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-132">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d5d1d-133">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-133">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d5d1d-134">Дополнительные сведения о конфигурации приложения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-134">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="d5d1d-135">Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-135">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="d5d1d-136">Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-136">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-137">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-137">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-138">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-138">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="d5d1d-139">Узел обычно создается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-139">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="d5d1d-140">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-140">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[Main](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="d5d1d-141">Для `WebHostBuilder` требуется [сервер, реализующий интерфейс IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-141">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="d5d1d-142">Встроенными серверами являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 сервер HTTP.sys назывался [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-142">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="d5d1d-143">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) задает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-143">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="d5d1d-144">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-144">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="d5d1d-145">Для получения корня содержимого по умолчанию для `UseContentRoot` используется метод [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-145">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="d5d1d-146">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-146">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="d5d1d-147">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-147">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="d5d1d-148">Чтобы использовать службы IIS в качестве обратного прокси-сервера, вызовите метод [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) в процессе создания узла.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-148">To use IIS as a reverse proxy, call [UseIISIntegration](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="d5d1d-149">Метод `UseIISIntegration` не настраивает *сервер*, как это делает метод [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-149">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="d5d1d-150">`UseIISIntegration` настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для создания обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-150">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="d5d1d-151">Чтобы использовать службы IIS с ASP.NET Core, нужно указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-151">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="d5d1d-152">`UseIISIntegration` активирует только при выполнении за службами IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-152">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="d5d1d-153">Дополнительные сведения см. в разделах [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-153">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="d5d1d-154">Минимальная реализация для настройки узла (и приложения ASP.NET Core) включает в себя указание сервера и настройку конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-154">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="d5d1d-155">При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-155">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="d5d1d-156">Если используется класс `Startup`, в нем должен быть определен метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-156">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="d5d1d-157">Дополнительные сведения см. в разделе [Запуск приложения в ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-157">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="d5d1d-158">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-158">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="d5d1d-159">При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-159">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="d5d1d-160">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="d5d1d-160">Host configuration values</span></span>

<span data-ttu-id="d5d1d-161">Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-161">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d5d1d-162">конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`,</span><span class="sxs-lookup"><span data-stu-id="d5d1d-162">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="d5d1d-163">Например, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-163">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="d5d1d-164">явные методы, такие как `CaptureStartupErrors`;</span><span class="sxs-lookup"><span data-stu-id="d5d1d-164">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="d5d1d-165">метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-165">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="d5d1d-166">Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-166">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="d5d1d-167">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-167">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="d5d1d-168">Дополнительные сведения см. в подразделе [Переопределение конфигурации](#overriding-configuration) в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-168">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="d5d1d-169">Перехват ошибок при загрузке</span><span class="sxs-lookup"><span data-stu-id="d5d1d-169">Capture Startup Errors</span></span>

<span data-ttu-id="d5d1d-170">Этот параметр управляет перехватом ошибок при загрузке.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-170">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="d5d1d-171">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="d5d1d-171">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="d5d1d-172">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d5d1d-172">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5d1d-173">**Значение по умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-173">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="d5d1d-174">**Задается с помощью**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-174">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="d5d1d-175">**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-175">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="d5d1d-176">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-176">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="d5d1d-177">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-177">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-178">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-178">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-179">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-179">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="d5d1d-180">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="d5d1d-180">Content Root</span></span>

<span data-ttu-id="d5d1d-181">Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-181">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="d5d1d-182">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d5d1d-182">**Key**: contentRoot</span></span>  
<span data-ttu-id="d5d1d-183">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-183">**Type**: *string*</span></span>  
<span data-ttu-id="d5d1d-184">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-184">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d5d1d-185">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-185">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d5d1d-186">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-186">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="d5d1d-187">Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-187">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="d5d1d-188">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-188">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-189">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-189">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-190">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-190">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="d5d1d-191">Подробные сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="d5d1d-191">Detailed Errors</span></span>

<span data-ttu-id="d5d1d-192">Определяет, следует ли перехватывать подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-192">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="d5d1d-193">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="d5d1d-193">**Key**: detailedErrors</span></span>  
<span data-ttu-id="d5d1d-194">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d5d1d-194">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5d1d-195">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="d5d1d-195">**Default**: false</span></span>  
<span data-ttu-id="d5d1d-196">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-196">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d5d1d-197">**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-197">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="d5d1d-198">Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-198">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-199">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-199">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-200">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-200">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="d5d1d-201">Среда</span><span class="sxs-lookup"><span data-stu-id="d5d1d-201">Environment</span></span>

<span data-ttu-id="d5d1d-202">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-202">Sets the app's environment.</span></span>

<span data-ttu-id="d5d1d-203">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="d5d1d-203">**Key**: environment</span></span>  
<span data-ttu-id="d5d1d-204">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-204">**Type**: *string*</span></span>  
<span data-ttu-id="d5d1d-205">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="d5d1d-205">**Default**: Production</span></span>  
<span data-ttu-id="d5d1d-206">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-206">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d5d1d-207">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-207">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="d5d1d-208">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-208">The environment can be set to any value.</span></span> <span data-ttu-id="d5d1d-209">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-209">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d5d1d-210">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-210">Values aren't case sensitive.</span></span> <span data-ttu-id="d5d1d-211">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-211">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="d5d1d-212">При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-212">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="d5d1d-213">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-213">For more information, see [Working with Multiple Environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-214">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-214">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-215">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-215">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="d5d1d-216">Начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="d5d1d-216">Hosting Startup Assemblies</span></span>

<span data-ttu-id="d5d1d-217">Задает начальные сборки размещения для приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-217">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="d5d1d-218">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="d5d1d-218">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="d5d1d-219">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-219">**Type**: *string*</span></span>  
<span data-ttu-id="d5d1d-220">**Значение по умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="d5d1d-220">**Default**: Empty string</span></span>  
<span data-ttu-id="d5d1d-221">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-221">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d5d1d-222">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-222">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="d5d1d-223">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-223">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="d5d1d-224">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-224">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="d5d1d-225">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-225">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="d5d1d-226">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-226">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-227">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-227">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-228">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-228">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-229">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-229">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="d5d1d-230">Предпочитать URL-адреса размещения</span><span class="sxs-lookup"><span data-stu-id="d5d1d-230">Prefer Hosting URLs</span></span>

<span data-ttu-id="d5d1d-231">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-231">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="d5d1d-232">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="d5d1d-232">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="d5d1d-233">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d5d1d-233">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5d1d-234">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="d5d1d-234">**Default**: true</span></span>  
<span data-ttu-id="d5d1d-235">**Задается с помощью**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-235">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="d5d1d-236">**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-236">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="d5d1d-237">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-237">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-238">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-238">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-239">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-239">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-240">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-240">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="d5d1d-241">Запретить запуск размещения</span><span class="sxs-lookup"><span data-stu-id="d5d1d-241">Prevent Hosting Startup</span></span>

<span data-ttu-id="d5d1d-242">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-242">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="d5d1d-243">Дополнительные сведения см. в разделе [Добавление функций приложения из внешней сборки с помощью IHostingStartup](xref:host-and-deploy/ihostingstartup).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-243">See [Add app features from an external assembly using IHostingStartup](xref:host-and-deploy/ihostingstartup) for more information.</span></span>

<span data-ttu-id="d5d1d-244">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="d5d1d-244">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="d5d1d-245">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="d5d1d-245">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="d5d1d-246">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="d5d1d-246">**Default**: false</span></span>  
<span data-ttu-id="d5d1d-247">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-247">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="d5d1d-248">**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-248">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="d5d1d-249">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-249">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-250">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-250">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-251">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-251">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-252">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-252">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="d5d1d-253">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="d5d1d-253">Server URLs</span></span>

<span data-ttu-id="d5d1d-254">Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-254">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="d5d1d-255">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="d5d1d-255">**Key**: urls</span></span>  
<span data-ttu-id="d5d1d-256">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-256">**Type**: *string*</span></span>  
<span data-ttu-id="d5d1d-257">**Значение по умолчанию**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="d5d1d-257">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="d5d1d-258">**Задается с помощью**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-258">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="d5d1d-259">**Переменная среды**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-259">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="d5d1d-260">Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-260">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="d5d1d-261">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-261">For example, `http://localhost:123`.</span></span> <span data-ttu-id="d5d1d-262">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-262">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="d5d1d-263">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-263">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="d5d1d-264">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-264">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-265">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-265">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="d5d1d-266">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-266">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="d5d1d-267">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-267">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-268">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-268">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="d5d1d-269">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="d5d1d-269">Shutdown Timeout</span></span>

<span data-ttu-id="d5d1d-270">Определяет, как долго необходимо ожидать завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-270">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="d5d1d-271">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="d5d1d-271">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="d5d1d-272">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-272">**Type**: *int*</span></span>  
<span data-ttu-id="d5d1d-273">**Значение по умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="d5d1d-273">**Default**: 5</span></span>  
<span data-ttu-id="d5d1d-274">**Задается с помощью**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-274">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="d5d1d-275">**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-275">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="d5d1d-276">Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения `UseShutdownTimeout` принимает `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-276">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the `UseShutdownTimeout` extension method takes a `TimeSpan`.</span></span> <span data-ttu-id="d5d1d-277">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-277">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-278">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-278">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-279">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-279">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-280">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-280">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="d5d1d-281">Стартовая сборка</span><span class="sxs-lookup"><span data-stu-id="d5d1d-281">Startup Assembly</span></span>

<span data-ttu-id="d5d1d-282">Определяет сборку, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-282">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="d5d1d-283">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="d5d1d-283">**Key**: startupAssembly</span></span>  
<span data-ttu-id="d5d1d-284">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-284">**Type**: *string*</span></span>  
<span data-ttu-id="d5d1d-285">**Значение по умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="d5d1d-285">**Default**: The app's assembly</span></span>  
<span data-ttu-id="d5d1d-286">**Задается с помощью**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-286">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="d5d1d-287">**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-287">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="d5d1d-288">На сборку можно ссылаться по имени (`string`) или типу (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-288">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="d5d1d-289">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-289">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-290">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-290">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-291">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-291">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="d5d1d-292">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="d5d1d-292">Web Root</span></span>

<span data-ttu-id="d5d1d-293">Задает относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-293">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="d5d1d-294">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="d5d1d-294">**Key**: webroot</span></span>  
<span data-ttu-id="d5d1d-295">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d5d1d-295">**Type**: *string*</span></span>  
<span data-ttu-id="d5d1d-296">**Значение по умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-296">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="d5d1d-297">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-297">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="d5d1d-298">**Задается с помощью**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-298">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="d5d1d-299">**Переменная среды**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="d5d1d-299">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="d5d1d-302">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="d5d1d-302">Overriding configuration</span></span>

<span data-ttu-id="d5d1d-303">Для настройки хоста используется [конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-303">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="d5d1d-304">В приведенном ниже примере необязательная конфигурация хоста задается в файле *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-304">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="d5d1d-305">Любую конфигурацию, загружаемую из файла *hosting.json*, можно переопределить с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-305">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="d5d1d-306">Встроенная конфигурация (в `config`) используется для настройки узла с помощью `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-306">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-307">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-307">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d5d1d-308">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-308">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d5d1d-309">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-309">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-310">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-310">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-311">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-311">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="d5d1d-312">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-312">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="d5d1d-313">Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-313">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d5d1d-314">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-314">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="d5d1d-315">Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-315">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="d5d1d-316">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-316">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d5d1d-317">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-317">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d5d1d-318">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-318">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="d5d1d-319">Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение в окне командной строки при выполнении команды `dotnet run`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing `dotnet run`.</span></span> <span data-ttu-id="d5d1d-320">Аргумент командной строки переопределяет значение `urls` из файла *hosting.json*, и сервер будет ожидать передачи данных через порт 8080.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-320">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="d5d1d-321">Запуск узла</span><span class="sxs-lookup"><span data-stu-id="d5d1d-321">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="d5d1d-322">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-322">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="d5d1d-323">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-323">**Run**</span></span>

<span data-ttu-id="d5d1d-324">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-324">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d5d1d-325">**Start**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-325">**Start**</span></span>

<span data-ttu-id="d5d1d-326">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-326">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d5d1d-327">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-327">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="d5d1d-328">Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-328">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="d5d1d-329">Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="d5d1d-329">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="d5d1d-330">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-330">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="d5d1d-331">Выполните запуск с помощью `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-331">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d5d1d-332">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d5d1d-332">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d5d1d-333">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-333">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d5d1d-334">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-334">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d5d1d-335">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-335">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="d5d1d-336">Выполните запуск с помощью URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-336">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="d5d1d-337">Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-337">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="d5d1d-338">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-338">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d5d1d-339">Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-339">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="d5d1d-340">В этом примере используйте следующие запросы в браузере:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-340">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="d5d1d-341">Запрос</span><span class="sxs-lookup"><span data-stu-id="d5d1d-341">Request</span></span>                                    | <span data-ttu-id="d5d1d-342">Ответ</span><span class="sxs-lookup"><span data-stu-id="d5d1d-342">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="d5d1d-343">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="d5d1d-343">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="d5d1d-344">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="d5d1d-344">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="d5d1d-345">Вызывает исключение со строкой "ooops!"</span><span class="sxs-lookup"><span data-stu-id="d5d1d-345">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="d5d1d-346">Вызывает исключение со строкой "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="d5d1d-346">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="d5d1d-347">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="d5d1d-347">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="d5d1d-348">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="d5d1d-348">Hello World!</span></span>                             |

<span data-ttu-id="d5d1d-349">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-349">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d5d1d-350">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-350">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d5d1d-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-351">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="d5d1d-352">Используйте URL-адрес и экземпляр `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-352">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="d5d1d-353">Результат будет тем же, что и при использовании **Start(Action<IRouteBuilder> routeBuilder)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-353">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="d5d1d-354">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-354">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d5d1d-355">Предоставьте делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-355">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d5d1d-356">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="d5d1d-356">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="d5d1d-357">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="d5d1d-358">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="d5d1d-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-359">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="d5d1d-360">Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-360">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="d5d1d-361">Результат будет тем же, что и при использовании **StartWith(Action<IApplicationBuilder> app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-361">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="d5d1d-362">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="d5d1d-362">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="d5d1d-363">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-363">**Run**</span></span>

<span data-ttu-id="d5d1d-364">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-364">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="d5d1d-365">**Start**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-365">**Start**</span></span>

<span data-ttu-id="d5d1d-366">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-366">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="d5d1d-367">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-367">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d5d1d-368">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d5d1d-368">IHostingEnvironment interface</span></span>

<span data-ttu-id="d5d1d-369">[Интерфейс IHostingEnvironment](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-369">The [IHostingEnvironment interface](/aspnet/core/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="d5d1d-370">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="d5d1d-370">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d5d1d-371">Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#startup-conventions).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-371">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="d5d1d-372">Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-372">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="d5d1d-373">Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-373">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="d5d1d-374">Подробные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-374">See [Working with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="d5d1d-375">Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-375">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="d5d1d-376">`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="d5d1d-376">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d5d1d-377">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d5d1d-377">IApplicationLifetime interface</span></span>

<span data-ttu-id="d5d1d-378">[Интерфейс IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-378">[IApplicationLifetime](/aspnet/core/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="d5d1d-379">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-379">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span> <span data-ttu-id="d5d1d-380">Также имеется метод `StopApplication`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-380">There's also a `StopApplication` method.</span></span>

| <span data-ttu-id="d5d1d-381">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="d5d1d-381">Cancellation Token</span></span>    | <span data-ttu-id="d5d1d-382">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="d5d1d-382">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="d5d1d-383">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-383">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="d5d1d-384">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-384">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d5d1d-385">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-385">Requests may still be processing.</span></span> <span data-ttu-id="d5d1d-386">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-386">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="d5d1d-387">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-387">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d5d1d-388">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-388">All requests should be processed.</span></span> <span data-ttu-id="d5d1d-389">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-389">Shutdown blocks until this event completes.</span></span> |

| <span data-ttu-id="d5d1d-390">Метод</span><span class="sxs-lookup"><span data-stu-id="d5d1d-390">Method</span></span>            | <span data-ttu-id="d5d1d-391">Действие</span><span class="sxs-lookup"><span data-stu-id="d5d1d-391">Action</span></span>                                           |
| ----------------- | ------------------------------------------------ |
| `StopApplication` | <span data-ttu-id="d5d1d-392">Запрашивает завершение работы текущего приложения.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-392">Requests termination of the current application.</span></span> |

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

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="d5d1d-393">Устранение неполадок, связанных с исключениями System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="d5d1d-393">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="d5d1d-394">**Относится только к ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="d5d1d-394">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="d5d1d-395">Узел может создаваться путем внедрения интерфейса `IStartup` непосредственно в контейнер внедрения зависимостей, а не путем вызова `UseStartup` или `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-395">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="d5d1d-396">Если узел создается таким способом, может произойти следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-396">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="d5d1d-397">Причина в том, что [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) требуется для проверки `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-397">This occurs because the [applicationName(ApplicationKey)](/aspnet/core/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="d5d1d-398">Если в приложении интерфейс `IStartup` вручную внедряется в контейнер внедрения зависимостей, добавьте следующий вызов в `WebHostBuilder` с указанием имени сборки:</span><span class="sxs-lookup"><span data-stu-id="d5d1d-398">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="d5d1d-399">Кроме того, в `WebHostBuilder` можно добавить метод-заглушку `Configure`, который автоматически задает `applicationName`(`ApplicationKey`):</span><span class="sxs-lookup"><span data-stu-id="d5d1d-399">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="d5d1d-400">**Примечание**. Это необходимо только в выпуске ASP.NET Core 2.0 и только в случае, если приложение не вызывает метод `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="d5d1d-400">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="d5d1d-401">Дополнительные сведения см. в [комментарии к объявлению об удалении пакета Microsoft.Extensions.PlatformAbstractions](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и в [примере использования StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="d5d1d-401">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5d1d-402">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d5d1d-402">Additional resources</span></span>

* [<span data-ttu-id="d5d1d-403">Размещение в Windows с помощью IIS</span><span class="sxs-lookup"><span data-stu-id="d5d1d-403">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="d5d1d-404">Размещение в Linux с использованием Nginx</span><span class="sxs-lookup"><span data-stu-id="d5d1d-404">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="d5d1d-405">Размещение в Linux с использованием Apache</span><span class="sxs-lookup"><span data-stu-id="d5d1d-405">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="d5d1d-406">Размещение в службе Windows</span><span class="sxs-lookup"><span data-stu-id="d5d1d-406">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
