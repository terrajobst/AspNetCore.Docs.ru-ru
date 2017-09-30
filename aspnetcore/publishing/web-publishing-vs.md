---
title: "Создание профилей публикации для Visual Studio и MSBuild"
author: rick-anderson
description: "Описание веб-публикации в Visual Studio."
keywords: "ASP.NET Core, веб-публикация, публикация, msbuild, веб-развертывание, публикации dotnet, Visual Studio 2017"
ms.author: riande
manager: wpickett
ms.date: 03/14/2017
ms.topic: article
ms.assetid: 0377a02d-8fda-47a5-929a-24a16e1d2c93
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/web-publishing-vs
ms.openlocfilehash: 665c98b5ac16bb9739af4ac204fca59a55dbb812
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="create-publish-profiles-for-visual-studio-and-msbuild-to-deploy-aspnet-core-apps"></a><span data-ttu-id="865a5-104">Создание профилей публикации для Visual Studio и MSBuild с целью развертывания приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="865a5-104">Create publish profiles for Visual Studio and MSBuild, to deploy ASP.NET Core apps</span></span>

<span data-ttu-id="865a5-105">Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="865a5-105">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="865a5-106">В этой статье рассказывается об использовании Visual Studio 2017 для создания профилей публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-106">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="865a5-107">Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="865a5-107">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span>

<span data-ttu-id="865a5-108">Представленный ниже файл *.csproj* был создан с помощью команды `dotnet new mvc`.</span><span class="sxs-lookup"><span data-stu-id="865a5-108">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="865a5-109">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="865a5-109">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="865a5-110">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="865a5-110">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="865a5-111">Атрибут `Sdk` в элементе `<Project>` (первая строка) показанной выше разметки выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="865a5-111">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="865a5-112">Импортирует файл `props` из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начало.</span><span class="sxs-lookup"><span data-stu-id="865a5-112">Imports the `props` file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="865a5-113">Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.</span><span class="sxs-lookup"><span data-stu-id="865a5-113">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="865a5-114">По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="865a5-114">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="865a5-115">`Microsoft.NET.Sdk.Web` зависит от следующих компонентов.</span><span class="sxs-lookup"><span data-stu-id="865a5-115">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="865a5-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="865a5-116">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="865a5-117">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="865a5-117">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="865a5-118">В связи с чем импортируются следующие файлы `props` и `targets`.</span><span class="sxs-lookup"><span data-stu-id="865a5-118">Which causes the following `props` and `targets` to be imported:</span></span>

* <span data-ttu-id="865a5-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="865a5-119">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="865a5-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="865a5-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="865a5-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="865a5-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="865a5-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="865a5-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="865a5-123">Целевые объекты публикации, в свою очередь, также импортируют соответствующий набор целевых объектов в зависимости от используемых методов публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-123">Publish targets will again import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="865a5-124">Когда MSBuild или Visual Studio загружает проект, на высоком уровне выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="865a5-124">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="865a5-125">сборка проекта;</span><span class="sxs-lookup"><span data-stu-id="865a5-125">Build project</span></span>
* <span data-ttu-id="865a5-126">вычисление файлов для публикации;</span><span class="sxs-lookup"><span data-stu-id="865a5-126">Compute files to publish</span></span>
* <span data-ttu-id="865a5-127">публикация файлов в месте назначения;</span><span class="sxs-lookup"><span data-stu-id="865a5-127">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="865a5-128">вычисление элементов проекта.</span><span class="sxs-lookup"><span data-stu-id="865a5-128">Compute project items</span></span>

