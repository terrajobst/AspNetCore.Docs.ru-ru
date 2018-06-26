---
title: Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup
author: guardrex
description: Узнайте, как улучшить приложение ASP.NET Core из внешней сборки, используя реализацию IHostingStartup.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: bd9605dd8efee2c3ba8bc82a81554cace40630be
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36278087"
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="6930c-103">Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="6930c-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="6930c-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="6930c-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="6930c-105">Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения.</span><span class="sxs-lookup"><span data-stu-id="6930c-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="6930c-106">Например, библиотека внешних средств может использовать реализацию `IHostingStartup`, чтобы предоставить приложению дополнительных поставщиков конфигурации или сервисы.</span><span class="sxs-lookup"><span data-stu-id="6930c-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="6930c-107">`IHostingStartup` *доступно в ASP.NET Core 2.0 и более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="6930c-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="6930c-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="6930c-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="6930c-109">Обнаружение загруженных начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="6930c-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="6930c-110">Для обнаружения начальных сборок размещения, загруженных приложением или библиотеками, включите ведение журнала и проверяйте журналы приложений.</span><span class="sxs-lookup"><span data-stu-id="6930c-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="6930c-111">В журнал вносятся ошибки, возникающие при загрузке сборок.</span><span class="sxs-lookup"><span data-stu-id="6930c-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="6930c-112">Загруженные начальные сборки размещения регистрируются на уровне отладки, также регистрируются все ошибки.</span><span class="sxs-lookup"><span data-stu-id="6930c-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="6930c-113">Пример приложения считывает [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) в массив `string` и отображает результат на странице индексов в приложении:</span><span class="sxs-lookup"><span data-stu-id="6930c-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="6930c-114">Отключение автоматической загрузки начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="6930c-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="6930c-115">Существует два способа отключить автоматическую загрузку начальных сборок размещения:</span><span class="sxs-lookup"><span data-stu-id="6930c-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="6930c-116">Задайте параметр конфигурации узла [Запретить запуск размещения](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="6930c-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="6930c-117">Задайте переменную среды `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="6930c-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="6930c-118">Если параметр узла или переменная среды имеют значение `true` или `1`, начальные сборки размещения не загружаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="6930c-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="6930c-119">Если заданы оба параметра, приоритет имеет параметр узла.</span><span class="sxs-lookup"><span data-stu-id="6930c-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="6930c-120">Параметр узла или переменная среды отключают начальные сборки размещения глобально и могут также отключить несколько характеристик приложения.</span><span class="sxs-lookup"><span data-stu-id="6930c-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="6930c-121">В настоящее время невозможно выборочно отключить начальную сборку размещения, добавленную библиотекой, если у библиотеки нет своего параметра конфигурации.</span><span class="sxs-lookup"><span data-stu-id="6930c-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="6930c-122">В следующем выпуске начальные сборки размещения можно будет отключать выборочно (см. [выпуск GitHub "aspnet/размещение" № 1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="6930c-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="6930c-123">Реализация IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="6930c-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="6930c-124">Создание сборки</span><span class="sxs-lookup"><span data-stu-id="6930c-124">Create the assembly</span></span>

<span data-ttu-id="6930c-125">Усовершенствование `IHostingStartup` разворачивается как сборка на основе консольного приложения без точки входа.</span><span class="sxs-lookup"><span data-stu-id="6930c-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="6930c-126">Сборка ссылается на пакет [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="6930c-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="6930c-127">Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) определяет класс как реализацию `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="6930c-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="6930c-128">В следующем примере используется пространство имен `StartupEnhancement` и класс `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="6930c-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="6930c-129">Класс реализует `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="6930c-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="6930c-130">Метод класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение.</span><span class="sxs-lookup"><span data-stu-id="6930c-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="6930c-131">`IHostingStartup.Configure` в стартовой сборке размещения вызывается средой выполнения до `Startup.Configure` в пользовательском коде, что позволяет пользовательскому коду перезаписать конфигурацию, предоставленную стартовой сборкой размещения.</span><span class="sxs-lookup"><span data-stu-id="6930c-131">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configruation provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="6930c-132">При создании проекта `IHostingStartup` файл зависимостей (*\*. deps.json*) задает расположение `runtime` для сборки в папке *bin*:</span><span class="sxs-lookup"><span data-stu-id="6930c-132">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="6930c-133">Показана только часть файла.</span><span class="sxs-lookup"><span data-stu-id="6930c-133">Only part of the file is shown.</span></span> <span data-ttu-id="6930c-134">Имя сборки в примере — `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="6930c-134">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="6930c-135">Обновление файла зависимостей</span><span class="sxs-lookup"><span data-stu-id="6930c-135">Update the dependencies file</span></span>

