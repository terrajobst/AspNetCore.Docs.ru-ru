---
title: Visual Studio профилей публикации для развертывания приложения ASP.NET Core
author: rick-anderson
description: Сведения о создании профилей публикации в Visual Studio и использовать их для управления ASP.NET Core развертывания приложений на различных целевых объектов.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3dc858793cd4ddb2630d05a5084f4b7caeaa30eb
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="f72ff-103">Visual Studio профилей публикации для развертывания приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f72ff-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="f72ff-104">Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f72ff-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f72ff-105">В этом документе описывается использование Visual Studio 2017 г. Создание и использование профилей публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="f72ff-106">Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="f72ff-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="f72ff-107">Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="f72ff-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="f72ff-108">Следующий файл проекта был создан с помощью команды `dotnet new mvc`:</span><span class="sxs-lookup"><span data-stu-id="f72ff-108">The following project file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72ff-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72ff-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.0" />
  </ItemGroup>

  <ItemGroup>
    <DotNetCliToolReference Include="Microsoft.VisualStudio.Web.CodeGeneration.Tools" Version="2.0.0" />
  </ItemGroup>

</Project>
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72ff-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72ff-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.5" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.6" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="f72ff-111">`<Project>` Элемента `Sdk` атрибута выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="f72ff-111">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="f72ff-112">Импортирует файл свойств из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начале.</span><span class="sxs-lookup"><span data-stu-id="f72ff-112">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="f72ff-113">Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.</span><span class="sxs-lookup"><span data-stu-id="f72ff-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="f72ff-114">По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="f72ff-115">`Microsoft.NET.Sdk.Web` Зависит от пакета SDK:</span><span class="sxs-lookup"><span data-stu-id="f72ff-115">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="f72ff-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="f72ff-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="f72ff-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="f72ff-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="f72ff-118">Что вызывает следующие свойства и целевые объекты для импорта:</span><span class="sxs-lookup"><span data-stu-id="f72ff-118">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="f72ff-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="f72ff-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="f72ff-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="f72ff-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="f72ff-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span><span class="sxs-lookup"><span data-stu-id="f72ff-121">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="f72ff-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span><span class="sxs-lookup"><span data-stu-id="f72ff-122">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="f72ff-123">Опубликуйте право набор целевых объектов в зависимости от способа публикации используется Импорт целевых объектов.</span><span class="sxs-lookup"><span data-stu-id="f72ff-123">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="f72ff-124">При загрузке проекта Visual Studio или MSBuild выполняются следующие высокоуровневые действия:</span><span class="sxs-lookup"><span data-stu-id="f72ff-124">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="f72ff-125">сборка проекта;</span><span class="sxs-lookup"><span data-stu-id="f72ff-125">Build project</span></span>
* <span data-ttu-id="f72ff-126">вычисление файлов для публикации;</span><span class="sxs-lookup"><span data-stu-id="f72ff-126">Compute files to publish</span></span>
* <span data-ttu-id="f72ff-127">публикация файлов в месте назначения;</span><span class="sxs-lookup"><span data-stu-id="f72ff-127">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="f72ff-128">вычисление элементов проекта.</span><span class="sxs-lookup"><span data-stu-id="f72ff-128">Compute project items</span></span>

