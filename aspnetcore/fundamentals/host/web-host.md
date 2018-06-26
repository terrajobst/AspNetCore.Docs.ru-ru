---
title: Веб-узел ASP.NET Core
author: guardrex
description: Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/web-host
ms.openlocfilehash: ce95599ec8e940635ca63c3bf9a3c28784a3f371
ms.sourcegitcommit: 43bd79667bbdc8a07bd39fb4cd6f7ad3e70212fb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34687494"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="528ce-103">Веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="528ce-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="528ce-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="528ce-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="528ce-105">Приложения ASP.NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="528ce-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="528ce-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="528ce-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="528ce-107">Узел настраивает как минимум сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="528ce-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="528ce-108">В этой статье рассматривается веб-узел ASP.NET ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), который используется для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="528ce-108">This topic covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)), which is useful for hosting web apps.</span></span> <span data-ttu-id="528ce-109">Сведения об универсальном узле .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="528ce-109">For coverage of the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="528ce-110">Создание узла</span><span class="sxs-lookup"><span data-stu-id="528ce-110">Set up a host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

<span data-ttu-id="528ce-112">Создайте узел с помощью экземпляра [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="528ce-112">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="528ce-113">Обычно это делается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="528ce-113">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="528ce-114">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="528ce-114">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="528ce-115">Обычно *Program.cs* вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла.</span><span class="sxs-lookup"><span data-stu-id="528ce-115">A typical *Program.cs* calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="528ce-116">Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.</span><span class="sxs-lookup"><span data-stu-id="528ce-116">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="528ce-117">Настраивает [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера и настраивает сервер с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-117">Configures [Kestrel](xref:fundamentals/servers/kestrel) as the web server and configures the server using the app's hosting configuration providers.</span></span> <span data-ttu-id="528ce-118">Параметры Kestrel по умолчанию см. в разделе [Параметры Kestrel](xref:fundamentals/servers/kestrel#kestrel-options) статьи "Общие сведения о реализации веб-сервера Kestrel в ASP.NET Core".</span><span class="sxs-lookup"><span data-stu-id="528ce-118">For the Kestrel default options, see [the Kestrel options section of Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel#kestrel-options).</span></span>
* <span data-ttu-id="528ce-119">В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="528ce-119">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="528ce-120">Дополнительно загружает [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) из следующих файлов и элементов:</span><span class="sxs-lookup"><span data-stu-id="528ce-120">Loads optional [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) from:</span></span>
  * <span data-ttu-id="528ce-121">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="528ce-121">*appsettings.json*.</span></span>
  * <span data-ttu-id="528ce-122">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="528ce-122">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="528ce-123">[секреты пользователя](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок;</span><span class="sxs-lookup"><span data-stu-id="528ce-123">[User secrets](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="528ce-124">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="528ce-124">Environment variables.</span></span>
  * <span data-ttu-id="528ce-125">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="528ce-125">Command-line arguments.</span></span>
* <span data-ttu-id="528ce-126">Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="528ce-126">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="528ce-127">Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="528ce-127">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="528ce-128">При выполнении за службами IIS обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index).</span><span class="sxs-lookup"><span data-stu-id="528ce-128">When running behind IIS, enables [IIS integration](xref:host-and-deploy/iis/index).</span></span> <span data-ttu-id="528ce-129">Настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="528ce-129">Configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module).</span></span> <span data-ttu-id="528ce-130">Модуль создает обратный прокси-сервер между службами IIS и Kestrel.</span><span class="sxs-lookup"><span data-stu-id="528ce-130">The module creates a reverse proxy between IIS and Kestrel.</span></span> <span data-ttu-id="528ce-131">Кроме того, настраивает [перехват приложением ошибок запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="528ce-131">Also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="528ce-132">Параметры служб IIS по умолчанию см. в разделе [Параметры служб IIS](xref:host-and-deploy/iis/index#iis-options) статьи "Размещение ASP.NET Core в Windows со службами IIS".</span><span class="sxs-lookup"><span data-stu-id="528ce-132">For the IIS default options, see [the IIS options section of Host ASP.NET Core on Windows with IIS](xref:host-and-deploy/iis/index#iis-options).</span></span>
* <span data-ttu-id="528ce-133">Устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="528ce-133">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="528ce-134">Дополнительные сведения см. в разделе [Проверка области](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="528ce-134">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="528ce-135">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="528ce-135">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="528ce-136">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="528ce-136">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="528ce-137">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="528ce-137">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="528ce-138">Дополнительные сведения о конфигурации приложения см. в разделе [Конфигурация в ASP.NET Core](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="528ce-138">For more information on app configuration, see [Configuration in ASP.NET Core](xref:fundamentals/configuration/index).</span></span>

> [!NOTE]
> <span data-ttu-id="528ce-139">Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="528ce-139">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="528ce-140">Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="528ce-140">For more information, see the ASP.NET Core 1.x tab.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-141">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-141">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

<span data-ttu-id="528ce-142">Создайте узел с помощью экземпляра [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="528ce-142">Create a host using an instance of [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span></span> <span data-ttu-id="528ce-143">Узел обычно создается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="528ce-143">Creating a host is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="528ce-144">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="528ce-144">In the project templates, `Main` is located in *Program.cs*:</span></span>

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

<span data-ttu-id="528ce-145">Для `WebHostBuilder` требуется [сервер, реализующий интерфейс IServer](xref:fundamentals/servers/index).</span><span class="sxs-lookup"><span data-stu-id="528ce-145">`WebHostBuilder` requires a [server that implements IServer](xref:fundamentals/servers/index).</span></span> <span data-ttu-id="528ce-146">Встроенными серверами являются [Kestrel](xref:fundamentals/servers/kestrel) и [HTTP.sys](xref:fundamentals/servers/httpsys) (до выхода ASP.NET Core 2.0 сервер HTTP.sys назывался [WebListener](xref:fundamentals/servers/weblistener)).</span><span class="sxs-lookup"><span data-stu-id="528ce-146">The built-in servers are [Kestrel](xref:fundamentals/servers/kestrel) and [HTTP.sys](xref:fundamentals/servers/httpsys) (prior to the release of ASP.NET Core 2.0, HTTP.sys was called [WebListener](xref:fundamentals/servers/weblistener)).</span></span> <span data-ttu-id="528ce-147">В этом примере [метод расширения UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) задает сервер Kestrel.</span><span class="sxs-lookup"><span data-stu-id="528ce-147">In this example, the [UseKestrel extension method](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) specifies the Kestrel server.</span></span>

<span data-ttu-id="528ce-148">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="528ce-148">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="528ce-149">Для получения корня содержимого по умолчанию для `UseContentRoot` используется метод [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="528ce-149">The default content root is obtained for `UseContentRoot` by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory?view=netcore-1.1).</span></span> <span data-ttu-id="528ce-150">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="528ce-150">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="528ce-151">Такое поведение по умолчанию принято в [Visual Studio](https://www.visualstudio.com/) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="528ce-151">This is the default used in [Visual Studio](https://www.visualstudio.com/) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="528ce-152">Чтобы использовать службы IIS в качестве обратного прокси-сервера, вызовите метод [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) в процессе создания узла.</span><span class="sxs-lookup"><span data-stu-id="528ce-152">To use IIS as a reverse proxy, call [UseIISIntegration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderiisextensions) as part of building the host.</span></span> <span data-ttu-id="528ce-153">Метод `UseIISIntegration` не настраивает *сервер*, как это делает метод [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="528ce-153">`UseIISIntegration` doesn't configure a *server*, like [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel?view=aspnetcore-1.1) does.</span></span> <span data-ttu-id="528ce-154">`UseIISIntegration` настраивает базовый путь и порт, через который сервер ожидает передачи данных при использовании [модуля ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) для создания обратного прокси-сервера между Kestrel и службами IIS.</span><span class="sxs-lookup"><span data-stu-id="528ce-154">`UseIISIntegration` configures the base path and port the server listens on when using the [ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) to create a reverse proxy between Kestrel and IIS.</span></span> <span data-ttu-id="528ce-155">Чтобы использовать службы IIS с ASP.NET Core, нужно указать `UseKestrel` и `UseIISIntegration`.</span><span class="sxs-lookup"><span data-stu-id="528ce-155">To use IIS with ASP.NET Core, `UseKestrel` and `UseIISIntegration` must be specified.</span></span> <span data-ttu-id="528ce-156">`UseIISIntegration` активирует только при выполнении за службами IIS или IIS Express.</span><span class="sxs-lookup"><span data-stu-id="528ce-156">`UseIISIntegration` only activates when running behind IIS or IIS Express.</span></span> <span data-ttu-id="528ce-157">Дополнительные сведения см. в разделах [Введение в модуль ASP.NET Core](xref:fundamentals/servers/aspnet-core-module) и [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module).</span><span class="sxs-lookup"><span data-stu-id="528ce-157">For more information, see [Introduction to ASP.NET Core Module](xref:fundamentals/servers/aspnet-core-module) and [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module).</span></span>

<span data-ttu-id="528ce-158">Минимальная реализация для настройки узла (и приложения ASP.NET Core) включает в себя указание сервера и настройку конвейера запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-158">A minimal implementation that configures a host (and an ASP.NET Core app) includes specifying a server and configuration of the app's request pipeline:</span></span>

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

<span data-ttu-id="528ce-159">При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="528ce-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="528ce-160">Если используется класс `Startup`, в нем должен быть определен метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="528ce-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="528ce-161">Дополнительные сведения см. в разделе [Запуск приложения в ASP.NET Core](xref:fundamentals/startup).</span><span class="sxs-lookup"><span data-stu-id="528ce-161">For more information, see [Application Startup in ASP.NET Core](xref:fundamentals/startup).</span></span> <span data-ttu-id="528ce-162">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="528ce-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="528ce-163">При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.</span><span class="sxs-lookup"><span data-stu-id="528ce-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="528ce-164">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="528ce-164">Host configuration values</span></span>

<span data-ttu-id="528ce-165">Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="528ce-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="528ce-166">конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`,</span><span class="sxs-lookup"><span data-stu-id="528ce-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="528ce-167">Например, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="528ce-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="528ce-168">Явные методы, такие как [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span><span class="sxs-lookup"><span data-stu-id="528ce-168">Explicit methods, such as [HostingAbstractionsWebHostBuilderExtensions.UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot).</span></span>
* <span data-ttu-id="528ce-169">метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="528ce-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="528ce-170">Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="528ce-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="528ce-171">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="528ce-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="528ce-172">Дополнительные сведения см. в подразделе [Переопределение конфигурации](#override-configuration) следующего раздела.</span><span class="sxs-lookup"><span data-stu-id="528ce-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="capture-startup-errors"></a><span data-ttu-id="528ce-173">Перехват ошибок при загрузке</span><span class="sxs-lookup"><span data-stu-id="528ce-173">Capture Startup Errors</span></span>

<span data-ttu-id="528ce-174">Этот параметр управляет перехватом ошибок при загрузке.</span><span class="sxs-lookup"><span data-stu-id="528ce-174">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="528ce-175">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="528ce-175">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="528ce-176">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="528ce-176">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="528ce-177">**Значение по умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="528ce-177">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="528ce-178">**Задается с помощью**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="528ce-178">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="528ce-179">**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="528ce-179">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="528ce-180">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="528ce-180">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="528ce-181">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="528ce-181">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-182">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-182">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-183">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-183">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .CaptureStartupErrors(true)
```

---

### <a name="content-root"></a><span data-ttu-id="528ce-184">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="528ce-184">Content Root</span></span>

<span data-ttu-id="528ce-185">Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC.</span><span class="sxs-lookup"><span data-stu-id="528ce-185">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="528ce-186">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="528ce-186">**Key**: contentRoot</span></span>  
<span data-ttu-id="528ce-187">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="528ce-187">**Type**: *string*</span></span>  
<span data-ttu-id="528ce-188">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-188">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="528ce-189">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="528ce-189">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="528ce-190">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="528ce-190">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="528ce-191">Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root).</span><span class="sxs-lookup"><span data-stu-id="528ce-191">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="528ce-192">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="528ce-192">If the path doesn't exist, the host fails to start.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-193">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-193">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-194">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-194">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseContentRoot("c:\\<content-root>")
```

---

### <a name="detailed-errors"></a><span data-ttu-id="528ce-195">Подробные сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="528ce-195">Detailed Errors</span></span>

<span data-ttu-id="528ce-196">Определяет, следует ли перехватывать подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="528ce-196">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="528ce-197">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="528ce-197">**Key**: detailedErrors</span></span>  
<span data-ttu-id="528ce-198">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="528ce-198">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="528ce-199">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="528ce-199">**Default**: false</span></span>  
<span data-ttu-id="528ce-200">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="528ce-200">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="528ce-201">**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="528ce-201">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="528ce-202">Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.</span><span class="sxs-lookup"><span data-stu-id="528ce-202">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-203">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-203">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-204">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-204">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

---

### <a name="environment"></a><span data-ttu-id="528ce-205">Среда</span><span class="sxs-lookup"><span data-stu-id="528ce-205">Environment</span></span>

<span data-ttu-id="528ce-206">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-206">Sets the app's environment.</span></span>

<span data-ttu-id="528ce-207">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="528ce-207">**Key**: environment</span></span>  
<span data-ttu-id="528ce-208">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="528ce-208">**Type**: *string*</span></span>  
<span data-ttu-id="528ce-209">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="528ce-209">**Default**: Production</span></span>  
<span data-ttu-id="528ce-210">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="528ce-210">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="528ce-211">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="528ce-211">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="528ce-212">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="528ce-212">The environment can be set to any value.</span></span> <span data-ttu-id="528ce-213">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="528ce-213">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="528ce-214">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="528ce-214">Values aren't case sensitive.</span></span> <span data-ttu-id="528ce-215">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="528ce-215">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="528ce-216">При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="528ce-216">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="528ce-217">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="528ce-217">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-218">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-218">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-219">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-219">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseEnvironment(EnvironmentName.Development)
```

---

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="528ce-220">Начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="528ce-220">Hosting Startup Assemblies</span></span>

<span data-ttu-id="528ce-221">Задает начальные сборки размещения для приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-221">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="528ce-222">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="528ce-222">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="528ce-223">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="528ce-223">**Type**: *string*</span></span>  
<span data-ttu-id="528ce-224">**Значение по умолчанию**: пустая строка</span><span class="sxs-lookup"><span data-stu-id="528ce-224">**Default**: Empty string</span></span>  
<span data-ttu-id="528ce-225">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="528ce-225">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="528ce-226">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="528ce-226">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="528ce-227">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="528ce-227">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span> <span data-ttu-id="528ce-228">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="528ce-228">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="528ce-229">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="528ce-230">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="528ce-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-231">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-231">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-232">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-232">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="528ce-233">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="528ce-233">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prefer-hosting-urls"></a><span data-ttu-id="528ce-234">Предпочитать URL-адреса размещения</span><span class="sxs-lookup"><span data-stu-id="528ce-234">Prefer Hosting URLs</span></span>

<span data-ttu-id="528ce-235">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="528ce-235">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="528ce-236">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="528ce-236">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="528ce-237">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="528ce-237">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="528ce-238">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="528ce-238">**Default**: true</span></span>  
<span data-ttu-id="528ce-239">**Задается с помощью**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="528ce-239">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="528ce-240">**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="528ce-240">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

<span data-ttu-id="528ce-241">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="528ce-241">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-242">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-242">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-243">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-243">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="528ce-244">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="528ce-244">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="prevent-hosting-startup"></a><span data-ttu-id="528ce-245">Запретить запуск размещения</span><span class="sxs-lookup"><span data-stu-id="528ce-245">Prevent Hosting Startup</span></span>

<span data-ttu-id="528ce-246">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-246">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="528ce-247">Дополнительные сведения см. в разделе [Усовершенствование приложения из внешней сборки с помощью IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration).</span><span class="sxs-lookup"><span data-stu-id="528ce-247">See [Enhance an app from an external assembly with IHostingStartup](xref:fundamentals/configuration/platform-specific-configuration) for more information.</span></span>

<span data-ttu-id="528ce-248">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="528ce-248">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="528ce-249">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="528ce-249">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="528ce-250">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="528ce-250">**Default**: false</span></span>  
<span data-ttu-id="528ce-251">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="528ce-251">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="528ce-252">**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="528ce-252">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

<span data-ttu-id="528ce-253">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="528ce-253">This feature is new in ASP.NET Core 2.0.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-254">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-254">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-255">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-255">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="528ce-256">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="528ce-256">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="server-urls"></a><span data-ttu-id="528ce-257">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="528ce-257">Server URLs</span></span>

<span data-ttu-id="528ce-258">Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="528ce-258">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="528ce-259">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="528ce-259">**Key**: urls</span></span>  
<span data-ttu-id="528ce-260">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="528ce-260">**Type**: *string*</span></span>  
<span data-ttu-id="528ce-261">**По умолчанию**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="528ce-261">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="528ce-262">**Задается с помощью**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="528ce-262">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="528ce-263">**Переменная среды**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="528ce-263">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="528ce-264">Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="528ce-264">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="528ce-265">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="528ce-265">For example, `http://localhost:123`.</span></span> <span data-ttu-id="528ce-266">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="528ce-266">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="528ce-267">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="528ce-267">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="528ce-268">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="528ce-268">Supported formats vary between servers.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-269">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-269">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="528ce-270">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="528ce-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="528ce-271">Дополнительные сведения см. в статье [Реализация веб-сервера Kestrel в ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="528ce-271">For more information, see [Kestrel web server implementation in ASP.NET Core](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#endpoint-configuration).</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-272">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-272">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

---

### <a name="shutdown-timeout"></a><span data-ttu-id="528ce-273">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="528ce-273">Shutdown Timeout</span></span>

<span data-ttu-id="528ce-274">Определяет, как долго необходимо ожидать завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="528ce-274">Specifies the amount of time to wait for the web host to shut down.</span></span>

<span data-ttu-id="528ce-275">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="528ce-275">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="528ce-276">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="528ce-276">**Type**: *int*</span></span>  
<span data-ttu-id="528ce-277">**Значение по умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="528ce-277">**Default**: 5</span></span>  
<span data-ttu-id="528ce-278">**Задается с помощью**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="528ce-278">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="528ce-279">**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="528ce-279">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="528ce-280">Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) принимает [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="528ce-280">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span> <span data-ttu-id="528ce-281">Это новая возможность в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="528ce-281">This feature is new in ASP.NET Core 2.0.</span></span>

<span data-ttu-id="528ce-282">Во время ожидания размещение:</span><span class="sxs-lookup"><span data-stu-id="528ce-282">During the timeout period, hosting:</span></span>

* <span data-ttu-id="528ce-283">Активирует [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="528ce-283">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="528ce-284">Пытается остановить размещенные службы, записывая в журнал все ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="528ce-284">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="528ce-285">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-285">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="528ce-286">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="528ce-286">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="528ce-287">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="528ce-287">If services require additional time to stop, increase the timeout.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-288">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-288">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-289">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-289">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="528ce-290">Эта возможность недоступна в ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="528ce-290">This feature is unavailable in ASP.NET Core 1.x.</span></span>

---

### <a name="startup-assembly"></a><span data-ttu-id="528ce-291">Стартовая сборка</span><span class="sxs-lookup"><span data-stu-id="528ce-291">Startup Assembly</span></span>

<span data-ttu-id="528ce-292">Определяет сборку, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="528ce-292">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="528ce-293">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="528ce-293">**Key**: startupAssembly</span></span>  
<span data-ttu-id="528ce-294">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="528ce-294">**Type**: *string*</span></span>  
<span data-ttu-id="528ce-295">**Значение по умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="528ce-295">**Default**: The app's assembly</span></span>  
<span data-ttu-id="528ce-296">**Задается с помощью**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="528ce-296">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="528ce-297">**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="528ce-297">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="528ce-298">На сборку можно ссылаться по имени (`string`) или типу (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="528ce-298">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="528ce-299">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="528ce-299">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-300">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-300">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-301">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-301">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseStartup("StartupAssemblyName")
```

```csharp
var host = new WebHostBuilder()
    .UseStartup<TStartup>()
```

---

### <a name="web-root"></a><span data-ttu-id="528ce-302">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="528ce-302">Web Root</span></span>

<span data-ttu-id="528ce-303">Задает относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-303">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="528ce-304">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="528ce-304">**Key**: webroot</span></span>  
<span data-ttu-id="528ce-305">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="528ce-305">**Type**: *string*</span></span>  
<span data-ttu-id="528ce-306">**Значение по умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии.</span><span class="sxs-lookup"><span data-stu-id="528ce-306">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="528ce-307">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="528ce-307">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="528ce-308">**Задается с помощью**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="528ce-308">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="528ce-309">**Переменная среды**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="528ce-309">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-310">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-310">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-311">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-311">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```csharp
var host = new WebHostBuilder()
    .UseWebRoot("public")
```

---

## <a name="override-configuration"></a><span data-ttu-id="528ce-312">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="528ce-312">Override configuration</span></span>

<span data-ttu-id="528ce-313">Для настройки хоста используется [конфигурация](xref:fundamentals/configuration/index).</span><span class="sxs-lookup"><span data-stu-id="528ce-313">Use [Configuration](xref:fundamentals/configuration/index) to configure the host.</span></span> <span data-ttu-id="528ce-314">В приведенном ниже примере необязательная конфигурация хоста задается в файле *hosting.json*.</span><span class="sxs-lookup"><span data-stu-id="528ce-314">In the following example, host configuration is optionally specified in a *hosting.json* file.</span></span> <span data-ttu-id="528ce-315">Любую конфигурацию, загружаемую из файла *hosting.json*, можно переопределить с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="528ce-315">Any configuration loaded from the *hosting.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="528ce-316">Встроенная конфигурация (в `config`) используется для настройки узла с помощью `UseConfiguration`.</span><span class="sxs-lookup"><span data-stu-id="528ce-316">The built configuration (in `config`) is used to configure the host with `UseConfiguration`.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-317">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-317">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="528ce-318">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="528ce-318">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="528ce-319">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="528ce-319">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-320">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-320">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="528ce-321">*hosting.json*:</span><span class="sxs-lookup"><span data-stu-id="528ce-321">*hosting.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

<span data-ttu-id="528ce-322">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hosting.json*, а затем с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="528ce-322">Overriding the configuration provided by `UseUrls` with *hosting.json* config first, command-line argument config second:</span></span>

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
> <span data-ttu-id="528ce-323">Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="528ce-323">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="528ce-324">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="528ce-324">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="528ce-325">Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="528ce-325">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="528ce-326">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="528ce-326">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="528ce-327">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="528ce-327">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="528ce-328">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="528ce-328">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="528ce-329">Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение из командной строки при выполнении команды [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="528ce-329">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="528ce-330">Аргумент командной строки переопределяет значение `urls` из файла *hosting.json*, и сервер будет ожидать передачи данных через порт 8080.</span><span class="sxs-lookup"><span data-stu-id="528ce-330">The command-line argument overrides the `urls` value from the *hosting.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="528ce-331">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="528ce-331">Manage the host</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="528ce-332">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="528ce-332">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="528ce-333">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="528ce-333">**Run**</span></span>

<span data-ttu-id="528ce-334">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="528ce-334">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="528ce-335">**Start**</span><span class="sxs-lookup"><span data-stu-id="528ce-335">**Start**</span></span>

<span data-ttu-id="528ce-336">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="528ce-336">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="528ce-337">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="528ce-337">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="528ce-338">Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода.</span><span class="sxs-lookup"><span data-stu-id="528ce-338">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="528ce-339">Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="528ce-339">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="528ce-340">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="528ce-340">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="528ce-341">Выполните запуск с помощью `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="528ce-341">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="528ce-342">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="528ce-342">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="528ce-343">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="528ce-343">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="528ce-344">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="528ce-344">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="528ce-345">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="528ce-345">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="528ce-346">Выполните запуск с помощью URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="528ce-346">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="528ce-347">Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="528ce-347">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="528ce-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="528ce-348">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="528ce-349">Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="528ce-349">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="528ce-350">В этом примере используйте следующие запросы в браузере:</span><span class="sxs-lookup"><span data-stu-id="528ce-350">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="528ce-351">Запрос</span><span class="sxs-lookup"><span data-stu-id="528ce-351">Request</span></span>                                    | <span data-ttu-id="528ce-352">Ответ</span><span class="sxs-lookup"><span data-stu-id="528ce-352">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="528ce-353">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="528ce-353">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="528ce-354">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="528ce-354">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="528ce-355">Вызывает исключение со строкой "ooops!"</span><span class="sxs-lookup"><span data-stu-id="528ce-355">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="528ce-356">Вызывает исключение со строкой "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="528ce-356">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="528ce-357">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="528ce-357">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="528ce-358">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="528ce-358">Hello World!</span></span>                             |

<span data-ttu-id="528ce-359">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="528ce-359">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="528ce-360">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="528ce-360">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="528ce-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="528ce-361">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="528ce-362">Используйте URL-адрес и экземпляр `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="528ce-362">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="528ce-363">Результат будет тем же, что и при использовании **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="528ce-363">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="528ce-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="528ce-364">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="528ce-365">Предоставьте делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="528ce-365">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="528ce-366">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="528ce-366">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="528ce-367">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="528ce-367">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="528ce-368">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="528ce-368">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="528ce-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="528ce-369">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="528ce-370">Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="528ce-370">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="528ce-371">Результат будет тем же, что и при использовании **StartWith(Action&lt;IApplicationBuilder&gt; app)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="528ce-371">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="528ce-372">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="528ce-372">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="528ce-373">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="528ce-373">**Run**</span></span>

<span data-ttu-id="528ce-374">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="528ce-374">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="528ce-375">**Start**</span><span class="sxs-lookup"><span data-stu-id="528ce-375">**Start**</span></span>

<span data-ttu-id="528ce-376">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="528ce-376">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="528ce-377">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="528ce-377">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="528ce-378">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="528ce-378">IHostingEnvironment interface</span></span>

<span data-ttu-id="528ce-379">[Интерфейс IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-379">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="528ce-380">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="528ce-380">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="528ce-381">Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#startup-conventions).</span><span class="sxs-lookup"><span data-stu-id="528ce-381">A [convention-based approach](xref:fundamentals/environments#startup-conventions) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="528ce-382">Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="528ce-382">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="528ce-383">Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="528ce-383">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="528ce-384">Подробные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="528ce-384">See [Use multiple environments](xref:fundamentals/environments) for details.</span></span>

<span data-ttu-id="528ce-385">Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="528ce-385">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

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

<span data-ttu-id="528ce-386">`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware/index#writing-middleware):</span><span class="sxs-lookup"><span data-stu-id="528ce-386">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/index#writing-middleware):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="528ce-387">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="528ce-387">IApplicationLifetime interface</span></span>

<span data-ttu-id="528ce-388">[Интерфейс IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="528ce-388">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="528ce-389">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="528ce-389">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="528ce-390">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="528ce-390">Cancellation Token</span></span>    | <span data-ttu-id="528ce-391">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="528ce-391">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="528ce-392">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="528ce-392">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="528ce-393">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="528ce-393">The host has fully started.</span></span> |
| [<span data-ttu-id="528ce-394">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="528ce-394">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="528ce-395">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="528ce-395">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="528ce-396">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="528ce-396">All requests should be processed.</span></span> <span data-ttu-id="528ce-397">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="528ce-397">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="528ce-398">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="528ce-398">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="528ce-399">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="528ce-399">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="528ce-400">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="528ce-400">Requests may still be processing.</span></span> <span data-ttu-id="528ce-401">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="528ce-401">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="528ce-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="528ce-402">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="528ce-403">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="528ce-403">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="528ce-404">Проверка области</span><span class="sxs-lookup"><span data-stu-id="528ce-404">Scope validation</span></span>

<span data-ttu-id="528ce-405">В ASP.NET Core 2.0 или более поздней версии [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="528ce-405">In ASP.NET Core 2.0 or later, [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="528ce-406">Если для `ValidateScopes` установлено значение `true`, поставщик службы по умолчанию проверяет, что:</span><span class="sxs-lookup"><span data-stu-id="528ce-406">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="528ce-407">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="528ce-407">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="528ce-408">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="528ce-408">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="528ce-409">Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="528ce-409">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="528ce-410">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="528ce-410">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="528ce-411">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="528ce-411">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="528ce-412">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="528ce-412">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="528ce-413">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="528ce-413">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="528ce-414">Чтобы всегда проверять области, в том числе в рабочей среде, настройте [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) с [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) в конструкторе узлов:</span><span class="sxs-lookup"><span data-stu-id="528ce-414">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="troubleshooting-systemargumentexception"></a><span data-ttu-id="528ce-415">Устранение неполадок, связанных с исключениями System.ArgumentException</span><span class="sxs-lookup"><span data-stu-id="528ce-415">Troubleshooting System.ArgumentException</span></span>

<span data-ttu-id="528ce-416">**Относится только к ASP.NET Core 2.0**</span><span class="sxs-lookup"><span data-stu-id="528ce-416">**Applies to ASP.NET Core 2.0 Only**</span></span>

<span data-ttu-id="528ce-417">Узел может создаваться путем внедрения интерфейса `IStartup` непосредственно в контейнер внедрения зависимостей, а не путем вызова `UseStartup` или `Configure`:</span><span class="sxs-lookup"><span data-stu-id="528ce-417">A host may be built by injecting `IStartup` directly into the dependency injection container rather than calling `UseStartup` or `Configure`:</span></span>

```csharp
services.AddSingleton<IStartup, Startup>();
```

<span data-ttu-id="528ce-418">Если узел создается таким способом, может произойти следующая ошибка:</span><span class="sxs-lookup"><span data-stu-id="528ce-418">If the host is built this way, the following error may occur:</span></span>

```
Unhandled Exception: System.ArgumentException: A valid non-empty application name must be provided.
```

<span data-ttu-id="528ce-419">Причина в том, что [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (текущая сборка) требуется для проверки `HostingStartupAttributes`.</span><span class="sxs-lookup"><span data-stu-id="528ce-419">This occurs because the [applicationName(ApplicationKey)](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults#Microsoft_AspNetCore_Hosting_WebHostDefaults_ApplicationKey) (the current assembly) is required to scan for `HostingStartupAttributes`.</span></span> <span data-ttu-id="528ce-420">Если в приложении интерфейс `IStartup` вручную внедряется в контейнер внедрения зависимостей, добавьте следующий вызов в `WebHostBuilder` с указанием имени сборки:</span><span class="sxs-lookup"><span data-stu-id="528ce-420">If the app manually injects `IStartup` into the dependency injection container, add the following call to `WebHostBuilder` with the assembly name specified:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("applicationName", "<Assembly Name>")
    ...
```

<span data-ttu-id="528ce-421">Кроме того, в `WebHostBuilder` можно добавить метод-заглушку `Configure`, который автоматически задает `applicationName`(`ApplicationKey`):</span><span class="sxs-lookup"><span data-stu-id="528ce-421">Alternatively, add a dummy `Configure` to the `WebHostBuilder`, which sets the `applicationName`(`ApplicationKey`) automatically:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .Configure(_ => { })
    ...
```

<span data-ttu-id="528ce-422">**Примечание**. Это необходимо только в выпуске ASP.NET Core 2.0 и только в случае, если приложение не вызывает метод `UseStartup` или `Configure`.</span><span class="sxs-lookup"><span data-stu-id="528ce-422">**NOTE**: This is only required with the ASP.NET Core 2.0 release and only when the app doesn't call `UseStartup` or `Configure`.</span></span>

<span data-ttu-id="528ce-423">Дополнительные сведения см. в [комментарии к объявлению об удалении пакета Microsoft.Extensions.PlatformAbstractions](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) и в [примере использования StartupInjection](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span><span class="sxs-lookup"><span data-stu-id="528ce-423">For more information, see [Announcements: Microsoft.Extensions.PlatformAbstractions has been removed (comment)](https://github.com/aspnet/Announcements/issues/237#issuecomment-323786938) and the [StartupInjection sample](https://github.com/aspnet/Hosting/blob/8377d226f1e6e1a97dabdb6769a845eeccc829ed/samples/SampleStartups/StartupInjection.cs).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="528ce-424">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="528ce-424">Additional resources</span></span>

* [<span data-ttu-id="528ce-425">Размещение в Windows с помощью IIS</span><span class="sxs-lookup"><span data-stu-id="528ce-425">Host on Windows with IIS</span></span>](xref:host-and-deploy/iis/index)
* [<span data-ttu-id="528ce-426">Размещение в Linux с использованием Nginx</span><span class="sxs-lookup"><span data-stu-id="528ce-426">Host on Linux with Nginx</span></span>](xref:host-and-deploy/linux-nginx)
* [<span data-ttu-id="528ce-427">Размещение в Linux с использованием Apache</span><span class="sxs-lookup"><span data-stu-id="528ce-427">Host on Linux with Apache</span></span>](xref:host-and-deploy/linux-apache)
* [<span data-ttu-id="528ce-428">Размещение в службе Windows</span><span class="sxs-lookup"><span data-stu-id="528ce-428">Host in a Windows Service</span></span>](xref:host-and-deploy/windows-service)
