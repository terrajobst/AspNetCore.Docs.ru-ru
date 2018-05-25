---
title: Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup
author: guardrex
description: Узнайте, как улучшить приложение ASP.NET Core из внешней сборки, используя реализацию IHostingStartup.
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/07/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 793169b491596cd7326d747a3f19d7fdaf7e2b65
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="enhance-an-app-from-an-external-assembly-in-aspnet-core-with-ihostingstartup"></a><span data-ttu-id="ce43d-103">Усовершенствование приложения из внешней сборки в ASP.NET Core с IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="ce43d-103">Enhance an app from an external assembly in ASP.NET Core with IHostingStartup</span></span>

<span data-ttu-id="ce43d-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="ce43d-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="ce43d-105">Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) позволяет при запуске добавлять в приложение улучшения из внешней сборки вне класса `Startup` приложения.</span><span class="sxs-lookup"><span data-stu-id="ce43d-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding enhancements to an app at startup from an external assembly outside of the app's `Startup` class.</span></span> <span data-ttu-id="ce43d-106">Например, библиотека внешних средств может использовать реализацию `IHostingStartup`, чтобы предоставить приложению дополнительных поставщиков конфигурации или сервисы.</span><span class="sxs-lookup"><span data-stu-id="ce43d-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="ce43d-107">`IHostingStartup` *доступно в ASP.NET Core 2.0 и более поздней версии.*</span><span class="sxs-lookup"><span data-stu-id="ce43d-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="ce43d-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="ce43d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="ce43d-109">Обнаружение загруженных начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="ce43d-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="ce43d-110">Для обнаружения начальных сборок размещения, загруженных приложением или библиотеками, включите ведение журнала и проверяйте журналы приложений.</span><span class="sxs-lookup"><span data-stu-id="ce43d-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="ce43d-111">В журнал вносятся ошибки, возникающие при загрузке сборок.</span><span class="sxs-lookup"><span data-stu-id="ce43d-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="ce43d-112">Загруженные начальные сборки размещения регистрируются на уровне отладки, также регистрируются все ошибки.</span><span class="sxs-lookup"><span data-stu-id="ce43d-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="ce43d-113">Пример приложения считывает [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) в массив `string` и отображает результат на странице индексов в приложении:</span><span class="sxs-lookup"><span data-stu-id="ce43d-113">The sample app reads the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[](platform-specific-configuration/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="ce43d-114">Отключение автоматической загрузки начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="ce43d-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="ce43d-115">Существует два способа отключить автоматическую загрузку начальных сборок размещения:</span><span class="sxs-lookup"><span data-stu-id="ce43d-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="ce43d-116">Задайте параметр конфигурации узла [Запретить запуск размещения](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="ce43d-116">Set the [Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="ce43d-117">Задайте переменную среды `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="ce43d-117">Set the `ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>