<span data-ttu-id="f72ff-129">После загрузки проекта вычисляются элементы проекта (файлы).</span><span class="sxs-lookup"><span data-stu-id="f72ff-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="f72ff-130">Порядок обработки файла определяется атрибутом `item type`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="f72ff-131">По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="f72ff-132">Файлы в списке элементов `Compile` компилируются.</span><span class="sxs-lookup"><span data-stu-id="f72ff-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="f72ff-133">`Content` Список элементов содержит файлы, которые публикуются в дополнение к выходных данных сборки.</span><span class="sxs-lookup"><span data-stu-id="f72ff-133">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="f72ff-134">По умолчанию файлы, соответствующие шаблону `wwwroot/**` включаются в `Content` элемента.</span><span class="sxs-lookup"><span data-stu-id="f72ff-134">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="f72ff-135">`wwwroot/\*\*` [Шаблон глобализацию](https://gruntjs.com/configuring-tasks#globbing-patterns) обозначает все файлы в *wwwroot* папки **и** вложенные папки.</span><span class="sxs-lookup"><span data-stu-id="f72ff-135">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="f72ff-136">Чтобы явно добавить файл к списку публикации, добавьте файл непосредственно в *.csproj* файла, как показано в [включаемых файлов](#include-files).</span><span class="sxs-lookup"><span data-stu-id="f72ff-136">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="f72ff-137">При выборе **публикации** кнопки в Visual Studio или при публикации из командной строки:</span><span class="sxs-lookup"><span data-stu-id="f72ff-137">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="f72ff-138">Вычисляются свойства и (или) элементы (файлы, требующие сборки).</span><span class="sxs-lookup"><span data-stu-id="f72ff-138">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="f72ff-139">**Visual Studio только**: пакеты NuGet восстанавливаются.</span><span class="sxs-lookup"><span data-stu-id="f72ff-139">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="f72ff-140">(Восстановление выполняется пользователем в интерфейсе командной строки.)</span><span class="sxs-lookup"><span data-stu-id="f72ff-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="f72ff-141">Выполняется сборка проекта.</span><span class="sxs-lookup"><span data-stu-id="f72ff-141">The project builds.</span></span>
* <span data-ttu-id="f72ff-142">Вычисляются публикуемые элементы (файлы, требующие публикации).</span><span class="sxs-lookup"><span data-stu-id="f72ff-142">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="f72ff-143">Публикация проекта (вычисляемый файлы копируются в место назначения публикации).</span><span class="sxs-lookup"><span data-stu-id="f72ff-143">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="f72ff-144">Когда ссылается на проект ASP.NET Core `Microsoft.NET.Sdk.Web` в файле проекта *app_offline.htm* файл помещается в корне каталога веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="f72ff-144">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="f72ff-145">Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="f72ff-145">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="f72ff-146">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="f72ff-146">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="f72ff-147">Основные публикации командной строки</span><span class="sxs-lookup"><span data-stu-id="f72ff-147">Basic command-line publishing</span></span>

<span data-ttu-id="f72ff-148">Публикация командной строки работает на всех платформах, поддерживаемых .NET Core и не требует Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f72ff-148">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="f72ff-149">В примере, показанном ниже [публикации dotnet](/dotnet/core/tools/dotnet-publish) команду следует запускать из каталога проекта (который содержит *.csproj* файла).</span><span class="sxs-lookup"><span data-stu-id="f72ff-149">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="f72ff-150">В противном случае в папке проекта явным образом передать в путь к файлу проекта.</span><span class="sxs-lookup"><span data-stu-id="f72ff-150">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="f72ff-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="f72ff-151">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="f72ff-152">Для создания и публикации веб-приложения выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="f72ff-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f72ff-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f72ff-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f72ff-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f72ff-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="f72ff-155">[Публикации dotnet](/dotnet/core/tools/dotnet-publish) команды выходные данные выглядят примерно следующим:</span><span class="sxs-lookup"><span data-stu-id="f72ff-155">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="f72ff-156">Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="f72ff-157">Значение по умолчанию для `$(Configuration)` — *отладки*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-157">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="f72ff-158">В предыдущем примере `<TargetFramework>` — `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-158">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="f72ff-159">`dotnet publish -h` отображает справку по публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="f72ff-160">Следующая команда определяет сборку `Release` и папку для публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="f72ff-161">[Публикации dotnet](/dotnet/core/tools/dotnet-publish) команда вызывает MSBuild, которая вызывает `Publish` цели.</span><span class="sxs-lookup"><span data-stu-id="f72ff-161">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="f72ff-162">Любые параметры, передаваемые в `dotnet publish` передаются в MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-162">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="f72ff-163">Параметр `-c` сопоставляется со свойством `Configuration` MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="f72ff-164">Параметр `-o` сопоставляется со свойством `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="f72ff-165">Свойства MSBuild могут передаваться с помощью любого из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="f72ff-165">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="f72ff-166">Следующая команда публикует сборку `Release` в общую сетевую папку.</span><span class="sxs-lookup"><span data-stu-id="f72ff-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="f72ff-167">Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f72ff-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="f72ff-168">Убедитесь, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="f72ff-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="f72ff-169">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="f72ff-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="f72ff-170">Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="f72ff-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="f72ff-171">Профили публикации</span><span class="sxs-lookup"><span data-stu-id="f72ff-171">Publish profiles</span></span>

<span data-ttu-id="f72ff-172">В этом разделе использует 2017 г. Visual Studio для создания профиля публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-172">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="f72ff-173">После создания публикации из Visual Studio или в командной строке доступна.</span><span class="sxs-lookup"><span data-stu-id="f72ff-173">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="f72ff-174">Публикация профилей можно упростить процесс публикации и могут иметь любое количество профилей.</span><span class="sxs-lookup"><span data-stu-id="f72ff-174">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="f72ff-175">Создание профиля публикации в Visual Studio, выбрав один из следующих путей:</span><span class="sxs-lookup"><span data-stu-id="f72ff-175">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="f72ff-176">Щелкните правой кнопкой мыши проект в обозревателе решений и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="f72ff-176">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="f72ff-177">Выберите **публикации &lt;project_name&gt;**  из **построения** меню.</span><span class="sxs-lookup"><span data-stu-id="f72ff-177">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="f72ff-178">**Публикации** отображается на странице приложения производственные мощности.</span><span class="sxs-lookup"><span data-stu-id="f72ff-178">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="f72ff-179">Если в проекте отсутствует профиль публикации, отображается со следующей страницы:</span><span class="sxs-lookup"><span data-stu-id="f72ff-179">If the project lacks a publish profile, the following page is displayed:</span></span>

![Вкладка публикации на странице приложения емкости](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="f72ff-181">Когда **папки** — флажок установлен, укажите путь к папке для хранения опубликованных ресурсов.</span><span class="sxs-lookup"><span data-stu-id="f72ff-181">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="f72ff-182">По умолчанию используется папка *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-182">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="f72ff-183">Нажмите кнопку **создать профиль** кнопки завершения.</span><span class="sxs-lookup"><span data-stu-id="f72ff-183">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="f72ff-184">После создания профиля публикации **публикации** вкладке изменения.</span><span class="sxs-lookup"><span data-stu-id="f72ff-184">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="f72ff-185">В раскрывающемся списке отображается только что созданный профиль.</span><span class="sxs-lookup"><span data-stu-id="f72ff-185">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="f72ff-186">Нажмите кнопку **Создание нового профиля** для создания другого нового профиля.</span><span class="sxs-lookup"><span data-stu-id="f72ff-186">Click **Create new profile** to create another new profile.</span></span>

![Вкладка публикации на странице приложения производственные мощности, показывающая FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="f72ff-188">Мастер публикации поддерживает следующие целевые объекты публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="f72ff-189">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="f72ff-189">Azure App Service</span></span>
* <span data-ttu-id="f72ff-190">Виртуальные машины Azure</span><span class="sxs-lookup"><span data-stu-id="f72ff-190">Azure Virtual Machines</span></span>
* <span data-ttu-id="f72ff-191">Службы IIS, FTP, т. д. (для любого веб-сервера)</span><span class="sxs-lookup"><span data-stu-id="f72ff-191">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="f72ff-192">Папка</span><span class="sxs-lookup"><span data-stu-id="f72ff-192">Folder</span></span>
* <span data-ttu-id="f72ff-193">Импорт профиля</span><span class="sxs-lookup"><span data-stu-id="f72ff-193">Import Profile</span></span>

<span data-ttu-id="f72ff-194">Дополнительные сведения см. в разделе [какие параметры публикации оптимальной](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="f72ff-194">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="f72ff-195">При создании профиля публикации с помощью Visual Studio *свойства/PublishProfiles/&lt;profile_name&gt;.pubxml* создается файл MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="f72ff-196">*.Pubxml* файл файла MSBuild, который содержит настройки конфигурации публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-196">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="f72ff-197">Этот файл можно изменить для настройки процесса построения и публикации процесс.</span><span class="sxs-lookup"><span data-stu-id="f72ff-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="f72ff-198">Этот файл считывается процессом публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-198">This file is read by the publishing process.</span></span> <span data-ttu-id="f72ff-199">`<LastUsedBuildConfiguration>` является специальной, так как он является глобальным и не должны быть в любом файле, который импортируется в сборке.</span><span class="sxs-lookup"><span data-stu-id="f72ff-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="f72ff-200">В разделе [MSBuild: как задать свойство конфигурации](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) для получения дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="f72ff-201">При публикации в целевом объекте Azure, *.pubxml* файл содержит ваш идентификатор подписки Azure.</span><span class="sxs-lookup"><span data-stu-id="f72ff-201">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="f72ff-202">С этим типом целевого добавлении этого файла в систему управления версиями не рекомендуется.</span><span class="sxs-lookup"><span data-stu-id="f72ff-202">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="f72ff-203">При публикации на целевой объект не Azure, можно безопасно выполнить возврат *.pubxml* файла.</span><span class="sxs-lookup"><span data-stu-id="f72ff-203">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="f72ff-204">Конфиденциальные данные (например, пароль публикации) шифруются на уровне пользователя или компьютера.</span><span class="sxs-lookup"><span data-stu-id="f72ff-204">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="f72ff-205">Он хранится в *свойства/PublishProfiles/&lt;profile_name&gt;. pubxml.user* файл.</span><span class="sxs-lookup"><span data-stu-id="f72ff-205">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="f72ff-206">Так как этот файл можно хранить конфиденциальные данные, его не должно проверяться в системе управления версиями.</span><span class="sxs-lookup"><span data-stu-id="f72ff-206">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="f72ff-207">Общие сведения о том, как опубликовать веб-приложения ASP.NET Core. в разделе [узла и развернуть](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="f72ff-207">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="f72ff-208">Задачи MSBuild и целевые объекты, необходимые для публикации приложения ASP.NET Core открытым исходным кодом на https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="f72ff-208">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="f72ff-209">`dotnet publish` можно использовать папку MSDeploy, и [Kudu](https://github.com/projectkudu/kudu/wiki) профили публикации:</span><span class="sxs-lookup"><span data-stu-id="f72ff-209">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="f72ff-210">Папка (кросс-платформенное):</span><span class="sxs-lookup"><span data-stu-id="f72ff-210">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="f72ff-211">MSDeploy (в настоящее время работает только в Windows, поскольку MSDeploy не кросс платформенных):</span><span class="sxs-lookup"><span data-stu-id="f72ff-211">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="f72ff-212">MSDeploy пакета (в настоящее время работает только в Windows, поскольку MSDeploy не кросс платформенных):</span><span class="sxs-lookup"><span data-stu-id="f72ff-212">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="f72ff-213">В предыдущем образцы **не** передачи `deployonbuild` для `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-213">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="f72ff-214">Дополнительные сведения см. в разделе [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="f72ff-214">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="f72ff-215">`dotnet publish` поддерживает Kudu API-интерфейсы для публикации в Azure с любой платформы.</span><span class="sxs-lookup"><span data-stu-id="f72ff-215">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="f72ff-216">Visual Studio опубликовать поддерживает Kudu API-интерфейсов, но поддерживается WebSDK для кросс платформенной публикации в Azure.</span><span class="sxs-lookup"><span data-stu-id="f72ff-216">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="f72ff-217">Добавить профиль публикации *свойства/PublishProfiles* папку со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="f72ff-217">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

```xml
<Project>
  <PropertyGroup>
    <PublishProtocol>Kudu</PublishProtocol>
    <PublishSiteName>nodewebapp</PublishSiteName>
    <UserName>username</UserName>
    <Password>password</Password>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="f72ff-218">Выполните следующую команду, чтобы архивировать содержимое публикации, а затем опубликовать его в Azure с помощью API-интерфейсы Kudu:</span><span class="sxs-lookup"><span data-stu-id="f72ff-218">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="f72ff-219">Для работы с профилем публикации задайте следующие свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-219">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="f72ff-220">При публикации с использованием профиля с именем *FolderProfile*, можно выполнить одну из приведенных ниже команд:</span><span class="sxs-lookup"><span data-stu-id="f72ff-220">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="f72ff-221">При вызове [построения dotnet](/dotnet/core/tools/dotnet-build), он вызывает `msbuild` для выполнения сборки и процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-221">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="f72ff-222">Вызов любого `dotnet build` или `msbuild` эквивалентно при передаче в папке профиля.</span><span class="sxs-lookup"><span data-stu-id="f72ff-222">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="f72ff-223">При вызове MSBuild непосредственно в Windows, .NET Framework версии MSBuild используется.</span><span class="sxs-lookup"><span data-stu-id="f72ff-223">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="f72ff-224">В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows.</span><span class="sxs-lookup"><span data-stu-id="f72ff-224">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="f72ff-225">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="f72ff-225">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="f72ff-226">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows).</span><span class="sxs-lookup"><span data-stu-id="f72ff-226">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="f72ff-227">Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.</span><span class="sxs-lookup"><span data-stu-id="f72ff-227">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="f72ff-228">Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:</span><span class="sxs-lookup"><span data-stu-id="f72ff-228">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework>netcoreapp1.1</PublishFramework>
    <ProjectGuid>c30c453c-312e-40c4-aec9-394a145dee0b</ProjectGuid>
    <publishUrl>\\r8\Release\AdminWeb</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
</Project>
```

<span data-ttu-id="f72ff-229">Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`.</span><span class="sxs-lookup"><span data-stu-id="f72ff-229">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="f72ff-230">При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-230">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="f72ff-231">`<LastUsedBuildConfiguration>` Свойство конфигурации является особенным и не должен переопределяться из импортированного файла MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-231">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="f72ff-232">Это свойство можно переопределить с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="f72ff-232">This property can be overridden from the command line.</span></span>

<span data-ttu-id="f72ff-233">С помощью .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="f72ff-233">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="f72ff-234">С использованием MSBuild</span><span class="sxs-lookup"><span data-stu-id="f72ff-234">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="f72ff-235">Публикация конечной точки MSDeploy из командной строки</span><span class="sxs-lookup"><span data-stu-id="f72ff-235">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="f72ff-236">Публикация может выполняться с помощью .NET Core CLI или MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-236">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="f72ff-237">Команда `dotnet publish` выполняется в контексте .NET Core.</span><span class="sxs-lookup"><span data-stu-id="f72ff-237">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="f72ff-238">`msbuild` Команде требуется .NET Framework, которая ограничивает его среды Windows.</span><span class="sxs-lookup"><span data-stu-id="f72ff-238">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="f72ff-239">Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.</span><span class="sxs-lookup"><span data-stu-id="f72ff-239">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="f72ff-240">В следующем примере создается веб-приложение ASP.NET Core (с помощью `dotnet new mvc`), и профиль публикации Azure добавляется с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f72ff-240">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="f72ff-241">Запустите `msbuild` из **Командная строка разработчика для VS 2017 г**.</span><span class="sxs-lookup"><span data-stu-id="f72ff-241">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="f72ff-242">Командная строка разработчика содержит правильные *msbuild.exe* в пути с некоторым набором переменных MSBuild.</span><span class="sxs-lookup"><span data-stu-id="f72ff-242">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="f72ff-243">MSBuild использует следующий синтаксис.</span><span class="sxs-lookup"><span data-stu-id="f72ff-243">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="f72ff-244">Получить `Password` из  *\<имя публикации >. PublishSettings* файл.</span><span class="sxs-lookup"><span data-stu-id="f72ff-244">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="f72ff-245">Загрузить *. PublishSettings* файл либо из:</span><span class="sxs-lookup"><span data-stu-id="f72ff-245">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="f72ff-246">Обозреватель решений: Щелкните правой кнопкой мыши на веб-приложения и выберите **загрузить профиль публикации**.</span><span class="sxs-lookup"><span data-stu-id="f72ff-246">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="f72ff-247">Портал Azure: щелкните **получение профиля публикации** на веб-приложения **Обзор** панель.</span><span class="sxs-lookup"><span data-stu-id="f72ff-247">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="f72ff-248">`Username` можно найти в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-248">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="f72ff-249">В следующем примере используется *Web11112 - веб-развертывания* профиль публикации:</span><span class="sxs-lookup"><span data-stu-id="f72ff-249">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="f72ff-250">Исключить файлы</span><span class="sxs-lookup"><span data-stu-id="f72ff-250">Exclude files</span></span>

<span data-ttu-id="f72ff-251">При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-251">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="f72ff-252">`msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="f72ff-252">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="f72ff-253">Например, следующая `<Content>` элемент исключает весь текст (*.txt*) файлов из *wwwroot/content* и всех ее вложенных папок.</span><span class="sxs-lookup"><span data-stu-id="f72ff-253">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="f72ff-254">Предыдущей разметки можно добавить профиль публикации или *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="f72ff-254">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="f72ff-255">При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="f72ff-255">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="f72ff-256">Следующие `<MsDeploySkipRules>` элемент исключает все файлы из *wwwroot/содержимого* папки:</span><span class="sxs-lookup"><span data-stu-id="f72ff-256">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="f72ff-257">`<MsDeploySkipRules>` не удалять *пропустить* целевых объектов с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="f72ff-257">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="f72ff-258">`<Content>` целевые файлы и папки будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="f72ff-258">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="f72ff-259">Например предположим, что развернутого веб-приложения имели следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="f72ff-259">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="f72ff-260">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f72ff-260">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="f72ff-261">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f72ff-261">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="f72ff-262">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="f72ff-262">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="f72ff-263">Если следующие `<MsDeploySkipRules>` добавляются элементы, эти файлы не будут удалены на сайте развертывания.</span><span class="sxs-lookup"><span data-stu-id="f72ff-263">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About1.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About2.cshtml</AbsolutePath>
  </MsDeploySkipRules>

  <MsDeploySkipRules Include="CustomSkipFile">
    <ObjectName>filePath</ObjectName>
    <AbsolutePath>Views\\Home\\About3.cshtml</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="f72ff-264">Приведенный выше `<MsDeploySkipRules>` элементы предотвратить *пропущен* файлов, развертывание.</span><span class="sxs-lookup"><span data-stu-id="f72ff-264">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="f72ff-265">Его не удалить эти файлы, как только они развернуты.</span><span class="sxs-lookup"><span data-stu-id="f72ff-265">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="f72ff-266">Следующие `<Content>` элемент удаляет целевых файлов на сайте развертывания:</span><span class="sxs-lookup"><span data-stu-id="f72ff-266">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="f72ff-267">С помощью развертывания из командной строки с предыдущим периодом `<Content>` элемент дает следующие результаты:</span><span class="sxs-lookup"><span data-stu-id="f72ff-267">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

```console
MSDeployPublish:
  Starting Web deployment task from source: manifest(C:\Webs\Web1\obj\Release\netcoreapp1.1\PubTmp\Web1.SourceManifest.
  xml) to Destination: auto().
  Deleting file (Web11112\Views\Home\About1.cshtml).
  Deleting file (Web11112\Views\Home\About2.cshtml).
  Deleting file (Web11112\Views\Home\About3.cshtml).
  Updating file (Web11112\web.config).
  Updating file (Web11112\Web1.deps.json).
  Updating file (Web11112\Web1.dll).
  Updating file (Web11112\Web1.pdb).
  Updating file (Web11112\Web1.runtimeconfig.json).
  Successfully executed Web deployment task.
  Publish Succeeded.
Done Building Project "C:\Webs\Web1\Web1.csproj" (default targets).
```

## <a name="include-files"></a><span data-ttu-id="f72ff-268">Включаемые файлы</span><span class="sxs-lookup"><span data-stu-id="f72ff-268">Include files</span></span>

<span data-ttu-id="f72ff-269">Включает следующую разметку *изображения* папку вне каталога проекта к *wwwroot/images* папку сайта публикации:</span><span class="sxs-lookup"><span data-stu-id="f72ff-269">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="f72ff-270">Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-270">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="f72ff-271">Если добавляется в *.csproj* файл, он включен в каждый профиль публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="f72ff-271">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="f72ff-272">Разметка, выделенная в приведенном ниже примере, показывает, как.</span><span class="sxs-lookup"><span data-stu-id="f72ff-272">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="f72ff-273">Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-273">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="f72ff-274">Исключить папку *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-274">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="f72ff-275">Исключить файл *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="f72ff-275">Exclude *Views\Home\About2.cshtml*.</span></span>

```xml
<?xml version="1.0" encoding="utf-8"?>
<!--
This file is used by the publish/package process of your Web project.
You can customize the behavior of this process by editing this 
MSBuild file.
-->
<Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
  <PropertyGroup>
    <WebPublishMethod>FileSystem</WebPublishMethod>
    <PublishProvider>FileSystem</PublishProvider>
    <LastUsedBuildConfiguration>Release</LastUsedBuildConfiguration>
    <LastUsedPlatform>Any CPU</LastUsedPlatform>
    <SiteUrlToLaunchAfterPublish />
    <LaunchSiteAfterPublish>True</LaunchSiteAfterPublish>
    <ExcludeApp_Data>False</ExcludeApp_Data>
    <PublishFramework />
    <ProjectGuid>afa9f185-7ce0-4935-9da1-ab676229d68a</ProjectGuid>
    <publishUrl>bin\Release\PublishOutput</publishUrl>
    <DeleteExistingFiles>False</DeleteExistingFiles>
  </PropertyGroup>
  <ItemGroup>
    <ResolvedFileToPublish Include="..\ReadMe2.MD">
      <RelativePath>wwwroot\ReadMe2.MD</RelativePath>
    </ResolvedFileToPublish>

    <Content Update="wwwroot\Content\**\*" CopyToPublishDirectory="Never" />
    <Content Update="Views\Home\About2.cshtml" CopyToPublishDirectory="Never" />

  </ItemGroup>
</Project>
```

<span data-ttu-id="f72ff-276">Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="f72ff-276">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="f72ff-277">Выполнение целевого объекта до или после публикации</span><span class="sxs-lookup"><span data-stu-id="f72ff-277">Run a target before or after publishing</span></span>

<span data-ttu-id="f72ff-278">Встроенная `BeforePublish` и `AfterPublish` целей выполнение целевого файла до или после назначения публикации.</span><span class="sxs-lookup"><span data-stu-id="f72ff-278">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="f72ff-279">Добавьте следующие элементы профиля публикации для журнала сообщений консоли до и после публикации:</span><span class="sxs-lookup"><span data-stu-id="f72ff-279">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="f72ff-280">Опубликовать на сервере, использующий Недоверенный сертификат</span><span class="sxs-lookup"><span data-stu-id="f72ff-280">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="f72ff-281">Добавить `<AllowUntrustedCertificate>` свойство со значением `True` для профиля публикации:</span><span class="sxs-lookup"><span data-stu-id="f72ff-281">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="f72ff-282">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="f72ff-282">The Kudu service</span></span>

<span data-ttu-id="f72ff-283">Чтобы просмотреть файлы в развертывании приложения веб-службы приложения Azure, используйте [Kudu службы](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="f72ff-283">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="f72ff-284">Добавление `scm` токен, имя веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="f72ff-284">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="f72ff-285">Пример:</span><span class="sxs-lookup"><span data-stu-id="f72ff-285">For example:</span></span>

| <span data-ttu-id="f72ff-286">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="f72ff-286">URL</span></span>                                    | <span data-ttu-id="f72ff-287">Результат</span><span class="sxs-lookup"><span data-stu-id="f72ff-287">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="f72ff-288">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="f72ff-288">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="f72ff-289">Kudu службы</span><span class="sxs-lookup"><span data-stu-id="f72ff-289">Kudu service</span></span> |

<span data-ttu-id="f72ff-290">Выберите [консоли отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console) пункт меню, чтобы просмотреть, изменить, удалить или добавить файлы.</span><span class="sxs-lookup"><span data-stu-id="f72ff-290">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f72ff-291">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f72ff-291">Additional resources</span></span>

* <span data-ttu-id="f72ff-292">[Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.</span><span class="sxs-lookup"><span data-stu-id="f72ff-292">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="f72ff-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): Файл, проблем и запросов возможности развертывания.</span><span class="sxs-lookup"><span data-stu-id="f72ff-293">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
