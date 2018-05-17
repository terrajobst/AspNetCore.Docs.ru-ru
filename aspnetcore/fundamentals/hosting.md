---
title: Размещение в ASP.NET Core
author: guardrex
description: Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования.
manager: wpickett
ms.author: riande
ms.date: 09/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/hosting
ms.openlocfilehash: 344bf5f0917f4c33d67eeb14176ff2aae3ae75da
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
---
# <a name="hosting-in-aspnet-core"></a><span data-ttu-id="848da-103">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="848da-103">Hosting in ASP.NET Core</span></span>

<span data-ttu-id="848da-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="848da-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="848da-105">Приложения ASP.NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="848da-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="848da-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="848da-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="848da-107">Узел настраивает как минимум сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="848da-107">At a minimum, the host configures a server and a request processing pipeline.</span></span>

## <a name="setting-up-a-host"></a><span data-ttu-id="848da-108">Настройка узла</span><span class="sxs-lookup"><span data-stu-id="848da-108">Setting up a host</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="848da-110">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="848da-110">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="848da-111">Обычно это делается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="848da-111">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="848da-112">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="848da-112">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="848da-113">Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.</span><span class="sxs-lookup"><span data-stu-id="848da-113">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

[!code-csharp[](../common/samples/WebApplication1DotNetCore2.0App/Program.cs?name=snippet_Main)]