<span data-ttu-id="865a5-129">После загрузки проекта вычисляются элементы проекта (файлы).</span><span class="sxs-lookup"><span data-stu-id="865a5-129">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="865a5-130">Порядок обработки файла определяется атрибутом `item type`.</span><span class="sxs-lookup"><span data-stu-id="865a5-130">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="865a5-131">По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`.</span><span class="sxs-lookup"><span data-stu-id="865a5-131">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="865a5-132">Файлы в списке элементов `Compile` компилируются.</span><span class="sxs-lookup"><span data-stu-id="865a5-132">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="865a5-133">Список элементов `Content` содержит предназначенные для публикации файлы и результаты сборки.</span><span class="sxs-lookup"><span data-stu-id="865a5-133">The `Content` item list contains files that will be published in addition to the build outputs.</span></span> <span data-ttu-id="865a5-134">По умолчанию файлы, соответствующие шаблону wwwroot/**, включаются в элемент `Content`.</span><span class="sxs-lookup"><span data-stu-id="865a5-134">By default, files matching the pattern wwwroot/** will be included in the `Content` item.</span></span> <span data-ttu-id="865a5-135">[wwwroot/** — это стандартная маска](https://gruntjs.com/configuring-tasks#globbing-patterns), которая охватывает все файлы в папке *wwwroot* **и** ее подпапках.</span><span class="sxs-lookup"><span data-stu-id="865a5-135">[wwwroot/** is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="865a5-136">Чтобы напрямую добавить в список публикации отдельный файл, можно добавить этот файл в папку *.csproj*, как показано в разделе [Включение файлов](#including-files).</span><span class="sxs-lookup"><span data-stu-id="865a5-136">If you need to explicitly add a file to the publish list you can add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="865a5-137">При нажатии кнопки **Публикация** в Visual Studio или публикации из командной строки происходит следующее.</span><span class="sxs-lookup"><span data-stu-id="865a5-137">When you select the **Publish** button in Visual Studio or when you publish from command line:</span></span>

- <span data-ttu-id="865a5-138">Вычисляются свойства и (или) элементы (файлы, требующие сборки).</span><span class="sxs-lookup"><span data-stu-id="865a5-138">The properties/items are computed (the files that are needed to build).</span></span>
- <span data-ttu-id="865a5-139">Только в Visual Studio: восстанавливаются пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="865a5-139">Visual Studio only: NuGet packages are restored.</span></span>  <span data-ttu-id="865a5-140">(Восстановление выполняется пользователем в интерфейсе командной строки.)</span><span class="sxs-lookup"><span data-stu-id="865a5-140">(Restore needs to be explicit by the user on the CLI.)</span></span>
- <span data-ttu-id="865a5-141">Выполняется сборка проекта.</span><span class="sxs-lookup"><span data-stu-id="865a5-141">The project builds.</span></span>
- <span data-ttu-id="865a5-142">Вычисляются публикуемые элементы (файлы, требующие публикации).</span><span class="sxs-lookup"><span data-stu-id="865a5-142">The publish items are computed (the files that are needed to publish).</span></span>
- <span data-ttu-id="865a5-143">Проект публикуется.</span><span class="sxs-lookup"><span data-stu-id="865a5-143">The project is published.</span></span> <span data-ttu-id="865a5-144">(Вычисляемые файлы копируются в место назначения публикации.)</span><span class="sxs-lookup"><span data-stu-id="865a5-144">(The computed files are copied to the publish destination.)</span></span>

## <a name="simple-command-line-publishing"></a><span data-ttu-id="865a5-145">Простая публикация из командной строки</span><span class="sxs-lookup"><span data-stu-id="865a5-145">Simple command line publishing</span></span>

<span data-ttu-id="865a5-146">Этот метод работает на всех поддерживаемых платформах .NET Core и не требует Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="865a5-146">This section works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="865a5-147">В приведенных ниже примерах команда `dotnet publish` выполняется из папки проекта (содержащей файл *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="865a5-147">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="865a5-148">Если вы не находитесь в папке проекта, путь к файлу проекта можно передать явным образом.</span><span class="sxs-lookup"><span data-stu-id="865a5-148">If you're not in the project folder, you can explicitly pass in the project file path.</span></span> <span data-ttu-id="865a5-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="865a5-149">For example:</span></span>

```console
dotnet publish  c:/webs/web1
```

<span data-ttu-id="865a5-150">Для создания и публикации веб-приложения выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="865a5-150">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet restore
dotnet publish
```

