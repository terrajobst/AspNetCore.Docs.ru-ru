---
title: Универсальный узел .NET
author: guardrex
description: Сведения об универсальном узле в .NET, который отвечает за запуск приложений и управление временем существования.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/generic-host
ms.openlocfilehash: a851f2faf13792b2c232c124371d07710ae1fce3
ms.sourcegitcommit: 726ffab258070b4fe6cf950bf030ce10c0c07bb4
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2018
ms.locfileid: "34734475"
---
# <a name="net-generic-host"></a><span data-ttu-id="364af-103">Универсальный узел .NET</span><span class="sxs-lookup"><span data-stu-id="364af-103">.NET Generic Host</span></span>

<span data-ttu-id="364af-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="364af-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="364af-105">Приложения .NET настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="364af-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="364af-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="364af-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="364af-107">В этом разделе рассматриваются универсальный узел ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), используемый для размещения приложений, которые не умеют обрабатывать запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="364af-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="364af-108">Сведения о веб-узле ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="364af-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="364af-109">Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла.</span><span class="sxs-lookup"><span data-stu-id="364af-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="364af-110">Обмен сообщениями, фоновые задачи и другие рабочие нагрузки типа "не HTTP", использующие перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="364af-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="364af-111">Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением.</span><span class="sxs-lookup"><span data-stu-id="364af-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="364af-112">Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="364af-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="364af-113">Универсальный узел разрабатывается с целью заменить веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".</span><span class="sxs-lookup"><span data-stu-id="364af-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="364af-114">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="364af-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="364af-115">При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*.</span><span class="sxs-lookup"><span data-stu-id="364af-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="364af-116">Не выполняйте пример в `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="364af-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="364af-117">Настройка консоли в Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="364af-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="364af-118">Откройте файл *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="364af-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="364af-119">В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**.</span><span class="sxs-lookup"><span data-stu-id="364af-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="364af-120">Измените значение на `externalTerminal` или `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="364af-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="364af-121">Вступление</span><span class="sxs-lookup"><span data-stu-id="364af-121">Introduction</span></span>

