---
title: Универсальный узел .NET
author: guardrex
description: Сведения об универсальном узле в .NET, который отвечает за запуск приложений и управление временем существования.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/generic-host
ms.openlocfilehash: 33e5829ce4a09e132743b4174a588cf232a44775
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276268"
---
# <a name="net-generic-host"></a><span data-ttu-id="d089c-103">Универсальный узел .NET</span><span class="sxs-lookup"><span data-stu-id="d089c-103">.NET Generic Host</span></span>

<span data-ttu-id="d089c-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="d089c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d089c-105">Приложения .NET настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="d089c-105">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="d089c-106">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="d089c-106">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="d089c-107">В этом разделе рассматриваются универсальный узел ASP.NET Core ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), используемый для размещения приложений, которые не умеют обрабатывать запросы HTTP.</span><span class="sxs-lookup"><span data-stu-id="d089c-107">This topic covers the ASP.NET Core Generic Host ([HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder)), which is useful for hosting apps that don't process HTTP requests.</span></span> <span data-ttu-id="d089c-108">Сведения о веб-узле ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)) см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="d089c-108">For coverage of the Web Host ([WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder)), see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>

<span data-ttu-id="d089c-109">Универсальный узел предназначен для отделения конвейера HTTP от API веб-узла, чтобы можно было иметь больше сценариев узла.</span><span class="sxs-lookup"><span data-stu-id="d089c-109">The goal of the Generic Host is to decouple the HTTP pipeline from the Web Host API to enable a wider array of host scenarios.</span></span> <span data-ttu-id="d089c-110">Обмен сообщениями, фоновые задачи и другие рабочие нагрузки типа "не HTTP", использующие перекрестные возможности универсальных узлов, такие как конфигурация, внедрение зависимости (DI) и ведения журналов.</span><span class="sxs-lookup"><span data-stu-id="d089c-110">Messaging, background tasks, and other non-HTTP workloads based on the Generic Host benefit from cross-cutting capabilities, such as configuration, dependency injection (DI), and logging.</span></span>

<span data-ttu-id="d089c-111">Универсальный узел является новой возможностью ASP.NET Core 2.1 и не подходит для сценариев с веб-размещением.</span><span class="sxs-lookup"><span data-stu-id="d089c-111">The Generic Host is new in ASP.NET Core 2.1 and isn't suitable for web hosting scenarios.</span></span> <span data-ttu-id="d089c-112">Сведения о сценариях с веб-размещением см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="d089c-112">For web hosting scenarios, use the [Web Host](xref:fundamentals/host/web-host).</span></span> <span data-ttu-id="d089c-113">Универсальный узел разрабатывается с целью заменить веб-узел в будущих версиях. Он будет выступать основным узлом API для сценариев HTTP и "не HTTP".</span><span class="sxs-lookup"><span data-stu-id="d089c-113">The Generic Host is under development to replace the Web Host in a future release and act as the primary host API in both HTTP and non-HTTP scenarios.</span></span>

<span data-ttu-id="d089c-114">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d089c-114">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d089c-115">При выполнении примера приложения в [Visual Studio Code](https://code.visualstudio.com/) используйте *внешний или встроенный терминал*.</span><span class="sxs-lookup"><span data-stu-id="d089c-115">When running the sample app in [Visual Studio Code](https://code.visualstudio.com/), use an *external or integrated terminal*.</span></span> <span data-ttu-id="d089c-116">Не выполняйте пример в `internalConsole`.</span><span class="sxs-lookup"><span data-stu-id="d089c-116">Don't run the sample in an `internalConsole`.</span></span>

<span data-ttu-id="d089c-117">Настройка консоли в Visual Studio Code:</span><span class="sxs-lookup"><span data-stu-id="d089c-117">To set the console in Visual Studio Code:</span></span>

1. <span data-ttu-id="d089c-118">Откройте файл *.vscode/launch.json*.</span><span class="sxs-lookup"><span data-stu-id="d089c-118">Open the *.vscode/launch.json* file.</span></span>
1. <span data-ttu-id="d089c-119">В разделе конфигурации **.NET Core Launch (console)** найдите параметр **console**.</span><span class="sxs-lookup"><span data-stu-id="d089c-119">In the **.NET Core Launch (console)** configuration, locate the **console** entry.</span></span> <span data-ttu-id="d089c-120">Измените значение на `externalTerminal` или `integratedTerminal`.</span><span class="sxs-lookup"><span data-stu-id="d089c-120">Set the value to either `externalTerminal` or `integratedTerminal`.</span></span>

## <a name="introduction"></a><span data-ttu-id="d089c-121">Вступление</span><span class="sxs-lookup"><span data-stu-id="d089c-121">Introduction</span></span>

<span data-ttu-id="d089c-122">Библиотека универсального узла находится в [пространстве имен Microsoft.Extensions.Hosting](/dotnet/api/microsoft.extensions.hosting) и включена в [пакет Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/).</span><span class="sxs-lookup"><span data-stu-id="d089c-122">The Generic Host library is available in the [Microsoft.Extensions.Hosting namespace](/dotnet/api/microsoft.extensions.hosting) and provided by the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting/) package.</span></span> <span data-ttu-id="d089c-123">Пакет `Microsoft.Extensions.Hosting` входит в состав [метапакета Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 или более поздней версии).</span><span class="sxs-lookup"><span data-stu-id="d089c-123">The `Microsoft.Extensions.Hosting` package is included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) (ASP.NET Core 2.1 or later).</span></span>

