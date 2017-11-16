---
title: "Создание профилей публикации для Visual Studio и MSBuild"
author: rick-anderson
description: "Описание веб-публикации в Visual Studio."
keywords: "ASP.NET Core, веб-публикация, публикация, msbuild, веб-развертывание, публикации dotnet, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 09/26/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: f010f9d90165ce4d6718fe1440e600985f21a01d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="334d6-104">Создание профилей публикации для Visual Studio и MSBuild с целью развертывания приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="334d6-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="334d6-105">Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="334d6-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="334d6-106">В этой статье рассказывается об использовании Visual Studio 2017 для создания профилей публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="334d6-107">Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="334d6-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="334d6-108">Статья содержит сведения о процессе публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-108">The article provides details of the publishing process.</span></span> <span data-ttu-id="334d6-109">Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="334d6-109">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="334d6-110">Представленный ниже файл *.csproj* был создан с помощью команды `dotnet new mvc`.</span><span class="sxs-lookup"><span data-stu-id="334d6-110">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="334d6-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="334d6-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="334d6-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="334d6-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp1.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore" Version="1.1.1" />
    <PackageReference Include="Microsoft.AspNetCore.Mvc" Version="1.1.2" />
    <PackageReference Include="Microsoft.AspNetCore.StaticFiles" Version="1.1.1" />
  </ItemGroup>