<span data-ttu-id="6930c-136">Расположение среды выполнения указывается в файле *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="6930c-136">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="6930c-137">Для активации улучшения элемент `runtime` должен указывать расположение среды выполнения сборки для улучшения.</span><span class="sxs-lookup"><span data-stu-id="6930c-137">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="6930c-138">Расположение `runtime` должно иметь префикс `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span><span class="sxs-lookup"><span data-stu-id="6930c-138">Prefix the `runtime` location with `lib/<TARGET_FRAMEWORK_MONIKER>/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="6930c-139">В примере приложения изменение файла *\*.deps.json* выполняется скриптом [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="6930c-139">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="6930c-140">Скрипт PowerShell запускается автоматически целевым объектом сборки в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="6930c-140">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="6930c-141">Активация улучшения</span><span class="sxs-lookup"><span data-stu-id="6930c-141">Enhancement activation</span></span>

<span data-ttu-id="6930c-142">**Размещение файла сборки**</span><span class="sxs-lookup"><span data-stu-id="6930c-142">**Place the assembly file**</span></span>

<span data-ttu-id="6930c-143">Файл сборки реализации `IHostingStartup` должен быть развернут в папке *bin* в приложении или помещен в [хранилище среды выполнения](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="6930c-143">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="6930c-144">Для индивидуального использования поместите сборку в хранилище среды выполнения профиля пользователя:</span><span class="sxs-lookup"><span data-stu-id="6930c-144">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="6930c-145">Для глобального использования поместите сборку в хранилище среды выполнения установки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="6930c-145">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\<TARGET_FRAMEWORK_MONIKER>\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\<TARGET_FRAMEWORK_MONIKER>\
```

<span data-ttu-id="6930c-146">При развертывании сборки в хранилище среды выполнения файл символов тоже можно развернуть, но это необязательно для оптимизации работы.</span><span class="sxs-lookup"><span data-stu-id="6930c-146">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="6930c-147">**Размещение файла зависимостей**</span><span class="sxs-lookup"><span data-stu-id="6930c-147">**Place the dependencies file**</span></span>

<span data-ttu-id="6930c-148">Файл реализации *\*.deps.json* должен находиться в доступном расположении.</span><span class="sxs-lookup"><span data-stu-id="6930c-148">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="6930c-149">Для индивидуального использования поместите файл в папку `additonalDeps` в параметрах профиля пользователя `.dotnet`:</span><span class="sxs-lookup"><span data-stu-id="6930c-149">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="6930c-150">Для глобального использования поместите файл в папку `additonalDeps` в установке .NET Core:</span><span class="sxs-lookup"><span data-stu-id="6930c-150">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\
```

<span data-ttu-id="6930c-151">Версия общей платформы отражает версию общей среды выполнения, которую использует целевое приложение.</span><span class="sxs-lookup"><span data-stu-id="6930c-151">The shared framework version reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="6930c-152">Общая среда выполнения указана в файле *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="6930c-152">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="6930c-153">В примере приложения общая среда выполнения задается в файле *HostingStartupSample.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="6930c-153">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="6930c-154">**Настройка переменных среды**</span><span class="sxs-lookup"><span data-stu-id="6930c-154">**Set environment variables**</span></span>

<span data-ttu-id="6930c-155">Задайте следующие переменные среды в контексте приложения, которое использует улучшение.</span><span class="sxs-lookup"><span data-stu-id="6930c-155">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="6930c-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="6930c-156">ASPNETCORE_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="6930c-157">Только начальные сборки размещения проверяются на наличие `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="6930c-157">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="6930c-158">Имя сборки реализации предоставляется в этой переменной среды.</span><span class="sxs-lookup"><span data-stu-id="6930c-158">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="6930c-159">В пример приложения установлено значение `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="6930c-159">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="6930c-160">Значение также можно задать с помощью параметра конфигурации узла [Начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="6930c-160">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="6930c-161">При наличии нескольких стартовых сборок размещения их методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) выполняются в порядке расположения сборок.</span><span class="sxs-lookup"><span data-stu-id="6930c-161">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

<span data-ttu-id="6930c-162">DOTNET_ADDITIONAL_DEPS</span><span class="sxs-lookup"><span data-stu-id="6930c-162">DOTNET_ADDITIONAL_DEPS</span></span>

<span data-ttu-id="6930c-163">Расположение файла реализации *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="6930c-163">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="6930c-164">Если файл расположен в папке *.dotnet* в профиле пользователя для индивидуального использования:</span><span class="sxs-lookup"><span data-stu-id="6930c-164">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="6930c-165">Если файл расположен в установке .NET Core для глобального использования, укажите полный путь к файлу:</span><span class="sxs-lookup"><span data-stu-id="6930c-165">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\<SHARED_FRAMEWORK_VERSION>\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="6930c-166">В пример приложения установлено значение:</span><span class="sxs-lookup"><span data-stu-id="6930c-166">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="6930c-167">Примеры установки переменных среды для различных операционных систем см. в разделе [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="6930c-167">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="6930c-168">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="6930c-168">Sample app</span></span>

<span data-ttu-id="6930c-169">В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([как загрузить](xref:tutorials/index#how-to-download-a-sample)) используется `IHostingStartup` для создания средства диагностики.</span><span class="sxs-lookup"><span data-stu-id="6930c-169">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="6930c-170">Это средство добавляет в приложение при запуске два ПО промежуточного слоя, которые предоставляют диагностические сведения:</span><span class="sxs-lookup"><span data-stu-id="6930c-170">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="6930c-171">Зарегистрированные службы</span><span class="sxs-lookup"><span data-stu-id="6930c-171">Registered services</span></span>
* <span data-ttu-id="6930c-172">Адрес: схема, узел, базовый путь, путь, строка запроса</span><span class="sxs-lookup"><span data-stu-id="6930c-172">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="6930c-173">Подключение: удаленный IP, удаленный порт, локальный IP, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="6930c-173">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="6930c-174">Заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="6930c-174">Request headers</span></span>
* <span data-ttu-id="6930c-175">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="6930c-175">Environment variables</span></span>

<span data-ttu-id="6930c-176">Для выполнения образца:</span><span class="sxs-lookup"><span data-stu-id="6930c-176">To run the sample:</span></span>

1. <span data-ttu-id="6930c-177">В проекте диагностики запуска используется [PowerShell](/powershell/scripting/powershell-scripting) для изменения файла *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="6930c-177">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="6930c-178">PowerShell устанавливается по умолчанию в операционной системе Windows начиная с Windows 7 с пакетом обновления 1 (SP1) и Windows Server 2008 R2 с пакетом обновления 1 (SP1).</span><span class="sxs-lookup"><span data-stu-id="6930c-178">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="6930c-179">Для установки PowerShell на других платформах см. раздел [Установка Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="6930c-179">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="6930c-180">Создайте проект диагностики запуска.</span><span class="sxs-lookup"><span data-stu-id="6930c-180">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="6930c-181">Целевой объект в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="6930c-181">A build target in the project file:</span></span>
   * <span data-ttu-id="6930c-182">Перемещает сборку и файлы символов в хранилище среды выполнения в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="6930c-182">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="6930c-183">Запускает скрипт PowerShell, чтобы изменить файл *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="6930c-183">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="6930c-184">Перемещает файл *StartupDiagnostics.deps.json* в папку `additionalDeps` в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="6930c-184">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="6930c-185">Задайте переменные среды:</span><span class="sxs-lookup"><span data-stu-id="6930c-185">Set the environment variables:</span></span>
    * <span data-ttu-id="6930c-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="6930c-186">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="6930c-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="6930c-187">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="6930c-188">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="6930c-188">Run the sample app.</span></span>
5. <span data-ttu-id="6930c-189">Запросите конечную точку `/services` для просмотра зарегистрированных служб приложения.</span><span class="sxs-lookup"><span data-stu-id="6930c-189">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="6930c-190">Запросите конечную точку `/diag` для просмотра диагностических сведений.</span><span class="sxs-lookup"><span data-stu-id="6930c-190">Request the `/diag` endpoint to see the diagnostic information.</span></span>