<span data-ttu-id="d089c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) — это точка входа для выполнения кода.</span><span class="sxs-lookup"><span data-stu-id="d089c-124">[IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) is the entry point to code execution.</span></span> <span data-ttu-id="d089c-125">Каждая реализация `IHostedService` выполняется в порядке [регистрации служб в ConfigureServices](#configureservices).</span><span class="sxs-lookup"><span data-stu-id="d089c-125">Each `IHostedService` implementation is executed in the order of [service registration in ConfigureServices](#configureservices).</span></span> <span data-ttu-id="d089c-126">Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) вызывается в каждом `IHostedService` при запуске узла, а метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) вызывается в порядке, обратном регистрации, когда узел корректно завершает работу.</span><span class="sxs-lookup"><span data-stu-id="d089c-126">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) is called on each `IHostedService` when the host starts, and [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) is called in reverse registration order when the host shuts down gracefully.</span></span>

## <a name="set-up-a-host"></a><span data-ttu-id="d089c-127">Создание узла</span><span class="sxs-lookup"><span data-stu-id="d089c-127">Set up a host</span></span>

<span data-ttu-id="d089c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) является основным компонентом, который библиотеки и приложения используют для инициализации, построения и запуска узла:</span><span class="sxs-lookup"><span data-stu-id="d089c-128">[IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) is the main component that libraries and apps use to initialize, build, and run the host:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_HostBuilder)]

## <a name="host-configuration"></a><span data-ttu-id="d089c-129">Конфигурация узла</span><span class="sxs-lookup"><span data-stu-id="d089c-129">Host configuration</span></span>

<span data-ttu-id="d089c-130">Для задания значений конфигурации узла класс [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) поддерживает следующие подходы:</span><span class="sxs-lookup"><span data-stu-id="d089c-130">[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) relies on the following approaches to set the host configuration values:</span></span>

* <span data-ttu-id="d089c-131">Построитель конфигурации</span><span class="sxs-lookup"><span data-stu-id="d089c-131">Configuration builder</span></span>
* <span data-ttu-id="d089c-132">Настройка метода расширения</span><span class="sxs-lookup"><span data-stu-id="d089c-132">Extension method configuration</span></span>

### <a name="configuration-builder"></a><span data-ttu-id="d089c-133">Построитель конфигурации</span><span class="sxs-lookup"><span data-stu-id="d089c-133">Configuration builder</span></span>

<span data-ttu-id="d089c-134">Конфигурация построителя узла создается путем вызова метода [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d089c-134">Host builder configuration is created by calling [ConfigureHostConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configurehostconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="d089c-135">Метод `ConfigureHostConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для узла.</span><span class="sxs-lookup"><span data-stu-id="d089c-135">`ConfigureHostConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the host.</span></span> <span data-ttu-id="d089c-136">Построитель конфигурации инициализирует [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) для использования в процессе построения приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-136">The configuration builder initializes the [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) for use in the app's build process.</span></span>

<span data-ttu-id="d089c-137">Конфигурация переменной среды не добавляется по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d089c-137">Environment variable configuration isn't added by default.</span></span> <span data-ttu-id="d089c-138">Вызовите [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) в конструкторе узла, чтобы настроить узел из переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d089c-138">Call [AddEnvironmentVariables](/dotnet/api/microsoft.extensions.configuration.environmentvariablesextensions.addenvironmentvariables) on the host builder to configure the host from environment variables.</span></span> <span data-ttu-id="d089c-139">`AddEnvironmentVariables` принимает необязательный определяемый пользователем префикс.</span><span class="sxs-lookup"><span data-stu-id="d089c-139">`AddEnvironmentVariables` accepts an optional user-defined prefix.</span></span> <span data-ttu-id="d089c-140">В примере приложения используется префикс `PREFIX_`.</span><span class="sxs-lookup"><span data-stu-id="d089c-140">The sample app uses a prefix of `PREFIX_`.</span></span> <span data-ttu-id="d089c-141">Префикс удаляется при чтении переменных среды.</span><span class="sxs-lookup"><span data-stu-id="d089c-141">The prefix is removed when the environment variables are read.</span></span> <span data-ttu-id="d089c-142">После настройки узла в примере приложения значение переменной среды для `PREFIX_ENVIRONMENT` становится значением конфигурации узла для ключа `environment`.</span><span class="sxs-lookup"><span data-stu-id="d089c-142">When the sample app's host is configured, the environment variable value for `PREFIX_ENVIRONMENT` becomes the host configuration value for the `environment` key.</span></span>

<span data-ttu-id="d089c-143">Во время разработки с использованием [Visual Studio](https://www.visualstudio.com/) или запуска приложения с помощью `dotnet run` переменные среды можно задать в файле *Properties/launchSettings.json*.</span><span class="sxs-lookup"><span data-stu-id="d089c-143">During development when using [Visual Studio](https://www.visualstudio.com/) or running an app with `dotnet run`, environment variables may be set in the *Properties/launchSettings.json* file.</span></span> <span data-ttu-id="d089c-144">В [Visual Studio Code](https://code.visualstudio.com/) переменные среды можно задавать в файле *.vscode/launch.json* во время разработки.</span><span class="sxs-lookup"><span data-stu-id="d089c-144">In [Visual Studio Code](https://code.visualstudio.com/), environment variables may be set in the *.vscode/launch.json* file during development.</span></span> <span data-ttu-id="d089c-145">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d089c-145">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="d089c-146">Метод `ConfigureHostConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="d089c-146">`ConfigureHostConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="d089c-147">Хост использует значение, заданное последним.</span><span class="sxs-lookup"><span data-stu-id="d089c-147">The host uses whichever option sets a value last.</span></span>

<span data-ttu-id="d089c-148">*hostsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d089c-148">*hostsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/hostsettings.json)]

<span data-ttu-id="d089c-149">Пример конфигурации `HostBuilder` с использованием `ConfigureHostConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d089c-149">Example `HostBuilder` configuration using `ConfigureHostConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureHostConfiguration)]