<span data-ttu-id="865a5-151">Команда `dotnet publish` выдает результаты следующего вида.</span><span class="sxs-lookup"><span data-stu-id="865a5-151">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.1.548.43366
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp1.1\Web1.dll
```

<span data-ttu-id="865a5-152">Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="865a5-152">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="865a5-153">Значение параметра `$(Configuration)` по умолчанию — Debug (Отладка).</span><span class="sxs-lookup"><span data-stu-id="865a5-153">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="865a5-154">В приведенном выше примере `<TargetFramework>` — это `netcoreapp1.1`.</span><span class="sxs-lookup"><span data-stu-id="865a5-154">In the sample above, the `<TargetFramework>` is `netcoreapp1.1`.</span></span> <span data-ttu-id="865a5-155">Фактический путь в приведенном выше примере — *bin\Debug\netcoreapp1.1\publish*.</span><span class="sxs-lookup"><span data-stu-id="865a5-155">The actual path in the sample above is *bin\Debug\netcoreapp1.1\publish*.</span></span>

<span data-ttu-id="865a5-156">`dotnet publish -h` отображает справку по публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-156">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="865a5-157">Следующая команда определяет сборку `Release` и папку для публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-157">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="865a5-158">Команда `dotnet publish` вызывает метод `MSBuild`, который, в свою очередь, вызывает целевой объект `Publish`.</span><span class="sxs-lookup"><span data-stu-id="865a5-158">The `dotnet publish` command calls `MSBuild` which invokes the `Publish` target.</span></span> <span data-ttu-id="865a5-159">Все параметры, передаваемые в `dotnet publish`, передаются в `MSBuild`.</span><span class="sxs-lookup"><span data-stu-id="865a5-159">Any parameters passed to `dotnet publish` are passed to `MSBuild`.</span></span> <span data-ttu-id="865a5-160">Параметр `-c` сопоставляется со свойством `Configuration` MSBuild.</span><span class="sxs-lookup"><span data-stu-id="865a5-160">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="865a5-161">Параметр `-o` сопоставляется со свойством `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="865a5-161">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="865a5-162">Свойства MSBuild можно передавать в любом из следующих форматов.</span><span class="sxs-lookup"><span data-stu-id="865a5-162">You can pass MSBuild properties using either of the following formats:</span></span>

- ` p:<NAME>=<VALUE>`
- `/p:<NAME>=<VALUE>`

<span data-ttu-id="865a5-163">Следующая команда публикует сборку `Release` в общую сетевую папку.</span><span class="sxs-lookup"><span data-stu-id="865a5-163">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="865a5-164">Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.</span><span class="sxs-lookup"><span data-stu-id="865a5-164">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="865a5-165">Убедитесь, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="865a5-165">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="865a5-166">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="865a5-166">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="865a5-167">Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="865a5-167">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="865a5-168">Профили публикации</span><span class="sxs-lookup"><span data-stu-id="865a5-168">Publish profiles</span></span>

<span data-ttu-id="865a5-169">В этом разделе для создания профилей публикации используется Visual Studio 2017 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="865a5-169">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="865a5-170">После создания профилей публикацию можно произвести из Visual Studio или из командной строки.</span><span class="sxs-lookup"><span data-stu-id="865a5-170">Once created, you can publish from Visual Studio or the command line.</span></span>

<span data-ttu-id="865a5-171">Профили публикации позволяют упростить процесс публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-171">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="865a5-172">Таких профилей может быть несколько.</span><span class="sxs-lookup"><span data-stu-id="865a5-172">You can have multiple publish profiles.</span></span> <span data-ttu-id="865a5-173">Чтобы создать профиль публикации в Visual Studio, щелкните проект в обозревателе решений правой кнопкой мыши и выберите команду **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="865a5-173">To create a publish profile in Visual Studio, right click on the project in Solution Explore and select **Publish**.</span></span> <span data-ttu-id="865a5-174">Другой вариант: открыть меню сборки и выбрать команду **Опубликовать     \<имя проекта >**.</span><span class="sxs-lookup"><span data-stu-id="865a5-174">Alternatively, you can select **Publish     \<project name>** from the build menu.</span></span> <span data-ttu-id="865a5-175">Откроется вкладка **Публикации** на странице свойств приложения.</span><span class="sxs-lookup"><span data-stu-id="865a5-175">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="865a5-176">Если проект не содержит профиль публикации, откроется следующая страница.</span><span class="sxs-lookup"><span data-stu-id="865a5-176">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка" и выбранным разделом Azure](web-publishing-vs/_static/az.png)