<span data-ttu-id="ce43d-118">Если параметр узла или переменная среды имеют значение `true` или `1`, начальные сборки размещения не загружаются автоматически.</span><span class="sxs-lookup"><span data-stu-id="ce43d-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="ce43d-119">Если заданы оба параметра, приоритет имеет параметр узла.</span><span class="sxs-lookup"><span data-stu-id="ce43d-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="ce43d-120">Параметр узла или переменная среды отключают начальные сборки размещения глобально и могут также отключить несколько характеристик приложения.</span><span class="sxs-lookup"><span data-stu-id="ce43d-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several characteristics of an app.</span></span> <span data-ttu-id="ce43d-121">В настоящее время невозможно выборочно отключить начальную сборку размещения, добавленную библиотекой, если у библиотеки нет своего параметра конфигурации.</span><span class="sxs-lookup"><span data-stu-id="ce43d-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="ce43d-122">В следующем выпуске начальные сборки размещения можно будет отключать выборочно (см. [выпуск GitHub "aspnet/размещение" № 1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="ce43d-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup"></a><span data-ttu-id="ce43d-123">Реализация IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="ce43d-123">Implement IHostingStartup</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="ce43d-124">Создание сборки</span><span class="sxs-lookup"><span data-stu-id="ce43d-124">Create the assembly</span></span>

<span data-ttu-id="ce43d-125">Усовершенствование `IHostingStartup` разворачивается как сборка на основе консольного приложения без точки входа.</span><span class="sxs-lookup"><span data-stu-id="ce43d-125">An `IHostingStartup` enhancement is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="ce43d-126">Сборка ссылается на пакет [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="ce43d-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/snapshot_sample/StartupEnhancement.csproj)]

<span data-ttu-id="ce43d-127">Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) определяет класс как реализацию `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="ce43d-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="ce43d-128">В следующем примере используется пространство имен `StartupEnhancement` и класс `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="ce43d-128">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="ce43d-129">Класс реализует `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="ce43d-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="ce43d-130">Метод класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение:</span><span class="sxs-lookup"><span data-stu-id="ce43d-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app:</span></span>

[!code-csharp[](platform-specific-configuration/snapshot_sample/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="ce43d-131">При создании проекта `IHostingStartup` файл зависимостей (*\*. deps.json*) задает расположение `runtime` для сборки в папке *bin*:</span><span class="sxs-lookup"><span data-stu-id="ce43d-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ce43d-132">Показана только часть файла.</span><span class="sxs-lookup"><span data-stu-id="ce43d-132">Only part of the file is shown.</span></span> <span data-ttu-id="ce43d-133">Имя сборки в примере — `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="ce43d-133">The assembly name in the example is `StartupEnhancement`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="ce43d-134">Обновление файла зависимостей</span><span class="sxs-lookup"><span data-stu-id="ce43d-134">Update the dependencies file</span></span>

<span data-ttu-id="ce43d-135">Расположение среды выполнения указывается в файле *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ce43d-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="ce43d-136">Для активации улучшения элемент `runtime` должен указывать расположение среды выполнения сборки для улучшения.</span><span class="sxs-lookup"><span data-stu-id="ce43d-136">To active the enhancement, the `runtime` element must specify the location of the enhancement's runtime assembly.</span></span> <span data-ttu-id="ce43d-137">Расположение `runtime` должно иметь префикс `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="ce43d-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[](platform-specific-configuration/snapshot_sample/StartupEnhancement2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="ce43d-138">В примере приложения изменение файла *\*.deps.json* выполняется скриптом [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="ce43d-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="ce43d-139">Скрипт PowerShell запускается автоматически целевым объектом сборки в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="ce43d-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="enhancement-activation"></a><span data-ttu-id="ce43d-140">Активация улучшения</span><span class="sxs-lookup"><span data-stu-id="ce43d-140">Enhancement activation</span></span>

<span data-ttu-id="ce43d-141">**Размещение файла сборки**</span><span class="sxs-lookup"><span data-stu-id="ce43d-141">**Place the assembly file**</span></span>

<span data-ttu-id="ce43d-142">Файл сборки реализации `IHostingStartup` должен быть развернут в папке *bin* в приложении или помещен в [хранилище среды выполнения](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="ce43d-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="ce43d-143">Для индивидуального использования поместите сборку в хранилище среды выполнения профиля пользователя:</span><span class="sxs-lookup"><span data-stu-id="ce43d-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="ce43d-144">Для глобального использования поместите сборку в хранилище среды выполнения установки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ce43d-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<ENHANCEMENT_ASSEMBLY_NAME>\<ENHANCEMENT_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="ce43d-145">При развертывании сборки в хранилище среды выполнения файл символов тоже можно развернуть, но это необязательно для оптимизации работы.</span><span class="sxs-lookup"><span data-stu-id="ce43d-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the enhancement to work.</span></span>

<span data-ttu-id="ce43d-146">**Размещение файла зависимостей**</span><span class="sxs-lookup"><span data-stu-id="ce43d-146">**Place the dependencies file**</span></span>

<span data-ttu-id="ce43d-147">Файл реализации *\*.deps.json* должен находиться в доступном расположении.</span><span class="sxs-lookup"><span data-stu-id="ce43d-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="ce43d-148">Для индивидуального использования поместите файл в папку `additonalDeps` в параметрах профиля пользователя `.dotnet`:</span><span class="sxs-lookup"><span data-stu-id="ce43d-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="ce43d-149">Для глобального использования поместите файл в папку `additonalDeps` в установке .NET Core:</span><span class="sxs-lookup"><span data-stu-id="ce43d-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="ce43d-150">Версия `2.0.0` — это версия общей среды выполнения, которую использует целевое приложение.</span><span class="sxs-lookup"><span data-stu-id="ce43d-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="ce43d-151">Общая среда выполнения указана в файле *\*.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="ce43d-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="ce43d-152">В примере приложения общая среда выполнения задается в файле *HostingStartupSample.runtimeconfig.json*.</span><span class="sxs-lookup"><span data-stu-id="ce43d-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="ce43d-153">**Настройка переменных среды**</span><span class="sxs-lookup"><span data-stu-id="ce43d-153">**Set environment variables**</span></span>

<span data-ttu-id="ce43d-154">Задайте следующие переменные среды в контексте приложения, которое использует улучшение.</span><span class="sxs-lookup"><span data-stu-id="ce43d-154">Set the following environment variables in the context of the app that uses the enhancement.</span></span>

<span data-ttu-id="ce43d-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="ce43d-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="ce43d-156">Только начальные сборки размещения проверяются на наличие `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="ce43d-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="ce43d-157">Имя сборки реализации предоставляется в этой переменной среды.</span><span class="sxs-lookup"><span data-stu-id="ce43d-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="ce43d-158">В пример приложения установлено значение `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="ce43d-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="ce43d-159">Значение также можно задать с помощью параметра конфигурации узла [Начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="ce43d-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="ce43d-160">DOTNET\_ADDITIONAL\_DEPS</span><span class="sxs-lookup"><span data-stu-id="ce43d-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="ce43d-161">Расположение файла реализации *\*.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ce43d-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="ce43d-162">Если файл расположен в папке *.dotnet* в профиле пользователя для индивидуального использования:</span><span class="sxs-lookup"><span data-stu-id="ce43d-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="ce43d-163">Если файл расположен в установке .NET Core для глобального использования, укажите полный путь к файлу:</span><span class="sxs-lookup"><span data-stu-id="ce43d-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<ENHANCEMENT_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<ENHANCEMENT_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="ce43d-164">В пример приложения установлено значение:</span><span class="sxs-lookup"><span data-stu-id="ce43d-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="ce43d-165">Примеры установки переменных среды для различных операционных систем см. в разделе [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="ce43d-165">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="ce43d-166">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="ce43d-166">Sample app</span></span>

<span data-ttu-id="ce43d-167">В [примере приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([как загрузить](xref:tutorials/index#how-to-download-a-sample)) используется `IHostingStartup` для создания средства диагностики.</span><span class="sxs-lookup"><span data-stu-id="ce43d-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/configuration/platform-specific-configuration/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="ce43d-168">Это средство добавляет в приложение при запуске два ПО промежуточного слоя, которые предоставляют диагностические сведения:</span><span class="sxs-lookup"><span data-stu-id="ce43d-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="ce43d-169">Зарегистрированные службы</span><span class="sxs-lookup"><span data-stu-id="ce43d-169">Registered services</span></span>
* <span data-ttu-id="ce43d-170">Адрес: схема, узел, базовый путь, путь, строка запроса</span><span class="sxs-lookup"><span data-stu-id="ce43d-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="ce43d-171">Подключение: удаленный IP, удаленный порт, локальный IP, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="ce43d-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="ce43d-172">Заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="ce43d-172">Request headers</span></span>
* <span data-ttu-id="ce43d-173">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="ce43d-173">Environment variables</span></span>

<span data-ttu-id="ce43d-174">Для выполнения образца:</span><span class="sxs-lookup"><span data-stu-id="ce43d-174">To run the sample:</span></span>

1. <span data-ttu-id="ce43d-175">В проекте диагностики запуска используется [PowerShell](/powershell/scripting/powershell-scripting) для изменения файла *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ce43d-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="ce43d-176">PowerShell устанавливается по умолчанию в операционной системе Windows начиная с Windows 7 с пакетом обновления 1 (SP1) и Windows Server 2008 R2 с пакетом обновления 1 (SP1).</span><span class="sxs-lookup"><span data-stu-id="ce43d-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="ce43d-177">Для установки PowerShell на других платформах см. раздел [Установка Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="ce43d-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="ce43d-178">Создайте проект диагностики запуска.</span><span class="sxs-lookup"><span data-stu-id="ce43d-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="ce43d-179">Целевой объект в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="ce43d-179">A build target in the project file:</span></span>
   * <span data-ttu-id="ce43d-180">Перемещает сборку и файлы символов в хранилище среды выполнения в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="ce43d-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="ce43d-181">Запускает скрипт PowerShell, чтобы изменить файл *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="ce43d-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="ce43d-182">Перемещает файл *StartupDiagnostics.deps.json* в папку `additionalDeps` в профиле пользователя.</span><span class="sxs-lookup"><span data-stu-id="ce43d-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="ce43d-183">Задайте переменные среды:</span><span class="sxs-lookup"><span data-stu-id="ce43d-183">Set the environment variables:</span></span>
    * <span data-ttu-id="ce43d-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="ce43d-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="ce43d-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="ce43d-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="ce43d-186">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="ce43d-186">Run the sample app.</span></span>
5. <span data-ttu-id="ce43d-187">Запросите конечную точку `/services` для просмотра зарегистрированных служб приложения.</span><span class="sxs-lookup"><span data-stu-id="ce43d-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="ce43d-188">Запросите конечную точку `/diag` для просмотра диагностических сведений.</span><span class="sxs-lookup"><span data-stu-id="ce43d-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