> [!NOTE]
> <span data-ttu-id="d089c-150">Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="d089c-150">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d089c-151">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:environment`).</span><span class="sxs-lookup"><span data-stu-id="d089c-151">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:environment`).</span></span> <span data-ttu-id="d089c-152">Метод `AddConfiguration` ожидает, что ключи соответствуют ключам `HostBuilder` (например, `environment`).</span><span class="sxs-lookup"><span data-stu-id="d089c-152">The `AddConfiguration` method expects the keys to match the `HostBuilder` keys (for example, `environment`).</span></span> <span data-ttu-id="d089c-153">Наличие имени раздела в ключах предотвращает настройку узла с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="d089c-153">The presence of the section name on the keys prevents the section's values from configuring the host.</span></span> <span data-ttu-id="d089c-154">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="d089c-154">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d089c-155">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="d089c-155">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

### <a name="extension-method-configuration"></a><span data-ttu-id="d089c-156">Настройка метода расширения</span><span class="sxs-lookup"><span data-stu-id="d089c-156">Extension method configuration</span></span>

<span data-ttu-id="d089c-157">Методы расширения вызываются в реализации `IHostBuilder`, чтобы настроить корень содержимого и среду.</span><span class="sxs-lookup"><span data-stu-id="d089c-157">Extension methods are called on the `IHostBuilder` implementation to configure the content root and the environment.</span></span>

#### <a name="content-root"></a><span data-ttu-id="d089c-158">Корень содержимого</span><span class="sxs-lookup"><span data-stu-id="d089c-158">Content Root</span></span>

<span data-ttu-id="d089c-159">Этот параметр определяет, где узел начинает искать файлы содержимого.</span><span class="sxs-lookup"><span data-stu-id="d089c-159">This setting determines where the host begins searching for content files.</span></span>