<span data-ttu-id="865a5-179">Если выбран раздел **Папка**, кнопка **Опубликовать** создает профиль публикации в папку и выполняет публикацию.</span><span class="sxs-lookup"><span data-stu-id="865a5-179">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка"](web-publishing-vs/_static/pub1.png)

<span data-ttu-id="865a5-181">После того, как профиль публикации будет создан, вкладка **Публикация** изменит вид, и вы сможете выбрать пункт **Создать профиль**, чтобы создать новый профиль.</span><span class="sxs-lookup"><span data-stu-id="865a5-181">Once a publish profile is created, the **Publish** tab changes, and you select **Create new profile** to create a new profile.</span></span>

![Вкладка **Публикация** на странице свойств приложения с созданным на предыдущем шаге профилем FolderProfile и ссылкой "Создать профиль".](web-publishing-vs/_static/create_new.png)

<span data-ttu-id="865a5-183">Мастер публикации поддерживает следующие целевые объекты публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-183">The Publish wizard supports the following publish targets:</span></span>

- <span data-ttu-id="865a5-184">Служба приложений Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="865a5-184">Microsoft Azure App Service</span></span>
- <span data-ttu-id="865a5-185">IIS, FTP и т. д (для любого веб-сервера)</span><span class="sxs-lookup"><span data-stu-id="865a5-185">IIS, FTP, etc (for any web server)</span></span>
- <span data-ttu-id="865a5-186">Папка</span><span class="sxs-lookup"><span data-stu-id="865a5-186">Folder</span></span>
- <span data-ttu-id="865a5-187">Импорт профиля (позволяет импортировать профиль)</span><span class="sxs-lookup"><span data-stu-id="865a5-187">Import profile (allows you to import a profile).</span></span>
- <span data-ttu-id="865a5-188">Виртуальные машины Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="865a5-188">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="865a5-189">Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="865a5-189">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="865a5-190">При создании профиля публикации с помощью Visual Studio создается файл MSBuild *Properties/PublishProfiles/\<имя публикации>.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="865a5-190">When you create a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="865a5-191">Это файл с расширением *.pubxml* представляет собой файл MSBuild, содержащий настройки конфигурации публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-191">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="865a5-192">Процесс сборки и публикации можно настроить, скорректировав этот файл.</span><span class="sxs-lookup"><span data-stu-id="865a5-192">You can change this file to customize the build and publish process.</span></span> <span data-ttu-id="865a5-193">Этот файл считывается процессом публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-193">This file is read by the publishing process.</span></span> <span data-ttu-id="865a5-194">Свойство `<LastUsedBuildConfiguration>` отличается тем, что является глобальным и не должно присутствовать ни в одном файле, импортируемом в сборку.</span><span class="sxs-lookup"><span data-stu-id="865a5-194">`<LastUsedBuildConfiguration>` is special because it’s a global property and shouldn’t be in any file that’s imported in the build.</span></span> <span data-ttu-id="865a5-195">Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).</span><span class="sxs-lookup"><span data-stu-id="865a5-195">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="865a5-196">Файл с расширением *.pubxml* не должен возвращаться в систему управления версиями, так как зависит от файла *.user*.</span><span class="sxs-lookup"><span data-stu-id="865a5-196">The *.pubxml* file should not be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="865a5-197">Файл *.user* ни в коем случае не должен возвращаться в систему управления версиями, так как может содержать конфиденциальные сведения и быть действительным только для одного пользователя и компьютера.</span><span class="sxs-lookup"><span data-stu-id="865a5-197">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="865a5-198">Конфиденциальные данные (такие как пароль публикации) шифруются на уровне пользователя или компьютера и хранятся в файле *Properties/PublishProfiles/\<имя публикации>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="865a5-198">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="865a5-199">Так как этот файл может содержать конфиденциальные данные, он **не** должен возвращаться в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="865a5-199">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="865a5-200">Общие сведения о том, как опубликовать веб-приложение в ASP.NET Core, см. в статье [Публикация и развертывание](index.md).</span><span class="sxs-lookup"><span data-stu-id="865a5-200">For an overview of how to publish a web app on ASP.NET Core see [Publishing and Deployment](index.md).</span></span> <span data-ttu-id="865a5-201">[Публикация и развертывание](index.md) — это проект с открытым кодом на сайте https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="865a5-201">[Publishing and Deployment](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="865a5-202">В настоящее время `dotnet publish` не позволяет использовать профили публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-202">Currently `dotnet publish` doesn’t have the ability to use publish profiles.</span></span> <span data-ttu-id="865a5-203">Для работы с профилями публикации используйте `dotnet build`.</span><span class="sxs-lookup"><span data-stu-id="865a5-203">To use publish profiles, use `dotnet build`.</span></span> <span data-ttu-id="865a5-204">`dotnet build` вызывает MSBuild в проекте.</span><span class="sxs-lookup"><span data-stu-id="865a5-204">`dotnet build` invokes MSBuild on the project.</span></span> <span data-ttu-id="865a5-205">Кроме того, `msbuild` можно вызвать напрямую.</span><span class="sxs-lookup"><span data-stu-id="865a5-205">Alternatively, call `msbuild` directly.</span></span>

<span data-ttu-id="865a5-206">Для работы с профилем публикации задайте следующие свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="865a5-206">Set the following MSBuild properties when using a publish profile:</span></span>

- `DeployOnBuild=true`
- `PublishProfile=<Publish profile name>`

<span data-ttu-id="865a5-207">Например, при публикации с использованием профиля *FolderProfile* можно выполнить одну из указанных ниже команд.</span><span class="sxs-lookup"><span data-stu-id="865a5-207">For example, when publishing with a profile named *FolderProfile* you can execute either of the commands below.</span></span>

- `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
- `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="865a5-208">При вызове `dotnet build` он вызывает `msbuild` для выполнения сборки и публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-208">When you invoke `dotnet build` it will call `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="865a5-209">Вызов `dotnet build` или `msbuild` по сути работает как передача профиля папки.</span><span class="sxs-lookup"><span data-stu-id="865a5-209">Calling `dotnet build` or `msbuild` is essentially equivalent when you pass in a folder profile.</span></span> <span data-ttu-id="865a5-210">Вызывая MSBuild в Windows напрямую, вы получаете версию MSBuild .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="865a5-210">When calling MSBuild directly on Windows you get the .NET Framework version of MSBuild.</span></span>  <span data-ttu-id="865a5-211">В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows.</span><span class="sxs-lookup"><span data-stu-id="865a5-211">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="865a5-212">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="865a5-212">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="865a5-213">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows).</span><span class="sxs-lookup"><span data-stu-id="865a5-213">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="865a5-214">Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.</span><span class="sxs-lookup"><span data-stu-id="865a5-214">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="865a5-215">Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:</span><span class="sxs-lookup"><span data-stu-id="865a5-215">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile.pubxml?highlight=5,9,11,18)]

<span data-ttu-id="865a5-216">Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`.</span><span class="sxs-lookup"><span data-stu-id="865a5-216">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span>  <span data-ttu-id="865a5-217">При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-217">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="865a5-218">Свойство конфигурации `<LastUsedBuildConfiguration>` имеет особое значение и не должно переопределяться в импортированном файле MSBuild.</span><span class="sxs-lookup"><span data-stu-id="865a5-218">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn’t be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="865a5-219">Переопределить это свойство можно из командной строки.</span><span class="sxs-lookup"><span data-stu-id="865a5-219">You can override this property from the command line.</span></span> <span data-ttu-id="865a5-220">Пример:</span><span class="sxs-lookup"><span data-stu-id="865a5-220">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="865a5-221">С использованием MSBuild</span><span class="sxs-lookup"><span data-stu-id="865a5-221">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="865a5-222">Публикация конечной точки MSDeploy из командной строки</span><span class="sxs-lookup"><span data-stu-id="865a5-222">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="865a5-223">Как уже говорилось, для публикации можно использовать команды `dotnet publish` и `msbuild`.</span><span class="sxs-lookup"><span data-stu-id="865a5-223">As previously mentioned, you can publish using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="865a5-224">Команда `dotnet publish` выполняется в контексте .NET Core.</span><span class="sxs-lookup"><span data-stu-id="865a5-224">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="865a5-225">Команда `msbuild` требует платформы .NET Framework и работает только в средах Windows.</span><span class="sxs-lookup"><span data-stu-id="865a5-225">`msbuild` requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="865a5-226">Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.</span><span class="sxs-lookup"><span data-stu-id="865a5-226">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="865a5-227">В следующем примере мы создали веб-приложение ASP.NET Core (используя команду `dotnet new mvc`) и добавили профиль публикации Azure с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="865a5-227">In the following sample, I created an ASP.NET Core web app ( using `dotnet new mvc`) and added an Azure publish profile with Visual Studio.</span></span>

<span data-ttu-id="865a5-228">Команда `msbuild` выполняется в **командной строке разработчика для VS 2017**.</span><span class="sxs-lookup"><span data-stu-id="865a5-228">You run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="865a5-229">Командная строка разработчика задаст соответствующий файл *msbuild.exe* в пути и некоторые переменные MSBuild.</span><span class="sxs-lookup"><span data-stu-id="865a5-229">The Developer Command Prompt will have the correct *msbuild.exe* in its path and set some MSBuild variables.</span></span>

<span data-ttu-id="865a5-230">MSBuild использует следующий синтаксис.</span><span class="sxs-lookup"><span data-stu-id="865a5-230">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile>  /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="865a5-231">Значение `Password` можно получить из файла *\<Имя публикации>.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="865a5-231">You can get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="865a5-232">Файл *.PublishSettings* можно загрузить следующим образом.</span><span class="sxs-lookup"><span data-stu-id="865a5-232">You can download the *.PublishSettings* file from:</span></span>

- <span data-ttu-id="865a5-233">Обозреватель решений: щелкните веб-приложения правой кнопкой мыши и выберите пункт **Загрузить профиль публикации**.</span><span class="sxs-lookup"><span data-stu-id="865a5-233">Solution Explorer: Right click on the Web App and select **Download Publish Profile**.</span></span>
- <span data-ttu-id="865a5-234">Портал управления Azure: выберите пункт **Получить профиль публикации** в колонке "Веб-приложение".</span><span class="sxs-lookup"><span data-stu-id="865a5-234">The Azure Management Portal: Select **Get publish profile** from the  Web App blade.</span></span>

<span data-ttu-id="865a5-235">`Username` можно найти в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-235">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="865a5-236">В следующем примере используется профиль публикации "Web11112 - Web Deploy".</span><span class="sxs-lookup"><span data-stu-id="865a5-236">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="865a5-237">Исключение файлов</span><span class="sxs-lookup"><span data-stu-id="865a5-237">Excluding files</span></span>

<span data-ttu-id="865a5-238">При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="865a5-238">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="865a5-239">`msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="865a5-239">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="865a5-240">Например, представленная ниже разметка элемента `<Content>` исключит все текстовые файлы (*.txt*) из папки *wwwroot/content* и всех ее подпапок.</span><span class="sxs-lookup"><span data-stu-id="865a5-240">For example, the following `<Content>` element markup will exclude all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="865a5-241">Показанная выше разметка может быть добавлена в профиль публикации или в файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="865a5-241">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="865a5-242">При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="865a5-242">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="865a5-243">Представленная ниже разметка элемента `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot/content*.</span><span class="sxs-lookup"><span data-stu-id="865a5-243">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="865a5-244">Элемент `<MsDeploySkipRules>` не удаляет с сайта развертывания *пропускаемые* целевые объекты.</span><span class="sxs-lookup"><span data-stu-id="865a5-244">`<MsDeploySkipRules>`  will not delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="865a5-245">С сайта развертывания будут удалены файлы и папки, указанные в элементе `<Content>`.</span><span class="sxs-lookup"><span data-stu-id="865a5-245">`<Content>` targeted files and folders will be deleted from the deployment site.</span></span> <span data-ttu-id="865a5-246">Допустим, например, что вы развернули веб-приложение со следующими файлами.</span><span class="sxs-lookup"><span data-stu-id="865a5-246">For example, suppose you had deployed a web app with the following files:</span></span>

- <span data-ttu-id="865a5-247">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="865a5-247">*Views/Home/About1.cshtml*</span></span>
- <span data-ttu-id="865a5-248">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="865a5-248">*Views/Home/About2.cshtml*</span></span>
- <span data-ttu-id="865a5-249">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="865a5-249">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="865a5-250">Если при этом вы добавили указанную ниже разметку `<MsDeploySkipRules>`, эти файлы не будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="865a5-250">If you added the following `<MsDeploySkipRules>` markup, those files would not be deleted on the deployment site.</span></span>

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

<span data-ttu-id="865a5-251">Показанная выше разметка `<MsDeploySkipRules>` предотвращает развертывание *пропускаемых* файлов, но не удаляет эти файлы после развертывания.</span><span class="sxs-lookup"><span data-stu-id="865a5-251">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed, but will not delete those files once they are deployed.</span></span>

<span data-ttu-id="865a5-252">Следующая разметка `<Content>` удалит целевые файлы на сайте развертывания.</span><span class="sxs-lookup"><span data-stu-id="865a5-252">The following `<Content>` markup would delete the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="865a5-253">Развертывание из командной строки с указанной выше разметкой `<Content>` даст результаты следующего вида.</span><span class="sxs-lookup"><span data-stu-id="865a5-253">Using command line deployment with the `<Content>` markup above would result in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="865a5-254">Включение файлов</span><span class="sxs-lookup"><span data-stu-id="865a5-254">Including files</span></span>

<span data-ttu-id="865a5-255">Показанная ниже разметка позволяет включить папку *изображений*, расположенную не в папке проекта, в папку *wwwroot/images* на сайте публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-255">The following markup can be used to include an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="865a5-256">Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-256">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="865a5-257">При добавлении в файл *.csproj* она будет включаться в каждый профиль публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="865a5-257">If it's added to the *.csproj* file, it will be included in each publish profile in the project.</span></span>

<span data-ttu-id="865a5-258">Разметка, выделенная в приведенном ниже примере, показывает, как.</span><span class="sxs-lookup"><span data-stu-id="865a5-258">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="865a5-259">Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="865a5-259">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="865a5-260">Исключить папку *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="865a5-260">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="865a5-261">Исключить файл *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="865a5-261">Exclude *Views\Home\About2.cshtml*.</span></span>

[!code-xml[Main](web-publishing-vs/sample/FolderProfile2.pubxml?highlight=21-29)]

<span data-ttu-id="865a5-262">Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="865a5-262">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="865a5-263">Выполнение целевого объекта до или после публикации</span><span class="sxs-lookup"><span data-stu-id="865a5-263">Run a target before or after publishing</span></span>

<span data-ttu-id="865a5-264">Встроенные целевые объекты `BeforePublish` и `AfterPublish` можно использовать для выполнения целевого объекта до или после его публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-264">The builtin `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="865a5-265">В профиль публикации можно добавить показанную ниже разметку — в этом случае сообщения будут добавлены в вывод консоли до или после публикации.</span><span class="sxs-lookup"><span data-stu-id="865a5-265">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="865a5-266">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="865a5-266">The Kudu service</span></span>

<span data-ttu-id="865a5-267">Просматривать файлы веб-приложения Azure можно в [службе Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="865a5-267">To view the files on your Azure Web App, use the [kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="865a5-268">Для этого добавьте к имени веб-приложения маркер `scm`.</span><span class="sxs-lookup"><span data-stu-id="865a5-268">Append the `scm` token to the name or your Web App.</span></span> <span data-ttu-id="865a5-269">Пример:</span><span class="sxs-lookup"><span data-stu-id="865a5-269">For example:</span></span>

| <span data-ttu-id="865a5-270">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="865a5-270">URL</span></span>               | <span data-ttu-id="865a5-271">Результат</span><span class="sxs-lookup"><span data-stu-id="865a5-271">Result</span></span>|
| ----------------- | ------------ |
| `http://mysite.azurewebsites.net/` | <span data-ttu-id="865a5-272">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="865a5-272">Web App</span></span> |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="865a5-273">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="865a5-273">Kudu sevice</span></span> |

<span data-ttu-id="865a5-274">Для просмотра, редактирования, удаления и добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="865a5-274">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="865a5-275">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="865a5-275">Additional resources</span></span>

- <span data-ttu-id="865a5-276">[Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (msdeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.</span><span class="sxs-lookup"><span data-stu-id="865a5-276">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy)  (msdeploy) simplifies deployment of Web applications and Web sites to IIS servers.</span></span>

- <span data-ttu-id="865a5-277">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и особенности запросов для развертывания.</span><span class="sxs-lookup"><span data-stu-id="865a5-277">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
