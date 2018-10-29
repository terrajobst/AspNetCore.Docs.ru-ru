---
title: Профили публикации Visual Studio для развертывания приложений ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать профили публикации в Visual Studio и применять их для управления развертыванием приложений ASP.NET Core в разных целевых объектах.
ms.author: riande
ms.custom: mvc
ms.date: 10/24/2018
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 3e626f99b06b0343360d6c46447e357890433dda
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148932"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="eca6b-103">Профили публикации Visual Studio для развертывания приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="eca6b-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="eca6b-104">Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="eca6b-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="eca6b-105">Этот документ посвящен использованию Visual Studio 2017 для создания и применения профилей публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-105">This document focuses on using Visual Studio 2017 to create and use publish profiles.</span></span> <span data-ttu-id="eca6b-106">Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="eca6b-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="eca6b-107">Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="eca6b-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="eca6b-108">Представленный ниже файл проекта был создан с помощью команды `dotnet new mvc`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-108">The following project file was created with the command `dotnet new mvc`:</span></span>

::: moniker range=">= aspnetcore-2.1"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="= aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.All" Version="2.0.9" />
  </ItemGroup>

</Project>
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.7" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.8" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.3" />
  </ItemGroup>

</Project>
```

::: moniker-end

<span data-ttu-id="eca6b-109">Атрибут `Sdk` элемента `<Project>` выполняет следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="eca6b-109">The `<Project>` element's `Sdk` attribute accomplishes the following tasks:</span></span>

* <span data-ttu-id="eca6b-110">Импортирует файл свойств из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начало.</span><span class="sxs-lookup"><span data-stu-id="eca6b-110">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="eca6b-111">Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.</span><span class="sxs-lookup"><span data-stu-id="eca6b-111">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="eca6b-112">По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-112">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="eca6b-113">Пакет SDK `Microsoft.NET.Sdk.Web` имеет следующие зависимости:</span><span class="sxs-lookup"><span data-stu-id="eca6b-113">The `Microsoft.NET.Sdk.Web` SDK depends on:</span></span>

* <span data-ttu-id="eca6b-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="eca6b-114">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="eca6b-115">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="eca6b-115">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="eca6b-116">Это означает, что он импортирует следующие свойства и целевые объекты:</span><span class="sxs-lookup"><span data-stu-id="eca6b-116">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="eca6b-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*;</span><span class="sxs-lookup"><span data-stu-id="eca6b-117">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="eca6b-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*;</span><span class="sxs-lookup"><span data-stu-id="eca6b-118">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets*</span></span>
* <span data-ttu-id="eca6b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*;</span><span class="sxs-lookup"><span data-stu-id="eca6b-119">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props*</span></span>
* <span data-ttu-id="eca6b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-120">*$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets*</span></span>

<span data-ttu-id="eca6b-121">Целевые объекты публикации импортируют набор нужных целевых объектов в зависимости от используемых методов публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-121">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="eca6b-122">Когда MSBuild или Visual Studio загружает проект, выполняются следующие обобщенные действия:</span><span class="sxs-lookup"><span data-stu-id="eca6b-122">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="eca6b-123">сборка проекта;</span><span class="sxs-lookup"><span data-stu-id="eca6b-123">Build project</span></span>
* <span data-ttu-id="eca6b-124">вычисление файлов для публикации;</span><span class="sxs-lookup"><span data-stu-id="eca6b-124">Compute files to publish</span></span>
* <span data-ttu-id="eca6b-125">публикация файлов в месте назначения;</span><span class="sxs-lookup"><span data-stu-id="eca6b-125">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="eca6b-126">вычисление элементов проекта.</span><span class="sxs-lookup"><span data-stu-id="eca6b-126">Compute project items</span></span>

<span data-ttu-id="eca6b-127">После загрузки проекта вычисляются элементы проекта (файлы).</span><span class="sxs-lookup"><span data-stu-id="eca6b-127">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="eca6b-128">Порядок обработки файла определяется атрибутом `item type`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-128">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="eca6b-129">По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-129">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="eca6b-130">Файлы в списке элементов `Compile` компилируются.</span><span class="sxs-lookup"><span data-stu-id="eca6b-130">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="eca6b-131">Список элементов `Content` содержит файлы, предназначенные для публикации, а также результаты сборки.</span><span class="sxs-lookup"><span data-stu-id="eca6b-131">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="eca6b-132">По умолчанию файлы, соответствующие шаблону `wwwroot/**`, включаются в элемент `Content`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-132">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="eca6b-133">[Стандартная маска](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot/\*\*` соответствует всем файлам в папке *wwwroot* **и** всех ее подпапках.</span><span class="sxs-lookup"><span data-stu-id="eca6b-133">The `wwwroot/\*\*` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="eca6b-134">Чтобы явным образом добавить в список публикации конкретный файл, поместите его в *CSPROJ*-файл, как показано в разделе [Включаемые файлы](#include-files).</span><span class="sxs-lookup"><span data-stu-id="eca6b-134">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Include Files](#include-files).</span></span>

<span data-ttu-id="eca6b-135">При нажатии кнопки **Публикация** в Visual Studio или при публикации из командной строки происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="eca6b-135">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="eca6b-136">Вычисляются свойства и (или) элементы (файлы, требующие сборки).</span><span class="sxs-lookup"><span data-stu-id="eca6b-136">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="eca6b-137">(**Только в Visual Studio**). Восстанавливаются пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="eca6b-137">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="eca6b-138">(Восстановление выполняется пользователем в интерфейсе командной строки.)</span><span class="sxs-lookup"><span data-stu-id="eca6b-138">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="eca6b-139">Выполняется сборка проекта.</span><span class="sxs-lookup"><span data-stu-id="eca6b-139">The project builds.</span></span>
* <span data-ttu-id="eca6b-140">Вычисляются публикуемые элементы (файлы, требующие публикации).</span><span class="sxs-lookup"><span data-stu-id="eca6b-140">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="eca6b-141">Публикуется проект (вычисляемые файлы копируются в место назначения публикации.)</span><span class="sxs-lookup"><span data-stu-id="eca6b-141">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="eca6b-142">Когда проект ASP.NET Core ссылается на `Microsoft.NET.Sdk.Web` в файле проекта, файл *app_offline.htm* помещается в корневой каталог веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="eca6b-142">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="eca6b-143">Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="eca6b-143">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="eca6b-144">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="eca6b-144">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="eca6b-145">Простая публикация из командной строки</span><span class="sxs-lookup"><span data-stu-id="eca6b-145">Basic command-line publishing</span></span>

<span data-ttu-id="eca6b-146">Публикация из командной строки работает на всех платформах, поддерживаемых .NET Core, и не требует наличия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eca6b-146">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="eca6b-147">В приведенных ниже примерах команда [dotnet publish](/dotnet/core/tools/dotnet-publish) выполняется из папки проекта (где хранится файл *CSPROJ*).</span><span class="sxs-lookup"><span data-stu-id="eca6b-147">In the samples below, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="eca6b-148">Если вы не находитесь в папке проекта, укажите путь к файлу проекта явным образом.</span><span class="sxs-lookup"><span data-stu-id="eca6b-148">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="eca6b-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="eca6b-149">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="eca6b-150">Для создания и публикации веб-приложения выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="eca6b-150">Run the following commands to create and publish a web app:</span></span>

::: moniker range=">= aspnetcore-2.0"

```console
dotnet new mvc
dotnet publish
```

::: moniker-end

::: moniker range="< aspnetcore-2.0"

```console
dotnet new mvc
dotnet restore
dotnet publish
```

::: moniker-end

<span data-ttu-id="eca6b-151">Выходные данные команды [dotnet publish](/dotnet/core/tools/dotnet-publish) выглядят примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="eca6b-151">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="eca6b-152">Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="eca6b-153">Для параметра `$(Configuration)` по умолчанию подставляется значение *Debug*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-153">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="eca6b-154">В предыдущем примере `<TargetFramework>` имеет значение `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-154">In the preceding sample, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="eca6b-155">`dotnet publish -h` отображает справку по публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-155">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="eca6b-156">Следующая команда определяет сборку `Release` и папку для публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-156">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="eca6b-157">Команда [dotnet publish](/dotnet/core/tools/dotnet-publish) вызывает MSBuild, что в свою очередь вызывает целевой объект `Publish`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-157">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="eca6b-158">Все параметры, передаваемые в `dotnet publish`, передаются далее в MSBuild.</span><span class="sxs-lookup"><span data-stu-id="eca6b-158">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="eca6b-159">Параметр `-c` сопоставляется со свойством `Configuration` MSBuild.</span><span class="sxs-lookup"><span data-stu-id="eca6b-159">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="eca6b-160">Параметр `-o` сопоставляется со свойством `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-160">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="eca6b-161">Свойства MSBuild можно передавать в любом из следующих форматов.</span><span class="sxs-lookup"><span data-stu-id="eca6b-161">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="eca6b-162">Следующая команда публикует сборку `Release` в общую сетевую папку.</span><span class="sxs-lookup"><span data-stu-id="eca6b-162">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="eca6b-163">Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.</span><span class="sxs-lookup"><span data-stu-id="eca6b-163">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="eca6b-164">Убедитесь, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="eca6b-164">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="eca6b-165">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="eca6b-165">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="eca6b-166">Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="eca6b-166">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="eca6b-167">Профили публикации</span><span class="sxs-lookup"><span data-stu-id="eca6b-167">Publish profiles</span></span>

<span data-ttu-id="eca6b-168">В этом разделе для создания профиля публикации используется Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="eca6b-168">This section uses Visual Studio 2017 to create a publishing profile.</span></span> <span data-ttu-id="eca6b-169">После создания профилей публикацию можно выполнять из Visual Studio или из командной строки.</span><span class="sxs-lookup"><span data-stu-id="eca6b-169">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="eca6b-170">Профили публикации упрощают процесс публикации. Вы можете создать любое количество профилей.</span><span class="sxs-lookup"><span data-stu-id="eca6b-170">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="eca6b-171">Создайте профиль публикации в Visual Studio, выбрав один из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="eca6b-171">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="eca6b-172">В обозревателе решений щелкните проект правой кнопкой мыши и выберите команду **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="eca6b-172">Right-click the project in Solution Explorer and select **Publish**.</span></span>
* <span data-ttu-id="eca6b-173">Выберите команду **Опубликовать &lt;имя_проекта&gt;** в меню **Сборка**.</span><span class="sxs-lookup"><span data-stu-id="eca6b-173">Select **Publish &lt;project_name&gt;** from the **Build** menu.</span></span>

<span data-ttu-id="eca6b-174">Откроется вкладка **Публикация** на странице свойств приложения.</span><span class="sxs-lookup"><span data-stu-id="eca6b-174">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="eca6b-175">Если в проекте нет профиля публикации, откроется следующая страница:</span><span class="sxs-lookup"><span data-stu-id="eca6b-175">If the project lacks a publish profile, the following page is displayed:</span></span>

![Вкладка "Публикация" на странице свойств приложения](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="eca6b-177">Выберите **Папка** и укажите путь к папке, в которой будут храниться опубликованные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="eca6b-177">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="eca6b-178">По умолчанию используется папка *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-178">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="eca6b-179">Нажмите кнопку **Создать профиль**, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="eca6b-179">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="eca6b-180">Когда профиль публикации будет создан, изменения отразятся на вкладке **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="eca6b-180">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="eca6b-181">Новый профиль появится в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="eca6b-181">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="eca6b-182">Щелкните **Создать новый профиль**, чтобы создать еще один профиль.</span><span class="sxs-lookup"><span data-stu-id="eca6b-182">Click **Create new profile** to create another new profile.</span></span>

![Вкладка "Публикация" на странице свойств приложения с профилем FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="eca6b-184">Мастер публикации поддерживает следующие целевые объекты публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-184">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="eca6b-185">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="eca6b-185">Azure App Service</span></span>
* <span data-ttu-id="eca6b-186">Виртуальные машины Azure</span><span class="sxs-lookup"><span data-stu-id="eca6b-186">Azure Virtual Machines</span></span>
* <span data-ttu-id="eca6b-187">IIS, FTP и т. д. (для любого веб-сервера)</span><span class="sxs-lookup"><span data-stu-id="eca6b-187">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="eca6b-188">Папка</span><span class="sxs-lookup"><span data-stu-id="eca6b-188">Folder</span></span>
* <span data-ttu-id="eca6b-189">Профиль импорта</span><span class="sxs-lookup"><span data-stu-id="eca6b-189">Import Profile</span></span>

<span data-ttu-id="eca6b-190">Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="eca6b-190">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="eca6b-191">При создании профиля публикации с помощью Visual Studio создается файл MSBuild *Properties/PublishProfiles/&lt;имя_профиля&gt;.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-191">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="eca6b-192">Этот файл MSBuild с расширением *.pubxml* содержит параметры конфигурации публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-192">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="eca6b-193">Вы можете изменить этот файл, чтобы настроить процесс сборки и публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-193">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="eca6b-194">Этот файл считывается процессом публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-194">This file is read by the publishing process.</span></span> <span data-ttu-id="eca6b-195">Особое свойство `<LastUsedBuildConfiguration>` является глобальным и не должно присутствовать ни в одном файле, импортируемом в сборку.</span><span class="sxs-lookup"><span data-stu-id="eca6b-195">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="eca6b-196">Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).</span><span class="sxs-lookup"><span data-stu-id="eca6b-196">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="eca6b-197">Если публикация выполняется в целевой объект Azure, файл с расширением *.pubxml* содержит идентификатор подписки Azure.</span><span class="sxs-lookup"><span data-stu-id="eca6b-197">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="eca6b-198">При таком типе целевого объекта мы не рекомендуем добавлять этот файл в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="eca6b-198">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="eca6b-199">Если публикация выполняется в целевой объект, не относящийся к Azure, файл с расширением *.pubxml* можно безопасно вернуть в систему.</span><span class="sxs-lookup"><span data-stu-id="eca6b-199">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="eca6b-200">Конфиденциальные данные (например, пароль публикации) шифруются на уровне пользователя или компьютера.</span><span class="sxs-lookup"><span data-stu-id="eca6b-200">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="eca6b-201">Они сохраняются в файле *Properties/PublishProfiles/&lt;имя_профиля&gt;.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-201">It's stored in the *Properties/PublishProfiles/&lt;profile_name&gt;.pubxml.user* file.</span></span> <span data-ttu-id="eca6b-202">Так как этот файл может содержать конфиденциальные данные, его не следует возвращать в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="eca6b-202">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="eca6b-203">Общие сведения о публикации веб-приложений в ASP.NET Core см. в статье [Размещение и развертывание ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="eca6b-203">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="eca6b-204">Открытый исходный код задач MSBuild и целевых объектов, необходимых для публикации приложения ASP.NET Core, размещен в https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="eca6b-204">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="eca6b-205">`dotnet publish` может использовать профили публикации папки, MSDeploy и [Kudu](https://github.com/projectkudu/kudu/wiki).</span><span class="sxs-lookup"><span data-stu-id="eca6b-205">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="eca6b-206">Папка (работает на всех платформах):</span><span class="sxs-lookup"><span data-stu-id="eca6b-206">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="eca6b-207">MSDeploy (сейчас работает только в Windows, так как MSDeploy не является кроссплатформенным):</span><span class="sxs-lookup"><span data-stu-id="eca6b-207">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="eca6b-208">Пакет MSDeploy (сейчас работает только в Windows, так как MSDeploy не является кроссплатформенным):</span><span class="sxs-lookup"><span data-stu-id="eca6b-208">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="eca6b-209">В примерах выше **не** передавайте `deployonbuild` в `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-209">In the preceding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="eca6b-210">Дополнительные сведения см. в [документации по Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="eca6b-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="eca6b-211">`dotnet publish` поддерживает API-интерфейсы Kudu для публикации в Azure с любой платформы.</span><span class="sxs-lookup"><span data-stu-id="eca6b-211">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="eca6b-212">Публикация с помощью Visual Studio поддерживает API Kudu, но для кроссплатформенной публикации в Azure их поддерживает WebSDK.</span><span class="sxs-lookup"><span data-stu-id="eca6b-212">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="eca6b-213">Добавьте в папку *Properties/PublishProfiles* профиль публикации со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="eca6b-213">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="eca6b-214">Выполните следующую команду, чтобы архивировать содержимое публикации и опубликовать его в Azure с помощью API-интерфейсов Kudu.</span><span class="sxs-lookup"><span data-stu-id="eca6b-214">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="eca6b-215">Для работы с профилем публикации задайте следующие свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="eca6b-215">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="eca6b-216">Для публикации с использованием профиля *FolderProfile* вы можете выполнить одну из указанных ниже команд.</span><span class="sxs-lookup"><span data-stu-id="eca6b-216">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="eca6b-217">Команда [dotnet build](/dotnet/core/tools/dotnet-build) вызывает `msbuild` для сборки и публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-217">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="eca6b-218">Вызовы `dotnet build` и `msbuild` эквивалентны, если они передаются в профиле папки.</span><span class="sxs-lookup"><span data-stu-id="eca6b-218">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="eca6b-219">При прямом вызове MSBuild на платформе Windows используется версия MSBuild для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="eca6b-219">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="eca6b-220">В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows.</span><span class="sxs-lookup"><span data-stu-id="eca6b-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="eca6b-221">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="eca6b-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="eca6b-222">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows).</span><span class="sxs-lookup"><span data-stu-id="eca6b-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="eca6b-223">Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.</span><span class="sxs-lookup"><span data-stu-id="eca6b-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="eca6b-224">Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:</span><span class="sxs-lookup"><span data-stu-id="eca6b-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="eca6b-225">Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="eca6b-226">При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="eca6b-227">Свойство конфигурации `<LastUsedBuildConfiguration>` имеет особое значение и его не следует переопределять в импортируемом файле MSBuild.</span><span class="sxs-lookup"><span data-stu-id="eca6b-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="eca6b-228">Это свойство можно переопределить из командной строки.</span><span class="sxs-lookup"><span data-stu-id="eca6b-228">This property can be overridden from the command line.</span></span>

<span data-ttu-id="eca6b-229">С использованием .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="eca6b-229">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="eca6b-230">С использованием MSBuild</span><span class="sxs-lookup"><span data-stu-id="eca6b-230">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="eca6b-231">Публикация конечной точки MSDeploy из командной строки</span><span class="sxs-lookup"><span data-stu-id="eca6b-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="eca6b-232">Публикацию можно выполнять с помощью .NET Core CLI или MSBuild.</span><span class="sxs-lookup"><span data-stu-id="eca6b-232">Publishing can be accomplished using the .NET Core CLI or MSBuild.</span></span> <span data-ttu-id="eca6b-233">Команда `dotnet publish` выполняется в контексте .NET Core.</span><span class="sxs-lookup"><span data-stu-id="eca6b-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="eca6b-234">Для команды `msbuild` требуется платформа .NET Framework, а значит ее можно использовать только в средах Windows.</span><span class="sxs-lookup"><span data-stu-id="eca6b-234">The `msbuild` command requires .NET Framework, which limits it to Windows environments.</span></span>

<span data-ttu-id="eca6b-235">Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.</span><span class="sxs-lookup"><span data-stu-id="eca6b-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="eca6b-236">В следующем примере создается веб-приложение ASP.NET Core (с помощью команды `dotnet new mvc`), и добавляется профиль публикации Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="eca6b-236">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="eca6b-237">Выполните `msbuild` в **командной строке разработчика для VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="eca6b-237">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="eca6b-238">Командная строка разработчика использует правильный файл *msbuild.exe* и некоторые переменные MSBuild.</span><span class="sxs-lookup"><span data-stu-id="eca6b-238">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="eca6b-239">MSBuild использует следующий синтаксис.</span><span class="sxs-lookup"><span data-stu-id="eca6b-239">MSBuild uses the following syntax:</span></span>

```console
msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>
```

<span data-ttu-id="eca6b-240">Значение `Password` из файла *\<Имя_публикации>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-240">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="eca6b-241">Файл с расширением *.PublishSettings* можно скачать одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="eca6b-241">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="eca6b-242">Обозреватель решений: щелкните веб-приложение правой кнопкой мыши и выберите пункт **Загрузить профиль публикации**.</span><span class="sxs-lookup"><span data-stu-id="eca6b-242">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="eca6b-243">Портал Azure: щелкните **Скачать профиль публикации** на панели **Обзор** веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="eca6b-243">Azure portal: Click **Get publish profile** on the Web App's **Overview** panel.</span></span>

<span data-ttu-id="eca6b-244">`Username` можно найти в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="eca6b-245">В следующем примере используется профиль публикации *Web11112 - Web Deploy*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-245">The following sample uses the *Web11112 - Web Deploy* publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="exclude-files"></a><span data-ttu-id="eca6b-246">Исключение файлов</span><span class="sxs-lookup"><span data-stu-id="eca6b-246">Exclude files</span></span>

<span data-ttu-id="eca6b-247">При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="eca6b-248">`msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="eca6b-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="eca6b-249">Например, представленный ниже элемент `<Content>` исключит все текстовые файлы (*.txt*) из папки *wwwroot/content* и всех ее подпапок.</span><span class="sxs-lookup"><span data-stu-id="eca6b-249">For example, the following `<Content>` element excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="eca6b-250">Предложенную выше разметку можно добавить в профиль публикации или в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-250">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="eca6b-251">При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="eca6b-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="eca6b-252">Следующий элемент `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot/content*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-252">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="eca6b-253">Элемент `<MsDeploySkipRules>` не удаляет *пропускаемые* целевые объекты с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="eca6b-253">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="eca6b-254">Целевые файлы и папки, указанные в элементе `<Content>`, будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="eca6b-254">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="eca6b-255">Допустим, что в развернутом веб-приложении есть следующие файлы.</span><span class="sxs-lookup"><span data-stu-id="eca6b-255">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="eca6b-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="eca6b-256">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="eca6b-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="eca6b-257">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="eca6b-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="eca6b-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="eca6b-259">Если вы добавите указанные ниже элементы `<MsDeploySkipRules>`, эти файлы не будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="eca6b-259">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="eca6b-260">Приведенные выше элементы `<MsDeploySkipRules>` запрещают развертывание *пропускаемых* файлов.</span><span class="sxs-lookup"><span data-stu-id="eca6b-260">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="eca6b-261">Эти файлы не будут удалены, если они уже развернуты.</span><span class="sxs-lookup"><span data-stu-id="eca6b-261">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="eca6b-262">Следующий элемент `<Content>` удаляет целевые файлы на сайте развертывания.</span><span class="sxs-lookup"><span data-stu-id="eca6b-262">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="eca6b-263">Применение указанного выше элемента `<Content>` при развертывании из командной строки дает следующий результат:</span><span class="sxs-lookup"><span data-stu-id="eca6b-263">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="eca6b-264">Включаемые файлы</span><span class="sxs-lookup"><span data-stu-id="eca6b-264">Include files</span></span>

<span data-ttu-id="eca6b-265">Показанная ниже разметка позволяет включить папку *images*, расположенную за пределами папки проекта, в папку *wwwroot/images* на сайте публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-265">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="eca6b-266">Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-266">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="eca6b-267">Если добавить ее в файл *CSPROJ*, она будет включаться в каждый профиль публикации этого проекта.</span><span class="sxs-lookup"><span data-stu-id="eca6b-267">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="eca6b-268">Разметка, выделенная в приведенном ниже примере, показывает, как.</span><span class="sxs-lookup"><span data-stu-id="eca6b-268">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="eca6b-269">Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-269">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="eca6b-270">Исключить папку *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-270">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="eca6b-271">Исключить файл *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="eca6b-271">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="eca6b-272">Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="eca6b-272">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="eca6b-273">Выполнение целевого объекта до или после публикации</span><span class="sxs-lookup"><span data-stu-id="eca6b-273">Run a target before or after publishing</span></span>

<span data-ttu-id="eca6b-274">Встроенные целевые объекты `BeforePublish` и `AfterPublish` позволяют выполнить целевой объект до или после его публикации.</span><span class="sxs-lookup"><span data-stu-id="eca6b-274">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="eca6b-275">Добавьте следующие элементы в профиль публикации, чтобы сохранять сообщения в консоли, возвращаемые до и после публикации:</span><span class="sxs-lookup"><span data-stu-id="eca6b-275">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="eca6b-276">Публикация на сервере с использованием сертификата без доверия</span><span class="sxs-lookup"><span data-stu-id="eca6b-276">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="eca6b-277">Добавьте свойство `<AllowUntrustedCertificate>` со значением `True` в профиль публикации:</span><span class="sxs-lookup"><span data-stu-id="eca6b-277">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="eca6b-278">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="eca6b-278">The Kudu service</span></span>

<span data-ttu-id="eca6b-279">Чтобы просмотреть файлы в развертывании веб-приложения службы приложений Azure, используйте [службу Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="eca6b-279">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="eca6b-280">Добавьте к имени веб-приложения маркер `scm`.</span><span class="sxs-lookup"><span data-stu-id="eca6b-280">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="eca6b-281">Пример:</span><span class="sxs-lookup"><span data-stu-id="eca6b-281">For example:</span></span>

| <span data-ttu-id="eca6b-282">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="eca6b-282">URL</span></span>                                    | <span data-ttu-id="eca6b-283">Результат</span><span class="sxs-lookup"><span data-stu-id="eca6b-283">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="eca6b-284">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="eca6b-284">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="eca6b-285">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="eca6b-285">Kudu service</span></span> |

<span data-ttu-id="eca6b-286">Для просмотра, редактирования, удаления и (или) добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="eca6b-286">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="eca6b-287">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="eca6b-287">Additional resources</span></span>

* <span data-ttu-id="eca6b-288">[Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.</span><span class="sxs-lookup"><span data-stu-id="eca6b-288">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="eca6b-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и возможности запросов для развертывания.</span><span class="sxs-lookup"><span data-stu-id="eca6b-289">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="eca6b-290">Публикация веб-приложения ASP.NET в виртуальную машину Azure из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eca6b-290">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