<span data-ttu-id="d089c-160">**Ключ**: contentRoot</span><span class="sxs-lookup"><span data-stu-id="d089c-160">**Key**: contentRoot</span></span>  
<span data-ttu-id="d089c-161">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d089c-161">**Type**: *string*</span></span>  
<span data-ttu-id="d089c-162">**Значение по умолчанию**: папка, в которой находится сборка приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-162">**Default**: Defaults to the folder where the app assembly resides.</span></span>  
<span data-ttu-id="d089c-163">**Задается с помощью**: `UseContentRoot`</span><span class="sxs-lookup"><span data-stu-id="d089c-163">**Set using**: `UseContentRoot`</span></span>  
<span data-ttu-id="d089c-164">**Переменная среды**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="d089c-164">**Environment variable**: `<PREFIX_>CONTENTROOT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="d089c-165">Если путь не существует, узел не запускается.</span><span class="sxs-lookup"><span data-stu-id="d089c-165">If the path doesn't exist, the host fails to start.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseContentRoot)]

#### <a name="environment"></a><span data-ttu-id="d089c-166">Среда</span><span class="sxs-lookup"><span data-stu-id="d089c-166">Environment</span></span>

<span data-ttu-id="d089c-167">Задает [среду](xref:fundamentals/environments) приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-167">Sets the app's [environment](xref:fundamentals/environments).</span></span>

<span data-ttu-id="d089c-168">**Ключ**: environment</span><span class="sxs-lookup"><span data-stu-id="d089c-168">**Key**: environment</span></span>  
<span data-ttu-id="d089c-169">**Тип**: *string*</span><span class="sxs-lookup"><span data-stu-id="d089c-169">**Type**: *string*</span></span>  
<span data-ttu-id="d089c-170">**Значение по умолчанию**: Production</span><span class="sxs-lookup"><span data-stu-id="d089c-170">**Default**: Production</span></span>  
<span data-ttu-id="d089c-171">**Задается с помощью**: `UseEnvironment`</span><span class="sxs-lookup"><span data-stu-id="d089c-171">**Set using**: `UseEnvironment`</span></span>  
<span data-ttu-id="d089c-172">**Переменная среды**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` [необязательно и определяется пользователем](#configuration-builder))</span><span class="sxs-lookup"><span data-stu-id="d089c-172">**Environment variable**: `<PREFIX_>ENVIRONMENT` (`<PREFIX_>` is [optional and user-defined](#configuration-builder))</span></span>

<span data-ttu-id="d089c-173">В качестве среды можно указать любое значение.</span><span class="sxs-lookup"><span data-stu-id="d089c-173">The environment can be set to any value.</span></span> <span data-ttu-id="d089c-174">В платформе определены значения `Development`, `Staging` и `Production`.</span><span class="sxs-lookup"><span data-stu-id="d089c-174">Framework-defined values include `Development`, `Staging`, and `Production`.</span></span> <span data-ttu-id="d089c-175">Регистр символов в значениях не учитывается.</span><span class="sxs-lookup"><span data-stu-id="d089c-175">Values aren't case sensitive.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseEnvironment)]

## <a name="configureappconfiguration"></a><span data-ttu-id="d089c-176">ConfigureAppConfiguration</span><span class="sxs-lookup"><span data-stu-id="d089c-176">ConfigureAppConfiguration</span></span>

<span data-ttu-id="d089c-177">Конфигурация построителя приложения создается путем вызова метода [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) в реализации [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder).</span><span class="sxs-lookup"><span data-stu-id="d089c-177">App builder configuration is created by calling [ConfigureAppConfiguration](/dotnet/api/microsoft.extensions.hosting.ihostbuilder.configureappconfiguration) on the [IHostBuilder](/dotnet/api/microsoft.extensions.hosting.ihostbuilder) implementation.</span></span> <span data-ttu-id="d089c-178">Метод `ConfigureAppConfiguration` использует [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder), чтобы создать экземпляр [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) для приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-178">`ConfigureAppConfiguration` uses an [IConfigurationBuilder](/dotnet/api/microsoft.extensions.configuration.iconfigurationbuilder) to create an [IConfiguration](/dotnet/api/microsoft.extensions.configuration.iconfiguration) for the app.</span></span> <span data-ttu-id="d089c-179">Метод `ConfigureAppConfiguration` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="d089c-179">`ConfigureAppConfiguration` can be called multiple times with additive results.</span></span> <span data-ttu-id="d089c-180">Приложение использует то значение, которое задано последним.</span><span class="sxs-lookup"><span data-stu-id="d089c-180">The app uses whichever option sets a value last.</span></span> <span data-ttu-id="d089c-181">Конфигурация, созданная с помощью `ConfigureAppConfiguration`, доступна в свойствах [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) для последующих операций и в [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span><span class="sxs-lookup"><span data-stu-id="d089c-181">The configuration created by `ConfigureAppConfiguration` is available at [HostBuilderContext.Configuration](/dotnet/api/microsoft.extensions.hosting.hostbuildercontext.configuration) for subsequent operations and in [Services](/dotnet/api/microsoft.extensions.hosting.ihost.services).</span></span>

<span data-ttu-id="d089c-182">Пример конфигурации приложения с использованием `ConfigureAppConfiguration`:</span><span class="sxs-lookup"><span data-stu-id="d089c-182">Example app configuration using `ConfigureAppConfiguration`:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureAppConfiguration)]

<span data-ttu-id="d089c-183">*appsettings.json*:</span><span class="sxs-lookup"><span data-stu-id="d089c-183">*appsettings.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.json)]

<span data-ttu-id="d089c-184">*appsettings.Development.json*:</span><span class="sxs-lookup"><span data-stu-id="d089c-184">*appsettings.Development.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Development.json)]

<span data-ttu-id="d089c-185">*appsettings.Production.json*:</span><span class="sxs-lookup"><span data-stu-id="d089c-185">*appsettings.Production.json*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/appsettings.Production.json)]

