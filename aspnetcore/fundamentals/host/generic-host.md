---
title: Универсальный узел .NET
author: guardrex
description: Сведения об универсальном узле в .NET, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 0924e2764958911dc1711d5427f6dd58e8873739
ms.sourcegitcommit: f5d403004f3550e8c46585fdbb16c49e75f495f3
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/20/2018
ms.locfileid: "49477609"
---
# <a name="net-generic-host"></a><span data-ttu-id="64119-103">Универсальный узел .NET</span><span class="sxs-lookup"><span data-stu-id="64119-103">.NET Generic Host</span></span>

<span data-ttu-id="64119-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="64119-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="64119-105">Приложения .NET Core настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="64119-105">.NET Core apps configure and launch a *host*.</span></span> <span data-ttu-id="64119-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="64119-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="64119-107">В этом разделе рассматриваются универсальный узел ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), используемый для размещения приложений, которые не умеют обрабатывать запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="64119-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="64119-108">Сведения о веб-узле ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) см. в статье <xref:fundamentals/host/web-host>.</span><span class="sxs-lookup"><span data-stu-id="64119-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see <xref:fundamentals/host/web-host>.</span></span>

<span data-ttu-id="64119-109">Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла.</span><span class="sxs-lookup"><span data-stu-id="64119-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="64119-110">Обмен сообщениями, фоновые задачи и другие рабочие нагрузки типа "не HTTP", использующие перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="64119-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="64119-111">Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением.</span><span class="sxs-lookup"><span data-stu-id="64119-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="64119-112">Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="64119-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="64119-113">Универсальный узел разрабатывается с целью заменить веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".</span><span class="sxs-lookup"><span data-stu-id="64119-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="64119-114">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="64119-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="64119-115">При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*.</span><span class="sxs-lookup"><span data-stu-id="64119-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="64119-116">Не выполняйте пример в `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="64119-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="64119-117">Настройка консоли в Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="64119-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="64119-118">Откройте файл *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="64119-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="64119-119">В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**.</span><span class="sxs-lookup"><span data-stu-id="64119-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="64119-120">Измените значение на `externalTerminal` или `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="64119-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="64119-121">Вступление</span><span class="sxs-lookup"><span data-stu-id="64119-121">Introduction</span></span>

