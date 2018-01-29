---
title: "Добавление компонентов приложения из внешней сборки с помощью IHostingStartup в ASP.NET Core"
author: guardrex
description: "Узнайте, как добавить компоненты к приложению ASP.NET Core из внешней сборки, используя реализацию IHostingStartup."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 12/07/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/ihostingstartup
ms.openlocfilehash: 6013091d94302f0f9ecbc777d3ef64774b49aebe
ms.sourcegitcommit: 12e5194936b7e820efc5505a2d5d4f84e88eb5ef
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/11/2018
---
# <a name="add-app-features-from-an-external-assembly-using-ihostingstartup-in-aspnet-core"></a><span data-ttu-id="e3bd6-103">Добавление компонентов приложения из внешней сборки с помощью IHostingStartup в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e3bd6-103">Add app features from an external assembly using IHostingStartup in ASP.NET Core</span></span>

<span data-ttu-id="e3bd6-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="e3bd6-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="e3bd6-105">[IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) реализация позволяет добавлять компоненты к приложению во время запуска из-за пределами приложения `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) implementation allows adding features to an app at startup from outside of the app's `Startup` class.</span></span> <span data-ttu-id="e3bd6-106">Например, можно использовать библиотеку внешние инструменты `IHostingStartup` реализации для предоставления дополнительной настройки поставщиков или служб для приложения.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-106">For example, an external tooling library can use an `IHostingStartup` implementation to provide additional configuration providers or services to an app.</span></span> <span data-ttu-id="e3bd6-107">`IHostingStartup`*доступен в ASP.NET Core 2.0 и более поздних.*</span><span class="sxs-lookup"><span data-stu-id="e3bd6-107">`IHostingStartup` *is available in ASP.NET Core 2.0 and later.*</span></span>

<span data-ttu-id="e3bd6-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="e3bd6-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="e3bd6-109">Обнаружение сборок, загруженных размещения запуска</span><span class="sxs-lookup"><span data-stu-id="e3bd6-109">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="e3bd6-110">Для получения размещение загрузки сборки, загруженные приложений или библиотек, включите ведение журнала и проверяйте журналы приложений.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-110">To discover hosting startup assemblies loaded by the app or by libraries, enable logging and check the application logs.</span></span> <span data-ttu-id="e3bd6-111">Регистрируются ошибки, возникающие при загрузке сборки.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-111">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="e3bd6-112">Загрузки сборок, загруженных размещения регистрируются на уровне отладки и регистрируются все ошибки.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-112">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

<span data-ttu-id="e3bd6-113">Образец приложения чтения [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) в `string` массива и отображает результат в страницу индекса приложения:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-113">The sample app reads the the [HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey) into a `string` array and displays the result in the app's Index page:</span></span>

[!code-csharp[Main](ihostingstartup/sample/HostingStartupSample/Pages/Index.cshtml.cs?name=snippet1&highlight=14-16)]

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="e3bd6-114">Отключить загрузку автоматическое размещение загрузки сборок</span><span class="sxs-lookup"><span data-stu-id="e3bd6-114">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="e3bd6-115">Чтобы отключить автоматическое загрузку загрузки сборок, на котором размещается двумя способами.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-115">There are two ways to disable the automatic loading of hosting startup assemblies:</span></span>

* <span data-ttu-id="e3bd6-116">Задать [избежать запуска размещения](xref:fundamentals/hosting#prevent-hosting-startup) размещения параметра конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-116">Set the [Prevent Hosting Startup](xref:fundamentals/hosting#prevent-hosting-startup) host configuration setting.</span></span>
* <span data-ttu-id="e3bd6-117">Задать `ASPNETCORE_preventHostingStartup` переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-117">Set the `ASPNETCORE_preventHostingStartup` environment variable.</span></span>

<span data-ttu-id="e3bd6-118">Если параметр узла или переменная среды имеет значение `true` или `1`, размещения не автоматической загрузки сборки при запуске.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-118">When either the host setting or the environment variable is set to `true` or `1`, hosting startup assemblies aren't automatically loaded.</span></span> <span data-ttu-id="e3bd6-119">Если заданы оба параметра узла управляет поведением.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-119">If both are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="e3bd6-120">Отключение размещения сборок при запуске, с помощью узла параметр или переменную среды глобально отключает и может отключить некоторые функции приложения.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-120">Disabling hosting startup assemblies using the host setting or environment variable disables them globally and may disable several features of an app.</span></span> <span data-ttu-id="e3bd6-121">В настоящее время невозможно выборочно отключить размещения стартовой сборки, не библиотеке есть свой собственный вариант конфигурации добавлены в библиотеке.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-121">It isn't currently possible to selectively disable a hosting startup assembly added by a library unless the library offers its own configuration option.</span></span> <span data-ttu-id="e3bd6-122">Следующем выпуске будет предлагать возможность выборочно отключить размещения сборок при запуске (в разделе [GitHub выдачи aspnet или размещения #1243](https://github.com/aspnet/Hosting/pull/1243)).</span><span class="sxs-lookup"><span data-stu-id="e3bd6-122">A future release will offer the ability to selectively disable hosting startup assemblies (see [GitHub issue aspnet/Hosting #1243](https://github.com/aspnet/Hosting/pull/1243)).</span></span>

## <a name="implement-ihostingstartup-features"></a><span data-ttu-id="e3bd6-123">Реализации функций IHostingStartup</span><span class="sxs-lookup"><span data-stu-id="e3bd6-123">Implement IHostingStartup features</span></span>

### <a name="create-the-assembly"></a><span data-ttu-id="e3bd6-124">Создать сборку</span><span class="sxs-lookup"><span data-stu-id="e3bd6-124">Create the assembly</span></span>

<span data-ttu-id="e3bd6-125">`IHostingStartup` Функция развертывается как сборки с консольного приложения без точки входа.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-125">An `IHostingStartup` feature is deployed as an assembly based on a console app without an entry point.</span></span> <span data-ttu-id="e3bd6-126">Ссылки на сборку [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) пакета:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-126">The assembly references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[Main](ihostingstartup/snapshot_sample/StartupFeature.csproj)]

<span data-ttu-id="e3bd6-127">Объект [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) атрибут идентифицирует класс как реализация `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="e3bd6-127">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="e3bd6-128">В следующем примере пространство имен является `StartupFeature`, а класс является `StartupFeatureHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-128">In the following example, the namespace is `StartupFeature`, and the class is `StartupFeatureHostingStartup`:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet1)]

<span data-ttu-id="e3bd6-129">Класс реализует `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-129">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="e3bd6-130">Класс [Настройка](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует метод [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления в приложение функций:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-130">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add features to an app:</span></span>