> [!NOTE]
> <span data-ttu-id="d089c-186">Метод расширения [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) в настоящее время не может анализировать раздел конфигурации, возвращаемый методом [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (например, `.AddConfiguration(Configuration.GetSection("section"))`).</span><span class="sxs-lookup"><span data-stu-id="d089c-186">The [AddConfiguration](/dotnet/api/microsoft.extensions.configuration.chainedbuilderextensions.addconfiguration) extension method isn't currently capable of parsing a configuration section returned by [GetSection](/dotnet/api/microsoft.extensions.configuration.iconfiguration.getsection) (for example, `.AddConfiguration(Configuration.GetSection("section"))`.</span></span> <span data-ttu-id="d089c-187">Метод `GetSection` фильтрует ключи конфигурации до запрошенного раздела, но оставляет имя раздела в ключах (например, `section:Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="d089c-187">The `GetSection` method filters the configuration keys to the section requested but leaves the section name on the keys (for example, `section:Logging:LogLevel:Default`).</span></span> <span data-ttu-id="d089c-188">Метод `AddConfiguration` ожидает точное соответствие ключей конфигурации (например, `Logging:LogLevel:Default`).</span><span class="sxs-lookup"><span data-stu-id="d089c-188">The `AddConfiguration` method expects an exact match to configuration keys (for example, `Logging:LogLevel:Default`).</span></span> <span data-ttu-id="d089c-189">Наличие имени раздела в ключах предотвращает настройку приложения с помощью значений этого раздела.</span><span class="sxs-lookup"><span data-stu-id="d089c-189">The presence of the section name on the keys prevents the section's values from configuring the app.</span></span> <span data-ttu-id="d089c-190">Эта проблема будет устранена в следующем выпуске.</span><span class="sxs-lookup"><span data-stu-id="d089c-190">This issue will be addressed in an upcoming release.</span></span> <span data-ttu-id="d089c-191">Дополнительные сведения и способы решения см. в описании проблемы [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839) (При передаче раздела конфигурации в WebHostBuilder.UseConfiguration используются полные ключи).</span><span class="sxs-lookup"><span data-stu-id="d089c-191">For more information and workarounds, see [Passing configuration section into WebHostBuilder.UseConfiguration uses full keys](https://github.com/aspnet/Hosting/issues/839).</span></span>

## <a name="configureservices"></a><span data-ttu-id="d089c-192">ConfigureServices</span><span class="sxs-lookup"><span data-stu-id="d089c-192">ConfigureServices</span></span>

<span data-ttu-id="d089c-193">Метод [ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) добавляет службы в контейнер [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-193">[ConfigureServices](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configureservices) adds services to the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="d089c-194">Метод `ConfigureServices` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="d089c-194">`ConfigureServices` can be called multiple times with additive results.</span></span>

<span data-ttu-id="d089c-195">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="d089c-195">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="d089c-196">Дополнительные сведения см. в статье [Background tasks with hosted services in ASP.NET Core](xref:fundamentals/host/hosted-services) (Фоновые задачи с размещенными службами в ASP.NET Core).</span><span class="sxs-lookup"><span data-stu-id="d089c-196">For more information, see the [Background tasks with hosted services](xref:fundamentals/host/hosted-services) topic.</span></span>

<span data-ttu-id="d089c-197">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует метод расширения `AddHostedService` для добавления службы для событий времени жизни (`LifetimeEventsHostedService`) и синхронизированной фоновой задачи (`TimedHostedService`):</span><span class="sxs-lookup"><span data-stu-id="d089c-197">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses the `AddHostedService` extension method to add a service for lifetime events, `LifetimeEventsHostedService`, and a timed background task, `TimedHostedService`, to the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureServices)]

## <a name="configurelogging"></a><span data-ttu-id="d089c-198">ConfigureLogging</span><span class="sxs-lookup"><span data-stu-id="d089c-198">ConfigureLogging</span></span>

<span data-ttu-id="d089c-199">Метод [ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) добавляет делегата для настройки указанного [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span><span class="sxs-lookup"><span data-stu-id="d089c-199">[ConfigureLogging](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.configurelogging) adds a delegate for configuring the provided [ILoggingBuilder](/dotnet/api/microsoft.extensions.logging.iloggingbuilder).</span></span> <span data-ttu-id="d089c-200">Метод `ConfigureLogging` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="d089c-200">`ConfigureLogging` may be called multiple times with additive results.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ConfigureLogging)]

### <a name="useconsolelifetime"></a><span data-ttu-id="d089c-201">UseConsoleLifetime</span><span class="sxs-lookup"><span data-stu-id="d089c-201">UseConsoleLifetime</span></span>

<span data-ttu-id="d089c-202">Метод [UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) прослушивает сигналы `Ctrl+C`/SIGINT или SIGTERM и вызывает метод [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) для запуска процесса завершения работы.</span><span class="sxs-lookup"><span data-stu-id="d089c-202">[UseConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.useconsolelifetime) listens for `Ctrl+C`/SIGINT or SIGTERM and calls [StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) to start the shutdown process.</span></span> <span data-ttu-id="d089c-203">Метод `UseConsoleLifetime` разблокирует расширения, такие как [RunAsync](#runasync) и [WaitForShutdownAsync](#waitforshutdownasync).</span><span class="sxs-lookup"><span data-stu-id="d089c-203">`UseConsoleLifetime` unblocks extensions such as [RunAsync](#runasync) and [WaitForShutdownAsync](#waitforshutdownasync).</span></span> <span data-ttu-id="d089c-204">Класс [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) предварительно зарегистрирован и является реализацией времени жизни по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="d089c-204">[ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) is pre-registered as the default lifetime implementation.</span></span> <span data-ttu-id="d089c-205">Используется то время жизни, которое зарегистрировано последним.</span><span class="sxs-lookup"><span data-stu-id="d089c-205">The last lifetime registered is used.</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_UseConsoleLifetime)]

## <a name="container-configuration"></a><span data-ttu-id="d089c-206">Конфигурация контейнера</span><span class="sxs-lookup"><span data-stu-id="d089c-206">Container configuration</span></span>

<span data-ttu-id="d089c-207">Для поддержки подключения к другим контейнерам узел может принимать [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span><span class="sxs-lookup"><span data-stu-id="d089c-207">To support plugging in other containers, the host can accept an [IServiceProviderFactory](/dotnet/api/microsoft.extensions.dependencyinjection.iserviceproviderfactory-1).</span></span> <span data-ttu-id="d089c-208">Предоставляет фабрику, не являющуюся частью регистрации контейнера внедрения зависимостей, а являющуюся встроенной функцией узла, которая используется для создания конкретных контейнеров внедрения зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d089c-208">Providing a factory isn't part of the DI container registration but is instead a host intrinsic used to create the concrete DI container.</span></span> <span data-ttu-id="d089c-209">[UseServiceProviderFactory (IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) переопределяет фабрику по умолчанию, используемую для создания поставщика службы приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-209">[UseServiceProviderFactory(IServiceProviderFactory&lt;TContainerBuilder&gt;)](/dotnet/api/microsoft.extensions.hosting.hostbuilder.useserviceproviderfactory) overrides the default factory used to create the app's service provider.</span></span>

<span data-ttu-id="d089c-210">Пользовательская конфигурация контейнера управляется методом [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer).</span><span class="sxs-lookup"><span data-stu-id="d089c-210">Custom container configuration is managed by the [ConfigureContainer](/dotnet/api/microsoft.extensions.hosting.hostbuilder.configurecontainer) method.</span></span> <span data-ttu-id="d089c-211">Метод `ConfigureContainer` обеспечивает возможности строго типизированной настройки контейнера на основе базового API узла.</span><span class="sxs-lookup"><span data-stu-id="d089c-211">`ConfigureContainer` provides a strongly-typed experience for configuring the container on top of the underlying host API.</span></span> <span data-ttu-id="d089c-212">Метод `ConfigureContainer` может вызываться несколько раз с накоплением результатов.</span><span class="sxs-lookup"><span data-stu-id="d089c-212">`ConfigureContainer` can be called multiple times with additive results.</span></span>

<span data-ttu-id="d089c-213">Создание контейнера службы для приложения:</span><span class="sxs-lookup"><span data-stu-id="d089c-213">Create a service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainer.cs)]