<span data-ttu-id="64119-122">Библиотека универсального узла находится в [пространстве имен Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) и включена в [пакет Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="64119-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="64119-123">Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="64119-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="64119-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) — это точка входа для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="64119-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="64119-125">Каждая реализация `IHostedService` выполняется в порядке [регистрации служб в ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="64119-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="64119-126">Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) вызывается в каждом `IHostedService` при запуске узла, а метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.</span><span class="sxs-lookup"><span data-stu-id="64119-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="64119-127">Создание узла</span><span class="sxs-lookup"><span data-stu-id="64119-127">Set up a host</span></span>

<span data-ttu-id="64119-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:</span><span class="sxs-lookup"><span data-stu-id="64119-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="default-services"></a><span data-ttu-id="64119-129">Службы по умолчанию</span><span class="sxs-lookup"><span data-stu-id="64119-129">Default services</span></span>

<span data-ttu-id="64119-130">Во время инициализации узла регистрируются следующие службы:</span><span class="sxs-lookup"><span data-stu-id="64119-130">The following services are registered during host initialization:</span></span>

* <span data-ttu-id="64119-131">[Окружение](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span><span class="sxs-lookup"><span data-stu-id="64119-131">[Environment](xref:fundamentals/environments) (<xref:Microsoft.Extensions.Hosting.IHostingEnvironment>)</span></span>
* <xref:Microsoft.Extensions.Hosting.HostBuilderContext>
* <span data-ttu-id="64119-132">[Конфигурация](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span><span class="sxs-lookup"><span data-stu-id="64119-132">[Configuration](xref:fundamentals/configuration/index) (<xref:Microsoft.Extensions.Configuration.IConfiguration>)</span></span>
* <span data-ttu-id="64119-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span><span class="sxs-lookup"><span data-stu-id="64119-133"><xref:Microsoft.Extensions.Hosting.IApplicationLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ApplicationLifetime>)</span></span>
* <span data-ttu-id="64119-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span><span class="sxs-lookup"><span data-stu-id="64119-134"><xref:Microsoft.Extensions.Hosting.IHostLifetime> (<xref:Microsoft.Extensions.Hosting.Internal.ConsoleLifetime>)</span></span>
* <xref:Microsoft.Extensions.Hosting.IHost>
* <span data-ttu-id="64119-135">[Параметры](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span><span class="sxs-lookup"><span data-stu-id="64119-135">[Options](xref:fundamentals/configuration/options) (<xref:Microsoft.Extensions.DependencyInjection.OptionsServiceCollectionExtensions.AddOptions*>)</span></span>
* <span data-ttu-id="64119-136">[Ведение журнала](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span><span class="sxs-lookup"><span data-stu-id="64119-136">[Logging](xref:fundamentals/logging/index) (<xref:Microsoft.Extensions.DependencyInjection.LoggingServiceCollectionExtensions.AddLogging*>)</span></span>

## <a name="host-configuration"></a><span data-ttu-id="64119-137">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="64119-137">Host configuration</span></span>

<span data-ttu-id="64119-138">Для задания значений конфигурации узла класс [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="64119-138">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="64119-139">Построитель конфигурации</span><span class="sxs-lookup"><span data-stu-id="64119-139">Configuration builder</span></span>
* <span data-ttu-id="64119-140">Настройка метода расширения</span><span class="sxs-lookup"><span data-stu-id="64119-140">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="64119-141">Построитель конфигурации</span><span class="sxs-lookup"><span data-stu-id="64119-141">Configuration builder</span></span>

<span data-ttu-id="64119-142">Конфигурация построителя узла создается путем вызова метода [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="64119-142">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="64119-143">Метод `ConfigureHostConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для узла.</span><span class="sxs-lookup"><span data-stu-id="64119-143">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="64119-144">Построитель конфигурации инициализирует [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) для использования в процессе построения приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-144">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="64119-145">Конфигурация переменной среды не добавляется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="64119-145">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="64119-146">Вызовите [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в конструкторе узла, чтобы настроить узел из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="64119-146">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="64119-147">`AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс.</span><span class="sxs-lookup"><span data-stu-id="64119-147">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="64119-148">В примере приложения используется префикс `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="64119-148">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="64119-149">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="64119-149">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="64119-150">После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="64119-150">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="64119-151">Во время разработки с использованием [Visual Studio](https://www.visualstudio.com/) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="64119-151">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="64119-152">В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки.</span><span class="sxs-lookup"><span data-stu-id="64119-152">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="64119-153">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="64119-153">For more information, see <xref:fundamentals/environments>.</span></span>

<span data-ttu-id="64119-154">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="64119-154">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="64119-155">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="64119-155">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="64119-156">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="64119-156">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="64119-157">Пример конфигурации `HostBuilder` с использованием `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="64119-157">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="64119-158">Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="64119-158">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="64119-159">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="64119-159">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="64119-160">Метод `AddConfiguration` ожидает, что ключи соответствуют ключам `HostBuilder` (например, `environment`).</span><span class="sxs-lookup"><span data-stu-id="64119-160">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="64119-161">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="64119-161">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="64119-162">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="64119-162">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="64119-163">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="64119-163">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="64119-164">Настройка метода расширения</span><span class="sxs-lookup"><span data-stu-id="64119-164">Extension method configuration</span></span>

<span data-ttu-id="64119-165">Методы расширения вызываются в реализации `IHostBuilder`, чтобы настроить корень содержимого и среду.</span><span class="sxs-lookup"><span data-stu-id="64119-165">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="application-key-name"></a><span data-ttu-id="64119-166">Ключ приложения (имя)</span><span class="sxs-lookup"><span data-stu-id="64119-166">Application Key (Name)</span></span>

<span data-ttu-id="64119-167">Свойство [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) задается в конфигурации узла во время создания узла.</span><span class="sxs-lookup"><span data-stu-id="64119-167">The [IHostingEnvironment.ApplicationName](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment.applicationname) property is set from host configuration during host construction.</span></span> <span data-ttu-id="64119-168">Чтобы явно задать значение, используйте [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey).</span><span class="sxs-lookup"><span data-stu-id="64119-168">To set the value explicitly, use the [HostDefaults.ApplicationKey](/dotnet/api/microsoft.extensions.hosting.hostdefaults.applicationkey):</span></span>

<span data-ttu-id="64119-169">**Ключ**: applicationName</span><span class="sxs-lookup"><span data-stu-id="64119-169">**Key**: applicationName</span></span>  
<span data-ttu-id="64119-170">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="64119-170">**Type**: *string*</span></span>  
<span data-ttu-id="64119-171">**По умолчанию**: имя сборки, содержащей точку входа приложения</span><span class="sxs-lookup"><span data-stu-id="64119-171">**Default**: The name of the assembly containing the app's entry point.</span></span>  
<span data-ttu-id="64119-172">**Задается с помощью**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span><span class="sxs-lookup"><span data-stu-id="64119-172">**Set using**: `HostBuilderContext.HostingEnvironment.ApplicationName`</span></span>  
<span data-ttu-id="64119-173">**Переменная среды**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="64119-173">**Environment variable**: `<PREFIX_>APPLICATIONNAME` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

```csharp
var host = new HostBuilder()
    .ConfigureAppConfiguration((hostContext, configApp) =>
    {
        hostContext.HostingEnvironment.ApplicationName = "CustomApplicationName";
    })
```

#### <a name="content-root"></a><span data-ttu-id="64119-174">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="64119-174">Content Root</span></span>

<span data-ttu-id="64119-175">Этот параметр определяет, где узел начинает искать файлы содержимого.</span><span class="sxs-lookup"><span data-stu-id="64119-175">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="64119-176">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="64119-176">**Key**: contentRoot</span></span>  
<span data-ttu-id="64119-177">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="64119-177">**Type**: *string*</span></span>  
<span data-ttu-id="64119-178">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-178">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="64119-179">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="64119-179">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="64119-180">**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="64119-180">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="64119-181">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="64119-181">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="64119-182">Среда</span><span class="sxs-lookup"><span data-stu-id="64119-182">Environment</span></span>

<span data-ttu-id="64119-183">Задает [среду](xref:fundamentals/environments) приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-183">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="64119-184">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="64119-184">**Key**: environment</span></span>  
<span data-ttu-id="64119-185">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="64119-185">**Type**: *string*</span></span>  
<span data-ttu-id="64119-186">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="64119-186">**Default**: Production</span></span>  
<span data-ttu-id="64119-187">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="64119-187">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="64119-188">**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="64119-188">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="64119-189">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="64119-189">The environment can be set to any value.</span></span> <span data-ttu-id="64119-190">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="64119-190">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="64119-191">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="64119-191">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="64119-192">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="64119-192">ConfigureAppConfiguration</span></span>

<span data-ttu-id="64119-193">Конфигурация построителя приложения создается путем вызова метода [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="64119-193">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="64119-194">Метод `ConfigureAppConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-194">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="64119-195">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="64119-195">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="64119-196">Приложение использует то значение, которое задано последним.</span><span class="sxs-lookup"><span data-stu-id="64119-196">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="64119-197">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) для последующих операций и в [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="64119-197">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="64119-198">Пример конфигурации приложения с использованием `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="64119-198">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="64119-199">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="64119-199">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="64119-200">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="64119-200">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="64119-201">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="64119-201">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="64119-202">Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="64119-202">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="64119-203">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="64119-203">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="64119-204">Метод `AddConfiguration` ожидает точное соответствие ключей конфигурации (например, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="64119-204">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="64119-205">Наличие имени раздела в ключах предотвращает настройку приложения с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="64119-205">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="64119-206">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="64119-206">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="64119-207">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="64119-207">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

<span data-ttu-id="64119-208">Чтобы переместить файлы параметров в выходной каталог, укажите файлы параметров как [элементы проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items) в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="64119-208">To move settings files to the output directory, specify the settings files as [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) in the project file.</span></span> <span data-ttu-id="64119-209">Пример приложения перемещает свои файлы параметров приложения JSON и *hostsettings.json* со следующим элементом **&lt;Content:&gt;**.</span><span class="sxs-lookup"><span data-stu-id="64119-209">The sample app moves its JSON app settings files and *hostsettings.json* with the following **&lt;Content:&gt;** item:</span></span>

```xml
<ItemGroup>
  <Content Include="**\*.json" CopyToOutputDirectory="PreserveNewest" />
</ItemGroup>
```

## <a name="configureservices"></a><span data-ttu-id="64119-210">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="64119-210">ConfigureServices</span></span>

<span data-ttu-id="64119-211">Метод [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-211">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="64119-212">Метод `ConfigureServices` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="64119-212">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="64119-213">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="64119-213">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="64119-214">Для получения дополнительной информации см. <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="64119-214">For more information, see <xref:fundamentals/host/hosted-services>.</span></span>

<span data-ttu-id="64119-215">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="64119-215">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="64119-216">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="64119-216">ConfigureLogging</span></span>

<span data-ttu-id="64119-217">Метод [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) добавляет делегата для настройки указанного [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="64119-217">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="64119-218">Метод `ConfigureLogging` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="64119-218">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="64119-219">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="64119-219">UseConsoleLifetime</span></span>

<span data-ttu-id="64119-220">Метод [UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) прослушивает сигналы `Ctrl+C`/SIGINT или SIGTERM и вызывает метод [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="64119-220">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="64119-221">Метод `UseConsoleLifetime` разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="64119-221">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="64119-222">Класс [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) предварительно зарегистрирован и является реализацией времени жизни по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="64119-222">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="64119-223">Используется то время жизни, которое зарегистрировано последним.</span><span class="sxs-lookup"><span data-stu-id="64119-223">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="64119-224">Конфигурация контейнера</span><span class="sxs-lookup"><span data-stu-id="64119-224">Container configuration</span></span>

<span data-ttu-id="64119-225">Для поддержки подключения к другим контейнерам узел может принимать [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="64119-225">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="64119-226">Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="64119-226">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="64119-227">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-227">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="64119-228">Пользовательская конфигурация контейнера управляется методом [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="64119-228">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="64119-229">Метод `ConfigureContainer` обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла.</span><span class="sxs-lookup"><span data-stu-id="64119-229">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="64119-230">Метод `ConfigureContainer` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="64119-230">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="64119-231">Создание контейнера службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="64119-231">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="64119-232">Настройка фабрики контейнера службы:</span><span class="sxs-lookup"><span data-stu-id="64119-232">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="64119-233">Использование фабрики и настройка пользовательского контейнера служб для приложения:</span><span class="sxs-lookup"><span data-stu-id="64119-233">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="64119-234">Расширяемость</span><span class="sxs-lookup"><span data-stu-id="64119-234">Extensibility</span></span>

<span data-ttu-id="64119-235">Расширяемость узла выполняется с помощью методов расширения класса `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="64119-235">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="64119-236">В следующем примере показано, как метод расширения расширяет реализацию класса `IHostBuilder` с помощью класса [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks), пример которого приведен в статье <xref:fundamentals/host/hosted-services>.</span><span class="sxs-lookup"><span data-stu-id="64119-236">The following example shows how an extension method extends an `IHostBuilder` implementation with the [TimedHostedService](xref:fundamentals/host/hosted-services#timed-background-tasks) example demonstrated in <xref:fundamentals/host/hosted-services>.</span></span>

```csharp
var host = new HostBuilder()
    .UseHostedService<TimedHostedService>()
    .Build();

await host.StartAsync();
```

<span data-ttu-id="64119-237">Приложение устанавливает метод расширения `UseHostedService`, чтобы зарегистрировать размещенную службу, переданную в `T`:</span><span class="sxs-lookup"><span data-stu-id="64119-237">An app establishes the `UseHostedService` extension method to register the hosted service passed in `T`:</span></span>

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

## <a name="manage-the-host"></a><span data-ttu-id="64119-238">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="64119-238">Manage the host</span></span>

<span data-ttu-id="64119-239">Реализация [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) отвечает за запуск и остановку реализаций `IHostedService`, которые зарегистрированы в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="64119-239">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="64119-240">Запуск</span><span class="sxs-lookup"><span data-stu-id="64119-240">Run</span></span>

<span data-ttu-id="64119-241">Метод [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:</span><span class="sxs-lookup"><span data-stu-id="64119-241">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="64119-242">RunAsync</span><span class="sxs-lookup"><span data-stu-id="64119-242">RunAsync</span></span>

<span data-ttu-id="64119-243">Метод [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) запускает приложение и возвращает объект `Task`, который завершается при активации токена отмены или завершение работы:</span><span class="sxs-lookup"><span data-stu-id="64119-243">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="64119-244">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="64119-244">RunConsoleAsync</span></span>

<span data-ttu-id="64119-245">Метод [RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) включает поддержку консоли, собирает и запускает узел и ожидает сигналы `Ctrl+C`/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="64119-245">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="64119-246">Start и StopAsync</span><span class="sxs-lookup"><span data-stu-id="64119-246">Start and StopAsync</span></span>

<span data-ttu-id="64119-247">Метод [Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="64119-247">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="64119-248">Метод [StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="64119-248">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="64119-249">StartAsync и StopAsync</span><span class="sxs-lookup"><span data-stu-id="64119-249">StartAsync and StopAsync</span></span>

<span data-ttu-id="64119-250">Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="64119-250">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="64119-251">Метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) завершает работу приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-251">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="64119-252">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="64119-252">WaitForShutdown</span></span>

<span data-ttu-id="64119-253">Метод [WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) инициируется через [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), например через [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (прослушивает сигналы `Ctrl+C`/SIGINT и SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="64119-253">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="64119-254">Метод `WaitForShutdown` вызывает [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="64119-254">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="64119-255">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="64119-255">WaitForShutdownAsync</span></span>

<span data-ttu-id="64119-256">Метод [WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) возвращает объект `Task`, который завершается после активации завершения работы через токен, и вызывает метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="64119-256">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="64119-257">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="64119-257">External control</span></span>

<span data-ttu-id="64119-258">Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="64119-258">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="64119-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) вызывается в начале [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="64119-259">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="64119-260">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="64119-260">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="64119-261">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="64119-261">IHostingEnvironment interface</span></span>

<span data-ttu-id="64119-262">Интерфейс [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) предоставляет сведения о среде размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-262">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="64119-263">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="64119-263">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="64119-264">Для получения дополнительной информации см. <xref:fundamentals/environments>.</span><span class="sxs-lookup"><span data-stu-id="64119-264">For more information, see <xref:fundamentals/environments>.</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="64119-265">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="64119-265">IApplicationLifetime interface</span></span>

<span data-ttu-id="64119-266">Интерфейс [IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы.</span><span class="sxs-lookup"><span data-stu-id="64119-266">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="64119-267">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="64119-267">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="64119-268">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="64119-268">Cancellation Token</span></span> | <span data-ttu-id="64119-269">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="64119-269">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="64119-270">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="64119-270">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="64119-271">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="64119-271">The host has fully started.</span></span> |
| [<span data-ttu-id="64119-272">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="64119-272">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="64119-273">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="64119-273">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="64119-274">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="64119-274">All requests should be processed.</span></span> <span data-ttu-id="64119-275">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="64119-275">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="64119-276">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="64119-276">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="64119-277">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="64119-277">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="64119-278">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="64119-278">Requests may still be processing.</span></span> <span data-ttu-id="64119-279">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="64119-279">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="64119-280">Конструктор внедряет службу `IApplicationLifetime` в любой класс.</span><span class="sxs-lookup"><span data-stu-id="64119-280">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="64119-281">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию `IHostedService`) для регистрации событий.</span><span class="sxs-lookup"><span data-stu-id="64119-281">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses constructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="64119-282">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="64119-282">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="64119-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="64119-283">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="64119-284">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="64119-284">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="64119-285">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="64119-285">Additional resources</span></span>

* <xref:fundamentals/host/hosted-services>
* [<span data-ttu-id="64119-286">Репозиторий примеров размещения на GitHub</span><span class="sxs-lookup"><span data-stu-id="64119-286">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
