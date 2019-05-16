---
title: Веб-узел ASP.NET Core
author: guardrex
description: Сведения о веб-узле в ASP.NET Core, который отвечает за запуск приложений и управление временем существования.
ms.author: riande
ms.custom: mvc
ms.date: 05/11/2019
uid: fundamentals/host/web-host
ms.openlocfilehash: 48f3b664d901bdfb27cdf9e798fa60c0587d1def
ms.sourcegitcommit: 6afe57fb8d9055f88fedb92b16470398c4b9b24a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/14/2019
ms.locfileid: "65610290"
---
# <a name="aspnet-core-web-host"></a><span data-ttu-id="53bb3-103">Веб-узел ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="53bb3-103">ASP.NET Core Web Host</span></span>

<span data-ttu-id="53bb3-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="53bb3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="53bb3-105">Приложения ASP.NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="53bb3-105">ASP.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="53bb3-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="53bb3-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="53bb3-107">Узел настраивает как минимум сервер и конвейер обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="53bb3-107">At a minimum, the host configures a server and a request processing pipeline.</span></span> <span data-ttu-id="53bb3-108">Узел также может настроить ведение журнала, внедрение зависимостей и конфигурацию.</span><span class="sxs-lookup"><span data-stu-id="53bb3-108">The host can also set up logging, dependency injection, and configuration.</span></span>

::: moniker range="<= aspnetcore-1.1"