<span data-ttu-id="d089c-214">Настройка фабрики контейнера службы:</span><span class="sxs-lookup"><span data-stu-id="d089c-214">Provide a service container factory:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/ServiceContainerFactory.cs)]

<span data-ttu-id="d089c-215">Использование фабрики и настройка пользовательского контейнера служб для приложения:</span><span class="sxs-lookup"><span data-stu-id="d089c-215">Use the factory and configure the custom service container for the app:</span></span>

[!code-csharp[](generic-host/samples-snapshot/2.x/GenericHostSample/Program.cs?name=snippet_ContainerConfiguration)]

## <a name="extensibility"></a><span data-ttu-id="d089c-216">Расширение среды</span><span class="sxs-lookup"><span data-stu-id="d089c-216">Extensibility</span></span>

<span data-ttu-id="d089c-217">Расширяемость узла выполняется с помощью методов расширения класса `IHostBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d089c-217">Host extensibility is performed with extension methods on `IHostBuilder`.</span></span> <span data-ttu-id="d089c-218">В следующем примере показано, как метод расширения расширяет реализацию класса `IHostBuilder` с помощью [RabbitMQ](https://www.rabbitmq.com/).</span><span class="sxs-lookup"><span data-stu-id="d089c-218">The following example shows how an extension method extends an `IHostBuilder` implementation with [RabbitMQ](https://www.rabbitmq.com/).</span></span> <span data-ttu-id="d089c-219">Этот метод расширения (в любом месте приложения) регистрирует службу `IHostedService` RabbitMQ:</span><span class="sxs-lookup"><span data-stu-id="d089c-219">The extension method (elsewhere in the app) registers a RabbitMQ `IHostedService`:</span></span>

```csharp
// UseRabbitMq is an extension method that sets up RabbitMQ to handle incoming
// messages.
var host = new HostBuilder()
    .UseRabbitMq<MyMessageHandler>()
    .Build();