[!code-csharp[Main](ihostingstartup/snapshot_sample/StartupFeature.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="e3bd6-131">При построении `IHostingStartup` зависимости файла проекта (*\*. deps.json*) задает `runtime` расположение сборки для *bin* папки:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-131">When building an `IHostingStartup` project, the dependencies file (*\*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="e3bd6-132">Показана только часть файла.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-132">Only part of the file is shown.</span></span> <span data-ttu-id="e3bd6-133">Имя сборки, в примере является `StartupFeature`.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-133">The assembly name in the example is `StartupFeature`.</span></span>

### <a name="update-the-dependencies-file"></a><span data-ttu-id="e3bd6-134">Обновить файл зависимости</span><span class="sxs-lookup"><span data-stu-id="e3bd6-134">Update the dependencies file</span></span>

<span data-ttu-id="e3bd6-135">Среда выполнения расположение указывается в  *\*. deps.json* файл.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-135">The runtime location is specified in the *\*.deps.json* file.</span></span> <span data-ttu-id="e3bd6-136">Активный компонент `runtime` элемент должен указывать расположение сборки среды выполнения компонента.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-136">To active the feature, the `runtime` element must specify the location of the feature's runtime assembly.</span></span> <span data-ttu-id="e3bd6-137">Префикс `runtime` расположение с `lib/netcoreapp2.0/`:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-137">Prefix the `runtime` location with `lib/netcoreapp2.0/`:</span></span>

[!code-json[Main](ihostingstartup/snapshot_sample/StartupFeature2.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="e3bd6-138">В примере приложения, изменения  *\*. deps.json* файла выполняется путем [PowerShell](/powershell/scripting/powershell-scripting) сценария.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-138">In the sample app, modification of the *\*.deps.json* file is performed by a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span> <span data-ttu-id="e3bd6-139">Сценарий PowerShell автоматически запускается целевой объект сборки в файле проекта.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-139">The PowerShell script is automatically triggered by a build target in the project file.</span></span>

### <a name="feature-activation"></a><span data-ttu-id="e3bd6-140">Активация компонента</span><span class="sxs-lookup"><span data-stu-id="e3bd6-140">Feature activation</span></span>

<span data-ttu-id="e3bd6-141">**Поместите файл сборки**</span><span class="sxs-lookup"><span data-stu-id="e3bd6-141">**Place the assembly file**</span></span>

<span data-ttu-id="e3bd6-142">`IHostingStartup` Реализация файла сборки должно быть *bin*-развернут в приложении или в [хранилище времени выполнения](/dotnet/core/deploying/runtime-store):</span><span class="sxs-lookup"><span data-stu-id="e3bd6-142">The `IHostingStartup` implementation's assembly file must be *bin*-deployed in the app or placed in the [runtime store](/dotnet/core/deploying/runtime-store):</span></span>

<span data-ttu-id="e3bd6-143">Для использования на пользователя поместите сборку в профиль пользователя в хранилище среды выполнения:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-143">For per-user use, place the assembly in the user profile's runtime store at:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="e3bd6-144">Для использования глобального поместите сборку в хранилище установки .NET Core времени выполнения:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-144">For global use, place the assembly in the .NET Core installation's runtime store:</span></span>

```
<DRIVE>\Program Files\dotnet\store\x64\netcoreapp2.0\<FEATURE_ASSEMBLY_NAME>\<FEATURE_VERSION>\lib\netcoreapp2.0\
```

<span data-ttu-id="e3bd6-145">При развертывании сборки в хранилище среды выполнения, также могут быть развернуты файл символов, но не требуется для работы функции.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-145">When deploying the assembly to the runtime store, the symbols file may be deployed as well but isn't required for the feature to work.</span></span>

<span data-ttu-id="e3bd6-146">**Поместите файл зависимости**</span><span class="sxs-lookup"><span data-stu-id="e3bd6-146">**Place the dependencies file**</span></span>

<span data-ttu-id="e3bd6-147">Реализация  *\*. deps.json* файл должен находиться в доступном расположении.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-147">The implementation's *\*.deps.json* file must be in an accessible location.</span></span>

<span data-ttu-id="e3bd6-148">Для использования на пользователя, поместите файл в `additonalDeps` папку профиля пользователя `.dotnet` параметры:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-148">For per-user use, place the file in the `additonalDeps` folder of the user profile's `.dotnet` settings:</span></span> 

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="e3bd6-149">Для использования глобального, поместите файл в `additonalDeps` папку установки .NET Core:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-149">For global use, place the file in the `additonalDeps` folder of the .NET Core installation:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\
```

<span data-ttu-id="e3bd6-150">Обратите внимание на версию `2.0.0`, отражающее версию общей среды выполнения использует целевое приложение.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-150">Note the version, `2.0.0`, reflects the version of the shared runtime that the target app uses.</span></span> <span data-ttu-id="e3bd6-151">Общего времени выполнения отображается в  *\*. runtimeconfig.json* файл.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-151">The shared runtime is shown in the *\*.runtimeconfig.json* file.</span></span> <span data-ttu-id="e3bd6-152">В примере приложения общего времени выполнения задается в *HostingStartupSample.runtimeconfig.json* файла.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-152">In the sample app, the shared runtime is specified in the *HostingStartupSample.runtimeconfig.json* file.</span></span>

<span data-ttu-id="e3bd6-153">**Настройка переменных среды**</span><span class="sxs-lookup"><span data-stu-id="e3bd6-153">**Set environment variables**</span></span>

<span data-ttu-id="e3bd6-154">Задайте следующие переменные среды в контексте приложения, которое использует компонент.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-154">Set the following environment variables in the context of the app that uses the feature.</span></span>

<span data-ttu-id="e3bd6-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span><span class="sxs-lookup"><span data-stu-id="e3bd6-155">ASPNETCORE\_HOSTINGSTARTUPASSEMBLIES</span></span>

<span data-ttu-id="e3bd6-156">Только размещения запуска сборки проверяются на наличие `HostingStartupAttribute`.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-156">Only hosting startup assemblies are scanned for the `HostingStartupAttribute`.</span></span> <span data-ttu-id="e3bd6-157">Имя сборки реализации предоставляется в этой переменной среды.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-157">The assembly name of the implementation is provided in this environment variable.</span></span> <span data-ttu-id="e3bd6-158">Пример приложения это значение устанавливается равным `StartupDiagnostics`.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-158">The sample app sets this value to `StartupDiagnostics`.</span></span>

<span data-ttu-id="e3bd6-159">Значение можно также задать с помощью [размещения сборок запуска](xref:fundamentals/hosting#hosting-startup-assemblies) размещения параметра конфигурации.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-159">The value can also be set using the [Hosting Startup Assemblies](xref:fundamentals/hosting#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="e3bd6-160">DOTNET\_ДОПОЛНИТЕЛЬНЫЕ\_DEPS</span><span class="sxs-lookup"><span data-stu-id="e3bd6-160">DOTNET\_ADDITIONAL\_DEPS</span></span>

<span data-ttu-id="e3bd6-161">Расположение реализации  *\*. deps.json* файл.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-161">The location of the implementation's *\*.deps.json* file.</span></span>

<span data-ttu-id="e3bd6-162">Если файл помещается в профиле пользователя *.dotnet* папки для использования на пользователя:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-162">If the file is placed in the user profile's *.dotnet* folder for per-user use:</span></span>

```
<DRIVE>\Users\<USER>\.dotnet\x64\additionalDeps\
```

<span data-ttu-id="e3bd6-163">Если файл помещается в установку .NET Core для глобального использования, укажите полный путь к файлу:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-163">If the file is placed in the .NET Core installation for global use, provide the full path to the file:</span></span>

```
<DRIVE>\Program Files\dotnet\additionalDeps\<FEATURE_ASSEMBLY_NAME>\shared\Microsoft.NETCore.App\2.0.0\<FEATURE_ASSEMBLY_NAME>.deps.json
```

<span data-ttu-id="e3bd6-164">Пример приложения устанавливает это значение.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-164">The sample app sets this value to:</span></span>

```
%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\
```

<span data-ttu-id="e3bd6-165">Примеры для установки переменных среды для различных операционных систем см. в разделе [работа с несколькими средами](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="e3bd6-165">For examples of how to set environment variables for various operating systems, see [Working with multiple environments](xref:fundamentals/environments).</span></span>

## <a name="sample-app"></a><span data-ttu-id="e3bd6-166">Пример приложения</span><span class="sxs-lookup"><span data-stu-id="e3bd6-166">Sample app</span></span>

<span data-ttu-id="e3bd6-167">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) использует `IHostingStartup` для создания средства диагностики.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-167">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/ihostingstartup/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample)) uses `IHostingStartup` to create a diagnostics tool.</span></span> <span data-ttu-id="e3bd6-168">Добавляет два middlewares приложения во время запуска, которые предоставляют сведения диагностики:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-168">The tool adds two middlewares to the app at startup that provide diagnostic information:</span></span>

* <span data-ttu-id="e3bd6-169">Зарегистрированные службы</span><span class="sxs-lookup"><span data-stu-id="e3bd6-169">Registered services</span></span>
* <span data-ttu-id="e3bd6-170">Адрес: схему, узел, базовый путь, путь, строки запроса</span><span class="sxs-lookup"><span data-stu-id="e3bd6-170">Address: scheme, host, path base, path, query string</span></span>
* <span data-ttu-id="e3bd6-171">Подключение: удаленный IP, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента</span><span class="sxs-lookup"><span data-stu-id="e3bd6-171">Connection: remote IP, remote port, local IP, local port, client certificate</span></span>
* <span data-ttu-id="e3bd6-172">Заголовки запроса</span><span class="sxs-lookup"><span data-stu-id="e3bd6-172">Request headers</span></span>
* <span data-ttu-id="e3bd6-173">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="e3bd6-173">Environment variables</span></span>

<span data-ttu-id="e3bd6-174">Запуск образца:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-174">To run the sample:</span></span>

1. <span data-ttu-id="e3bd6-175">Запуск диагностики проект использует [PowerShell](/powershell/scripting/powershell-scripting) для изменения его *StartupDiagnostics.deps.json* файла.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-175">The Startup Diagnostic project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="e3bd6-176">PowerShell устанавливается по умолчанию в операционной системе Windows, начиная с Windows 7 с пакетом обновления 1 и Windows Server 2008 R2 SP1.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-176">PowerShell is installed by default on Windows OS starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="e3bd6-177">Для получения PowerShell на других платформах, обратитесь к [Установка Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span><span class="sxs-lookup"><span data-stu-id="e3bd6-177">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-windows-powershell).</span></span>
2. <span data-ttu-id="e3bd6-178">Выполните построение проекта запуска диагностики.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-178">Build the Startup Diagnostic project.</span></span> <span data-ttu-id="e3bd6-179">Целевой объект сборки в файле проекта:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-179">A build target in the project file:</span></span>
   * <span data-ttu-id="e3bd6-180">Перемещает сборки и файлы среды выполнения хранилище профиля пользователя символов.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-180">Moves the assembly and symbols files to the user profile's runtime store.</span></span>
   * <span data-ttu-id="e3bd6-181">Запускает сценарий PowerShell, чтобы изменить *StartupDiagnostics.deps.json* файла.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-181">Triggers the PowerShell script to modify the *StartupDiagnostics.deps.json* file.</span></span>
   * <span data-ttu-id="e3bd6-182">Перемещает *StartupDiagnostics.deps.json* файла профиля пользователя `additionalDeps` папки.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-182">Moves the *StartupDiagnostics.deps.json* file to the user profile's `additionalDeps` folder.</span></span>
3. <span data-ttu-id="e3bd6-183">Задание переменных среды:</span><span class="sxs-lookup"><span data-stu-id="e3bd6-183">Set the environment variables:</span></span>
    * <span data-ttu-id="e3bd6-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span><span class="sxs-lookup"><span data-stu-id="e3bd6-184">`ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`: `StartupDiagnostics`</span></span>
    * <span data-ttu-id="e3bd6-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span><span class="sxs-lookup"><span data-stu-id="e3bd6-185">`DOTNET_ADDITIONAL_DEPS`: `%UserProfile%\.dotnet\x64\additionalDeps\StartupDiagnostics\`</span></span>
4. <span data-ttu-id="e3bd6-186">Запуск образца приложения.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-186">Run the sample app.</span></span>
5. <span data-ttu-id="e3bd6-187">Запрос `/services` служб регистрации конечной точки для просмотра приложения.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-187">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="e3bd6-188">Запрос `/diag` конечной точки, чтобы просмотреть диагностические данные.</span><span class="sxs-lookup"><span data-stu-id="e3bd6-188">Request the `/diag` endpoint to see the diagnostic information.</span></span>