<span data-ttu-id="364af-122">Библиотека универсального узла находится в [пространстве имен Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) и включена в [пакет Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="364af-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="364af-123">Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="364af-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="364af-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) — это точка входа для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="364af-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="364af-125">Каждая реализация `IHostedService` выполняется в порядке [регистрации служб в ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="364af-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="364af-126">Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) вызывается в каждом `IHostedService` при запуске узла, а метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.</span><span class="sxs-lookup"><span data-stu-id="364af-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="364af-127">Создание узла</span><span class="sxs-lookup"><span data-stu-id="364af-127">Set up a host</span></span>

<span data-ttu-id="364af-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:</span><span class="sxs-lookup"><span data-stu-id="364af-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="364af-129">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="364af-129">Host configuration</span></span>

<span data-ttu-id="364af-130">Для задания значений конфигурации узла класс [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="364af-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="364af-131">Построитель конфигурации</span><span class="sxs-lookup"><span data-stu-id="364af-131">Configuration builder</span></span>
* <span data-ttu-id="364af-132">Настройка метода расширения</span><span class="sxs-lookup"><span data-stu-id="364af-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="364af-133">Построитель конфигурации</span><span class="sxs-lookup"><span data-stu-id="364af-133">Configuration builder</span></span>

<span data-ttu-id="364af-134">Конфигурация построителя узла создается путем вызова метода [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="364af-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="364af-135">Метод `ConfigureHostConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для узла.</span><span class="sxs-lookup"><span data-stu-id="364af-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="364af-136">Построитель конфигурации инициализирует [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) для использования в процессе построения приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span> <span data-ttu-id="364af-137">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="364af-137">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="364af-138">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="364af-138">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="364af-139">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="364af-139">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="364af-140">Пример конфигурации `HostBuilder` с использованием `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="364af-140">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="364af-141">Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="364af-141">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="364af-142">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="364af-142">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="364af-143">Метод `AddConfiguration` ожидает, что ключи соответствуют ключам `HostBuilder` (например, `environment`).</span><span class="sxs-lookup"><span data-stu-id="364af-143">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="364af-144">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="364af-144">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="364af-145">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="364af-145">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="364af-146">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="364af-146">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="364af-147">Настройка метода расширения</span><span class="sxs-lookup"><span data-stu-id="364af-147">Extension method configuration</span></span>

<span data-ttu-id="364af-148">Методы расширения вызываются в реализации `IHostBuilder`, чтобы настроить корень содержимого и среду.</span><span class="sxs-lookup"><span data-stu-id="364af-148">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="364af-149">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="364af-149">Content Root</span></span>

<span data-ttu-id="364af-150">Этот параметр определяет, где узел начинает искать файлы содержимого.</span><span class="sxs-lookup"><span data-stu-id="364af-150">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="364af-151">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="364af-151">**Key**: contentRoot</span></span>  
<span data-ttu-id="364af-152">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="364af-152">**Type**: *string*</span></span>  
<span data-ttu-id="364af-153">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-153">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="364af-154">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="364af-154">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="364af-155">**Переменная среды**: `ASPNETCORE_CONTENTROOT`</span><span class="sxs-lookup"><span data-stu-id="364af-155">**Environment variable**: `ASPNETCORE_CONTENTROOT`</span></span>

<span data-ttu-id="364af-156">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="364af-156">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="364af-157">Среда</span><span class="sxs-lookup"><span data-stu-id="364af-157">Environment</span></span>

<span data-ttu-id="364af-158">Задает [среду](xref:fundamentals/environments) приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-158">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="364af-159">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="364af-159">**Key**: environment</span></span>  
<span data-ttu-id="364af-160">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="364af-160">**Type**: *string*</span></span>  
<span data-ttu-id="364af-161">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="364af-161">**Default**: Production</span></span>  
<span data-ttu-id="364af-162">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="364af-162">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="364af-163">**Переменная среды**: `ASPNETCORE_ENVIRONMENT`</span><span class="sxs-lookup"><span data-stu-id="364af-163">**Environment variable**: `ASPNETCORE_ENVIRONMENT`</span></span>

<span data-ttu-id="364af-164">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="364af-164">The environment can be set to any value.</span></span> <span data-ttu-id="364af-165">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="364af-165">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="364af-166">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="364af-166">Values aren't case sensitive.</span></span> <span data-ttu-id="364af-167">По умолчанию значение параметра *Среда* считывается из переменной среды `ASPNETCORE_ENVIRONMENT`.</span><span class="sxs-lookup"><span data-stu-id="364af-167">By default, the *Environment* is read from the `ASPNETCORE_ENVIRONMENT` environment variable.</span></span> <span data-ttu-id="364af-168">При использовании [Visual Studio](https://www.visualstudio.com/) переменные среды можно задавать в файле *launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="364af-168">When using [Visual Studio](https://www.visualstudio.com/), environment variables may be set in the *launchSettings.json* file.</span></span> <span data-ttu-id="364af-169">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="364af-169">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="364af-170">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="364af-170">ConfigureAppConfiguration</span></span>

<span data-ttu-id="364af-171">Конфигурация построителя приложения создается путем вызова метода [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="364af-171">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="364af-172">Метод `ConfigureAppConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-172">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="364af-173">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="364af-173">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="364af-174">Приложение использует то значение, которое задано последним.</span><span class="sxs-lookup"><span data-stu-id="364af-174">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="364af-175">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) для последующих операций и в [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="364af-175">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="364af-176">Пример конфигурации приложения с использованием `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="364af-176">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="364af-177">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="364af-177">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="364af-178">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="364af-178">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="364af-179">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="364af-179">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="364af-180">Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="364af-180">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="364af-181">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="364af-181">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="364af-182">Метод `AddConfiguration` ожидает точное соответствие ключей конфигурации (например, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="364af-182">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="364af-183">Наличие имени раздела в ключах предотвращает настройку приложения с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="364af-183">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="364af-184">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="364af-184">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="364af-185">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="364af-185">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="364af-186">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="364af-186">ConfigureServices</span></span>

<span data-ttu-id="364af-187">Метод [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-187">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="364af-188">Метод `ConfigureServices` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="364af-188">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="364af-189">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="364af-189">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="364af-190">Дополнительные сведения см. в статье [Background tasks with hosted services in ASP.NET Core](xref:fundamentals/host/hosted-services) (Фоновые задачи с размещенными службами в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="364af-190">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="364af-191">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="364af-191">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="364af-192">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="364af-192">ConfigureLogging</span></span>

<span data-ttu-id="364af-193">Метод [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) добавляет делегата для настройки указанного [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="364af-193">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="364af-194">Метод `ConfigureLogging` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="364af-194">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="364af-195">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="364af-195">UseConsoleLifetime</span></span>

<span data-ttu-id="364af-196">Метод [UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) прослушивает сигналы `Ctrl+C`/SIGINT или SIGTERM и вызывает метод [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="364af-196">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="364af-197">Метод `UseConsoleLifetime` разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="364af-197">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="364af-198">Класс [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) предварительно зарегистрирован и является реализацией времени жизни по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="364af-198">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="364af-199">Используется то время жизни, которое зарегистрировано последним.</span><span class="sxs-lookup"><span data-stu-id="364af-199">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="364af-200">Конфигурация контейнера</span><span class="sxs-lookup"><span data-stu-id="364af-200">Container configuration</span></span>

<span data-ttu-id="364af-201">Для поддержки подключения к другим контейнерам узел может принимать [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="364af-201">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="364af-202">Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="364af-202">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="364af-203">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-203">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="364af-204">Пользовательская конфигурация контейнера управляется методом [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="364af-204">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="364af-205">Метод `ConfigureContainer` обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла.</span><span class="sxs-lookup"><span data-stu-id="364af-205">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="364af-206">Метод `ConfigureContainer` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="364af-206">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="364af-207">Создание контейнера службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="364af-207">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="364af-208">Настройка фабрики контейнера службы:</span><span class="sxs-lookup"><span data-stu-id="364af-208">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="364af-209">Использование фабрики и настройка пользовательского контейнера служб для приложения:</span><span class="sxs-lookup"><span data-stu-id="364af-209">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="364af-210">Расширение среды</span><span class="sxs-lookup"><span data-stu-id="364af-210">Extensibility</span></span>

<span data-ttu-id="364af-211">Расширяемость узла выполняется с помощью методов расширения класса `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="364af-211">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="364af-212">В следующем примере показано, как метод расширения расширяет реализацию класса `IHostBuilder` с помощью [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="364af-212">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="364af-213">Этот метод расширения (в любом месте приложения) регистрирует службу `IHostedService` RabbitMQ:</span><span class="sxs-lookup"><span data-stu-id="364af-213">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="364af-214">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="364af-214">Manage the host</span></span>

<span data-ttu-id="364af-215">Реализация [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) отвечает за запуск и остановку реализаций `IHostedService`, которые зарегистрированы в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="364af-215">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="364af-216">Выполнить</span><span class="sxs-lookup"><span data-stu-id="364af-216">Run</span></span>

<span data-ttu-id="364af-217">Метод [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:</span><span class="sxs-lookup"><span data-stu-id="364af-217">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="364af-218">RunAsync</span><span class="sxs-lookup"><span data-stu-id="364af-218">RunAsync</span></span>

<span data-ttu-id="364af-219">Метод [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) запускает приложение и возвращает объект `Task`, который завершается при активации токена отмены или завершение работы:</span><span class="sxs-lookup"><span data-stu-id="364af-219">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="364af-220">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="364af-220">RunConsoleAsync</span></span>

<span data-ttu-id="364af-221">Метод [RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) включает поддержку консоли, собирает и запускает узел и ожидает сигналы `Ctrl+C`/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="364af-221">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="364af-222">Start и StopAsync</span><span class="sxs-lookup"><span data-stu-id="364af-222">Start and StopAsync</span></span>

<span data-ttu-id="364af-223">Метод [Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="364af-223">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="364af-224">Метод [StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="364af-224">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="364af-225">StartAsync и StopAsync</span><span class="sxs-lookup"><span data-stu-id="364af-225">StartAsync and StopAsync</span></span>

<span data-ttu-id="364af-226">Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="364af-226">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="364af-227">Метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) завершает работу приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-227">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="364af-228">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="364af-228">WaitForShutdown</span></span>

<span data-ttu-id="364af-229">Метод [WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) инициируется через [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), например через [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (прослушивает сигналы `Ctrl+C`/SIGINT и SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="364af-229">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="364af-230">Метод `WaitForShutdown` вызывает [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="364af-230">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="364af-231">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="364af-231">WaitForShutdownAsync</span></span>

<span data-ttu-id="364af-232">Метод [WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) возвращает объект `Task`, который завершается после активации завершения работы через токен, и вызывает метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="364af-232">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="364af-233">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="364af-233">External control</span></span>

<span data-ttu-id="364af-234">Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="364af-234">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="364af-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) вызывается в начале [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="364af-235">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="364af-236">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="364af-236">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="364af-237">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="364af-237">IHostingEnvironment interface</span></span>

<span data-ttu-id="364af-238">Интерфейс [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) предоставляет сведения о среде размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-238">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="364af-239">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="364af-239">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="364af-240">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="364af-240">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="364af-241">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="364af-241">IApplicationLifetime interface</span></span>

<span data-ttu-id="364af-242">Интерфейс [IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы.</span><span class="sxs-lookup"><span data-stu-id="364af-242">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="364af-243">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="364af-243">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="364af-244">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="364af-244">Cancellation Token</span></span> | <span data-ttu-id="364af-245">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="364af-245">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="364af-246">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="364af-246">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="364af-247">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="364af-247">The host has fully started.</span></span> |
| [<span data-ttu-id="364af-248">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="364af-248">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="364af-249">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="364af-249">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="364af-250">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="364af-250">All requests should be processed.</span></span> <span data-ttu-id="364af-251">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="364af-251">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="364af-252">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="364af-252">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="364af-253">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="364af-253">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="364af-254">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="364af-254">Requests may still be processing.</span></span> <span data-ttu-id="364af-255">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="364af-255">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="364af-256">Конструктор внедряет службу `IApplicationLifetime` в любой класс.</span><span class="sxs-lookup"><span data-stu-id="364af-256">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="364af-257">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию `IHostedService`) для регистрации событий.</span><span class="sxs-lookup"><span data-stu-id="364af-257">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="364af-258">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="364af-258">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="364af-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="364af-259">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="364af-260">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="364af-260">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="364af-261">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="364af-261">Additional resources</span></span>

* [<span data-ttu-id="364af-262">Фоновые задачи с размещенными службами</span><span class="sxs-lookup"><span data-stu-id="364af-262">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="364af-263">Репозиторий примеров размещения на GitHub</span><span class="sxs-lookup"><span data-stu-id="364af-263">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