<span data-ttu-id="848da-114">Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.</span><span class="sxs-lookup"><span data-stu-id="848da-114">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="848da-115">Настраивает [Kestrel](servers/kestrel.md) в качестве веб-сервера.</span><span class="sxs-lookup"><span data-stu-id="848da-115">Configures [Kestrel](servers/kestrel.md) as the web server.</span></span> <span data-ttu-id="848da-116">Параметры Kestrel по умолчанию см. в разделе [Параметры Kestrel](xref:fundamentals/servers/kestrel#kestrel-options) статьи "Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="848da-116">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="848da-117">В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="848da-117">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="848da-118">Загружает дополнительные параметры конфигурации из следующих файлов и элементов:</span><span class="sxs-lookup"><span data-stu-id="848da-118">Loads optional configuration from:</span></span>
  * <span data-ttu-id="848da-119">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="848da-119">*appsettings.json*.</span></span>
  * <span data-ttu-id="848da-120">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="848da-120">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="848da-121">[секреты пользователя](xref:security/app-secrets), когда приложение выполняется в среде `Development`;</span><span class="sxs-lookup"><span data-stu-id="848da-121">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment.</span></span>
  * <span data-ttu-id="848da-122">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="848da-122">Environment variables.</span></span>
  * <span data-ttu-id="848da-123">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="848da-123">Command-line arguments.</span></span>
* <span data-ttu-id="848da-124">Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="848da-124">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="848da-125">Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="848da-125">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="848da-126">При выполнении за службами IIS обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="848da-126">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="848da-127">Настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="848da-127">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="848da-128">Модуль создает обратный прокси-сервер между службами IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="848da-128">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="848da-129">Кроме того, настраивает [перехват приложением ошибок запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="848da-129">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="848da-130">Параметры служб IIS по умолчанию см. в разделе [Параметры служб IIS](xref:host-and-deploy/iis/index#iis-options) статьи "Размещение ASP.NET Core в Windows со службами IIS".</span><span class="sxs-lookup"><span data-stu-id="848da-130">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="848da-131">Устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="848da-131">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="848da-132">Дополнительные сведения см. в разделе [Проверка области](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="848da-132">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="848da-133">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="848da-133">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="848da-134">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="848da-134">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="848da-135">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="848da-135">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="848da-136">Дополнительные сведения о конфигурации приложения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="848da-136">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="848da-137">Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="848da-137">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="848da-138">Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="848da-138">For more information, see the ASP.NET Core 1.x tab.</span></span>

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-139">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-139">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="848da-140">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="848da-140">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="848da-141">Узел обычно создается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="848da-141">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="848da-142">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="848da-142">In the project templates, `Main` is located in *Program.cs*:</span></span>

[!code-csharp[](../common/samples/WebApplication1/Program.cs)]

<span data-ttu-id="848da-143">Для `WebHostBuilder` требуется [сервер, реализующий интерфейс IServer](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="848da-143">`WebHostBuilder` requires a [server that implements IServer](servers/index.md).</span></span> <span data-ttu-id="848da-144">Встроенными серверами являются [Kestrel](servers/kestrel.md) и [HTTP.sys](servers/httpsys.md) (до выхода ASP.NET Core 2.0 сервер HTTP.sys назывался [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="848da-144">The built-in servers are [Kestrel](servers/kestrel.md) and [HTTP.sys](servers/httpsys.md) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="848da-145">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) задает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="848da-145">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="848da-146">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="848da-146">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="848da-147">Для получения корня содержимого по умолчанию для `UseContentRoot` используется метод [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="848da-147">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="848da-148">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="848da-148">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="848da-149">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="848da-149">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="848da-150">Чтобы использовать службы IIS в качестве обратного прокси-сервера, вызовите метод [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) в процессе создания узла.</span><span class="sxs-lookup"><span data-stu-id="848da-150">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="848da-151">Метод `UseIISIntegration` не настраивает *сервер*, как это делает метод [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="848da-151">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="848da-152">`UseIISIntegration` настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для создания обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="848da-152">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="848da-153">Чтобы использовать службы IIS с ASP.NET Core, нужно указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="848da-153">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="848da-154">`UseIISIntegration` активирует только при выполнении за службами IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="848da-154">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="848da-155">Дополнительные сведения см. в разделах [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="848da-155">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="848da-156">Минимальная реализация для настройки узла (и приложения ASP.NET Core) включает в себя указание сервера и настройку конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-156">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

* * *
<span data-ttu-id="848da-157">При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="848da-157">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="848da-158">Если используется класс `Startup`, в нем должен быть определен метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="848da-158">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="848da-159">Дополнительные сведения см. в разделе [Запуск приложения в ASP.NET Core](startup.md).</span><span class="sxs-lookup"><span data-stu-id="848da-159">For more information, see [Application Startup in ASP.NET Core](startup.md).</span></span> <span data-ttu-id="848da-160">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="848da-160">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="848da-161">При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.</span><span class="sxs-lookup"><span data-stu-id="848da-161">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="848da-162">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="848da-162">Host configuration values</span></span>

<span data-ttu-id="848da-163">Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="848da-163">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="848da-164">конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`,</span><span class="sxs-lookup"><span data-stu-id="848da-164">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="848da-165">Например, `ASPNETCORE_URLS`.</span><span class="sxs-lookup"><span data-stu-id="848da-165">For example, `ASPNETCORE_URLS`.</span></span>
* <span data-ttu-id="848da-166">явные методы, такие как `CaptureStartupErrors`;</span><span class="sxs-lookup"><span data-stu-id="848da-166">Explicit methods, such as `CaptureStartupErrors`.</span></span>
* <span data-ttu-id="848da-167">метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="848da-167">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="848da-168">Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="848da-168">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="848da-169">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="848da-169">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="848da-170">Дополнительные сведения см. в подразделе [Переопределение конфигурации](#overriding-configuration) в следующем разделе.</span><span class="sxs-lookup"><span data-stu-id="848da-170">For more information, see [Overriding configuration](#overriding-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="848da-171">Перехват ошибок при загрузке</span><span class="sxs-lookup"><span data-stu-id="848da-171">Capture Startup Errors</span></span>

<span data-ttu-id="848da-172">Этот параметр управляет перехватом ошибок при загрузке.</span><span class="sxs-lookup"><span data-stu-id="848da-172">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="848da-173">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="848da-173">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="848da-174">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="848da-174">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="848da-175">**Значение по умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="848da-175">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="848da-176">**Задается с помощью**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="848da-176">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="848da-177">**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="848da-177">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="848da-178">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="848da-178">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="848da-179">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="848da-179">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-180">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-180">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-181">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-181">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
    ...
```

---

### <a name="content-root"></a><span data-ttu-id="848da-182">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="848da-182">Content Root</span></span>

<span data-ttu-id="848da-183">Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC.</span><span class="sxs-lookup"><span data-stu-id="848da-183">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="848da-184">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="848da-184">**Key**: contentRoot</span></span>  
<span data-ttu-id="848da-185">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="848da-185">**Type**: *string*</span></span>  
<span data-ttu-id="848da-186">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-186">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="848da-187">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="848da-187">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="848da-188">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="848da-188">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="848da-189">Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root).</span><span class="sxs-lookup"><span data-stu-id="848da-189">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="848da-190">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="848da-190">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-191">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-191">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\mywebsite")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-192">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-192">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\mywebsite")
    ...
```

---

### <a name="detailed-errors"></a><span data-ttu-id="848da-193">Подробные сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="848da-193">Detailed Errors</span></span>

<span data-ttu-id="848da-194">Определяет, следует ли перехватывать подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="848da-194">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="848da-195">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="848da-195">**Key**: detailedErrors</span></span>  
<span data-ttu-id="848da-196">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="848da-196">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="848da-197">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="848da-197">**Default**: false</span></span>  
<span data-ttu-id="848da-198">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="848da-198">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="848da-199">**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="848da-199">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="848da-200">Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.</span><span class="sxs-lookup"><span data-stu-id="848da-200">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-201">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-201">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-202">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-202">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
    ...
```

---

### <a name="environment"></a><span data-ttu-id="848da-203">Среда</span><span class="sxs-lookup"><span data-stu-id="848da-203">Environment</span></span>

<span data-ttu-id="848da-204">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-204">Sets the app's environment.</span></span>

<span data-ttu-id="848da-205">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="848da-205">**Key**: environment</span></span>  
<span data-ttu-id="848da-206">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="848da-206">**Type**: *string*</span></span>  
<span data-ttu-id="848da-207">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="848da-207">**Default**: Production</span></span>  
<span data-ttu-id="848da-208">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="848da-208">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="848da-209">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="848da-209">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="848da-210">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="848da-210">The environment can be set to any value.</span></span> <span data-ttu-id="848da-211">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="848da-211">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="848da-212">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="848da-212">Values aren't case sensitive.</span></span> <span data-ttu-id="848da-213">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="848da-213">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="848da-214">При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="848da-214">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="848da-215">Дополнительные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="848da-215">For more information, see [Work with multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-216">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-216">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment("Development")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-217">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-217">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment("Development")
    ...
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="848da-218">Начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="848da-218">Hosting Startup Assemblies</span></span>

<span data-ttu-id="848da-219">Задает начальные сборки размещения для приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-219">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="848da-220">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="848da-220">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="848da-221">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="848da-221">**Type**: *string*</span></span>  
<span data-ttu-id="848da-222">**Значение по умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="848da-222">**Default**: Empty string</span></span>  
<span data-ttu-id="848da-223">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="848da-223">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="848da-224">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="848da-224">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="848da-225">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="848da-225">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="848da-226">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="848da-226">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="848da-227">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-227">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="848da-228">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="848da-228">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-229">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-229">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-230">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-230">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="848da-231">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="848da-231">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="848da-232">Предпочитать URL-адреса размещения</span><span class="sxs-lookup"><span data-stu-id="848da-232">Prefer Hosting URLs</span></span>

<span data-ttu-id="848da-233">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="848da-233">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="848da-234">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="848da-234">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="848da-235">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="848da-235">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="848da-236">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="848da-236">**Default**: true</span></span>  
<span data-ttu-id="848da-237">**Задается с помощью**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="848da-237">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="848da-238">**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="848da-238">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="848da-239">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="848da-239">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-240">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-240">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-241">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-241">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="848da-242">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="848da-242">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="848da-243">Запретить запуск размещения</span><span class="sxs-lookup"><span data-stu-id="848da-243">Prevent Hosting Startup</span></span>

<span data-ttu-id="848da-244">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-244">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="848da-245">Дополнительную информацию см. в разделе [Добавление компонентов приложения с использованием конфигурации для конкретной платформы](xref:host-and-deploy/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="848da-245">See [Add app features using a platform-specific configuration](xref:host-and-deploy/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="848da-246">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="848da-246">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="848da-247">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="848da-247">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="848da-248">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="848da-248">**Default**: false</span></span>  
<span data-ttu-id="848da-249">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="848da-249">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="848da-250">**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="848da-250">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="848da-251">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="848da-251">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-252">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-252">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-253">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-253">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="848da-254">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="848da-254">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="848da-255">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="848da-255">Server URLs</span></span>

<span data-ttu-id="848da-256">Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="848da-256">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="848da-257">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="848da-257">**Key**: urls</span></span>  
<span data-ttu-id="848da-258">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="848da-258">**Type**: *string*</span></span>  
<span data-ttu-id="848da-259">**По умолчанию**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="848da-259">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="848da-260">**Задается с помощью**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="848da-260">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="848da-261">**Переменная среды**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="848da-261">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="848da-262">Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="848da-262">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="848da-263">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="848da-263">For example, `http://localhost:123`.</span></span> <span data-ttu-id="848da-264">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="848da-264">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="848da-265">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="848da-265">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="848da-266">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="848da-266">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-267">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-267">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

<span data-ttu-id="848da-268">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="848da-268">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="848da-269">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="848da-269">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-270">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-270">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
    ...
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="848da-271">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="848da-271">Shutdown Timeout</span></span>

<span data-ttu-id="848da-272">Определяет, как долго необходимо ожидать завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="848da-272">Specifies the amount of time to wait for the web host to shutdown.</span></span>

<span data-ttu-id="848da-273">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="848da-273">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="848da-274">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="848da-274">**Type**: *int*</span></span>  
<span data-ttu-id="848da-275">**Значение по умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="848da-275">**Default**: 5</span></span>  
<span data-ttu-id="848da-276">**Задается с помощью**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="848da-276">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="848da-277">**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="848da-277">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="848da-278">Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) принимает [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="848da-278">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="848da-279">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="848da-279">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="848da-280">Во время ожидания размещение:</span><span class="sxs-lookup"><span data-stu-id="848da-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="848da-281">Активирует [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="848da-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="848da-282">Пытается остановить размещенные службы, записывая в журнал все ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="848da-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="848da-283">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="848da-284">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="848da-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="848da-285">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="848da-285">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-286">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-286">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-287">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-287">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="848da-288">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="848da-288">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="848da-289">Стартовая сборка</span><span class="sxs-lookup"><span data-stu-id="848da-289">Startup Assembly</span></span>

<span data-ttu-id="848da-290">Определяет сборку, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="848da-290">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="848da-291">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="848da-291">**Key**: startupAssembly</span></span>  
<span data-ttu-id="848da-292">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="848da-292">**Type**: *string*</span></span>  
<span data-ttu-id="848da-293">**Значение по умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="848da-293">**Default**: The app's assembly</span></span>  
<span data-ttu-id="848da-294">**Задается с помощью**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="848da-294">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="848da-295">**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="848da-295">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="848da-296">На сборку можно ссылаться по имени (`string`) или типу (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="848da-296">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="848da-297">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="848da-297">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-298">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-298">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-299">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-299">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

### <a name="web-root"></a><span data-ttu-id="848da-300">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="848da-300">Web Root</span></span>

<span data-ttu-id="848da-301">Задает относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-301">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="848da-302">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="848da-302">**Key**: webroot</span></span>  
<span data-ttu-id="848da-303">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="848da-303">**Type**: *string*</span></span>  
<span data-ttu-id="848da-304">**Значение по умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии.</span><span class="sxs-lookup"><span data-stu-id="848da-304">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="848da-305">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="848da-305">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="848da-306">**Задается с помощью**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="848da-306">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="848da-307">**Переменная среды**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="848da-307">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-308">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-308">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
    ...
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-309">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-309">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
    ...
```

---

## <a name="overriding-configuration"></a><span data-ttu-id="848da-310">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="848da-310">Overriding configuration</span></span>

<span data-ttu-id="848da-311">Для настройки хоста используется [конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="848da-311">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="848da-312">В приведенном ниже примере необязательная конфигурация хоста задается в файле *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="848da-312">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="848da-313">Любую конфигурацию, загружаемую из файла *hosting.json*, можно переопределить с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="848da-313">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="848da-314">Встроенная конфигурация (в `config`) используется для настройки узла с помощью `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="848da-314">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-315">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-315">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="848da-316">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="848da-316">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="848da-317">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="848da-317">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-318">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-318">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="848da-319">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="848da-319">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="848da-320">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="848da-320">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="848da-321">Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="848da-321">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="848da-322">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="848da-322">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="848da-323">Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="848da-323">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="848da-324">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="848da-324">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="848da-325">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="848da-325">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="848da-326">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="848da-326">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="848da-327">Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение из командной строки при выполнении команды [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="848da-327">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="848da-328">Аргумент командной строки переопределяет значение `urls` из файла *hosting.json*, и сервер будет ожидать передачи данных через порт 8080.</span><span class="sxs-lookup"><span data-stu-id="848da-328">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="starting-the-host"></a><span data-ttu-id="848da-329">Запуск узла</span><span class="sxs-lookup"><span data-stu-id="848da-329">Starting the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="848da-330">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="848da-330">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="848da-331">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="848da-331">**Run**</span></span>

<span data-ttu-id="848da-332">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="848da-332">The `Run` method starts the web app and blocks the calling thread until the host is shutdown:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="848da-333">**Start**</span><span class="sxs-lookup"><span data-stu-id="848da-333">**Start**</span></span>

<span data-ttu-id="848da-334">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="848da-334">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="848da-335">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="848da-335">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="848da-336">Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода.</span><span class="sxs-lookup"><span data-stu-id="848da-336">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="848da-337">Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="848da-337">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="848da-338">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="848da-338">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="848da-339">Выполните запуск с помощью `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="848da-339">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="848da-340">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="848da-340">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="848da-341">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="848da-341">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="848da-342">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="848da-342">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="848da-343">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="848da-343">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="848da-344">Выполните запуск с помощью URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="848da-344">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="848da-345">Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="848da-345">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="848da-346">**Start(Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="848da-346">**Start(Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="848da-347">Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="848da-347">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="848da-348">В этом примере используйте следующие запросы в браузере:</span><span class="sxs-lookup"><span data-stu-id="848da-348">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="848da-349">Запрос</span><span class="sxs-lookup"><span data-stu-id="848da-349">Request</span></span>                                    | <span data-ttu-id="848da-350">Ответ</span><span class="sxs-lookup"><span data-stu-id="848da-350">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="848da-351">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="848da-351">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="848da-352">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="848da-352">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="848da-353">Вызывает исключение со строкой "ooops!"</span><span class="sxs-lookup"><span data-stu-id="848da-353">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="848da-354">Вызывает исключение со строкой "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="848da-354">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="848da-355">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="848da-355">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="848da-356">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="848da-356">Hello World!</span></span>                             |

<span data-ttu-id="848da-357">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="848da-357">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="848da-358">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="848da-358">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="848da-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="848da-359">**Start(string url, Action<IRouteBuilder> routeBuilder)**</span></span>

<span data-ttu-id="848da-360">Используйте URL-адрес и экземпляр `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="848da-360">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="848da-361">Результат будет тем же, что и при использовании **Start(Action<IRouteBuilder> routeBuilder)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="848da-361">Produces the same result as **Start(Action<IRouteBuilder> routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="848da-362">**StartWith(Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="848da-362">**StartWith(Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="848da-363">Предоставьте делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="848da-363">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="848da-364">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="848da-364">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="848da-365">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="848da-365">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="848da-366">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="848da-366">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="848da-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span><span class="sxs-lookup"><span data-stu-id="848da-367">**StartWith(string url, Action<IApplicationBuilder> app)**</span></span>

<span data-ttu-id="848da-368">Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="848da-368">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="848da-369">Результат будет тем же, что и при использовании **StartWith(Action<IApplicationBuilder> app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="848da-369">Produces the same result as **StartWith(Action<IApplicationBuilder> app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="848da-370">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="848da-370">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="848da-371">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="848da-371">**Run**</span></span>

<span data-ttu-id="848da-372">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="848da-372">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="848da-373">**Start**</span><span class="sxs-lookup"><span data-stu-id="848da-373">**Start**</span></span>

<span data-ttu-id="848da-374">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="848da-374">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="848da-375">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="848da-375">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>


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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="848da-376">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="848da-376">IHostingEnvironment interface</span></span>

<span data-ttu-id="848da-377">[Интерфейс IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-377">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="848da-378">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="848da-378">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="848da-379">Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#startup-conventions).</span><span class="sxs-lookup"><span data-stu-id="848da-379">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="848da-380">Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="848da-380">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="848da-381">Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="848da-381">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="848da-382">Подробные сведения см. в статье [Работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="848da-382">See [Work with multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="848da-383">Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="848da-383">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="848da-384">`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="848da-384">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="848da-385">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="848da-385">IApplicationLifetime interface</span></span>

<span data-ttu-id="848da-386">[Интерфейс IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="848da-386">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="848da-387">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="848da-387">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="848da-388">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="848da-388">Cancellation Token</span></span>    | <span data-ttu-id="848da-389">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="848da-389">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| `ApplicationStarted`  | <span data-ttu-id="848da-390">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="848da-390">The host has fully started.</span></span> |
| `ApplicationStopping` | <span data-ttu-id="848da-391">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="848da-391">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="848da-392">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="848da-392">Requests may still be processing.</span></span> <span data-ttu-id="848da-393">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="848da-393">Shutdown blocks until this event completes.</span></span> |
| `ApplicationStopped`  | <span data-ttu-id="848da-394">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="848da-394">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="848da-395">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="848da-395">All requests should be processed.</span></span> <span data-ttu-id="848da-396">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="848da-396">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="848da-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="848da-397">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="848da-398">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="848da-398">The following class uses `StopApplication` to gracefully shutdown an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="848da-399">Проверка области</span><span class="sxs-lookup"><span data-stu-id="848da-399">Scope validation</span></span>

<span data-ttu-id="848da-400">В ASP.NET Core 2.0 или более поздней версии [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="848da-400">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="848da-401">Если для `ValidateScopes` установлено значение `true`, поставщик службы по умолчанию проверяет, что:</span><span class="sxs-lookup"><span data-stu-id="848da-401">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="848da-402">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="848da-402">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="848da-403">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="848da-403">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="848da-404">Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="848da-404">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="848da-405">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="848da-405">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="848da-406">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="848da-406">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="848da-407">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="848da-407">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="848da-408">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="848da-408">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="848da-409">Чтобы всегда проверять области, в том числе в рабочей среде, настройте [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) с [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) в конструкторе узлов:</span><span class="sxs-lookup"><span data-stu-id="848da-409">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="848da-410">Устранение неполадок, связанных с исключениями System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="848da-410">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="848da-411">**Относится только к ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="848da-411">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="848da-412">Узел может создаваться путем внедрения интерфейса `IStartup` непосредственно в контейнер внедрения зависимостей, а не путем вызова `UseStartup` или `Configure`:</span><span class="sxs-lookup"><span data-stu-id="848da-412">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="848da-413">Если узел создается таким способом, может произойти следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="848da-413">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="848da-414">Причина в том, что [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) требуется для проверки `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="848da-414">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="848da-415">Если в приложении интерфейс `IStartup` вручную внедряется в контейнер внедрения зависимостей, добавьте следующий вызов в `WebHostBuilder` с указанием имени сборки:</span><span class="sxs-lookup"><span data-stu-id="848da-415">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="848da-416">Кроме того, в `WebHostBuilder` можно добавить метод-заглушку `Configure`, который автоматически задает `applicationName`(`ApplicationKey`):</span><span class="sxs-lookup"><span data-stu-id="848da-416">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="848da-417">**Примечание**. Это необходимо только в выпуске ASP.NET Core 2.0 и только в случае, если приложение не вызывает метод `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="848da-417">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="848da-418">Дополнительные сведения см. в [комментарии к объявлению об удалении пакета Microsoft.Extensions.PlatformAbstractions](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и в [примере использования StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="848da-418">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="848da-419">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="848da-419">Additional resources</span></span>

* [<span data-ttu-id="848da-420">Размещение в Windows с помощью IIS</span><span class="sxs-lookup"><span data-stu-id="848da-420">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="848da-421">Размещение в Linux с использованием Nginx</span><span class="sxs-lookup"><span data-stu-id="848da-421">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="848da-422">Размещение в Linux с использованием Apache</span><span class="sxs-lookup"><span data-stu-id="848da-422">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="848da-423">Размещение в службе Windows</span><span class="sxs-lookup"><span data-stu-id="848da-423">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