<span data-ttu-id="53bb3-109">Для версии 1.1 этого раздела скачайте статью [Веб-узел ASP.NET Core (версия 1.1, PDF-файл)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span><span class="sxs-lookup"><span data-stu-id="53bb3-109">For the 1.1 version of this topic, download [ASP.NET Core Web Host (version 1.1, PDF)](https://webpifeed.blob.core.windows.net/webpifeed/Partners/Web-Host_1.1.pdf).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="53bb3-110">В этой статье рассматривается веб-узел ASP.NET Core (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), который предназначен для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="53bb3-110">This article covers the ASP.NET Core Web Host (<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder>), which is for hosting web apps.</span></span> <span data-ttu-id="53bb3-111">Сведения об универсальном узле .NET ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)) см. в статье <xref:fundamentals/host/generic-host>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-111">For information about the .NET Generic Host ([IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder)), see <xref:fundamentals/host/generic-host>.</span></span>

::: moniker-end

::: moniker range="> aspnetcore-2.2"

<span data-ttu-id="53bb3-112">В этой статье рассматривается веб-узел ASP.NET Core ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span><span class="sxs-lookup"><span data-stu-id="53bb3-112">This article covers the ASP.NET Core Web Host ([IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder)).</span></span> <span data-ttu-id="53bb3-113">В ASP.NET Core 3.0 универсальный узел заменяет веб-узел.</span><span class="sxs-lookup"><span data-stu-id="53bb3-113">In ASP.NET Core 3.0, Generic Host replaces Web Host.</span></span> <span data-ttu-id="53bb3-114">Дополнительную информацию см. в разделе [Узел](xref:fundamentals/index#host).</span><span class="sxs-lookup"><span data-stu-id="53bb3-114">For more information, see [The host](xref:fundamentals/index#host).</span></span>

::: moniker-end

## <a name="set-up-a-host"></a><span data-ttu-id="53bb3-115">Создание узла</span><span class="sxs-lookup"><span data-stu-id="53bb3-115">Set up a host</span></span>

<span data-ttu-id="53bb3-116">Создайте узел с помощью экземпляра [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="53bb3-116">Create a host using an instance of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="53bb3-117">Обычно это делается в точке входа в приложение, то есть в методе `Main`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-117">This is typically performed in the app's entry point, the `Main` method.</span></span> <span data-ttu-id="53bb3-118">Имя метода построителя `CreateWebHostBuilder` — это специальное имя, которое определяет метод построителя для внешних компонентов, таких как [Entity Framework](/ef/core/).</span><span class="sxs-lookup"><span data-stu-id="53bb3-118">The builder method name, `CreateWebHostBuilder`, is special name that identifies the builder method to external components, such as [Entity Framework](/ef/core/).</span></span>

<span data-ttu-id="53bb3-119">В шаблонах проектов метод `Main` находится в файле *Program.cs*.</span><span class="sxs-lookup"><span data-stu-id="53bb3-119">In the project templates, `Main` is located in *Program.cs*.</span></span> <span data-ttu-id="53bb3-120">Обычно приложение вызывает [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder), чтобы начать настройку узла:</span><span class="sxs-lookup"><span data-stu-id="53bb3-120">A typical app calls [CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) to start setting up a host:</span></span>

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

<span data-ttu-id="53bb3-121">Метод `CreateDefaultBuilder` выполняет указанные ниже задачи.</span><span class="sxs-lookup"><span data-stu-id="53bb3-121">`CreateDefaultBuilder` performs the following tasks:</span></span>

* <span data-ttu-id="53bb3-122">Настраивает сервер [Kestrel](xref:fundamentals/servers/kestrel) в качестве веб-сервера с помощью поставщиков конфигурации размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-122">Configures [Kestrel](xref:fundamentals/servers/kestrel) server as the web server using the app's hosting configuration providers.</span></span> <span data-ttu-id="53bb3-123">Параметры сервера Kestrel по умолчанию см. в разделе <xref:fundamentals/servers/kestrel#kestrel-options>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-123">For the Kestrel server's default options, see <xref:fundamentals/servers/kestrel#kestrel-options>.</span></span>
* <span data-ttu-id="53bb3-124">В качестве корня содержимого задает путь, возвращенный методом [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span><span class="sxs-lookup"><span data-stu-id="53bb3-124">Sets the content root to the path returned by [Directory.GetCurrentDirectory](/dotnet/api/system.io.directory.getcurrentdirectory).</span></span>
* <span data-ttu-id="53bb3-125">Загружает [конфигурацию узла](#host-configuration-values) из:</span><span class="sxs-lookup"><span data-stu-id="53bb3-125">Loads [host configuration](#host-configuration-values) from:</span></span>
  * <span data-ttu-id="53bb3-126">Переменные среды с префиксом `ASPNETCORE_` (например, `ASPNETCORE_ENVIRONMENT`).</span><span class="sxs-lookup"><span data-stu-id="53bb3-126">Environment variables prefixed with `ASPNETCORE_` (for example, `ASPNETCORE_ENVIRONMENT`).</span></span>
  * <span data-ttu-id="53bb3-127">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="53bb3-127">Command-line arguments.</span></span>
* <span data-ttu-id="53bb3-128">Загружает конфигурацию приложения в следующем порядке из:</span><span class="sxs-lookup"><span data-stu-id="53bb3-128">Loads app configuration in the following order from:</span></span>
  * <span data-ttu-id="53bb3-129">*appsettings.json*;</span><span class="sxs-lookup"><span data-stu-id="53bb3-129">*appsettings.json*.</span></span>
  * <span data-ttu-id="53bb3-130">*appsettings.{Environment}.json*;</span><span class="sxs-lookup"><span data-stu-id="53bb3-130">*appsettings.{Environment}.json*.</span></span>
  * <span data-ttu-id="53bb3-131">[Менеджера секретов](xref:security/app-secrets), когда приложение выполняется в среде `Development` с использованием начальных сборок.</span><span class="sxs-lookup"><span data-stu-id="53bb3-131">[Secret Manager](xref:security/app-secrets) when the app runs in the `Development` environment using the entry assembly.</span></span>
  * <span data-ttu-id="53bb3-132">Переменные среды.</span><span class="sxs-lookup"><span data-stu-id="53bb3-132">Environment variables.</span></span>
  * <span data-ttu-id="53bb3-133">аргументы командной строки.</span><span class="sxs-lookup"><span data-stu-id="53bb3-133">Command-line arguments.</span></span>
* <span data-ttu-id="53bb3-134">Настраивает [ведение журнала](xref:fundamentals/logging/index) для выходных данных консоли и отладки.</span><span class="sxs-lookup"><span data-stu-id="53bb3-134">Configures [logging](xref:fundamentals/logging/index) for console and debug output.</span></span> <span data-ttu-id="53bb3-135">Ведение журнала включает в себя правила [фильтрации журналов](xref:fundamentals/logging/index#log-filtering), заданные в разделе конфигурации ведения журнала в файле *appsettings.json* или *appsettings.{Environment}.json*.</span><span class="sxs-lookup"><span data-stu-id="53bb3-135">Logging includes [log filtering](xref:fundamentals/logging/index#log-filtering) rules specified in a Logging configuration section of an *appsettings.json* or *appsettings.{Environment}.json* file.</span></span>
* <span data-ttu-id="53bb3-136">При работе за службами IIS с [модулем ASP.NET Core](xref:host-and-deploy/aspnet-core-module) `CreateDefaultBuilder` обеспечивает [интеграцию со службами IIS](xref:host-and-deploy/iis/index) для настройки базового адреса и порта приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-136">When running behind IIS with the [ASP.NET Core Module](xref:host-and-deploy/aspnet-core-module), `CreateDefaultBuilder` enables [IIS Integration](xref:host-and-deploy/iis/index), which configures the app's base address and port.</span></span> <span data-ttu-id="53bb3-137">Кроме того, интеграция со службами IIS также позволяет настраивать [перехват приложением ошибок запуска](#capture-startup-errors).</span><span class="sxs-lookup"><span data-stu-id="53bb3-137">IIS Integration also configures the app to [capture startup errors](#capture-startup-errors).</span></span> <span data-ttu-id="53bb3-138">Параметры IIS по умолчанию см. в разделе <xref:host-and-deploy/iis/index#iis-options>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-138">For the IIS default options, see <xref:host-and-deploy/iis/index#iis-options>.</span></span>
* <span data-ttu-id="53bb3-139">Устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="53bb3-139">Sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span> <span data-ttu-id="53bb3-140">Дополнительные сведения см. в разделе [Проверка области](#scope-validation).</span><span class="sxs-lookup"><span data-stu-id="53bb3-140">For more information, see [Scope validation](#scope-validation).</span></span>

<span data-ttu-id="53bb3-141">Настройки, определенные `CreateDefaultBuilder`, можно переопределить и усилить с помощью [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging) и других методов и методов расширения [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="53bb3-141">The configuration defined by `CreateDefaultBuilder` can be overridden and augmented by [ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration), [ConfigureLogging](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configurelogging), and other methods and extension methods of [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="53bb3-142">Ниже приведены некоторые примеры:</span><span class="sxs-lookup"><span data-stu-id="53bb3-142">A few examples follow:</span></span>

* <span data-ttu-id="53bb3-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) используется для указания дополнительного объекта `IConfiguration` для приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-143">[ConfigureAppConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configureappconfiguration) is used to specify additional `IConfiguration` for the app.</span></span> <span data-ttu-id="53bb3-144">Следующий вызов `ConfigureAppConfiguration` добавляет делегат, чтобы включить конфигурацию приложения в файл *appsettings.xml*.</span><span class="sxs-lookup"><span data-stu-id="53bb3-144">The following `ConfigureAppConfiguration` call adds a delegate to include app configuration in the *appsettings.xml* file.</span></span> <span data-ttu-id="53bb3-145">`ConfigureAppConfiguration` можно вызывать несколько раз.</span><span class="sxs-lookup"><span data-stu-id="53bb3-145">`ConfigureAppConfiguration` may be called multiple times.</span></span> <span data-ttu-id="53bb3-146">Обратите внимание, что эта конфигурация не распространяется на узел (например, URL-адреса серверов или среду).</span><span class="sxs-lookup"><span data-stu-id="53bb3-146">Note that this configuration doesn't apply to the host (for example, server URLs or environment).</span></span> <span data-ttu-id="53bb3-147">См. раздел [Значения конфигурации узла](#host-configuration-values).</span><span class="sxs-lookup"><span data-stu-id="53bb3-147">See the [Host configuration values](#host-configuration-values) section.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureAppConfiguration((hostingContext, config) =>
        {
            config.AddXmlFile("appsettings.xml", optional: true, reloadOnChange: true);
        })
        ...
    ```

* <span data-ttu-id="53bb3-148">Следующий вызов `ConfigureLogging` добавляет делегата, чтобы настроить минимальный уровень ведения журнала ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) для [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span><span class="sxs-lookup"><span data-stu-id="53bb3-148">The following `ConfigureLogging` call adds a delegate to configure the minimum logging level ([SetMinimumLevel](/dotnet/api/microsoft.extensions.logging.loggingbuilderextensions.setminimumlevel)) to [LogLevel.Warning](/dotnet/api/microsoft.extensions.logging.loglevel).</span></span> <span data-ttu-id="53bb3-149">Этот параметр переопределяет параметры в *appsettings.Development.json* (`LogLevel.Debug`) и *appsettings.Production.json* (`LogLevel.Error`), заданные `CreateDefaultBuilder`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-149">This setting overrides the settings in *appsettings.Development.json* (`LogLevel.Debug`) and *appsettings.Production.json* (`LogLevel.Error`) configured by `CreateDefaultBuilder`.</span></span> <span data-ttu-id="53bb3-150">`ConfigureLogging` можно вызывать несколько раз.</span><span class="sxs-lookup"><span data-stu-id="53bb3-150">`ConfigureLogging` may be called multiple times.</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureLogging(logging => 
        {
            logging.SetMinimumLevel(LogLevel.Warning);
        })
        ...
    ```

::: moniker range=">= aspnetcore-2.2"

* <span data-ttu-id="53bb3-151">При следующем вызове к `ConfigureKestrel` переопределяется значение по умолчанию [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), равное 30 000 000 байтов, установленное при настройке Kestrel методом `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-151">The following call to `ConfigureKestrel` overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .ConfigureKestrel((context, options) =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

::: moniker range="< aspnetcore-2.2"

* <span data-ttu-id="53bb3-152">Следующий вызов [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) переопределяет значение по умолчанию [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize), равное 30 000 000 байтов, установленное при настройке Kestrel методом `CreateDefaultBuilder`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-152">The following call to [UseKestrel](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderkestrelextensions.usekestrel) overrides the default [Limits.MaxRequestBodySize](/dotnet/api/microsoft.aspnetcore.server.kestrel.core.kestrelserverlimits.maxrequestbodysize) of 30,000,000 bytes established when Kestrel was configured by `CreateDefaultBuilder`:</span></span>

    ```csharp
    WebHost.CreateDefaultBuilder(args)
        .UseKestrel(options =>
        {
            options.Limits.MaxRequestBodySize = 20000000;
        });
    ```

::: moniker-end

<span data-ttu-id="53bb3-153">*Корень содержимого* определяет, где узел ищет файлы содержимого, например файлы представлений MVC.</span><span class="sxs-lookup"><span data-stu-id="53bb3-153">The *content root* determines where the host searches for content files, such as MVC view files.</span></span> <span data-ttu-id="53bb3-154">При запуске приложения из корневой папки проекта эта папка используется в качестве корня содержимого.</span><span class="sxs-lookup"><span data-stu-id="53bb3-154">When the app is started from the project's root folder, the project's root folder is used as the content root.</span></span> <span data-ttu-id="53bb3-155">Такое поведение по умолчанию принято в [Visual Studio](https://visualstudio.microsoft.com) и [шаблонах dotnet new](/dotnet/core/tools/dotnet-new).</span><span class="sxs-lookup"><span data-stu-id="53bb3-155">This is the default used in [Visual Studio](https://visualstudio.microsoft.com) and the [dotnet new templates](/dotnet/core/tools/dotnet-new).</span></span>

<span data-ttu-id="53bb3-156">Дополнительные сведения о конфигурации приложения см. в разделе <xref:fundamentals/configuration/index>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-156">For more information on app configuration, see <xref:fundamentals/configuration/index>.</span></span>

> [!NOTE]
> <span data-ttu-id="53bb3-157">Помимо использования статического метода `CreateDefaultBuilder`, в ASP.NET Core 2.x поддерживается создание узла на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="53bb3-157">As an alternative to using the static `CreateDefaultBuilder` method, creating a host from [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) is a supported approach with ASP.NET Core 2.x.</span></span> <span data-ttu-id="53bb3-158">Дополнительные сведения см. на вкладке со сведениями об ASP.NET Core 1.x.</span><span class="sxs-lookup"><span data-stu-id="53bb3-158">For more information, see the ASP.NET Core 1.x tab.</span></span>

<span data-ttu-id="53bb3-159">При настройке узла можно предоставить методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) и [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1).</span><span class="sxs-lookup"><span data-stu-id="53bb3-159">When setting up a host, [Configure](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.configure?view=aspnetcore-1.1) and [ConfigureServices](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.configureservices?view=aspnetcore-1.1) methods can be provided.</span></span> <span data-ttu-id="53bb3-160">Если используется класс `Startup`, в нем должен быть определен метод `Configure`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-160">If a `Startup` class is specified, it must define a `Configure` method.</span></span> <span data-ttu-id="53bb3-161">Для получения дополнительной информации см. <xref:fundamentals/startup>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-161">For more information, see <xref:fundamentals/startup>.</span></span> <span data-ttu-id="53bb3-162">Несколько вызовов `ConfigureServices` добавляются друг к другу.</span><span class="sxs-lookup"><span data-stu-id="53bb3-162">Multiple calls to `ConfigureServices` append to one another.</span></span> <span data-ttu-id="53bb3-163">При нескольких вызовах `Configure` или `UseStartup` в `WebHostBuilder` предыдущие параметры заменяются.</span><span class="sxs-lookup"><span data-stu-id="53bb3-163">Multiple calls to `Configure` or `UseStartup` on the `WebHostBuilder` replace previous settings.</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="53bb3-164">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="53bb3-164">Host configuration values</span></span>

<span data-ttu-id="53bb3-165">Для задания значений конфигурации узла класс [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="53bb3-165">[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="53bb3-166">конфигурация построителя узла, которая включает в себя переменные среды в формате `ASPNETCORE_{configurationKey}`,</span><span class="sxs-lookup"><span data-stu-id="53bb3-166">Host builder configuration, which includes environment variables with the format `ASPNETCORE_{configurationKey}`.</span></span> <span data-ttu-id="53bb3-167">Например, `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-167">For example, `ASPNETCORE_ENVIRONMENT`.</span></span>
* <span data-ttu-id="53bb3-168">Расширения, такие как [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) и [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (см. раздел [Переопределение конфигурации](#override-configuration)).</span><span class="sxs-lookup"><span data-stu-id="53bb3-168">Extensions such as [UseContentRoot](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.usecontentroot) and [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) (see the [Override configuration](#override-configuration) section).</span></span>
* <span data-ttu-id="53bb3-169">метод [UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) и связанный ключ.</span><span class="sxs-lookup"><span data-stu-id="53bb3-169">[UseSetting](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder.usesetting) and the associated key.</span></span> <span data-ttu-id="53bb3-170">Значение, задаваемое с помощью `UseSetting`, всегда является строкой независимо от типа.</span><span class="sxs-lookup"><span data-stu-id="53bb3-170">When setting a value with `UseSetting`, the value is set as a string regardless of the type.</span></span>

<span data-ttu-id="53bb3-171">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="53bb3-171">The host uses whichever option sets a value last.</span></span> <span data-ttu-id="53bb3-172">Дополнительные сведения см. в подразделе [Переопределение конфигурации](#override-configuration) следующего раздела.</span><span class="sxs-lookup"><span data-stu-id="53bb3-172">For more information, see [Override configuration](#override-configuration) in the next section.</span></span>

### <a name="application-key-name"></a><span data-ttu-id="53bb3-173">Ключ приложения (имя)</span><span class="sxs-lookup"><span data-stu-id="53bb3-173">Application Key (Name)</span></span>

<span data-ttu-id="53bb3-174">Свойство [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) задается автоматически при вызове [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) или [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-174">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is automatically set when [UseStartup](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usestartup) or [Configure](/dotnet/api/microsoft.aspnetcore.hosting.istartup.configure) is called during host construction.</span></span> <span data-ttu-id="53bb3-175">Значение присваивается имени сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-175">The value is set to the name of the assembly containing the app's entry point.</span></span> <span data-ttu-id="53bb3-176">Чтобы явно задать значение, используйте [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey).</span><span class="sxs-lookup"><span data-stu-id="53bb3-176">To set the value explicitly, use the [WebHostDefaults.ApplicationKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.applicationkey):</span></span>

<span data-ttu-id="53bb3-177">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="53bb3-177">**Key**: applicationName</span></span>  
<span data-ttu-id="53bb3-178">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-178">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-179">**По умолчанию**: имя сборки, содержащей точку входа приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-179">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="53bb3-180">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="53bb3-180">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="53bb3-181">**Переменная среды**: `ASPNETCORE_APPLICATIONNAME`</span><span class="sxs-lookup"><span data-stu-id="53bb3-181">**Environment variable**: `ASPNETCORE_APPLICATIONNAME`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.ApplicationKey, "CustomApplicationName")
```

### <a name="capture-startup-errors"></a><span data-ttu-id="53bb3-182">Перехват ошибок при загрузке</span><span class="sxs-lookup"><span data-stu-id="53bb3-182">Capture Startup Errors</span></span>

<span data-ttu-id="53bb3-183">Этот параметр управляет перехватом ошибок при загрузке.</span><span class="sxs-lookup"><span data-stu-id="53bb3-183">This setting controls the capture of startup errors.</span></span>

<span data-ttu-id="53bb3-184">**Ключ**: captureStartupErrors</span><span class="sxs-lookup"><span data-stu-id="53bb3-184">**Key**: captureStartupErrors</span></span>  
<span data-ttu-id="53bb3-185">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="53bb3-185">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="53bb3-186">**По умолчанию**: `false`, если только приложение не работает с сервером Kestrel за службами IIS; в этом случае значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-186">**Default**: Defaults to `false` unless the app runs with Kestrel behind IIS, where the default is `true`.</span></span>  
<span data-ttu-id="53bb3-187">**Задается с помощью**: `CaptureStartupErrors`</span><span class="sxs-lookup"><span data-stu-id="53bb3-187">**Set using**: `CaptureStartupErrors`</span></span>  
<span data-ttu-id="53bb3-188">**Переменная среды**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span><span class="sxs-lookup"><span data-stu-id="53bb3-188">**Environment variable**: `ASPNETCORE_CAPTURESTARTUPERRORS`</span></span>

<span data-ttu-id="53bb3-189">Если задано значение `false`, ошибки во время запуска приводят к завершению работы узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-189">When `false`, errors during startup result in the host exiting.</span></span> <span data-ttu-id="53bb3-190">Если задано значение `true`, узел перехватывает исключения во время запуска и пытается запустить сервер.</span><span class="sxs-lookup"><span data-stu-id="53bb3-190">When `true`, the host captures exceptions during startup and attempts to start the server.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .CaptureStartupErrors(true)
```

### <a name="content-root"></a><span data-ttu-id="53bb3-191">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="53bb3-191">Content Root</span></span>

<span data-ttu-id="53bb3-192">Этот параметр определяет то, где ASP.NET Core начинает искать файлы содержимого, например представления MVC.</span><span class="sxs-lookup"><span data-stu-id="53bb3-192">This setting determines where ASP.NET Core begins searching for content files, such as MVC views.</span></span> 

<span data-ttu-id="53bb3-193">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="53bb3-193">**Key**: contentRoot</span></span>  
<span data-ttu-id="53bb3-194">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-194">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-195">**По умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-195">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="53bb3-196">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="53bb3-196">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="53bb3-197">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="53bb3-197">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="53bb3-198">Корень содержимого также используется в качестве базового пути для [корневого каталога документов](#web-root).</span><span class="sxs-lookup"><span data-stu-id="53bb3-198">The content root is also used as the base path for the [Web Root setting](#web-root).</span></span> <span data-ttu-id="53bb3-199">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="53bb3-199">If the path doesn't exist, the host fails to start.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseContentRoot("c:\\<content-root>")
```

### <a name="detailed-errors"></a><span data-ttu-id="53bb3-200">Подробные сообщения об ошибках</span><span class="sxs-lookup"><span data-stu-id="53bb3-200">Detailed Errors</span></span>

<span data-ttu-id="53bb3-201">Определяет, следует ли перехватывать подробные сообщения об ошибках.</span><span class="sxs-lookup"><span data-stu-id="53bb3-201">Determines if detailed errors should be captured.</span></span>

<span data-ttu-id="53bb3-202">**Ключ**: detailedErrors</span><span class="sxs-lookup"><span data-stu-id="53bb3-202">**Key**: detailedErrors</span></span>  
<span data-ttu-id="53bb3-203">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="53bb3-203">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="53bb3-204">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="53bb3-204">**Default**: false</span></span>  
<span data-ttu-id="53bb3-205">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="53bb3-205">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="53bb3-206">**Переменная среды**: `ASPNETCORE_DETAILEDERRORS`</span><span class="sxs-lookup"><span data-stu-id="53bb3-206">**Environment variable**: `ASPNETCORE_DETAILEDERRORS`</span></span>

<span data-ttu-id="53bb3-207">Если этот параметр включен (или если параметр <a href="#environment">Среда</a> имеет значение `Development`), приложение перехватывает подробные исключения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-207">When enabled (or when the <a href="#environment">Environment</a> is set to `Development`), the app captures detailed exceptions.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.DetailedErrorsKey, "true")
```

### <a name="environment"></a><span data-ttu-id="53bb3-208">Среда</span><span class="sxs-lookup"><span data-stu-id="53bb3-208">Environment</span></span>

<span data-ttu-id="53bb3-209">Задает среду приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-209">Sets the app's environment.</span></span>

<span data-ttu-id="53bb3-210">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="53bb3-210">**Key**: environment</span></span>  
<span data-ttu-id="53bb3-211">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-211">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-212">**По умолчанию**: Рабочие</span><span class="sxs-lookup"><span data-stu-id="53bb3-212">**Default**: Production</span></span>  
<span data-ttu-id="53bb3-213">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="53bb3-213">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="53bb3-214">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="53bb3-214">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="53bb3-215">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="53bb3-215">The environment can be set to any value.</span></span> <span data-ttu-id="53bb3-216">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-216">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="53bb3-217">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="53bb3-217">Values aren't case sensitive.</span></span> <span data-ttu-id="53bb3-218">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-218">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="53bb3-219">При использовании [Visual Studio](https://visualstudio.microsoft.com) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="53bb3-219">When using [Visual Studio](https://visualstudio.microsoft.com), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="53bb3-220">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-220">For more information, see <xref:fundamentals/environments>.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseEnvironment(EnvironmentName.Development)
```

### <a name="hosting-startup-assemblies"></a><span data-ttu-id="53bb3-221">Начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="53bb3-221">Hosting Startup Assemblies</span></span>

<span data-ttu-id="53bb3-222">Задает начальные сборки размещения для приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-222">Sets the app's hosting startup assemblies.</span></span>

<span data-ttu-id="53bb3-223">**Ключ**: hostingStartupAssemblies</span><span class="sxs-lookup"><span data-stu-id="53bb3-223">**Key**: hostingStartupAssemblies</span></span>  
<span data-ttu-id="53bb3-224">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-224">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-225">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="53bb3-225">**Default**: Empty string</span></span>  
<span data-ttu-id="53bb3-226">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="53bb3-226">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="53bb3-227">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="53bb3-227">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`</span></span>

<span data-ttu-id="53bb3-228">Разделенная точками с запятой строка начальных сборок размещения, загружаемых при запуске.</span><span class="sxs-lookup"><span data-stu-id="53bb3-228">A semicolon-delimited string of hosting startup assemblies to load on startup.</span></span>

<span data-ttu-id="53bb3-229">Хотя значением по умолчанию этого параметра конфигурации является пустая строка, начальные сборки размещения всегда включают в себя сборку приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-229">Although the configuration value defaults to an empty string, the hosting startup assemblies always include the app's assembly.</span></span> <span data-ttu-id="53bb3-230">Если начальные сборки размещения указаны, они добавляются к сборке приложения для загрузки во время построения приложением общих служб при запуске.</span><span class="sxs-lookup"><span data-stu-id="53bb3-230">When hosting startup assemblies are provided, they're added to the app's assembly for loading when the app builds its common services during startup.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupAssembliesKey, "assembly1;assembly2")
```

### <a name="https-port"></a><span data-ttu-id="53bb3-231">HTTPS-порт</span><span class="sxs-lookup"><span data-stu-id="53bb3-231">HTTPS Port</span></span>

<span data-ttu-id="53bb3-232">Задайте порт перенаправления HTTPS.</span><span class="sxs-lookup"><span data-stu-id="53bb3-232">Set the HTTPS redirect port.</span></span> <span data-ttu-id="53bb3-233">Используется при [принудительном применении HTTPS](xref:security/enforcing-ssl).</span><span class="sxs-lookup"><span data-stu-id="53bb3-233">Used in [enforcing HTTPS](xref:security/enforcing-ssl).</span></span>

<span data-ttu-id="53bb3-234">**Ключ**: https_port **Тип**: *string*
**По умолчанию**: значение по умолчанию не задано.</span><span class="sxs-lookup"><span data-stu-id="53bb3-234">**Key**: https_port **Type**: *string*
**Default**: A default value isn't set.</span></span>
<span data-ttu-id="53bb3-235">**Задается с помощью**: `UseSetting`
**переменной среды**: `ASPNETCORE_HTTPS_PORT`</span><span class="sxs-lookup"><span data-stu-id="53bb3-235">**Set using**: `UseSetting`
**Environment variable**: `ASPNETCORE_HTTPS_PORT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting("https_port", "8080")
```

### <a name="hosting-startup-exclude-assemblies"></a><span data-ttu-id="53bb3-236">Исключаемые начальные сборки размещения</span><span class="sxs-lookup"><span data-stu-id="53bb3-236">Hosting Startup Exclude Assemblies</span></span>

<span data-ttu-id="53bb3-237">Разделенная точками с запятой строка начальных сборок размещения, которые необходимо исключить при запуске.</span><span class="sxs-lookup"><span data-stu-id="53bb3-237">A semicolon-delimited string of hosting startup assemblies to exclude on startup.</span></span>

<span data-ttu-id="53bb3-238">**Ключ**: hostingStartupExcludeAssemblies</span><span class="sxs-lookup"><span data-stu-id="53bb3-238">**Key**: hostingStartupExcludeAssemblies</span></span>  
<span data-ttu-id="53bb3-239">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-239">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-240">**По умолчанию**: Пустая строка</span><span class="sxs-lookup"><span data-stu-id="53bb3-240">**Default**: Empty string</span></span>  
<span data-ttu-id="53bb3-241">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="53bb3-241">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="53bb3-242">**Переменная среды**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span><span class="sxs-lookup"><span data-stu-id="53bb3-242">**Environment variable**: `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.HostingStartupExcludeAssembliesKey, "assembly1;assembly2")
```

### <a name="prefer-hosting-urls"></a><span data-ttu-id="53bb3-243">Предпочитать URL-адреса размещения</span><span class="sxs-lookup"><span data-stu-id="53bb3-243">Prefer Hosting URLs</span></span>

<span data-ttu-id="53bb3-244">Указывает, должен ли узел ожидать передачи данных по URL-адресам, настроенным с помощью `WebHostBuilder`, вместо настроенных с помощью реализации `IServer`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-244">Indicates whether the host should listen on the URLs configured with the `WebHostBuilder` instead of those configured with the `IServer` implementation.</span></span>

<span data-ttu-id="53bb3-245">**Ключ**: preferHostingUrls</span><span class="sxs-lookup"><span data-stu-id="53bb3-245">**Key**: preferHostingUrls</span></span>  
<span data-ttu-id="53bb3-246">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="53bb3-246">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="53bb3-247">**Значение по умолчанию**: true</span><span class="sxs-lookup"><span data-stu-id="53bb3-247">**Default**: true</span></span>  
<span data-ttu-id="53bb3-248">**Задается с помощью**: `PreferHostingUrls`</span><span class="sxs-lookup"><span data-stu-id="53bb3-248">**Set using**: `PreferHostingUrls`</span></span>  
<span data-ttu-id="53bb3-249">**Переменная среды**: `ASPNETCORE_PREFERHOSTINGURLS`</span><span class="sxs-lookup"><span data-stu-id="53bb3-249">**Environment variable**: `ASPNETCORE_PREFERHOSTINGURLS`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .PreferHostingUrls(false)
```

### <a name="prevent-hosting-startup"></a><span data-ttu-id="53bb3-250">Запретить запуск размещения</span><span class="sxs-lookup"><span data-stu-id="53bb3-250">Prevent Hosting Startup</span></span>

<span data-ttu-id="53bb3-251">Запрещает автоматическую загрузку начальных сборок размещения, включая начальные сборки размещения, настроенные сборкой приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-251">Prevents the automatic loading of hosting startup assemblies, including hosting startup assemblies configured by the app's assembly.</span></span> <span data-ttu-id="53bb3-252">Для получения дополнительной информации см. <xref:fundamentals/configuration/platform-specific-configuration>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-252">For more information, see <xref:fundamentals/configuration/platform-specific-configuration>.</span></span>

<span data-ttu-id="53bb3-253">**Ключ**: preventHostingStartup</span><span class="sxs-lookup"><span data-stu-id="53bb3-253">**Key**: preventHostingStartup</span></span>  
<span data-ttu-id="53bb3-254">**Тип**: *bool* (`true` или `1`)</span><span class="sxs-lookup"><span data-stu-id="53bb3-254">**Type**: *bool* (`true` or `1`)</span></span>  
<span data-ttu-id="53bb3-255">**Значение по умолчанию**: false</span><span class="sxs-lookup"><span data-stu-id="53bb3-255">**Default**: false</span></span>  
<span data-ttu-id="53bb3-256">**Задается с помощью**: `UseSetting`</span><span class="sxs-lookup"><span data-stu-id="53bb3-256">**Set using**: `UseSetting`</span></span>  
<span data-ttu-id="53bb3-257">**Переменная среды**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span><span class="sxs-lookup"><span data-stu-id="53bb3-257">**Environment variable**: `ASPNETCORE_PREVENTHOSTINGSTARTUP`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseSetting(WebHostDefaults.PreventHostingStartupKey, "true")
```

### <a name="server-urls"></a><span data-ttu-id="53bb3-258">URL-адреса сервера</span><span class="sxs-lookup"><span data-stu-id="53bb3-258">Server URLs</span></span>

<span data-ttu-id="53bb3-259">Задает IP-адреса или адреса узлов с портами и протоколами, по которым сервер должен ожидать получения запросов.</span><span class="sxs-lookup"><span data-stu-id="53bb3-259">Indicates the IP addresses or host addresses with ports and protocols that the server should listen on for requests.</span></span>

<span data-ttu-id="53bb3-260">**Ключ**: urls</span><span class="sxs-lookup"><span data-stu-id="53bb3-260">**Key**: urls</span></span>  
<span data-ttu-id="53bb3-261">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-261">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-262">**По умолчанию**: http://localhost:5000</span><span class="sxs-lookup"><span data-stu-id="53bb3-262">**Default**: http://localhost:5000</span></span>  
<span data-ttu-id="53bb3-263">**Задается с помощью**: `UseUrls`</span><span class="sxs-lookup"><span data-stu-id="53bb3-263">**Set using**: `UseUrls`</span></span>  
<span data-ttu-id="53bb3-264">**Переменная среды**: `ASPNETCORE_URLS`</span><span class="sxs-lookup"><span data-stu-id="53bb3-264">**Environment variable**: `ASPNETCORE_URLS`</span></span>

<span data-ttu-id="53bb3-265">Укажите разделенный точками с запятой (;) список префиксов URL-адресов, на которые сервер должен отвечать.</span><span class="sxs-lookup"><span data-stu-id="53bb3-265">Set to a semicolon-separated (;) list of URL prefixes to which the server should respond.</span></span> <span data-ttu-id="53bb3-266">Например, `http://localhost:123`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-266">For example, `http://localhost:123`.</span></span> <span data-ttu-id="53bb3-267">Используйте символ "\*", чтобы указать, что сервер должен ожидать получения запросов через определенный порт и по определенному протоколу по любому IP-адресу или имени узла (например, `http://*:5000`).</span><span class="sxs-lookup"><span data-stu-id="53bb3-267">Use "\*" to indicate that the server should listen for requests on any IP address or hostname using the specified port and protocol (for example, `http://*:5000`).</span></span> <span data-ttu-id="53bb3-268">Протокол (`http://` или `https://`) должен указываться для каждого URL-адреса.</span><span class="sxs-lookup"><span data-stu-id="53bb3-268">The protocol (`http://` or `https://`) must be included with each URL.</span></span> <span data-ttu-id="53bb3-269">Поддерживаемые форматы зависят от сервера.</span><span class="sxs-lookup"><span data-stu-id="53bb3-269">Supported formats vary among servers.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseUrls("http://*:5000;http://localhost:5001;https://hostname:5002")
```

<span data-ttu-id="53bb3-270">Kestrel имеет собственный интерфейс API настройки конечных точек.</span><span class="sxs-lookup"><span data-stu-id="53bb3-270">Kestrel has its own endpoint configuration API.</span></span> <span data-ttu-id="53bb3-271">Для получения дополнительной информации см. <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-271">For more information, see <xref:fundamentals/servers/kestrel#endpoint-configuration>.</span></span>

### <a name="shutdown-timeout"></a><span data-ttu-id="53bb3-272">Время ожидания завершения работы</span><span class="sxs-lookup"><span data-stu-id="53bb3-272">Shutdown Timeout</span></span>

<span data-ttu-id="53bb3-273">Определяет, как долго необходимо ожидать завершения работы веб-узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-273">Specifies the amount of time to wait for Web Host to shut down.</span></span>

<span data-ttu-id="53bb3-274">**Ключ**: shutdownTimeoutSeconds</span><span class="sxs-lookup"><span data-stu-id="53bb3-274">**Key**: shutdownTimeoutSeconds</span></span>  
<span data-ttu-id="53bb3-275">**Тип**: *int*</span><span class="sxs-lookup"><span data-stu-id="53bb3-275">**Type**: *int*</span></span>  
<span data-ttu-id="53bb3-276">**По умолчанию**: 5</span><span class="sxs-lookup"><span data-stu-id="53bb3-276">**Default**: 5</span></span>  
<span data-ttu-id="53bb3-277">**Задается с помощью**: `UseShutdownTimeout`</span><span class="sxs-lookup"><span data-stu-id="53bb3-277">**Set using**: `UseShutdownTimeout`</span></span>  
<span data-ttu-id="53bb3-278">**Переменная среды**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span><span class="sxs-lookup"><span data-stu-id="53bb3-278">**Environment variable**: `ASPNETCORE_SHUTDOWNTIMEOUTSECONDS`</span></span>

<span data-ttu-id="53bb3-279">Хотя ключ принимает значение *int* с `UseSetting` (например, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), метод расширения [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) принимает [TimeSpan](/dotnet/api/system.timespan).</span><span class="sxs-lookup"><span data-stu-id="53bb3-279">Although the key accepts an *int* with `UseSetting` (for example, `.UseSetting(WebHostDefaults.ShutdownTimeoutKey, "10")`), the [UseShutdownTimeout](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useshutdowntimeout) extension method takes a [TimeSpan](/dotnet/api/system.timespan).</span></span>

<span data-ttu-id="53bb3-280">Во время ожидания размещение:</span><span class="sxs-lookup"><span data-stu-id="53bb3-280">During the timeout period, hosting:</span></span>

* <span data-ttu-id="53bb3-281">Активирует [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span><span class="sxs-lookup"><span data-stu-id="53bb3-281">Triggers [IApplicationLifetime.ApplicationStopping](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstopping).</span></span>
* <span data-ttu-id="53bb3-282">Пытается остановить размещенные службы, записывая в журнал все ошибки для служб, которые не удалось остановить.</span><span class="sxs-lookup"><span data-stu-id="53bb3-282">Attempts to stop hosted services, logging any errors for services that fail to stop.</span></span>

<span data-ttu-id="53bb3-283">Если время ожидания истекает до остановки всех размещенных служб, активные службы останавливаются при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-283">If the timeout period expires before all of the hosted services stop, any remaining active services are stopped when the app shuts down.</span></span> <span data-ttu-id="53bb3-284">Службы останавливаются даже в том случае, если еще не завершили обработку.</span><span class="sxs-lookup"><span data-stu-id="53bb3-284">The services stop even if they haven't finished processing.</span></span> <span data-ttu-id="53bb3-285">Если службе требуется дополнительное время для остановки, увеличьте время ожидания.</span><span class="sxs-lookup"><span data-stu-id="53bb3-285">If services require additional time to stop, increase the timeout.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseShutdownTimeout(TimeSpan.FromSeconds(10))
```

### <a name="startup-assembly"></a><span data-ttu-id="53bb3-286">Стартовая сборка</span><span class="sxs-lookup"><span data-stu-id="53bb3-286">Startup Assembly</span></span>

<span data-ttu-id="53bb3-287">Определяет сборку, в которой необходимо искать класс `Startup`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-287">Determines the assembly to search for the `Startup` class.</span></span>

<span data-ttu-id="53bb3-288">**Ключ**: startupAssembly</span><span class="sxs-lookup"><span data-stu-id="53bb3-288">**Key**: startupAssembly</span></span>  
<span data-ttu-id="53bb3-289">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-289">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-290">**По умолчанию**: сборка приложения</span><span class="sxs-lookup"><span data-stu-id="53bb3-290">**Default**: The app's assembly</span></span>  
<span data-ttu-id="53bb3-291">**Задается с помощью**: `UseStartup`</span><span class="sxs-lookup"><span data-stu-id="53bb3-291">**Set using**: `UseStartup`</span></span>  
<span data-ttu-id="53bb3-292">**Переменная среды**: `ASPNETCORE_STARTUPASSEMBLY`</span><span class="sxs-lookup"><span data-stu-id="53bb3-292">**Environment variable**: `ASPNETCORE_STARTUPASSEMBLY`</span></span>

<span data-ttu-id="53bb3-293">На сборку можно ссылаться по имени (`string`) или типу (`TStartup`).</span><span class="sxs-lookup"><span data-stu-id="53bb3-293">The assembly by name (`string`) or type (`TStartup`) can be referenced.</span></span> <span data-ttu-id="53bb3-294">При вызове нескольких методов `UseStartup` приоритет имеет последний.</span><span class="sxs-lookup"><span data-stu-id="53bb3-294">If multiple `UseStartup` methods are called, the last one takes precedence.</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup("StartupAssemblyName")
```

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseStartup<TStartup>()
```

### <a name="web-root"></a><span data-ttu-id="53bb3-295">Корневой каталог документов</span><span class="sxs-lookup"><span data-stu-id="53bb3-295">Web Root</span></span>

<span data-ttu-id="53bb3-296">Задает относительный путь к статическим ресурсам приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-296">Sets the relative path to the app's static assets.</span></span>

<span data-ttu-id="53bb3-297">**Ключ**: webroot</span><span class="sxs-lookup"><span data-stu-id="53bb3-297">**Key**: webroot</span></span>  
<span data-ttu-id="53bb3-298">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="53bb3-298">**Type**: *string*</span></span>  
<span data-ttu-id="53bb3-299">**По умолчанию**: если значение не задано, по умолчанию используется путь "(Корень содержимого)/wwwroot" при его наличии.</span><span class="sxs-lookup"><span data-stu-id="53bb3-299">**Default**: If not specified, the default is "(Content Root)/wwwroot", if the path exists.</span></span> <span data-ttu-id="53bb3-300">Если этот путь не существует, используется фиктивный поставщик файлов.</span><span class="sxs-lookup"><span data-stu-id="53bb3-300">If the path doesn't exist, then a no-op file provider is used.</span></span>  
<span data-ttu-id="53bb3-301">**Задается с помощью**: `UseWebRoot`</span><span class="sxs-lookup"><span data-stu-id="53bb3-301">**Set using**: `UseWebRoot`</span></span>  
<span data-ttu-id="53bb3-302">**Переменная среды**: `ASPNETCORE_WEBROOT`</span><span class="sxs-lookup"><span data-stu-id="53bb3-302">**Environment variable**: `ASPNETCORE_WEBROOT`</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseWebRoot("public")
```

## <a name="override-configuration"></a><span data-ttu-id="53bb3-303">Переопределение конфигурации</span><span class="sxs-lookup"><span data-stu-id="53bb3-303">Override configuration</span></span>

<span data-ttu-id="53bb3-304">Используйте [конфигурацию](xref:fundamentals/configuration/index) для настройки веб-узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-304">Use [Configuration](xref:fundamentals/configuration/index) to configure Web Host.</span></span> <span data-ttu-id="53bb3-305">В приведенном ниже примере необязательная конфигурация узла задается в файле *hostsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="53bb3-305">In the following example, host configuration is optionally specified in a *hostsettings.json* file.</span></span> <span data-ttu-id="53bb3-306">Любую конфигурацию, загружаемую из файла *hostsettings.json*, можно переопределить с помощью аргументов командной строки.</span><span class="sxs-lookup"><span data-stu-id="53bb3-306">Any configuration loaded from the *hostsettings.json* file may be overridden by command-line arguments.</span></span> <span data-ttu-id="53bb3-307">Встроенная конфигурация (в `config`) используется для настройки узла с помощью [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span><span class="sxs-lookup"><span data-stu-id="53bb3-307">The built configuration (in `config`) is used to configure the host with [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration).</span></span> <span data-ttu-id="53bb3-308">Конфигурация `IWebHostBuilder` добавляется в конфигурацию приложения, но не наоборот &mdash; `ConfigureAppConfiguration` не влияет на конфигурацию `IWebHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-308">`IWebHostBuilder` configuration is added to the app's configuration, but the converse isn't true&mdash;`ConfigureAppConfiguration` doesn't affect the `IWebHostBuilder` configuration.</span></span>

<span data-ttu-id="53bb3-309">Конфигурация, предоставленная методом `UseUrls`, сначала переопределяется конфигурацией из файла *hostsettings.json*, а затем с помощью аргументов командной строки:</span><span class="sxs-lookup"><span data-stu-id="53bb3-309">Overriding the configuration provided by `UseUrls` with *hostsettings.json* config first, command-line argument config second:</span></span>

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
            });
    }
}
```

<span data-ttu-id="53bb3-310">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="53bb3-310">*hostsettings.json*:</span></span>

```json
{
    urls: "http://*:5005"
}
```

> [!NOTE]
> <span data-ttu-id="53bb3-311">Метод расширения [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом `GetSection` (например, `.UseConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="53bb3-311">The [UseConfiguration](/dotnet/api/microsoft.aspnetcore.hosting.hostingabstractionswebhostbuilderextensions.useconfiguration) extension method isn't currently capable of parsing a configuration section returned by `GetSection` (for example, `.UseConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="53bb3-312">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:urls`, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="53bb3-312">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:urls`, `section:environment`).</span></span> <span data-ttu-id="53bb3-313">Метод `UseConfiguration` ожидает, что ключи соответствуют ключам `WebHostBuilder` (например, `urls`, `environment`).</span><span class="sxs-lookup"><span data-stu-id="53bb3-313">The `UseConfiguration` method expects the keys to match the `WebHostBuilder` keys (for example, `urls`, `environment`).</span></span> <span data-ttu-id="53bb3-314">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="53bb3-314">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="53bb3-315">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="53bb3-315">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="53bb3-316">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="53bb3-316">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>
>
> <span data-ttu-id="53bb3-317">`UseConfiguration` копирует ключи только из предоставленного объекта `IConfiguration` для конфигурации конструктора узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-317">`UseConfiguration` only copies keys from the provided `IConfiguration` to the host builder configuration.</span></span> <span data-ttu-id="53bb3-318">Поэтому указание `reloadOnChange: true` для файлов JSON, XML и INI ни на что не влияет.</span><span class="sxs-lookup"><span data-stu-id="53bb3-318">Therefore, setting `reloadOnChange: true` for JSON, INI, and XML settings files has no effect.</span></span>

<span data-ttu-id="53bb3-319">Чтобы указать узел, выполняющийся по определенному URL-адресу, можно передать нужное значение из командной строки при выполнении команды [dotnet run](/dotnet/core/tools/dotnet-run).</span><span class="sxs-lookup"><span data-stu-id="53bb3-319">To specify the host run on a particular URL, the desired value can be passed in from a command prompt when executing [dotnet run](/dotnet/core/tools/dotnet-run).</span></span> <span data-ttu-id="53bb3-320">Аргумент командной строки переопределяет значение `urls` из файла *hostsettings.json*, и сервер будет ожидать передачи данных через порт 8080:</span><span class="sxs-lookup"><span data-stu-id="53bb3-320">The command-line argument overrides the `urls` value from the *hostsettings.json* file, and the server listens on port 8080:</span></span>

```console
dotnet run --urls "http://*:8080"
```

## <a name="manage-the-host"></a><span data-ttu-id="53bb3-321">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="53bb3-321">Manage the host</span></span>

<span data-ttu-id="53bb3-322">**Выполнить**</span><span class="sxs-lookup"><span data-stu-id="53bb3-322">**Run**</span></span>

<span data-ttu-id="53bb3-323">Метод `Run` запускает веб-приложение и блокирует вызывающий поток до тех пор, пока работа узла не будет завершена.</span><span class="sxs-lookup"><span data-stu-id="53bb3-323">The `Run` method starts the web app and blocks the calling thread until the host is shut down:</span></span>

```csharp
host.Run();
```

<span data-ttu-id="53bb3-324">**Запуск**</span><span class="sxs-lookup"><span data-stu-id="53bb3-324">**Start**</span></span>

<span data-ttu-id="53bb3-325">Чтобы запустить узел без блокировки, вызовите метод `Start`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-325">Run the host in a non-blocking manner by calling its `Start` method:</span></span>

```csharp
using (host)
{
    host.Start();
    Console.ReadLine();
}
```

<span data-ttu-id="53bb3-326">Если в метод `Start` передается список URL-адресов, он будет ожидать передачи данных по указанным URL-адресам.</span><span class="sxs-lookup"><span data-stu-id="53bb3-326">If a list of URLs is passed to the `Start` method, it listens on the URLs specified:</span></span>

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

<span data-ttu-id="53bb3-327">Приложение может инициализировать и запустить новый узел с использованием предварительно настроенных значений по умолчанию `CreateDefaultBuilder` с помощью статического удобного метода.</span><span class="sxs-lookup"><span data-stu-id="53bb3-327">The app can initialize and start a new host using the pre-configured defaults of `CreateDefaultBuilder` using a static convenience method.</span></span> <span data-ttu-id="53bb3-328">Эти методы запускают сервер без вывода данных в консоль и со временем ожидания прерывания, равным [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) (Ctrl-C/SIGINT или SIGTERM):</span><span class="sxs-lookup"><span data-stu-id="53bb3-328">These methods start the server without console output and with [WaitForShutdown](/dotnet/api/microsoft.aspnetcore.hosting.webhostextensions.waitforshutdown) wait for a break (Ctrl-C/SIGINT or SIGTERM):</span></span>

<span data-ttu-id="53bb3-329">**Start(RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="53bb3-329">**Start(RequestDelegate app)**</span></span>

<span data-ttu-id="53bb3-330">Выполните запуск с помощью `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-330">Start with a `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start(app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="53bb3-331">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="53bb3-331">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="53bb3-332">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="53bb3-332">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="53bb3-333">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="53bb3-333">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="53bb3-334">**Start(string url, RequestDelegate app)**</span><span class="sxs-lookup"><span data-stu-id="53bb3-334">**Start(string url, RequestDelegate app)**</span></span>

<span data-ttu-id="53bb3-335">Выполните запуск с помощью URL-адреса и `RequestDelegate`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-335">Start with a URL and `RequestDelegate`:</span></span>

```csharp
using (var host = WebHost.Start("http://localhost:8080", app => app.Response.WriteAsync("Hello, World!")))
{
    Console.WriteLine("Use Ctrl-C to shutdown the host...");
    host.WaitForShutdown();
}
```

<span data-ttu-id="53bb3-336">Результат будет тем же, что и при использовании **Start(RequestDelegate app)**, но приложение отвечает по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-336">Produces the same result as **Start(RequestDelegate app)**, except the app responds on `http://localhost:8080`.</span></span>

<span data-ttu-id="53bb3-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="53bb3-337">**Start(Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="53bb3-338">Используйте экземпляр `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) для применения ПО промежуточного слоя маршрутизации:</span><span class="sxs-lookup"><span data-stu-id="53bb3-338">Use an instance of `IRouteBuilder` ([Microsoft.AspNetCore.Routing](https://www.nuget.org/packages/Microsoft.AspNetCore.Routing/)) to use routing middleware:</span></span>

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

<span data-ttu-id="53bb3-339">В этом примере используйте следующие запросы в браузере:</span><span class="sxs-lookup"><span data-stu-id="53bb3-339">Use the following browser requests with the example:</span></span>

| <span data-ttu-id="53bb3-340">Запрос</span><span class="sxs-lookup"><span data-stu-id="53bb3-340">Request</span></span>                                    | <span data-ttu-id="53bb3-341">Ответ</span><span class="sxs-lookup"><span data-stu-id="53bb3-341">Response</span></span>                                 |
| ------------------------------------------ | ---------------------------------------- |
| `http://localhost:5000/hello/Martin`       | <span data-ttu-id="53bb3-342">Hello, Martin!</span><span class="sxs-lookup"><span data-stu-id="53bb3-342">Hello, Martin!</span></span>                           |
| `http://localhost:5000/buenosdias/Catrina` | <span data-ttu-id="53bb3-343">Buenos dias, Catrina!</span><span class="sxs-lookup"><span data-stu-id="53bb3-343">Buenos dias, Catrina!</span></span>                    |
| `http://localhost:5000/throw/ooops!`       | <span data-ttu-id="53bb3-344">Вызывает исключение со строкой "ooops!"</span><span class="sxs-lookup"><span data-stu-id="53bb3-344">Throws an exception with string "ooops!"</span></span> |
| `http://localhost:5000/throw`              | <span data-ttu-id="53bb3-345">Вызывает исключение со строкой "Uh oh!"</span><span class="sxs-lookup"><span data-stu-id="53bb3-345">Throws an exception with string "Uh oh!"</span></span> |
| `http://localhost:5000/Sante/Kevin`        | <span data-ttu-id="53bb3-346">Sante, Kevin!</span><span class="sxs-lookup"><span data-stu-id="53bb3-346">Sante, Kevin!</span></span>                            |
| `http://localhost:5000`                    | <span data-ttu-id="53bb3-347">Пример "Здравствуй,</span><span class="sxs-lookup"><span data-stu-id="53bb3-347">Hello World!</span></span>                             |

<span data-ttu-id="53bb3-348">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="53bb3-348">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="53bb3-349">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="53bb3-349">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="53bb3-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span><span class="sxs-lookup"><span data-stu-id="53bb3-350">**Start(string url, Action&lt;IRouteBuilder&gt; routeBuilder)**</span></span>

<span data-ttu-id="53bb3-351">Используйте URL-адрес и экземпляр `IRouteBuilder`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-351">Use a URL and an instance of `IRouteBuilder`:</span></span>

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

<span data-ttu-id="53bb3-352">Результат будет тем же, что и при использовании **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-352">Produces the same result as **Start(Action&lt;IRouteBuilder&gt; routeBuilder)**, except the app responds at `http://localhost:8080`.</span></span>

<span data-ttu-id="53bb3-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="53bb3-353">**StartWith(Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="53bb3-354">Предоставьте делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-354">Provide a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="53bb3-355">В браузере выполните запрос по адресу `http://localhost:5000`, чтобы получить ответ "Hello World!"</span><span class="sxs-lookup"><span data-stu-id="53bb3-355">Make a request in the browser to `http://localhost:5000` to receive the response "Hello World!"</span></span> <span data-ttu-id="53bb3-356">`WaitForShutdown` блокируется, пока не будет создано прерывание (Ctrl-C/SIGINT или SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="53bb3-356">`WaitForShutdown` blocks until a break (Ctrl-C/SIGINT or SIGTERM) is issued.</span></span> <span data-ttu-id="53bb3-357">Приложение выводит сообщение `Console.WriteLine` и ожидает нажатия клавиши, после чего завершает работу.</span><span class="sxs-lookup"><span data-stu-id="53bb3-357">The app displays the `Console.WriteLine` message and waits for a keypress to exit.</span></span>

<span data-ttu-id="53bb3-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span><span class="sxs-lookup"><span data-stu-id="53bb3-358">**StartWith(string url, Action&lt;IApplicationBuilder&gt; app)**</span></span>

<span data-ttu-id="53bb3-359">Предоставьте URL-адрес и делегат для настройки `IApplicationBuilder`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-359">Provide a URL and a delegate to configure an `IApplicationBuilder`:</span></span>

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

<span data-ttu-id="53bb3-360">Результат будет тем же, что и при использовании **StartWith(Action&lt;IApplicationBuilder&gt; app)**, но приложение будет отвечать по адресу `http://localhost:8080`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-360">Produces the same result as **StartWith(Action&lt;IApplicationBuilder&gt; app)**, except the app responds on `http://localhost:8080`.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="53bb3-361">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="53bb3-361">IHostingEnvironment interface</span></span>

<span data-ttu-id="53bb3-362">[Интерфейс IHostingEnvironment](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) предоставляет сведения о среде веб-размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-362">The [IHostingEnvironment interface](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment) provides information about the app's web hosting environment.</span></span> <span data-ttu-id="53bb3-363">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="53bb3-363">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="53bb3-364">Для настройки приложения при запуске в соответствии со средой можно применять [подход на основе соглашения](xref:fundamentals/environments#environment-based-startup-class-and-methods).</span><span class="sxs-lookup"><span data-stu-id="53bb3-364">A [convention-based approach](xref:fundamentals/environments#environment-based-startup-class-and-methods) can be used to configure the app at startup based on the environment.</span></span> <span data-ttu-id="53bb3-365">Кроме того, можно внедрить интерфейс `IHostingEnvironment` в конструктор `Startup` для использования в `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-365">Alternatively, inject the `IHostingEnvironment` into the `Startup` constructor for use in `ConfigureServices`:</span></span>

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
> <span data-ttu-id="53bb3-366">Помимо метода расширения `IsDevelopment`, интерфейс `IHostingEnvironment` предоставляет методы `IsStaging`, `IsProduction` и `IsEnvironment(string environmentName)`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-366">In addition to the `IsDevelopment` extension method, `IHostingEnvironment` offers `IsStaging`, `IsProduction`, and `IsEnvironment(string environmentName)` methods.</span></span> <span data-ttu-id="53bb3-367">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="53bb3-367">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="53bb3-368">Службу `IHostingEnvironment` также можно внедрять непосредственно в метод `Configure` для настройки конвейера обработки:</span><span class="sxs-lookup"><span data-stu-id="53bb3-368">The `IHostingEnvironment` service can also be injected directly into the `Configure` method for setting up the processing pipeline:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
        // In Development, use the Developer Exception Page
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

<span data-ttu-id="53bb3-369">`IHostingEnvironment` можно внедрить в метод `Invoke` при создании пользовательского [ПО промежуточного слоя](xref:fundamentals/middleware/write):</span><span class="sxs-lookup"><span data-stu-id="53bb3-369">`IHostingEnvironment` can be injected into the `Invoke` method when creating custom [middleware](xref:fundamentals/middleware/write):</span></span>

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

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="53bb3-370">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="53bb3-370">IApplicationLifetime interface</span></span>

<span data-ttu-id="53bb3-371">[Интерфейс IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="53bb3-371">[IApplicationLifetime](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime) allows for post-startup and shutdown activities.</span></span> <span data-ttu-id="53bb3-372">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="53bb3-372">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="53bb3-373">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="53bb3-373">Cancellation Token</span></span>    | <span data-ttu-id="53bb3-374">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="53bb3-374">Triggered when&#8230;</span></span> |
| --------------------- | --------------------- |
| [<span data-ttu-id="53bb3-375">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="53bb3-375">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="53bb3-376">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="53bb3-376">The host has fully started.</span></span> |
| [<span data-ttu-id="53bb3-377">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="53bb3-377">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="53bb3-378">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-378">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="53bb3-379">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="53bb3-379">All requests should be processed.</span></span> <span data-ttu-id="53bb3-380">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="53bb3-380">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="53bb3-381">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="53bb3-381">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="53bb3-382">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="53bb3-382">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="53bb3-383">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="53bb3-383">Requests may still be processing.</span></span> <span data-ttu-id="53bb3-384">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="53bb3-384">Shutdown blocks until this event completes.</span></span> |

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

<span data-ttu-id="53bb3-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="53bb3-385">[StopApplication](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="53bb3-386">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="53bb3-386">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="scope-validation"></a><span data-ttu-id="53bb3-387">Проверка области</span><span class="sxs-lookup"><span data-stu-id="53bb3-387">Scope validation</span></span>

<span data-ttu-id="53bb3-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) устанавливает для [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) значение `true`, если приложение находится в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="53bb3-388">[CreateDefaultBuilder](/dotnet/api/microsoft.aspnetcore.webhost.createdefaultbuilder) sets [ServiceProviderOptions.ValidateScopes](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions.validatescopes) to `true` if the app's environment is Development.</span></span>

<span data-ttu-id="53bb3-389">Если для `ValidateScopes` установлено значение `true`, поставщик службы по умолчанию проверяет, что:</span><span class="sxs-lookup"><span data-stu-id="53bb3-389">When `ValidateScopes` is set to `true`, the default service provider performs checks to verify that:</span></span>

* <span data-ttu-id="53bb3-390">Службы с заданной областью не разрешаются из корневого поставщика службы, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="53bb3-390">Scoped services aren't directly or indirectly resolved from the root service provider.</span></span>
* <span data-ttu-id="53bb3-391">Службы с заданной областью не вводятся в одноэлементные объекты, прямо или косвенно.</span><span class="sxs-lookup"><span data-stu-id="53bb3-391">Scoped services aren't directly or indirectly injected into singletons.</span></span>

<span data-ttu-id="53bb3-392">Корневой поставщик службы создается при вызове [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider).</span><span class="sxs-lookup"><span data-stu-id="53bb3-392">The root service provider is created when [BuildServiceProvider](/dotnet/api/microsoft.extensions.dependencyinjection.servicecollectioncontainerbuilderextensions.buildserviceprovider) is called.</span></span> <span data-ttu-id="53bb3-393">Время существования корневого поставщика службы соответствует времени существования приложения или сервера — поставщик запускается с приложением и удаляется, когда приложение завершает работу.</span><span class="sxs-lookup"><span data-stu-id="53bb3-393">The root service provider's lifetime corresponds to the app/server's lifetime when the provider starts with the app and is disposed when the app shuts down.</span></span>

<span data-ttu-id="53bb3-394">Службы с заданной областью удаляются создавшим их контейнером.</span><span class="sxs-lookup"><span data-stu-id="53bb3-394">Scoped services are disposed by the container that created them.</span></span> <span data-ttu-id="53bb3-395">Если служба с заданной областью создается в корневом контейнере, время существования службы повышается до уровня одноэлементного объекта, поскольку она удаляется только корневым контейнером при завершении работы приложения или сервера.</span><span class="sxs-lookup"><span data-stu-id="53bb3-395">If a scoped service is created in the root container, the service's lifetime is effectively promoted to singleton because it's only disposed by the root container when app/server is shut down.</span></span> <span data-ttu-id="53bb3-396">Проверка областей службы перехватывает эти ситуации при вызове `BuildServiceProvider`.</span><span class="sxs-lookup"><span data-stu-id="53bb3-396">Validating service scopes catches these situations when `BuildServiceProvider` is called.</span></span>

<span data-ttu-id="53bb3-397">Чтобы всегда проверять области, в том числе в рабочей среде, настройте [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) с [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) в конструкторе узлов:</span><span class="sxs-lookup"><span data-stu-id="53bb3-397">To always validate scopes, including in the Production environment, configure the [ServiceProviderOptions](/dotnet/api/microsoft.extensions.dependencyinjection.serviceprovideroptions) with [UseDefaultServiceProvider](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilderextensions.usedefaultserviceprovider) on the host builder:</span></span>

```csharp
WebHost.CreateDefaultBuilder(args)
    .UseDefaultServiceProvider((context, options) => {
        options.ValidateScopes = true;
    })
```

## <a name="additional-resources"></a><span data-ttu-id="53bb3-398">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="53bb3-398">Additional resources</span></span>

* <xref:host-and-deploy/iis/index>
* <xref:host-and-deploy/linux-nginx>
* <xref:host-and-deploy/linux-apache>
* <xref:host-and-deploy/windows-service>