await host.StartAsync();
```

## <a name="manage-the-host"></a><span data-ttu-id="d089c-220">Управление узлом</span><span class="sxs-lookup"><span data-stu-id="d089c-220">Manage the host</span></span>

<span data-ttu-id="d089c-221">Реализация [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) отвечает за запуск и остановку реализаций `IHostedService`, которые зарегистрированы в контейнере службы.</span><span class="sxs-lookup"><span data-stu-id="d089c-221">The [IHost](/dotnet/api/microsoft.extensions.hosting.ihost) implementation is responsible for starting and stopping the `IHostedService` implementations that are registered in the service container.</span></span>

### <a name="run"></a><span data-ttu-id="d089c-222">Выполнить</span><span class="sxs-lookup"><span data-stu-id="d089c-222">Run</span></span>

<span data-ttu-id="d089c-223">Метод [Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) запускает приложение и блокирует вызывающий поток, пока работа узла не будет завершена:</span><span class="sxs-lookup"><span data-stu-id="d089c-223">[Run](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.run) runs the app and blocks the calling thread until the host is shut down:</span></span>

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

### <a name="runasync"></a><span data-ttu-id="d089c-224">RunAsync</span><span class="sxs-lookup"><span data-stu-id="d089c-224">RunAsync</span></span>

<span data-ttu-id="d089c-225">Метод [RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) запускает приложение и возвращает объект `Task`, который завершается при активации токена отмены или завершение работы:</span><span class="sxs-lookup"><span data-stu-id="d089c-225">[RunAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.runasync) runs the app and returns a `Task` that completes when the cancellation token or shutdown is triggered:</span></span>

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

### <a name="runconsoleasync"></a><span data-ttu-id="d089c-226">RunConsoleAsync</span><span class="sxs-lookup"><span data-stu-id="d089c-226">RunConsoleAsync</span></span>

<span data-ttu-id="d089c-227">Метод [RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) включает поддержку консоли, собирает и запускает узел и ожидает сигналы `Ctrl+C`/SIGINT или SIGTERM для завершения работы.</span><span class="sxs-lookup"><span data-stu-id="d089c-227">[RunConsoleAsync](/dotnet/api/microsoft.extensions.hosting.hostinghostbuilderextensions.runconsoleasync) enables console support, builds and starts the host, and waits for `Ctrl+C`/SIGINT or SIGTERM to shut down.</span></span>

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

### <a name="start-and-stopasync"></a><span data-ttu-id="d089c-228">Start и StopAsync</span><span class="sxs-lookup"><span data-stu-id="d089c-228">Start and StopAsync</span></span>

<span data-ttu-id="d089c-229">Метод [Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) запускает узел синхронно.</span><span class="sxs-lookup"><span data-stu-id="d089c-229">[Start](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.start) starts the host synchronously.</span></span>

<span data-ttu-id="d089c-230">Метод [StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) пытается остановить узел в течение указанного периода ожидания.</span><span class="sxs-lookup"><span data-stu-id="d089c-230">[StopAsync(TimeSpan)](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.stopasync) attempts to stop the host within the provided timeout.</span></span>

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

### <a name="startasync-and-stopasync"></a><span data-ttu-id="d089c-231">StartAsync и StopAsync</span><span class="sxs-lookup"><span data-stu-id="d089c-231">StartAsync and StopAsync</span></span>

<span data-ttu-id="d089c-232">Метод [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) запускает приложение.</span><span class="sxs-lookup"><span data-stu-id="d089c-232">[StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync) starts the app.</span></span>

<span data-ttu-id="d089c-233">Метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) завершает работу приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-233">[StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync) stops the app.</span></span>

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

### <a name="waitforshutdown"></a><span data-ttu-id="d089c-234">WaitForShutdown</span><span class="sxs-lookup"><span data-stu-id="d089c-234">WaitForShutdown</span></span>

<span data-ttu-id="d089c-235">Метод [WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) инициируется через [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), например через [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (прослушивает сигналы `Ctrl+C`/SIGINT и SIGTERM).</span><span class="sxs-lookup"><span data-stu-id="d089c-235">[WaitForShutdown](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdown) is triggered via the [IHostLifetime](/dotnet/api/microsoft.extensions.hosting.ihostlifetime), such as [ConsoleLifetime](/dotnet/api/microsoft.extensions.hosting.internal.consolelifetime) (listens for `Ctrl+C`/SIGINT or SIGTERM).</span></span> <span data-ttu-id="d089c-236">Метод `WaitForShutdown` вызывает [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="d089c-236">`WaitForShutdown` calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="waitforshutdownasync"></a><span data-ttu-id="d089c-237">WaitForShutdownAsync</span><span class="sxs-lookup"><span data-stu-id="d089c-237">WaitForShutdownAsync</span></span>

<span data-ttu-id="d089c-238">Метод [WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) возвращает объект `Task`, который завершается после активации завершения работы через токен, и вызывает метод [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span><span class="sxs-lookup"><span data-stu-id="d089c-238">[WaitForShutdownAsync](/dotnet/api/microsoft.extensions.hosting.hostingabstractionshostextensions.waitforshutdownasync) returns a `Task` that completes when shutdown is triggered via the given token and calls [StopAsync](/dotnet/api/microsoft.extensions.hosting.ihost.stopasync).</span></span>

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

### <a name="external-control"></a><span data-ttu-id="d089c-239">Внешнее управление</span><span class="sxs-lookup"><span data-stu-id="d089c-239">External control</span></span>

<span data-ttu-id="d089c-240">Внешнее управление узла может осуществляться с помощью методов, которые могут быть вызваны извне:</span><span class="sxs-lookup"><span data-stu-id="d089c-240">External control of the host can be achieved using methods that can be called externally:</span></span>

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

<span data-ttu-id="d089c-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) вызывается в начале [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), который ждет, пока он не будет завершен, прежде чем продолжить.</span><span class="sxs-lookup"><span data-stu-id="d089c-241">[IHostLifetime.WaitForStartAsync](/dotnet/api/microsoft.extensions.hosting.ihostlifetime.waitforstartasync) is called at the start of [StartAsync](/dotnet/api/microsoft.extensions.hosting.ihost.startasync), which waits until it's complete before continuing.</span></span> <span data-ttu-id="d089c-242">Можно отложить запуск до получения сигнала от внешнего события.</span><span class="sxs-lookup"><span data-stu-id="d089c-242">This can be used to delay startup until signaled by an external event.</span></span>

## <a name="ihostingenvironment-interface"></a><span data-ttu-id="d089c-243">Интерфейс IHostingEnvironment</span><span class="sxs-lookup"><span data-stu-id="d089c-243">IHostingEnvironment interface</span></span>

<span data-ttu-id="d089c-244">Интерфейс [IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) предоставляет сведения о среде размещения приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-244">[IHostingEnvironment](/dotnet/api/microsoft.extensions.hosting.ihostingenvironment) provides information about the app's hosting environment.</span></span> <span data-ttu-id="d089c-245">Чтобы получить интерфейс `IHostingEnvironment` для использования его свойств и методов расширения, воспользуйтесь [внедрением конструктора](xref:fundamentals/dependency-injection):</span><span class="sxs-lookup"><span data-stu-id="d089c-245">Use [constructor injection](xref:fundamentals/dependency-injection) to obtain the `IHostingEnvironment` in order to use its properties and extension methods:</span></span>

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

<span data-ttu-id="d089c-246">Дополнительные сведения см. в статье [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="d089c-246">For more information, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="iapplicationlifetime-interface"></a><span data-ttu-id="d089c-247">Интерфейс IApplicationLifetime</span><span class="sxs-lookup"><span data-stu-id="d089c-247">IApplicationLifetime interface</span></span>

<span data-ttu-id="d089c-248">Интерфейс [IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) позволяет выполнять действия после запуска и завершения работы, включая запросы о нормальном завершении работы.</span><span class="sxs-lookup"><span data-stu-id="d089c-248">[IApplicationLifetime](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime) allows for post-startup and shutdown activities, including graceful shutdown requests.</span></span> <span data-ttu-id="d089c-249">Три свойства этого интерфейса представляют собой токены отмены, которые служат для регистрации методов `Action`, определяющих события запуска и завершения работы.</span><span class="sxs-lookup"><span data-stu-id="d089c-249">Three properties on the interface are cancellation tokens used to register `Action` methods that define startup and shutdown events.</span></span>

| <span data-ttu-id="d089c-250">Токен отмены</span><span class="sxs-lookup"><span data-stu-id="d089c-250">Cancellation Token</span></span> | <span data-ttu-id="d089c-251">Условие инициации&#8230;</span><span class="sxs-lookup"><span data-stu-id="d089c-251">Triggered when&#8230;</span></span> |
| ------------------ | --------------------- |
| [<span data-ttu-id="d089c-252">ApplicationStarted</span><span class="sxs-lookup"><span data-stu-id="d089c-252">ApplicationStarted</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstarted) | <span data-ttu-id="d089c-253">Узел полностью запущен.</span><span class="sxs-lookup"><span data-stu-id="d089c-253">The host has fully started.</span></span> |
| [<span data-ttu-id="d089c-254">ApplicationStopped</span><span class="sxs-lookup"><span data-stu-id="d089c-254">ApplicationStopped</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopped) | <span data-ttu-id="d089c-255">Заканчивается нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="d089c-255">The host is completing a graceful shutdown.</span></span> <span data-ttu-id="d089c-256">Все запросы должны быть обработаны.</span><span class="sxs-lookup"><span data-stu-id="d089c-256">All requests should be processed.</span></span> <span data-ttu-id="d089c-257">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="d089c-257">Shutdown blocks until this event completes.</span></span> |
| [<span data-ttu-id="d089c-258">ApplicationStopping</span><span class="sxs-lookup"><span data-stu-id="d089c-258">ApplicationStopping</span></span>](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.applicationstopping) | <span data-ttu-id="d089c-259">Происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="d089c-259">The host is performing a graceful shutdown.</span></span> <span data-ttu-id="d089c-260">Запросы могут все еще обрабатываться.</span><span class="sxs-lookup"><span data-stu-id="d089c-260">Requests may still be processing.</span></span> <span data-ttu-id="d089c-261">Завершение работы блокируется до тех пор, пока это событие не завершится.</span><span class="sxs-lookup"><span data-stu-id="d089c-261">Shutdown blocks until this event completes.</span></span> |

<span data-ttu-id="d089c-262">Конструктор внедряет службу `IApplicationLifetime` в любой класс.</span><span class="sxs-lookup"><span data-stu-id="d089c-262">Constructor-inject the `IApplicationLifetime` service into any class.</span></span> <span data-ttu-id="d089c-263">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) использует внедрение в конструкторе в класс `LifetimeEventsHostedService` (реализацию `IHostedService`) для регистрации событий.</span><span class="sxs-lookup"><span data-stu-id="d089c-263">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/generic-host/samples/) uses contructor injection into a `LifetimeEventsHostedService` class (an `IHostedService` implementation) to register the events.</span></span>

<span data-ttu-id="d089c-264">*LifetimeEventsHostedService.cs*:</span><span class="sxs-lookup"><span data-stu-id="d089c-264">*LifetimeEventsHostedService.cs*:</span></span>

[!code-csharp[](generic-host/samples/2.x/GenericHostSample/LifetimeEventsHostedService.cs?name=snippet1)]

<span data-ttu-id="d089c-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) запрашивает остановку приложения.</span><span class="sxs-lookup"><span data-stu-id="d089c-265">[StopApplication](/dotnet/api/microsoft.extensions.hosting.iapplicationlifetime.stopapplication) requests termination of the app.</span></span> <span data-ttu-id="d089c-266">Следующий класс использует `StopApplication` для корректного завершения работы приложения при вызове метода класса `Shutdown`:</span><span class="sxs-lookup"><span data-stu-id="d089c-266">The following class uses `StopApplication` to gracefully shut down an app when the class's `Shutdown` method is called:</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="d089c-267">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d089c-267">Additional resources</span></span>

* [<span data-ttu-id="d089c-268">Фоновые задачи с размещенными службами</span><span class="sxs-lookup"><span data-stu-id="d089c-268">Background tasks with hosted services</span></span>](xref:fundamentals/host/hosted-services)
* [<span data-ttu-id="d089c-269">Репозиторий примеров размещения на GitHub</span><span class="sxs-lookup"><span data-stu-id="d089c-269">Hosting repo samples on GitHub</span></span>](https://github.com/aspnet/Hosting/tree/release/2.1/samples)