</Project>
```

---

<span data-ttu-id="334d6-113">Атрибут `Sdk` в элементе `<Project>` (первая строка) показанной выше разметки выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="334d6-113">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="334d6-114">Импортирует файл `props` из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начало.</span><span class="sxs-lookup"><span data-stu-id="334d6-114">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="334d6-115">Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.</span><span class="sxs-lookup"><span data-stu-id="334d6-115">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="334d6-116">По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="334d6-116">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="334d6-117">`Microsoft.NET.Sdk.Web` зависит от следующих компонентов.</span><span class="sxs-lookup"><span data-stu-id="334d6-117">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="334d6-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="334d6-118">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="334d6-119">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="334d6-119">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="334d6-120">В связи с чем импортируются следующие файлы `props` и `targets`.</span><span class="sxs-lookup"><span data-stu-id="334d6-120">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="334d6-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="334d6-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="334d6-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="334d6-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="334d6-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="334d6-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="334d6-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="334d6-124">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="334d6-125">Целевые объекты публикации, в свою очередь, также импортируют соответствующий набор целевых объектов в зависимости от используемых методов публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-125">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="334d6-126">Когда MSBuild или Visual Studio загружает проект, на высоком уровне выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="334d6-126">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="334d6-127">сборка проекта;</span><span class="sxs-lookup"><span data-stu-id="334d6-127">Build project</span></span>
* <span data-ttu-id="334d6-128">вычисление файлов для публикации;</span><span class="sxs-lookup"><span data-stu-id="334d6-128">Compute files to publish</span></span>
* <span data-ttu-id="334d6-129">публикация файлов в месте назначения;</span><span class="sxs-lookup"><span data-stu-id="334d6-129">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="334d6-130">вычисление элементов проекта.</span><span class="sxs-lookup"><span data-stu-id="334d6-130">Compute project items</span></span>

<span data-ttu-id="334d6-131">После загрузки проекта вычисляются элементы проекта (файлы).</span><span class="sxs-lookup"><span data-stu-id="334d6-131">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="334d6-132">Порядок обработки файла определяется атрибутом `item type`.</span><span class="sxs-lookup"><span data-stu-id="334d6-132">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="334d6-133">По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`.</span><span class="sxs-lookup"><span data-stu-id="334d6-133">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="334d6-134">Файлы в списке элементов `Compile` компилируются.</span><span class="sxs-lookup"><span data-stu-id="334d6-134">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="334d6-135">Список элементов `Content` содержит предназначенные для публикации файлы и результаты сборки.</span><span class="sxs-lookup"><span data-stu-id="334d6-135">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="334d6-136">По умолчанию файлы, соответствующие шаблону wwwroot/**, включаются в элемент `Content`.</span><span class="sxs-lookup"><span data-stu-id="334d6-136">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="334d6-137">[wwwroot/** — это стандартная маска](https://gruntjs.com/configuring-tasks#globbing-patterns), которая охватывает все файлы в папке *wwwroot* **и** ее подпапках.</span><span class="sxs-lookup"><span data-stu-id="334d6-137">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="334d6-138">Чтобы напрямую добавить в список публикации отдельный файл, можно добавить этот файл в файл *CSPROJ*, как показано в разделе [Включение файлов](#including-files).</span><span class="sxs-lookup"><span data-stu-id="334d6-138">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="334d6-139">При нажатии кнопки **Публикация** в Visual Studio или публикации из командной строки происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="334d6-139">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="334d6-140">Вычисляются свойства и (или) элементы (файлы, требующие сборки).</span><span class="sxs-lookup"><span data-stu-id="334d6-140">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="334d6-141">Только в Visual Studio: восстанавливаются пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="334d6-141">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="334d6-142">(Восстановление выполняется пользователем в интерфейсе командной строки.)</span><span class="sxs-lookup"><span data-stu-id="334d6-142">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="334d6-143">Выполняется сборка проекта.</span><span class="sxs-lookup"><span data-stu-id="334d6-143">The project builds.</span></span>
- <span data-ttu-id="334d6-144">Вычисляются публикуемые элементы (файлы, требующие публикации).</span><span class="sxs-lookup"><span data-stu-id="334d6-144">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="334d6-145">Проект публикуется.</span><span class="sxs-lookup"><span data-stu-id="334d6-145">The project is published.</span></span> <span data-ttu-id="334d6-146">(Вычисляемые файлы копируются в место назначения публикации.)</span><span class="sxs-lookup"><span data-stu-id="334d6-146">(The computed files are copied to the publish destination.)</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="334d6-147">Простая публикация из командной строки</span><span class="sxs-lookup"><span data-stu-id="334d6-147">Basic command line publishing</span></span>

<span data-ttu-id="334d6-148">Этот метод работает на всех поддерживаемых платформах .NET Core и не требует Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="334d6-148">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="334d6-149">В приведенных ниже примерах команда `dotnet publish` выполняется из папки проекта (содержащей файл *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="334d6-149">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="334d6-150">Если вы не находитесь в папке проекта, путь к файлу проекта можно передать явным образом.</span><span class="sxs-lookup"><span data-stu-id="334d6-150">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="334d6-151">Пример:</span><span class="sxs-lookup"><span data-stu-id="334d6-151">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="334d6-152">Для создания и публикации веб-приложения выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="334d6-152">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="334d6-153">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="334d6-153">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="334d6-154">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="334d6-154">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

--------------

<span data-ttu-id="334d6-155">Команда `dotnet publish` выдает результаты следующего вида.</span><span class="sxs-lookup"><span data-stu-id="334d6-155">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="334d6-156">Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="334d6-156">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="334d6-157">Значение параметра `$(Configuration)` по умолчанию — Debug (Отладка).</span><span class="sxs-lookup"><span data-stu-id="334d6-157">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="334d6-158">В приведенном выше примере `<TargetFramework>` — это `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="334d6-158">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="334d6-159">`dotnet publish -h` отображает справку по публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-159">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="334d6-160">Следующая команда определяет сборку `Release` и папку для публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-160">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="334d6-161">Команда `dotnet publish` вызывает метод `MSBuild`, который, в свою очередь, вызывает целевой объект `Publish`.</span><span class="sxs-lookup"><span data-stu-id="334d6-161">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="334d6-162">Все параметры, передаваемые в `dotnet publish`, передаются в `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="334d6-162">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="334d6-163">Параметр `-c` сопоставляется со свойством `Configuration` MSBuild.</span><span class="sxs-lookup"><span data-stu-id="334d6-163">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="334d6-164">Параметр `-o` сопоставляется со свойством `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="334d6-164">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="334d6-165">Свойства MSBuild можно передавать в любом из следующих форматов.</span><span class="sxs-lookup"><span data-stu-id="334d6-165">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="334d6-166">Следующая команда публикует сборку `Release` в общую сетевую папку.</span><span class="sxs-lookup"><span data-stu-id="334d6-166">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="334d6-167">Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.</span><span class="sxs-lookup"><span data-stu-id="334d6-167">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="334d6-168">Убедитесь, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="334d6-168">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="334d6-169">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="334d6-169">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="334d6-170">Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="334d6-170">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="334d6-171">Профили публикации</span><span class="sxs-lookup"><span data-stu-id="334d6-171">Publish profiles</span></span>

<span data-ttu-id="334d6-172">В этом разделе для создания профилей публикации используется Visual Studio 2017 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="334d6-172">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="334d6-173">После создания профилей публикацию можно произвести из Visual Studio или из командной строки.</span><span class="sxs-lookup"><span data-stu-id="334d6-173">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="334d6-174">Профили публикации позволяют упростить процесс публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-174">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="334d6-175">Таких профилей может быть несколько.</span><span class="sxs-lookup"><span data-stu-id="334d6-175">You can have multiple publish profiles.</span></span> <span data-ttu-id="334d6-176">Чтобы создать профиль публикации в Visual Studio, щелкните проект в обозревателе решений правой кнопкой мыши и выберите команду **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="334d6-176">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="334d6-177">Другой вариант: открыть меню сборки и выбрать команду **Опубликовать     \<имя проекта >**.</span><span class="sxs-lookup"><span data-stu-id="334d6-177">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="334d6-178">Откроется вкладка **Публикации** на странице свойств приложения.</span><span class="sxs-lookup"><span data-stu-id="334d6-178">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="334d6-179">Если проект не содержит профиль публикации, откроется следующая страница.</span><span class="sxs-lookup"><span data-stu-id="334d6-179">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка" и выбранным разделом Azure](web-publishing-vs/_static/az.png)

<span data-ttu-id="334d6-182">Если выбран раздел **Папка**, кнопка **Опубликовать** создает профиль публикации в папку и выполняет публикацию.</span><span class="sxs-lookup"><span data-stu-id="334d6-182">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка"](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="334d6-184">После того, как профиль публикации будет создан, вкладка **Публикация** изменит вид, и вы сможете выбрать пункт **Создать профиль**, чтобы создать новый профиль.</span><span class="sxs-lookup"><span data-stu-id="334d6-184">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![Вкладка **Публикация** на странице свойств приложения с созданным на предыдущем шаге профилем FolderProfile и ссылкой "Создать профиль".](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="334d6-186">Мастер публикации поддерживает следующие целевые объекты публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-186">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="334d6-187">Служба приложений Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="334d6-187">Microsoft Azure App Service</span></span>
- <span data-ttu-id="334d6-188">IIS, FTP и т. д (для любого веб-сервера)</span><span class="sxs-lookup"><span data-stu-id="334d6-188">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="334d6-189">Папка</span><span class="sxs-lookup"><span data-stu-id="334d6-189">Folder</span></span>
- <span data-ttu-id="334d6-190">Импорт профиля (позволяет импортировать профиль)</span><span class="sxs-lookup"><span data-stu-id="334d6-190">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="334d6-191">Виртуальные машины Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="334d6-191">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="334d6-192">Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="334d6-192">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="334d6-193">При создании профиля публикации с помощью Visual Studio создается файл MSBuild *Properties/PublishProfiles/\<имя публикации>.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="334d6-193">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="334d6-194">Это файл с расширением *.pubxml* представляет собой файл MSBuild, содержащий настройки конфигурации публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-194">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="334d6-195">Процесс сборки и публикации можно настроить, скорректировав этот файл.</span><span class="sxs-lookup"><span data-stu-id="334d6-195">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="334d6-196">Этот файл считывается процессом публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-196">This file is read by the publishing process.</span></span> <span data-ttu-id="334d6-197">Свойство `<LastUsedBuildConfiguration>` отличается тем, что является глобальным и не должно присутствовать ни в одном файле, импортируемом в сборку.</span><span class="sxs-lookup"><span data-stu-id="334d6-197">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="334d6-198">Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).</span><span class="sxs-lookup"><span data-stu-id="334d6-198">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="334d6-199">Файл с расширением *.pubxml* не должен возвращаться в систему управления версиями, так как зависит от файла *.user*.</span><span class="sxs-lookup"><span data-stu-id="334d6-199">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="334d6-200">Файл *.user* ни в коем случае не должен возвращаться в систему управления версиями, так как может содержать конфиденциальные сведения и быть действительным только для одного пользователя и компьютера.</span><span class="sxs-lookup"><span data-stu-id="334d6-200">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="334d6-201">Конфиденциальные данные (такие как пароль публикации) шифруются на уровне пользователя или компьютера и хранятся в файле *Properties/PublishProfiles/\<имя публикации>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="334d6-201">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="334d6-202">Так как этот файл может содержать конфиденциальные данные, он **не** должен возвращаться в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="334d6-202">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="334d6-203">Общие сведения о том, как опубликовать веб-приложение в ASP.NET Core, см. в статье [Публикация и развертывание](index.md).</span><span class="sxs-lookup"><span data-stu-id="334d6-203">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="334d6-204">[Публикация и развертывание](index.md) — это проект с открытым кодом на сайте https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="334d6-204">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

 <span data-ttu-id="334d6-205">`dotnet publish` может использовать профили публикации папки, Msdeploy и [KUDU](https://github.com/projectkudu/kudu/wiki):</span><span class="sxs-lookup"><span data-stu-id="334d6-205">`dotnet publish` can use folder, Msdeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="334d6-206">Папка (работает на всех платформах): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="334d6-206">Folder (works cross-platform) `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="334d6-207">Msdeploy (сейчас работает только в Windows, так как эта команда не поддерживается на всех платформах): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="334d6-207">Msdeploy (currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="334d6-208">Пакет Msdeploy (сейчас работает только в Windows, так как Msdeploy не поддерживается на всех платформах): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="334d6-208">Msdeploy package(currently this only works in windows since msdeploy is not cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="334d6-209">В примерах выше **не** передавайте `deployonbuild` в `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="334d6-209">In the preceeding samples, **don’t** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="334d6-210">Дополнительные сведения см. в [документации по пакету Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="334d6-210">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish)</span></span>

<span data-ttu-id="334d6-211">`dotnet publish` поддерживает API KUDU для публикации в Azure с любой платформы.</span><span class="sxs-lookup"><span data-stu-id="334d6-211">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="334d6-212">Публикация с помощью Visual Studio не поддерживает API KUDU, но эти API поддерживаются websdk для кроссплатформенной публикации в Azure.</span><span class="sxs-lookup"><span data-stu-id="334d6-212">Visual Studio publish does support the KUDU APIs but it is supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="334d6-213">Добавьте профиль публикации в папку *Properties/PublishProfiles* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="334d6-213">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="334d6-214">Выполните следующую команду, чтобы архивировать содержимое публикации и опубликовать его в Azure с помощью API-интерфейсов KUDU.</span><span class="sxs-lookup"><span data-stu-id="334d6-214">Running the following command will zip up the publish contents and publish it to Azure using the KUDU APIs.</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="334d6-215">Для работы с профилем публикации задайте следующие свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="334d6-215">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="334d6-216">Например, при публикации с использованием профиля *FolderProfile* можно выполнить одну из указанных ниже команд.</span><span class="sxs-lookup"><span data-stu-id="334d6-216">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="334d6-217">При вызове `dotnet build` он вызывает `msbuild` для выполнения сборки и публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-217">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="334d6-218">Вызов `dotnet build` или `msbuild` по сути работает как передача профиля папки.</span><span class="sxs-lookup"><span data-stu-id="334d6-218">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="334d6-219">Вызывая MSBuild в Windows напрямую, вы получаете версию MSBuild .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="334d6-219">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="334d6-220">В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows.</span><span class="sxs-lookup"><span data-stu-id="334d6-220">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="334d6-221">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="334d6-221">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="334d6-222">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows).</span><span class="sxs-lookup"><span data-stu-id="334d6-222">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="334d6-223">Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.</span><span class="sxs-lookup"><span data-stu-id="334d6-223">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="334d6-224">Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:</span><span class="sxs-lookup"><span data-stu-id="334d6-224">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="334d6-225">Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`.</span><span class="sxs-lookup"><span data-stu-id="334d6-225">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="334d6-226">При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-226">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="334d6-227">Свойство конфигурации `<LastUsedBuildConfiguration>` имеет особое значение и не должно переопределяться в импортированном файле MSBuild.</span><span class="sxs-lookup"><span data-stu-id="334d6-227">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="334d6-228">Переопределить это свойство можно из командной строки.</span><span class="sxs-lookup"><span data-stu-id="334d6-228">You can override this property from the command line.</span></span> <span data-ttu-id="334d6-229">Пример:</span><span class="sxs-lookup"><span data-stu-id="334d6-229">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="334d6-230">С использованием MSBuild</span><span class="sxs-lookup"><span data-stu-id="334d6-230">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="334d6-231">Публикация конечной точки MSDeploy из командной строки</span><span class="sxs-lookup"><span data-stu-id="334d6-231">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="334d6-232">Как уже говорилось, для публикации можно использовать команды `dotnet publish` и `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="334d6-232">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="334d6-233">Команда `dotnet publish` выполняется в контексте .NET Core.</span><span class="sxs-lookup"><span data-stu-id="334d6-233">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="334d6-234">Команда `msbuild` требует платформы .NET Framework и работает только в средах Windows.</span><span class="sxs-lookup"><span data-stu-id="334d6-234">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="334d6-235">Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.</span><span class="sxs-lookup"><span data-stu-id="334d6-235">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="334d6-236">В следующем примере создается веб-приложение ASP.NET Core (с помощью команды `dotnet new mvc`), и добавляется профиль публикации Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="334d6-236">In the following sample, an ASP.NET Core web app is created ( using `dotnet new mvc`) and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="334d6-237">Команда `msbuild` выполняется в **командной строке разработчика для VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="334d6-237">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="334d6-238">Командная строка разработчика задаст соответствующий файл *msbuild.exe* в пути и некоторые переменные MSBuild.</span><span class="sxs-lookup"><span data-stu-id="334d6-238">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="334d6-239">MSBuild использует следующий синтаксис.</span><span class="sxs-lookup"><span data-stu-id="334d6-239">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="334d6-240">Значение `Password` можно получить из файла *\<Имя публикации>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="334d6-240">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="334d6-241">Файл *.PublishSettings* можно загрузить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="334d6-241">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="334d6-242">Обозреватель решений: щелкните веб-приложения правой кнопкой мыши и выберите пункт **Загрузить профиль публикации**.</span><span class="sxs-lookup"><span data-stu-id="334d6-242">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="334d6-243">Портал управления Azure: выберите пункт **Получить профиль публикации** в колонке "Веб-приложение".</span><span class="sxs-lookup"><span data-stu-id="334d6-243">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="334d6-244">`Username` можно найти в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-244">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="334d6-245">В следующем примере используется профиль публикации "Web11112 - Web Deploy".</span><span class="sxs-lookup"><span data-stu-id="334d6-245">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="334d6-246">Исключение файлов</span><span class="sxs-lookup"><span data-stu-id="334d6-246">Excluding files</span></span>

<span data-ttu-id="334d6-247">При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="334d6-247">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="334d6-248">`msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="334d6-248">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="334d6-249">Например, представленная ниже разметка элемента `<Content>` исключит все текстовые файлы (*.txt*) из папки *wwwroot/content* и всех ее подпапок.</span><span class="sxs-lookup"><span data-stu-id="334d6-249">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="334d6-250">Показанная выше разметка может быть добавлена в профиль публикации или в файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="334d6-250">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="334d6-251">При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="334d6-251">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="334d6-252">Представленная ниже разметка элемента `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot/content*.</span><span class="sxs-lookup"><span data-stu-id="334d6-252">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="334d6-253">Элемент `<MsDeploySkipRules>` не удаляет с сайта развертывания *пропускаемые* целевые объекты.</span><span class="sxs-lookup"><span data-stu-id="334d6-253">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="334d6-254">С сайта развертывания будут удалены файлы и папки, указанные в элементе `<Content>`.</span><span class="sxs-lookup"><span data-stu-id="334d6-254">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="334d6-255">Допустим, например, что вы развернули веб-приложение со следующими файлами.</span><span class="sxs-lookup"><span data-stu-id="334d6-255">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="334d6-256">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="334d6-256">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="334d6-257">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="334d6-257">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="334d6-258">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="334d6-258">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="334d6-259">Если при этом вы добавили указанную ниже разметку `<MsDeploySkipRules>`, эти файлы не будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="334d6-259">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

``` xml
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

<span data-ttu-id="334d6-260">Показанная выше разметка `<MsDeploySkipRules>` предотвращает развертывание *пропускаемых* файлов, но не удаляет эти файлы после развертывания.</span><span class="sxs-lookup"><span data-stu-id="334d6-260">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="334d6-261">Следующая разметка `<Content>` удалит целевые файлы на сайте развертывания.</span><span class="sxs-lookup"><span data-stu-id="334d6-261">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="334d6-262">Развертывание из командной строки с указанной выше разметкой `<Content>` даст результаты следующего вида.</span><span class="sxs-lookup"><span data-stu-id="334d6-262">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

``` console
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

## <a name="including-files"></a><span data-ttu-id="334d6-263">Включение файлов</span><span class="sxs-lookup"><span data-stu-id="334d6-263">Including files</span></span>

<span data-ttu-id="334d6-264">Показанная ниже разметка позволяет включить папку *изображений*, расположенную не в папке проекта, в папку *wwwroot/images* на сайте публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-264">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="334d6-265">Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-265">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="334d6-266">При добавлении в файл *.csproj* она будет включаться в каждый профиль публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="334d6-266">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="334d6-267">Разметка, выделенная в приведенном ниже примере, показывает, как.</span><span class="sxs-lookup"><span data-stu-id="334d6-267">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="334d6-268">Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="334d6-268">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="334d6-269">Исключить папку *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="334d6-269">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="334d6-270">Исключить файл *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="334d6-270">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="334d6-271">Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="334d6-271">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="334d6-272">Выполнение целевого объекта до или после публикации</span><span class="sxs-lookup"><span data-stu-id="334d6-272">Run a target before or after publishing</span></span>

<span data-ttu-id="334d6-273">Встроенные целевые объекты `BeforePublish` и `AfterPublish` можно использовать для выполнения целевого объекта до или после его публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-273">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="334d6-274">В профиль публикации можно добавить показанную ниже разметку — в этом случае сообщения будут добавлены в вывод консоли до или после публикации.</span><span class="sxs-lookup"><span data-stu-id="334d6-274">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="334d6-275">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="334d6-275">The Kudu service</span></span>

<span data-ttu-id="334d6-276">Просматривать файлы веб-приложения Azure можно в [службе Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="334d6-276">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="334d6-277">Для этого добавьте к имени веб-приложения маркер `scm`.</span><span class="sxs-lookup"><span data-stu-id="334d6-277">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="334d6-278">Пример:</span><span class="sxs-lookup"><span data-stu-id="334d6-278">For example:</span></span>

| <span data-ttu-id="334d6-279">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="334d6-279">URL</span></span>               | <span data-ttu-id="334d6-280">Результат</span><span class="sxs-lookup"><span data-stu-id="334d6-280">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="334d6-281">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="334d6-281">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="334d6-282">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="334d6-282">Kudu sevice</span></span> |

<span data-ttu-id="334d6-283">Для просмотра, редактирования, удаления и добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="334d6-283">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="334d6-284">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="334d6-284">Additional resources</span></span>

- <span data-ttu-id="334d6-285">[Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.</span><span class="sxs-lookup"><span data-stu-id="334d6-285">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="334d6-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и особенности запросов для развертывания.</span><span class="sxs-lookup"><span data-stu-id="334d6-286">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
