---
title: Использование начальных сборок размещения в ASP.NET Core
author: guardrex
description: Узнайте, как улучшить приложение ASP.NET Core из внешней сборки, используя реализацию IHostingStartup.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/02/2019
uid: fundamentals/configuration/platform-specific-configuration
ms.openlocfilehash: 3be036d9b4fc6c9898faf14e8a60a8cc7a8683b7
ms.sourcegitcommit: b5e63714afc26e94be49a92619586df5189ed93a
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/02/2019
ms.locfileid: "68739551"
---
# <a name="use-hosting-startup-assemblies-in-aspnet-core"></a><span data-ttu-id="f8e05-103">Использование начальных сборок размещения в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f8e05-103">Use hosting startup assemblies in ASP.NET Core</span></span>

<span data-ttu-id="f8e05-104">Авторы [Люк Латэм](https://github.com/guardrex) (Luke Latham) и [Павел Крымец](https://github.com/pakrym) (Pavel Krymets)</span><span class="sxs-lookup"><span data-stu-id="f8e05-104">By [Luke Latham](https://github.com/guardrex) and [Pavel Krymets](https://github.com/pakrym)</span></span>

<span data-ttu-id="f8e05-105">Реализация [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (размещение при запуске) позволяет добавлять в приложение улучшения из внешней сборки при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-105">An [IHostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup) (hosting startup) implementation adds enhancements to an app at startup from an external assembly.</span></span> <span data-ttu-id="f8e05-106">Например, внешняя библиотека может использовать реализацию размещения при запуске, чтобы доставить дополнительные поставщики конфигурации или службы для приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-106">For example, an external library can use a hosting startup implementation to provide additional configuration providers or services to an app.</span></span>

<span data-ttu-id="f8e05-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="f8e05-107">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="hostingstartup-attribute"></a><span data-ttu-id="f8e05-108">Атрибут HostingStartup</span><span class="sxs-lookup"><span data-stu-id="f8e05-108">HostingStartup attribute</span></span>

<span data-ttu-id="f8e05-109">Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) указывает на наличие начальной сборки размещения, которая будет активирована во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-109">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute indicates the presence of a hosting startup assembly to activate at runtime.</span></span>

<span data-ttu-id="f8e05-110">Входная сборка или сборка, содержащая класс `Startup`, автоматически сканируется на наличие атрибута `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-110">The entry assembly or the assembly containing the `Startup` class is automatically scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="f8e05-111">Список сборок, в котором будет выполняться поиск атрибута `HostingStartup`, загружается во время выполнения из конфигурации в [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="f8e05-111">The list of assemblies to search for `HostingStartup` attributes is loaded at runtime from configuration in the [WebHostDefaults.HostingStartupAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupassemblieskey).</span></span> <span data-ttu-id="f8e05-112">Список сборок, которые необходимо исключить из обнаружения, загружается из [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span><span class="sxs-lookup"><span data-stu-id="f8e05-112">The list of assemblies to exclude from discovery is loaded from the [WebHostDefaults.HostingStartupExcludeAssembliesKey](/dotnet/api/microsoft.aspnetcore.hosting.webhostdefaults.hostingstartupexcludeassemblieskey).</span></span> <span data-ttu-id="f8e05-113">Дополнительные сведения см. в руководстве по [настройке Размещение начальных сборок](xref:fundamentals/host/web-host#hosting-startup-assemblies) и [Веб-узел: исключаемые начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="f8e05-113">For more information, see [Web Host: Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) and [Web Host: Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span></span>

<span data-ttu-id="f8e05-114">В следующем примере пространство имен для начальной сборки размещения — `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-114">In the following example, the namespace of the hosting startup assembly is `StartupEnhancement`.</span></span> <span data-ttu-id="f8e05-115">Класс, содержащий код запуска размещения, — `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="f8e05-115">The class containing the hosting startup code is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="f8e05-116">Атрибут `HostingStartup` обычно находится в файле класса реализации начальной сборки размещения `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-116">The `HostingStartup` attribute is typically located in the hosting startup assembly's `IHostingStartup` implementation class file.</span></span>

## <a name="discover-loaded-hosting-startup-assemblies"></a><span data-ttu-id="f8e05-117">Обнаружение загруженных начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="f8e05-117">Discover loaded hosting startup assemblies</span></span>

<span data-ttu-id="f8e05-118">Для обнаружения загруженных сборок размещения при запуске включите ведение журнала и проверьте журналы приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-118">To discover loaded hosting startup assemblies, enable logging and check the app's logs.</span></span> <span data-ttu-id="f8e05-119">В журнал вносятся ошибки, возникающие при загрузке сборок.</span><span class="sxs-lookup"><span data-stu-id="f8e05-119">Errors that occur when loading assemblies are logged.</span></span> <span data-ttu-id="f8e05-120">Загруженные начальные сборки размещения регистрируются на уровне отладки, также регистрируются все ошибки.</span><span class="sxs-lookup"><span data-stu-id="f8e05-120">Loaded hosting startup assemblies are logged at the Debug level, and all errors are logged.</span></span>

## <a name="disable-automatic-loading-of-hosting-startup-assemblies"></a><span data-ttu-id="f8e05-121">Отключение автоматической загрузки начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="f8e05-121">Disable automatic loading of hosting startup assemblies</span></span>

<span data-ttu-id="f8e05-122">Чтобы отключить автоматическую загрузку начальных сборок размещения, используйте один из следующих подходов:</span><span class="sxs-lookup"><span data-stu-id="f8e05-122">To disable automatic loading of hosting startup assemblies, use one of the following approaches:</span></span>

* <span data-ttu-id="f8e05-123">Чтобы предотвратить загрузку всех начальных сборок размещения, установите значение `true` или `1` для одного из следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="f8e05-123">To prevent all hosting startup assemblies from loading, set one of the following to `true` or `1`:</span></span>
  * <span data-ttu-id="f8e05-124">Параметр конфигурации узла [Запретить размещение при запуске](xref:fundamentals/host/web-host#prevent-hosting-startup).</span><span class="sxs-lookup"><span data-stu-id="f8e05-124">[Prevent Hosting Startup](xref:fundamentals/host/web-host#prevent-hosting-startup) host configuration setting.</span></span>
  * <span data-ttu-id="f8e05-125">Переменная среды `ASPNETCORE_PREVENTHOSTINGSTARTUP`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-125">`ASPNETCORE_PREVENTHOSTINGSTARTUP` environment variable.</span></span>
* <span data-ttu-id="f8e05-126">Чтобы предотвратить загрузку конкретных сборок размещения, установите строку, содержащую разделенный точками с запятой список сборок размещения, которые необходимо исключить при запуске, в качестве значения одного из следующих параметров:</span><span class="sxs-lookup"><span data-stu-id="f8e05-126">To prevent specific hosting startup assemblies from loading, set one of the following to a semicolon-delimited string of hosting startup assemblies to exclude at startup:</span></span>
  * <span data-ttu-id="f8e05-127">Параметр конфигурации узла [Исключаемые сборки размещения при запуске](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies).</span><span class="sxs-lookup"><span data-stu-id="f8e05-127">[Hosting Startup Exclude Assemblies](xref:fundamentals/host/web-host#hosting-startup-exclude-assemblies) host configuration setting.</span></span>
  * <span data-ttu-id="f8e05-128">Переменная среды `ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-128">`ASPNETCORE_HOSTINGSTARTUPEXCLUDEASSEMBLIES` environment variable.</span></span>

<span data-ttu-id="f8e05-129">Если заданы параметры конфигурации узла и переменная среды, на поведение влияют параметры узла.</span><span class="sxs-lookup"><span data-stu-id="f8e05-129">If both the host configuration setting and the environment variable are set, the host setting controls the behavior.</span></span>

<span data-ttu-id="f8e05-130">При отключении сборок размещения при запуске с использованием параметра узла или переменной среды сборка отключается на глобальном уровне, и также могут быть отключены некоторые характеристики приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-130">Disabling hosting startup assemblies using the host setting or environment variable disables the assembly globally and may disable several characteristics of an app.</span></span>

## <a name="project"></a><span data-ttu-id="f8e05-131">Проект</span><span class="sxs-lookup"><span data-stu-id="f8e05-131">Project</span></span>

<span data-ttu-id="f8e05-132">Создайте размещение при запуске для любого из указанных ниже типов проектов:</span><span class="sxs-lookup"><span data-stu-id="f8e05-132">Create a hosting startup with either of the following project types:</span></span>

* [<span data-ttu-id="f8e05-133">Библиотека классов</span><span class="sxs-lookup"><span data-stu-id="f8e05-133">Class library</span></span>](#class-library)
* [<span data-ttu-id="f8e05-134">Консольное приложение без точки входа</span><span class="sxs-lookup"><span data-stu-id="f8e05-134">Console app without an entry point</span></span>](#console-app-without-an-entry-point)

### <a name="class-library"></a><span data-ttu-id="f8e05-135">Библиотека классов</span><span class="sxs-lookup"><span data-stu-id="f8e05-135">Class library</span></span>

<span data-ttu-id="f8e05-136">Расширение размещения при запуске можно указать в библиотеке классов.</span><span class="sxs-lookup"><span data-stu-id="f8e05-136">A hosting startup enhancement can be provided in a class library.</span></span> <span data-ttu-id="f8e05-137">Библиотека содержит атрибут `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-137">The library contains a `HostingStartup` attribute.</span></span>

<span data-ttu-id="f8e05-138">[Пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) включает приложение Razor Pages *HostingStartupApp* и библиотеку классов *HostingStartupLibrary*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-138">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) includes a Razor Pages app, *HostingStartupApp*, and a class library, *HostingStartupLibrary*.</span></span> <span data-ttu-id="f8e05-139">Библиотека классов:</span><span class="sxs-lookup"><span data-stu-id="f8e05-139">The class library:</span></span>

* <span data-ttu-id="f8e05-140">Содержит класс размещения при запуске `ServiceKeyInjection`, который реализует `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-140">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="f8e05-141">`ServiceKeyInjection` добавляет пару строк службы в конфигурацию приложения с помощью поставщика конфигурации, размещаемой в памяти ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span><span class="sxs-lookup"><span data-stu-id="f8e05-141">`ServiceKeyInjection` adds a pair of service strings to the app's configuration using the in-memory configuration provider ([AddInMemoryCollection](/dotnet/api/microsoft.extensions.configuration.memoryconfigurationbuilderextensions.addinmemorycollection)).</span></span>
* <span data-ttu-id="f8e05-142">Включает атрибут `HostingStartup`, определяющий пространство имен и класс размещения при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-142">Includes a `HostingStartup` attribute that identifies the hosting startup's namespace and class.</span></span>

<span data-ttu-id="f8e05-143">Метод `ServiceKeyInjection` класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение.</span><span class="sxs-lookup"><span data-stu-id="f8e05-143">The `ServiceKeyInjection` class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span>

<span data-ttu-id="f8e05-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8e05-144">*HostingStartupLibrary/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupLibrary/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="f8e05-145">Страница индекса приложения считывает и отображает значения конфигурации для двух ключей, устанавливаемых начальной сборкой размещения библиотеки класса:</span><span class="sxs-lookup"><span data-stu-id="f8e05-145">The app's Index page reads and renders the configuration values for the two keys set by the class library's hosting startup assembly:</span></span>

<span data-ttu-id="f8e05-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8e05-146">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=5-6,11-12)]

<span data-ttu-id="f8e05-147">[Пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) также включает проект пакета NuGet, предоставляющий отдельное размещение при запуске *HostingStartupPackage*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-147">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) also includes a NuGet package project that provides a separate hosting startup, *HostingStartupPackage*.</span></span> <span data-ttu-id="f8e05-148">Характеристики пакета аналогичны характеристикам библиотеки классов, приведенным ранее.</span><span class="sxs-lookup"><span data-stu-id="f8e05-148">The package has the same characteristics of the class library described earlier.</span></span> <span data-ttu-id="f8e05-149">Пакет:</span><span class="sxs-lookup"><span data-stu-id="f8e05-149">The package:</span></span>

* <span data-ttu-id="f8e05-150">Содержит класс размещения при запуске `ServiceKeyInjection`, который реализует `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-150">Contains a hosting startup class, `ServiceKeyInjection`, which implements `IHostingStartup`.</span></span> <span data-ttu-id="f8e05-151">`ServiceKeyInjection` добавляет пару строк службы в конфигурацию приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-151">`ServiceKeyInjection` adds a pair of service strings to the app's configuration.</span></span>
* <span data-ttu-id="f8e05-152">Включает атрибут `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-152">Includes a `HostingStartup` attribute.</span></span>

<span data-ttu-id="f8e05-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8e05-153">*HostingStartupPackage/ServiceKeyInjection.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupPackage/ServiceKeyInjection.cs?name=snippet1)]

<span data-ttu-id="f8e05-154">Страница индекса приложения считывает и отображает значения конфигурации для двух ключей, устанавливаемых начальной сборкой размещения пакета:</span><span class="sxs-lookup"><span data-stu-id="f8e05-154">The app's Index page reads and renders the configuration values for the two keys set by the package's hosting startup assembly:</span></span>

<span data-ttu-id="f8e05-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span><span class="sxs-lookup"><span data-stu-id="f8e05-155">*HostingStartupApp/Pages/Index.cshtml.cs*:</span></span>

[!code-csharp[](platform-specific-configuration/samples/2.x/HostingStartupApp/Pages/Index.cshtml.cs?name=snippet1&highlight=7-8,13-14)]

### <a name="console-app-without-an-entry-point"></a><span data-ttu-id="f8e05-156">Консольное приложение без точки входа</span><span class="sxs-lookup"><span data-stu-id="f8e05-156">Console app without an entry point</span></span>

<span data-ttu-id="f8e05-157">*Этот подход может использоваться только для приложений .NET Core, но не для приложений .NET Framework.*</span><span class="sxs-lookup"><span data-stu-id="f8e05-157">*This approach is only available for .NET Core apps, not .NET Framework.*</span></span>

<span data-ttu-id="f8e05-158">Расширение динамического размещения при запуске, для активации которого не требуется ссылка во время компиляции, может быть реализовано в консольном приложении без точки входа, содержащем атрибут `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-158">A dynamic hosting startup enhancement that doesn't require a compile-time reference for activation can be provided in a console app without an entry point that contains a `HostingStartup` attribute.</span></span> <span data-ttu-id="f8e05-159">При публикации консольного приложения создается начальная сборка размещения, которая может использоваться в хранилище среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-159">Publishing the console app produces a hosting startup assembly that can be consumed from the runtime store.</span></span>

<span data-ttu-id="f8e05-160">В этом процессе используется консольное приложение без точки входа, так как:</span><span class="sxs-lookup"><span data-stu-id="f8e05-160">A console app without an entry point is used in this process because:</span></span>

* <span data-ttu-id="f8e05-161">Файл зависимостей необходим для функционирования размещения при запуске в начальной сборке размещения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-161">A dependencies file is required to consume the hosting startup in the hosting startup assembly.</span></span> <span data-ttu-id="f8e05-162">Файл зависимостей является ресурсом исполняемого приложения, который создается путем публикации приложения, а не библиотеки.</span><span class="sxs-lookup"><span data-stu-id="f8e05-162">A dependencies file is a runnable app asset that's produced by publishing an app, not a library.</span></span>
* <span data-ttu-id="f8e05-163">Библиотеку невозможно добавить непосредственно в [хранилище пакетов среды выполнения](/dotnet/core/deploying/runtime-store), для которого требуется запускаемый проект для общей среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-163">A library can't be added directly to the [runtime package store](/dotnet/core/deploying/runtime-store), which requires a runnable project that targets the shared runtime.</span></span>

<span data-ttu-id="f8e05-164">В ходе создания динамического размещения при запуске:</span><span class="sxs-lookup"><span data-stu-id="f8e05-164">In the creation of a dynamic hosting startup:</span></span>

* <span data-ttu-id="f8e05-165">Начальная сборка размещения создается из консольного приложения без точки входа, которое:</span><span class="sxs-lookup"><span data-stu-id="f8e05-165">A hosting startup assembly is created from the console app without an entry point that:</span></span>
  * <span data-ttu-id="f8e05-166">содержит класс с реализацией `IHostingStartup`;</span><span class="sxs-lookup"><span data-stu-id="f8e05-166">Includes a class that contains the `IHostingStartup` implementation.</span></span>
  * <span data-ttu-id="f8e05-167">содержит атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) для определения класса реализации `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-167">Includes a [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute to identify the `IHostingStartup` implementation class.</span></span>
* <span data-ttu-id="f8e05-168">Консольное приложение публикуется для получения зависимостей начальной загрузки.</span><span class="sxs-lookup"><span data-stu-id="f8e05-168">The console app is published to obtain the hosting startup's dependencies.</span></span> <span data-ttu-id="f8e05-169">После публикации консольного приложения неиспользуемые зависимости исключаются из файла зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f8e05-169">A consequence of publishing the console app is that unused dependencies are trimmed from the dependencies file.</span></span>
* <span data-ttu-id="f8e05-170">Файл зависимостей изменяется, чтобы задать расположение среды выполнения начальной сборки размещения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-170">The dependencies file is modified to set the runtime location of the hosting startup assembly.</span></span>
* <span data-ttu-id="f8e05-171">Начальная сборка размещения и соответствующий файл зависимостей размещаются в хранилище пакетов среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-171">The hosting startup assembly and its dependencies file is placed into the runtime package store.</span></span> <span data-ttu-id="f8e05-172">Чтобы можно было обнаружить начальную сборку размещения и ее файл зависимостей, они указываются в паре переменных среды.</span><span class="sxs-lookup"><span data-stu-id="f8e05-172">To discover the hosting startup assembly and its dependencies file, they're listed in a pair of environment variables.</span></span>

<span data-ttu-id="f8e05-173">Консольное приложение ссылается на пакет [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/):</span><span class="sxs-lookup"><span data-stu-id="f8e05-173">The console app references the [Microsoft.AspNetCore.Hosting.Abstractions](https://www.nuget.org/packages/Microsoft.AspNetCore.Hosting.Abstractions/) package:</span></span>

[!code-xml[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.csproj)]

<span data-ttu-id="f8e05-174">Атрибут [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) определяет класс как реализацию `IHostingStartup` для загрузки и выполнения при построении [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span><span class="sxs-lookup"><span data-stu-id="f8e05-174">A [HostingStartup](/dotnet/api/microsoft.aspnetcore.hosting.hostingstartupattribute) attribute identifies a class as an implementation of `IHostingStartup` for loading and execution when building the [IWebHost](/dotnet/api/microsoft.aspnetcore.hosting.iwebhost).</span></span> <span data-ttu-id="f8e05-175">В следующем примере используется пространство имен `StartupEnhancement` и класс `StartupEnhancementHostingStartup`:</span><span class="sxs-lookup"><span data-stu-id="f8e05-175">In the following example, the namespace is `StartupEnhancement`, and the class is `StartupEnhancementHostingStartup`:</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet1)]

<span data-ttu-id="f8e05-176">Класс реализует `IHostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-176">A class implements `IHostingStartup`.</span></span> <span data-ttu-id="f8e05-177">Метод класса [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) использует [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) для добавления улучшений в приложение.</span><span class="sxs-lookup"><span data-stu-id="f8e05-177">The class's [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) method uses an [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) to add enhancements to an app.</span></span> <span data-ttu-id="f8e05-178">`IHostingStartup.Configure` в начальной сборке размещения вызывается средой выполнения до `Startup.Configure` в пользовательском коде, что позволяет пользовательскому коду перезаписать конфигурацию, предоставленную начальной сборкой размещения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-178">`IHostingStartup.Configure` in the hosting startup assembly is called by the runtime before `Startup.Configure` in user code, which allows user code to overwrite any configuration provided by the hosting startup assembly.</span></span>

[!code-csharp[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement.cs?name=snippet2&highlight=3,5)]

<span data-ttu-id="f8e05-179">При создании проекта `IHostingStartup` файл зависимостей ( *.deps.json*) задает расположение `runtime` для сборки в папке *bin*:</span><span class="sxs-lookup"><span data-stu-id="f8e05-179">When building an `IHostingStartup` project, the dependencies file (*.deps.json*) sets the `runtime` location of the assembly to the *bin* folder:</span></span>

[!code-json[](platform-specific-configuration/samples-snapshot/2.x/StartupEnhancement1.deps.json?range=2-13&highlight=8)]

<span data-ttu-id="f8e05-180">Показана только часть файла.</span><span class="sxs-lookup"><span data-stu-id="f8e05-180">Only part of the file is shown.</span></span> <span data-ttu-id="f8e05-181">Имя сборки в примере — `StartupEnhancement`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-181">The assembly name in the example is `StartupEnhancement`.</span></span>

## <a name="configuration-provided-by-the-hosting-startup"></a><span data-ttu-id="f8e05-182">Конфигурация, предоставляемая размещением при запуске</span><span class="sxs-lookup"><span data-stu-id="f8e05-182">Configuration provided by the hosting startup</span></span>

<span data-ttu-id="f8e05-183">Существует два подхода к подготовке конфигурации в зависимости от того, какой конфигурацией вы хотите отдать приоритет — конфигурации размещения при запуске или конфигурации приложения:</span><span class="sxs-lookup"><span data-stu-id="f8e05-183">There are two approaches to handling configuration depending on whether you want the hosting startup's configuration to take precedence or the app's configuration to take precedence:</span></span>

1. <span data-ttu-id="f8e05-184">Задайте конфигурацию приложению, используя <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> для загрузки конфигурации после выполнения делегатов приложения <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f8e05-184">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> to load the configuration after the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="f8e05-185">При таком подходе конфигурация размещения при запуске будет иметь приоритет над конфигурацией приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-185">Hosting startup configuration takes priority over the app's configuration using this approach.</span></span>
1. <span data-ttu-id="f8e05-186">Задайте конфигурацию приложению, используя <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> для загрузки конфигурации до выполнения делегатов приложения <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*>.</span><span class="sxs-lookup"><span data-stu-id="f8e05-186">Provide configuration to the app using <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseConfiguration*> to load the configuration before the app's <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder.ConfigureAppConfiguration*> delegates execute.</span></span> <span data-ttu-id="f8e05-187">При таком подходе конфигурация приложения будет иметь приоритет над конфигурацией размещения при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-187">The app's configuration values take priority over those provided by the hosting startup using this approach.</span></span>

```csharp
public class ConfigurationInjection : IHostingStartup
{
    public void Configure(IWebHostBuilder builder)
    {
        Dictionary<string, string> dict;

        builder.ConfigureAppConfiguration(config =>
        {
            dict = new Dictionary<string, string>
            {
                {"ConfigurationKey1", 
                    "From IHostingStartup: Higher priority than the app's configuration."},
            };

            config.AddInMemoryCollection(dict);
        });

        dict = new Dictionary<string, string>
        {
            {"ConfigurationKey2", 
                "From IHostingStartup: Lower priority than the app's configuration."},
        };

        var builtConfig = new ConfigurationBuilder()
            .AddInMemoryCollection(dict)
            .Build();

        builder.UseConfiguration(builtConfig);
    }
}
```

## <a name="specify-the-hosting-startup-assembly"></a><span data-ttu-id="f8e05-188">Указание начальных сборок размещения</span><span class="sxs-lookup"><span data-stu-id="f8e05-188">Specify the hosting startup assembly</span></span>

<span data-ttu-id="f8e05-189">Для библиотеки класса или размещения при запуске с помощью консольного приложения укажите имя начальной сборки размещения в переменной среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-189">For either a class library- or console app-supplied hosting startup, specify the hosting startup assembly's name in the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span> <span data-ttu-id="f8e05-190">Переменная среды — это список сборок, разделенный точками с запятой.</span><span class="sxs-lookup"><span data-stu-id="f8e05-190">The environment variable is a semicolon-delimited list of assemblies.</span></span>

<span data-ttu-id="f8e05-191">Только начальные сборки размещения проверяются на наличие атрибута `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-191">Only hosting startup assemblies are scanned for the `HostingStartup` attribute.</span></span> <span data-ttu-id="f8e05-192">Для примера приложения *HostingStartupApp* необходимо установить следующее значение для переменной среды, чтобы обнаружить сборки размещения при запуске, описанные ранее:</span><span class="sxs-lookup"><span data-stu-id="f8e05-192">For the sample app, *HostingStartupApp*, to discover the hosting startups described earlier, the environment variable is set to the following value:</span></span>

```
HostingStartupLibrary;HostingStartupPackage;StartupDiagnostics
```

<span data-ttu-id="f8e05-193">Начальную сборку размещения также можно задать с помощью параметра конфигурации узла [Начальные сборки размещения](xref:fundamentals/host/web-host#hosting-startup-assemblies).</span><span class="sxs-lookup"><span data-stu-id="f8e05-193">A hosting startup assembly can also be set using the [Hosting Startup Assemblies](xref:fundamentals/host/web-host#hosting-startup-assemblies) host configuration setting.</span></span>

<span data-ttu-id="f8e05-194">При наличии нескольких стартовых сборок размещения их методы [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) выполняются в порядке расположения сборок.</span><span class="sxs-lookup"><span data-stu-id="f8e05-194">When multiple hosting startup assembles are present, their [Configure](/dotnet/api/microsoft.aspnetcore.hosting.ihostingstartup.configure) methods are executed in the order that the assemblies are listed.</span></span>

## <a name="activation"></a><span data-ttu-id="f8e05-195">Активация</span><span class="sxs-lookup"><span data-stu-id="f8e05-195">Activation</span></span>

<span data-ttu-id="f8e05-196">Ниже приведены варианты активации размещения при запуске:</span><span class="sxs-lookup"><span data-stu-id="f8e05-196">Options for hosting startup activation are:</span></span>

* <span data-ttu-id="f8e05-197">[Хранилище среды выполнения](#runtime-store) &ndash; для активации не требуется ссылка во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="f8e05-197">[Runtime store](#runtime-store) &ndash; Activation doesn't require a compile-time reference for activation.</span></span> <span data-ttu-id="f8e05-198">Пример приложения помещает начальную сборку размещения и файлы зависимостей в папку *deployment*, чтобы облегчить развертывание размещения при запуске в среде с несколькими компьютерами.</span><span class="sxs-lookup"><span data-stu-id="f8e05-198">The sample app places the hosting startup assembly and dependencies files into a folder, *deployment*, to facilitate deployment of the hosting startup in a multimachine environment.</span></span> <span data-ttu-id="f8e05-199">Папка *deployment* также включает сценарий PowerShell, который создает или изменяет переменные среды в системе развертывания, чтобы включить размещение при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-199">The *deployment* folder also includes a PowerShell script that creates or modifies environment variables on the deployment system to enable the hosting startup.</span></span>
* <span data-ttu-id="f8e05-200">Ссылки во время компиляции, необходимые для активации</span><span class="sxs-lookup"><span data-stu-id="f8e05-200">Compile-time reference required for activation</span></span>
  * [<span data-ttu-id="f8e05-201">Пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="f8e05-201">NuGet package</span></span>](#nuget-package)
  * [<span data-ttu-id="f8e05-202">Папка bin проекта</span><span class="sxs-lookup"><span data-stu-id="f8e05-202">Project bin folder</span></span>](#project-bin-folder)

### <a name="runtime-store"></a><span data-ttu-id="f8e05-203">Хранилище среды выполнения</span><span class="sxs-lookup"><span data-stu-id="f8e05-203">Runtime store</span></span>

<span data-ttu-id="f8e05-204">Реализация размещения при запуске помещается в [хранилище среды выполнения](/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="f8e05-204">The hosting startup implementation is placed in the [runtime store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="f8e05-205">Ссылка на сборку во время компиляции не требуется расширенному приложению.</span><span class="sxs-lookup"><span data-stu-id="f8e05-205">A compile-time reference to the assembly isn't required by the enhanced app.</span></span>

<span data-ttu-id="f8e05-206">После начальной сборки размещения создается хранилище среды выполнения с помощью файла манифеста проекта и команды [dotnet store](/dotnet/core/tools/dotnet-store).</span><span class="sxs-lookup"><span data-stu-id="f8e05-206">After the hosting startup is built, a runtime store is generated using the manifest project file and the [dotnet store](/dotnet/core/tools/dotnet-store) command.</span></span>

```console
dotnet store --manifest {MANIFEST FILE} --runtime {RUNTIME IDENTIFIER} --output {OUTPUT LOCATION} --skip-optimization
```

<span data-ttu-id="f8e05-207">В примере приложения (проект *RuntimeStore*) используется такая команда:</span><span class="sxs-lookup"><span data-stu-id="f8e05-207">In the sample app (*RuntimeStore* project) the following command is used:</span></span>

``` console
dotnet store --manifest store.manifest.csproj --runtime win7-x64 --output ./deployment/store --skip-optimization
```

<span data-ttu-id="f8e05-208">Чтобы среда выполнения могла обнаружить хранилище среды выполнения, расположение хранилища среды выполнения добавляется к переменной среды `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-208">For the runtime to discover the runtime store, the runtime store's location is added to the `DOTNET_SHARED_STORE` environment variable.</span></span>

<span data-ttu-id="f8e05-209">**Изменение и размещение файла зависимостей размещения при запуске**</span><span class="sxs-lookup"><span data-stu-id="f8e05-209">**Modify and place the hosting startup's dependencies file**</span></span>

<span data-ttu-id="f8e05-210">Чтобы активировать расширение без ссылки на пакет расширения, укажите дополнительные зависимости в среде выполнения с помощью `additionalDeps`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-210">To activate the enhancement without a package reference to the enhancement, specify additional dependencies to the runtime with `additionalDeps`.</span></span> <span data-ttu-id="f8e05-211">`additionalDeps` предоставляет следующие возможности:</span><span class="sxs-lookup"><span data-stu-id="f8e05-211">`additionalDeps` allows you to:</span></span>

* <span data-ttu-id="f8e05-212">Расширять граф библиотеки приложения, предоставляя набор дополнительных файлов *.deps.json* для объединения с собственным файлом *.deps.json* приложения при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-212">Extend the app's library graph by providing a set of additional *.deps.json* files to merge with the app's own *.deps.json* file on startup.</span></span>
* <span data-ttu-id="f8e05-213">Обнаруживать и скачивать начальную сборку размещения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-213">Make the hosting startup assembly discoverable and loadable.</span></span>

<span data-ttu-id="f8e05-214">Рекомендуемые действия при создании файла дополнительных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f8e05-214">The recommended approach for generating the additional dependencies file is to:</span></span>

 1. <span data-ttu-id="f8e05-215">Выполните команду `dotnet publish` для файла манифеста хранилища среды выполнения, ссылка на который создавалась в предыдущем разделе.</span><span class="sxs-lookup"><span data-stu-id="f8e05-215">Execute `dotnet publish` on the runtime store manifest file referenced in the previous section.</span></span>
 1. <span data-ttu-id="f8e05-216">Удалите ссылку на манифест из библиотек и раздела `runtime` итогового файла *deps.json*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-216">Remove the manifest reference from libraries and the `runtime` section of the resulting *.deps.json* file.</span></span>

<span data-ttu-id="f8e05-217">В примере проекта свойство `store.manifest/1.0.0` удаляется из разделов `targets` и `libraries`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-217">In the example project, the `store.manifest/1.0.0` property is removed from the `targets` and `libraries` section:</span></span>

```json
{
  "runtimeTarget": {
    "name": ".NETCoreApp,Version=v2.1",
    "signature": "4ea77c7b75ad1895ae1ea65e6ba2399010514f99"
  },
  "compilationOptions": {},
  "targets": {
    ".NETCoreApp,Version=v2.1": {
      "store.manifest/1.0.0": {
        "dependencies": {
          "StartupDiagnostics": "1.0.0"
        },
        "runtime": {
          "store.manifest.dll": {}
        }
      },
      "StartupDiagnostics/1.0.0": {
        "runtime": {
          "lib/netcoreapp2.1/StartupDiagnostics.dll": {
            "assemblyVersion": "1.0.0.0",
            "fileVersion": "1.0.0.0"
          }
        }
      }
    }
  },
  "libraries": {
    "store.manifest/1.0.0": {
      "type": "project",
      "serviceable": false,
      "sha512": ""
    },
    "StartupDiagnostics/1.0.0": {
      "type": "package",
      "serviceable": true,
      "sha512": "sha512-oiQr60vBQW7+nBTmgKLSldj06WNLRTdhOZpAdEbCuapoZ+M2DJH2uQbRLvFT8EGAAv4TAKzNtcztpx5YOgBXQQ==",
      "path": "startupdiagnostics/1.0.0",
      "hashPath": "startupdiagnostics.1.0.0.nupkg.sha512"
    }
  }
}
```

<span data-ttu-id="f8e05-218">Поместите файл *.deps.json* в следующее расположение:</span><span class="sxs-lookup"><span data-stu-id="f8e05-218">Place the *.deps.json* file into the following location:</span></span>

```
{ADDITIONAL DEPENDENCIES PATH}/shared/{SHARED FRAMEWORK NAME}/{SHARED FRAMEWORK VERSION}/{ENHANCEMENT ASSEMBLY NAME}.deps.json
```

* <span data-ttu-id="f8e05-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; расположение, добавленное к переменной среды `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-219">`{ADDITIONAL DEPENDENCIES PATH}` &ndash; Location added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
* <span data-ttu-id="f8e05-220">`{SHARED FRAMEWORK NAME}` &ndash; общая платформа, требуемая для этого файла дополнительных зависимостей.</span><span class="sxs-lookup"><span data-stu-id="f8e05-220">`{SHARED FRAMEWORK NAME}` &ndash; Shared framework required for this additional dependencies file.</span></span>
* <span data-ttu-id="f8e05-221">`{SHARED FRAMEWORK VERSION}` &ndash; минимальная версия общей платформы.</span><span class="sxs-lookup"><span data-stu-id="f8e05-221">`{SHARED FRAMEWORK VERSION}` &ndash; Minimum shared framework version.</span></span>
* <span data-ttu-id="f8e05-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; имя сборки расширения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-222">`{ENHANCEMENT ASSEMBLY NAME}` &ndash; The enhancement's assembly name.</span></span>

<span data-ttu-id="f8e05-223">В примере приложения (проект *RuntimeStore*) файл дополнительных зависимостей помещается в следующее расположение:</span><span class="sxs-lookup"><span data-stu-id="f8e05-223">In the sample app (*RuntimeStore* project), the additional dependencies file is placed into the following location:</span></span>

```
additionalDeps/shared/Microsoft.AspNetCore.App/2.1.0/StartupDiagnostics.deps.json
```

<span data-ttu-id="f8e05-224">Чтобы среда выполнения могла обнаружить хранилище среды выполнения, расположение файла дополнительных зависимостей добавляется к переменной среды `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-224">For runtime to discover the runtime store location, the additional dependencies file location is added to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>

<span data-ttu-id="f8e05-225">В примере приложения (проект *RuntimeStore*) для сборки хранилища среды выполнения и создания файла дополнительных зависимостей используется скрипт [PowerShell](/powershell/scripting/powershell-scripting).</span><span class="sxs-lookup"><span data-stu-id="f8e05-225">In the sample app (*RuntimeStore* project), building the runtime store and generating the additional dependencies file is accomplished using a [PowerShell](/powershell/scripting/powershell-scripting) script.</span></span>

<span data-ttu-id="f8e05-226">Примеры установки переменных среды для различных операционных систем см. в разделе [Использование нескольких сред](xref:fundamentals/environments).</span><span class="sxs-lookup"><span data-stu-id="f8e05-226">For examples of how to set environment variables for various operating systems, see [Use multiple environments](xref:fundamentals/environments).</span></span>

<span data-ttu-id="f8e05-227">**Развертывание**</span><span class="sxs-lookup"><span data-stu-id="f8e05-227">**Deployment**</span></span>

<span data-ttu-id="f8e05-228">Чтобы упростить развертывание размещения при запуске в среде с несколькими компьютерами, пример приложения создает в опубликованном результате папку *deployment*. Эта папка содержит:</span><span class="sxs-lookup"><span data-stu-id="f8e05-228">To facilitate the deployment of a hosting startup in a multimachine environment, the sample app creates a *deployment* folder in published output that contains:</span></span>

* <span data-ttu-id="f8e05-229">Хранилище среды выполнения размещения при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-229">The hosting startup runtime store.</span></span>
* <span data-ttu-id="f8e05-230">Файл зависимостей размещения при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-230">The hosting startup dependencies file.</span></span>
* <span data-ttu-id="f8e05-231">Скрипт PowerShell, который создает или изменяет `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE` и `DOTNET_ADDITIONAL_DEPS` для поддержки активации размещения при запуске.</span><span class="sxs-lookup"><span data-stu-id="f8e05-231">A PowerShell script that creates or modifies the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`, `DOTNET_SHARED_STORE`, and `DOTNET_ADDITIONAL_DEPS` to support the activation of the hosting startup.</span></span> <span data-ttu-id="f8e05-232">Запустите сценарий из командной строки PowerShell с правами администратора в системе развертывания.</span><span class="sxs-lookup"><span data-stu-id="f8e05-232">Run the script from an administrative PowerShell command prompt on the deployment system.</span></span>

### <a name="nuget-package"></a><span data-ttu-id="f8e05-233">Пакет NuGet</span><span class="sxs-lookup"><span data-stu-id="f8e05-233">NuGet package</span></span>

<span data-ttu-id="f8e05-234">Расширение размещения при запуске можно указать в пакете NuGet.</span><span class="sxs-lookup"><span data-stu-id="f8e05-234">A hosting startup enhancement can be provided in a NuGet package.</span></span> <span data-ttu-id="f8e05-235">Пакет содержит атрибут `HostingStartup`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-235">The package has a `HostingStartup` attribute.</span></span> <span data-ttu-id="f8e05-236">Типы размещения при запуске, предоставляемые пакетом, становятся доступными для приложения с использованием одного из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="f8e05-236">The hosting startup types provided by the package are made available to the app using either of the following approaches:</span></span>

* <span data-ttu-id="f8e05-237">Файл проекта расширенного приложения создает ссылку на пакет для размещения при запуске в файле проекта приложения (ссылка во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="f8e05-237">The enhanced app's project file makes a package reference for the hosting startup in the app's project file (a compile-time reference).</span></span> <span data-ttu-id="f8e05-238">Со ссылкой во время компиляции начальная сборка размещения и все ее зависимости включаются в файл зависимостей приложения ( *.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="f8e05-238">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="f8e05-239">Этот подход применяется к пакету начальной сборки размещения, опубликованному на [nuget.org](https://www.nuget.org/).</span><span class="sxs-lookup"><span data-stu-id="f8e05-239">This approach applies to a hosting startup assembly package published to [nuget.org](https://www.nuget.org/).</span></span>
* <span data-ttu-id="f8e05-240">Файл зависимостей размещения при запуске становится доступным для расширенного приложения, как описано в разделе [Хранилище среды выполнения](#runtime-store) (без ссылок во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="f8e05-240">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>

<span data-ttu-id="f8e05-241">Дополнительные сведения о пакетах NuGet и хранилище среды выполнения см. в разделах:</span><span class="sxs-lookup"><span data-stu-id="f8e05-241">For more information on NuGet packages and the runtime store, see the following topics:</span></span>

* [<span data-ttu-id="f8e05-242">Создание пакета NuGet с помощью кроссплатформенных средств</span><span class="sxs-lookup"><span data-stu-id="f8e05-242">How to Create a NuGet Package with Cross Platform Tools</span></span>](/dotnet/core/deploying/creating-nuget-packages)
* [<span data-ttu-id="f8e05-243">Публикация пакетов</span><span class="sxs-lookup"><span data-stu-id="f8e05-243">Publishing packages</span></span>](/nuget/create-packages/publish-a-package)
* [<span data-ttu-id="f8e05-244">Хранилище пакетов среды выполнения</span><span class="sxs-lookup"><span data-stu-id="f8e05-244">Runtime package store</span></span>](/dotnet/core/deploying/runtime-store)

### <a name="project-bin-folder"></a><span data-ttu-id="f8e05-245">Папка bin проекта</span><span class="sxs-lookup"><span data-stu-id="f8e05-245">Project bin folder</span></span>

<span data-ttu-id="f8e05-246">Расширение размещения при запуске может быть представлено сборкой, разворачиваемой в папке *bin* расширенного приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-246">A hosting startup enhancement can be provided by a *bin*-deployed assembly in the enhanced app.</span></span> <span data-ttu-id="f8e05-247">Типы размещения при запуске, предоставляемые сборкой, становятся доступными для приложения с использованием одного из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="f8e05-247">The hosting startup types provided by the assembly are made available to the app using one of the following approaches:</span></span>

* <span data-ttu-id="f8e05-248">Файл проекта расширенного приложения создает ссылку сборки на размещение при запуске (ссылка во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="f8e05-248">The enhanced app's project file makes an assembly reference to the hosting startup (a compile-time reference).</span></span> <span data-ttu-id="f8e05-249">Со ссылкой во время компиляции начальная сборка размещения и все ее зависимости включаются в файл зависимостей приложения ( *.deps.json*).</span><span class="sxs-lookup"><span data-stu-id="f8e05-249">With the compile-time reference in place, the hosting startup assembly and all of its dependencies are incorporated into the app's dependency file (*.deps.json*).</span></span> <span data-ttu-id="f8e05-250">Этот подход применяется при вызове сценария развертывания для создания ссылки времени компиляции на сборку запуска размещения (*DLL*-файл) и перемещения сборки в одно из следующих мест:</span><span class="sxs-lookup"><span data-stu-id="f8e05-250">This approach applies when the deployment scenario calls for making a compile-time reference to the hosting startup's assembly (*.dll* file) and moving the assembly to either:</span></span>
  * <span data-ttu-id="f8e05-251">Проект, который будет использовать ее.</span><span class="sxs-lookup"><span data-stu-id="f8e05-251">The consuming project.</span></span>
  * <span data-ttu-id="f8e05-252">Расположение, доступное использующему проекту.</span><span class="sxs-lookup"><span data-stu-id="f8e05-252">A location accessible by the consuming project.</span></span>
* <span data-ttu-id="f8e05-253">Файл зависимостей размещения при запуске становится доступным для расширенного приложения, как описано в разделе [Хранилище среды выполнения](#runtime-store) (без ссылок во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="f8e05-253">The hosting startup's dependencies file is made available to the enhanced app as described in the [Runtime store](#runtime-store) section (without a compile-time reference).</span></span>
* <span data-ttu-id="f8e05-254">При нацеливании на .NET Framework сборка загружается в контексте загрузки по умолчанию, что для .NET Framework означает, что сборка находится в одном из следующих расположений:</span><span class="sxs-lookup"><span data-stu-id="f8e05-254">When targeting the .NET Framework, the assembly is loadable in the default load context, which on .NET Framework means that the assembly is located at either of the following locations:</span></span>
  * <span data-ttu-id="f8e05-255">Базовый путь приложения &ndash; Папка *bin*, в которой находится исполняемый файл приложения ( *.exe*).</span><span class="sxs-lookup"><span data-stu-id="f8e05-255">Application base path &ndash; The *bin* folder where the app's executable (*.exe*) is located.</span></span>
  * <span data-ttu-id="f8e05-256">Глобальный кэш сборок &ndash; В глобальном кэше сборок сохраняются сборки, которые могут использоваться несколькими приложениями .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f8e05-256">Global Assembly Cache (GAC) &ndash; The GAC stores assemblies that several .NET Framework apps share.</span></span> <span data-ttu-id="f8e05-257">Дополнительные сведения см. в разделе [Практическое руководство. Установка сборки в глобальный кэш сборок](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) в документации по .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="f8e05-257">For more information, see [How to: Install an assembly into the global assembly cache](/dotnet/framework/app-domains/how-to-install-an-assembly-into-the-gac) in the .NET Framework documentation.</span></span>

## <a name="sample-code"></a><span data-ttu-id="f8e05-258">Пример кода</span><span class="sxs-lookup"><span data-stu-id="f8e05-258">Sample code</span></span>

<span data-ttu-id="f8e05-259">В [примере кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([инструкции по скачиванию примера](xref:index#how-to-download-a-sample)) показаны сценарии реализации размещения при запуске:</span><span class="sxs-lookup"><span data-stu-id="f8e05-259">The [sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/platform-specific-configuration/samples/) ([how to download](xref:index#how-to-download-a-sample)) demonstrates hosting startup implementation scenarios:</span></span>

* <span data-ttu-id="f8e05-260">В каждой из двух сборок начального размещения (библиотеки классов) устанавливаются две пары "ключ-значение", хранящиеся в памяти:</span><span class="sxs-lookup"><span data-stu-id="f8e05-260">Two hosting startup assemblies (class libraries) set a pair of in-memory configuration key-value pairs each:</span></span>
  * <span data-ttu-id="f8e05-261">Пакет NuGet (*HostingStartupPackage*)</span><span class="sxs-lookup"><span data-stu-id="f8e05-261">NuGet package (*HostingStartupPackage*)</span></span>
  * <span data-ttu-id="f8e05-262">Библиотека классов (*HostingStartupLibrary*)</span><span class="sxs-lookup"><span data-stu-id="f8e05-262">Class library (*HostingStartupLibrary*)</span></span>
* <span data-ttu-id="f8e05-263">Размещения при запуске активируется из сборки среды выполнения, развертываемой из хранилища (*StartupDiagnostics*).</span><span class="sxs-lookup"><span data-stu-id="f8e05-263">A hosting startup is activated from a runtime store-deployed assembly (*StartupDiagnostics*).</span></span> <span data-ttu-id="f8e05-264">Эта сборка добавляет два вида ПО промежуточного слоя, которые предоставляют диагностические сведения о следующих показателях:</span><span class="sxs-lookup"><span data-stu-id="f8e05-264">The assembly adds two middlewares to the app at startup that provide diagnostic information on:</span></span>
  * <span data-ttu-id="f8e05-265">Зарегистрированные службы</span><span class="sxs-lookup"><span data-stu-id="f8e05-265">Registered services</span></span>
  * <span data-ttu-id="f8e05-266">Адрес (схема, узел, базовый путь, путь, строка запроса)</span><span class="sxs-lookup"><span data-stu-id="f8e05-266">Address (scheme, host, path base, path, query string)</span></span>
  * <span data-ttu-id="f8e05-267">Подключение (удаленный IP-адрес, удаленный порт, локальный IP-адрес, локальный порт, сертификат клиента)</span><span class="sxs-lookup"><span data-stu-id="f8e05-267">Connection (remote IP, remote port, local IP, local port, client certificate)</span></span>
  * <span data-ttu-id="f8e05-268">Заголовки запросов</span><span class="sxs-lookup"><span data-stu-id="f8e05-268">Request headers</span></span>
  * <span data-ttu-id="f8e05-269">Переменные среды</span><span class="sxs-lookup"><span data-stu-id="f8e05-269">Environment variables</span></span>

<span data-ttu-id="f8e05-270">Для выполнения образца:</span><span class="sxs-lookup"><span data-stu-id="f8e05-270">To run the sample:</span></span>

<span data-ttu-id="f8e05-271">**Активация из пакета NuGet**</span><span class="sxs-lookup"><span data-stu-id="f8e05-271">**Activation from a NuGet package**</span></span>

1. <span data-ttu-id="f8e05-272">Скомпилируйте пакет *HostingStartupPackage* с помощью команды [dotnet pack](/dotnet/core/tools/dotnet-pack).</span><span class="sxs-lookup"><span data-stu-id="f8e05-272">Compile the *HostingStartupPackage* package with the [dotnet pack](/dotnet/core/tools/dotnet-pack) command.</span></span>
1. <span data-ttu-id="f8e05-273">Добавьте имя сборки пакета *HostingStartupPackage* в переменную среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-273">Add the package's assembly name of the *HostingStartupPackage* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="f8e05-274">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f8e05-274">Compile and run the app.</span></span> <span data-ttu-id="f8e05-275">Ссылка на пакет появится в расширенном приложении (ссылка во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="f8e05-275">A package reference is present in the enhanced app (a compile-time reference).</span></span> <span data-ttu-id="f8e05-276">Объект `<PropertyGroup>` в файле проекта приложения определяет выходные данные проекта пакета ( *../ HostingStartupPackage/bin/Debug*) в качестве источника пакета.</span><span class="sxs-lookup"><span data-stu-id="f8e05-276">A `<PropertyGroup>` in the app's project file specifies the package project's output (*../HostingStartupPackage/bin/Debug*) as a package source.</span></span> <span data-ttu-id="f8e05-277">Это позволяет приложению использовать пакет без отправки пакета на сайт [nuget.org](https://www.nuget.org/). Дополнительные сведения см. в примечаниях в файле проекта HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="f8e05-277">This allows the app to use the package without uploading the package to [nuget.org](https://www.nuget.org/). For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <PropertyGroup>
     <RestoreSources>$(RestoreSources);https://api.nuget.org/v3/index.json;../HostingStartupPackage/bin/Debug</RestoreSources>
   </PropertyGroup>
   ```

1. <span data-ttu-id="f8e05-278">Обратите внимание, что ключевые значения конфигурации службы, отображаемой на индексной странице, соответствуют значениям, установленным с помощью метода пакета `ServiceKeyInjection.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-278">Observe that the service configuration key values rendered by the Index page match the values set by the package's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="f8e05-279">Если вы внесли изменения в проект *HostingStartupPackage* и перекомпилировали его, очистите локальные кэши пакета NuGet, чтобы убедиться, что *HostingStartupApp* получает обновленный пакет, а не устаревший пакет из локального кэша.</span><span class="sxs-lookup"><span data-stu-id="f8e05-279">If you make changes to the *HostingStartupPackage* project and recompile it, clear the local NuGet package caches to ensure that the *HostingStartupApp* receives the updated package and not a stale package from the local cache.</span></span> <span data-ttu-id="f8e05-280">Чтобы очистить локальные кэши NuGet, выполните следующую команду [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals):</span><span class="sxs-lookup"><span data-stu-id="f8e05-280">To clear the local NuGet caches, execute the following [dotnet nuget locals](/dotnet/core/tools/dotnet-nuget-locals) command:</span></span>

```console
dotnet nuget locals all --clear
```

<span data-ttu-id="f8e05-281">**Активации из библиотеки классов**</span><span class="sxs-lookup"><span data-stu-id="f8e05-281">**Activation from a class library**</span></span>

1. <span data-ttu-id="f8e05-282">Скомпилируйте библиотеку классов *HostingStartupLibrary* с помощью команды [dotnet build](/dotnet/core/tools/dotnet-build).</span><span class="sxs-lookup"><span data-stu-id="f8e05-282">Compile the *HostingStartupLibrary* class library with the [dotnet build](/dotnet/core/tools/dotnet-build) command.</span></span>
1. <span data-ttu-id="f8e05-283">Добавьте имя сборки библиотеки классов *HostingStartupLibrary* в переменную среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-283">Add the class library's assembly name of *HostingStartupLibrary* to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
1. <span data-ttu-id="f8e05-284">Разверните сборку библиотеки классов в папку *bin* приложения. Для этого скопируйте файл *HostingStartupLibrary.dll* из выходной папки компиляции библиотеки в папку *bin/Debug* приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-284">*bin*-deploy the class library's assembly to the app by copying the *HostingStartupLibrary.dll* file from the class library's compiled output to the app's *bin/Debug* folder.</span></span>
1. <span data-ttu-id="f8e05-285">Скомпилируйте и запустите приложение.</span><span class="sxs-lookup"><span data-stu-id="f8e05-285">Compile and run the app.</span></span> <span data-ttu-id="f8e05-286">`<ItemGroup>` в файле проекта приложения ссылается на сборку библиотеки класса ( *.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (ссылка во время компиляции).</span><span class="sxs-lookup"><span data-stu-id="f8e05-286">An `<ItemGroup>` in the app's project file references the class library's assembly (*.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll*) (a compile-time reference).</span></span> <span data-ttu-id="f8e05-287">Дополнительные сведения см. в примечаниях в файле проекта HostingStartupApp.</span><span class="sxs-lookup"><span data-stu-id="f8e05-287">For more information, see the notes in the HostingStartupApp's project file.</span></span>

   ```xml
   <ItemGroup>
     <Reference Include=".\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll">
       <HintPath>.\bin\Debug\netcoreapp2.1\HostingStartupLibrary.dll</HintPath>
       <SpecificVersion>False</SpecificVersion>
     </Reference>
   </ItemGroup>
   ```

1. <span data-ttu-id="f8e05-288">Обратите внимание, что ключевые значения конфигурации службы, отображаемой на индексной странице, соответствуют значениям, установленным с помощью метода библиотеки класса `ServiceKeyInjection.Configure`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-288">Observe that the service configuration key values rendered by the Index page match the values set by the class library's `ServiceKeyInjection.Configure` method.</span></span>

<span data-ttu-id="f8e05-289">**Активация из сборки среды выполнения, развернутой в хранилище**</span><span class="sxs-lookup"><span data-stu-id="f8e05-289">**Activation from a runtime store-deployed assembly**</span></span>

1. <span data-ttu-id="f8e05-290">В проекте *StartupDiagnostics* используется [PowerShell](/powershell/scripting/powershell-scripting) для изменения файла *StartupDiagnostics.deps.json*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-290">The *StartupDiagnostics* project uses [PowerShell](/powershell/scripting/powershell-scripting) to modify its *StartupDiagnostics.deps.json* file.</span></span> <span data-ttu-id="f8e05-291">PowerShell устанавливается по умолчанию в Windows начиная с Windows 7 с пакетом обновления 1 (SP1) и Windows Server 2008 R2 с пакетом обновления 1 (SP1).</span><span class="sxs-lookup"><span data-stu-id="f8e05-291">PowerShell is installed by default on Windows starting with Windows 7 SP1 and Windows Server 2008 R2 SP1.</span></span> <span data-ttu-id="f8e05-292">Для установки PowerShell на других платформах см. раздел [Установка Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span><span class="sxs-lookup"><span data-stu-id="f8e05-292">To obtain PowerShell on other platforms, see [Installing Windows PowerShell](/powershell/scripting/setup/installing-powershell#powershell-core).</span></span>
1. <span data-ttu-id="f8e05-293">Выполните сценарий *build.ps1*, находящийся в папке *RuntimeStore*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-293">Execute the *build.ps1* script in the *RuntimeStore* folder.</span></span> <span data-ttu-id="f8e05-294">Скрипт выполняет следующее:</span><span class="sxs-lookup"><span data-stu-id="f8e05-294">The script:</span></span>
   * <span data-ttu-id="f8e05-295">создает пакет `StartupDiagnostics`;</span><span class="sxs-lookup"><span data-stu-id="f8e05-295">Generates the `StartupDiagnostics` package.</span></span>
   * <span data-ttu-id="f8e05-296">создает хранилище среды выполнения для `StartupDiagnostics` в папке для *хранения*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-296">Generates the runtime store for `StartupDiagnostics` in the *store* folder.</span></span> <span data-ttu-id="f8e05-297">Для размещения при запуске, развернутом в Windows, команда `dotnet store` в сценарии использует [идентификатор среды выполнения (RID)](/dotnet/core/rid-catalog) `win7-x64`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-297">The `dotnet store` command in the script uses the `win7-x64` [runtime identifier (RID)](/dotnet/core/rid-catalog) for a hosting startup deployed to Windows.</span></span> <span data-ttu-id="f8e05-298">При выполнении размещения при запуске для другой среды выполнения укажите соответствующий RID в строке 37 скрипта.</span><span class="sxs-lookup"><span data-stu-id="f8e05-298">When providing the hosting startup for a different runtime, substitute the correct RID on line 37 of the script.</span></span>
   * <span data-ttu-id="f8e05-299">Создает `additionalDeps` для `StartupDiagnostics` в папке *additionalDeps/shared/Microsoft.AspNetCore.App/{Версия_общей_платформы}/* .</span><span class="sxs-lookup"><span data-stu-id="f8e05-299">Generates the `additionalDeps` for `StartupDiagnostics` in the *additionalDeps/shared/Microsoft.AspNetCore.App/{Shared Framework Version}/* folder.</span></span>
   * <span data-ttu-id="f8e05-300">Размещает файл *deploy.ps1* в папке для *развертывания*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-300">Places the *deploy.ps1* file in the *deployment* folder.</span></span>
1. <span data-ttu-id="f8e05-301">Выполните сценарий *deploy.ps1*, находящийся в папке *deployment*.</span><span class="sxs-lookup"><span data-stu-id="f8e05-301">Run the *deploy.ps1* script in the *deployment* folder.</span></span> <span data-ttu-id="f8e05-302">Скрипт присоединяет:</span><span class="sxs-lookup"><span data-stu-id="f8e05-302">The script appends:</span></span>
   * <span data-ttu-id="f8e05-303">`StartupDiagnostics` к переменной среды `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-303">`StartupDiagnostics` to the `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` environment variable.</span></span>
   * <span data-ttu-id="f8e05-304">Путь зависимостей размещения при запуске к переменной среды `DOTNET_ADDITIONAL_DEPS`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-304">The hosting startup dependencies path to the `DOTNET_ADDITIONAL_DEPS` environment variable.</span></span>
   * <span data-ttu-id="f8e05-305">Путь хранения среды выполнения к переменной среды `DOTNET_SHARED_STORE`.</span><span class="sxs-lookup"><span data-stu-id="f8e05-305">The runtime store path to the `DOTNET_SHARED_STORE` environment variable.</span></span>
1. <span data-ttu-id="f8e05-306">Выполните пример приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-306">Run the sample app.</span></span>
1. <span data-ttu-id="f8e05-307">Запросите конечную точку `/services` для просмотра зарегистрированных служб приложения.</span><span class="sxs-lookup"><span data-stu-id="f8e05-307">Request the `/services` endpoint to see the app's registered services.</span></span> <span data-ttu-id="f8e05-308">Запросите конечную точку `/diag` для просмотра диагностических сведений.</span><span class="sxs-lookup"><span data-stu-id="f8e05-308">Request the `/diag` endpoint to see the diagnostic information.</span></span>
