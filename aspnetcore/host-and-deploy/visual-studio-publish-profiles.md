---
title: "Visual Studio профилей публикации для развертывания приложения ASP.NET Core"
author: rick-anderson
description: "Узнайте, как для создания профилей публикации для приложения ASP.NET Core в Visual Studio."
ms.author: riande
manager: wpickett
ms.custom: mvc
ms.date: 09/26/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: 1f403447c39db4ebfe3dafda591602f0dc9db8c3
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="4204e-103">Visual Studio профилей публикации для развертывания приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4204e-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="4204e-104">Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="4204e-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4204e-105">В этой статье рассказывается об использовании Visual Studio 2017 для создания профилей публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-105">This article focuses on using Visual Studio 2017 to create publish profiles.</span></span> <span data-ttu-id="4204e-106">Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="4204e-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio 2017.</span></span> <span data-ttu-id="4204e-107">Статья содержит сведения о процессе публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-107">The article provides details of the publishing process.</span></span> <span data-ttu-id="4204e-108">Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="4204e-108">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="4204e-109">Представленный ниже файл *.csproj* был создан с помощью команды `dotnet new mvc`.</span><span class="sxs-lookup"><span data-stu-id="4204e-109">The following *.csproj* file was created with the command `dotnet new mvc`:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4204e-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4204e-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

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

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4204e-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4204e-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="4204e-112">Атрибут `Sdk` в элементе `<Project>` (первая строка) показанной выше разметки выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="4204e-112">The `Sdk` attribute in the `<Project>` element (in the first line) of the markup above does the following:</span></span>

* <span data-ttu-id="4204e-113">Импортирует файл свойств из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* в начале.</span><span class="sxs-lookup"><span data-stu-id="4204e-113">Imports the properties file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.Props* at the beginning.</span></span>
* <span data-ttu-id="4204e-114">Импортирует целевой файл из *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* в конец.</span><span class="sxs-lookup"><span data-stu-id="4204e-114">Imports the targets file from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets* at the end.</span></span>

<span data-ttu-id="4204e-115">По умолчанию `MSBuildSDKsPath` (с Visual Studio 2017 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="4204e-115">The default location for `MSBuildSDKsPath` (with Visual Studio 2017 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2017\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="4204e-116">`Microsoft.NET.Sdk.Web` зависит от следующих компонентов.</span><span class="sxs-lookup"><span data-stu-id="4204e-116">`Microsoft.NET.Sdk.Web` depends on:</span></span>

* <span data-ttu-id="4204e-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span><span class="sxs-lookup"><span data-stu-id="4204e-117">*Microsoft.NET.Sdk.Web.ProjectSystem*</span></span>
* <span data-ttu-id="4204e-118">*Microsoft.NET.Sdk.Publish*</span><span class="sxs-lookup"><span data-stu-id="4204e-118">*Microsoft.NET.Sdk.Publish*</span></span>

<span data-ttu-id="4204e-119">Что вызывает следующие свойства и целевые объекты для импорта:</span><span class="sxs-lookup"><span data-stu-id="4204e-119">Which causes the following properties and targets to be imported:</span></span>

* <span data-ttu-id="4204e-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="4204e-120">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="4204e-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="4204e-121">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web.ProjectSystem\Sdk\Sdk.targets</span></span>
* <span data-ttu-id="4204e-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span><span class="sxs-lookup"><span data-stu-id="4204e-122">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.Props</span></span>
* <span data-ttu-id="4204e-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span><span class="sxs-lookup"><span data-stu-id="4204e-123">$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Publish\Sdk\Sdk.targets</span></span>

<span data-ttu-id="4204e-124">Опубликуйте право набор целевых объектов в зависимости от способа публикации используется Импорт целевых объектов.</span><span class="sxs-lookup"><span data-stu-id="4204e-124">Publish targets import the right set of targets based on the publish method used.</span></span>

<span data-ttu-id="4204e-125">Когда MSBuild или Visual Studio загружает проект, на высоком уровне выполняются следующие действия:</span><span class="sxs-lookup"><span data-stu-id="4204e-125">When MSBuild or Visual Studio loads a project, the following high level actions are performed:</span></span>

* <span data-ttu-id="4204e-126">сборка проекта;</span><span class="sxs-lookup"><span data-stu-id="4204e-126">Build project</span></span>
* <span data-ttu-id="4204e-127">вычисление файлов для публикации;</span><span class="sxs-lookup"><span data-stu-id="4204e-127">Compute files to publish</span></span>
* <span data-ttu-id="4204e-128">публикация файлов в месте назначения;</span><span class="sxs-lookup"><span data-stu-id="4204e-128">Publish files to destination</span></span>

### <a name="compute-project-items"></a><span data-ttu-id="4204e-129">вычисление элементов проекта.</span><span class="sxs-lookup"><span data-stu-id="4204e-129">Compute project items</span></span>

<span data-ttu-id="4204e-130">После загрузки проекта вычисляются элементы проекта (файлы).</span><span class="sxs-lookup"><span data-stu-id="4204e-130">When the project is loaded, the project items (files) are computed.</span></span> <span data-ttu-id="4204e-131">Порядок обработки файла определяется атрибутом `item type`.</span><span class="sxs-lookup"><span data-stu-id="4204e-131">The `item type` attribute determines how the file is processed.</span></span> <span data-ttu-id="4204e-132">По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`.</span><span class="sxs-lookup"><span data-stu-id="4204e-132">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="4204e-133">Файлы в списке элементов `Compile` компилируются.</span><span class="sxs-lookup"><span data-stu-id="4204e-133">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="4204e-134">`Content` Список элементов содержит файлы, которые публикуются в дополнение к выходных данных сборки.</span><span class="sxs-lookup"><span data-stu-id="4204e-134">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="4204e-135">По умолчанию файлы, соответствующие шаблону `wwwroot/**` включаются в `Content` элемента.</span><span class="sxs-lookup"><span data-stu-id="4204e-135">By default, files matching the pattern `wwwroot/**` are included in the `Content` item.</span></span> <span data-ttu-id="4204e-136">[wwwroot /\* \* -это шаблон глобализацию](https://gruntjs.com/configuring-tasks#globbing-patterns) указывает все файлы в *wwwroot* папки **и** вложенные папки.</span><span class="sxs-lookup"><span data-stu-id="4204e-136">[wwwroot/\*\* is a globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) that specifies all files in the *wwwroot* folder **and** subfolders.</span></span> <span data-ttu-id="4204e-137">Чтобы напрямую добавить в список публикации отдельный файл, можно добавить этот файл в файл *CSPROJ*, как показано в разделе [Включение файлов](#including-files).</span><span class="sxs-lookup"><span data-stu-id="4204e-137">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in [Including Files](#including-files).</span></span>

<span data-ttu-id="4204e-138">При выборе **публикации** кнопки в Visual Studio или при публикации из командной строки:</span><span class="sxs-lookup"><span data-stu-id="4204e-138">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="4204e-139">Вычисляются свойства и (или) элементы (файлы, требующие сборки).</span><span class="sxs-lookup"><span data-stu-id="4204e-139">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="4204e-140">Только в Visual Studio: восстанавливаются пакеты NuGet.</span><span class="sxs-lookup"><span data-stu-id="4204e-140">Visual Studio only: NuGet packages are restored.</span></span> <span data-ttu-id="4204e-141">(Восстановление выполняется пользователем в интерфейсе командной строки.)</span><span class="sxs-lookup"><span data-stu-id="4204e-141">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="4204e-142">Выполняется сборка проекта.</span><span class="sxs-lookup"><span data-stu-id="4204e-142">The project builds.</span></span>
* <span data-ttu-id="4204e-143">Вычисляются публикуемые элементы (файлы, требующие публикации).</span><span class="sxs-lookup"><span data-stu-id="4204e-143">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="4204e-144">Проект публикуется.</span><span class="sxs-lookup"><span data-stu-id="4204e-144">The project is published.</span></span> <span data-ttu-id="4204e-145">(Вычисляемые файлы копируются в место назначения публикации.)</span><span class="sxs-lookup"><span data-stu-id="4204e-145">(The computed files are copied to the publish destination.)</span></span>

<span data-ttu-id="4204e-146">Когда ссылается на проект ASP.NET Core `Microsoft.NET.Sdk.Web` в файле проекта *app_offline.htm* файл помещается в корне каталога веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="4204e-146">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="4204e-147">Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="4204e-147">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="4204e-148">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span><span class="sxs-lookup"><span data-stu-id="4204e-148">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#appofflinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="4204e-149">Основные публикации командной строки</span><span class="sxs-lookup"><span data-stu-id="4204e-149">Basic command-line publishing</span></span>

<span data-ttu-id="4204e-150">Публикация командной строки работает на всех платформах .NET Core поддерживается и не требует Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4204e-150">Command-line publishing works on all .NET Core supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="4204e-151">В приведенных ниже примерах команда `dotnet publish` выполняется из папки проекта (содержащей файл *.csproj*).</span><span class="sxs-lookup"><span data-stu-id="4204e-151">In the samples below, the `dotnet publish` command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="4204e-152">В противном случае в папке проекта явным образом передать в путь к файлу проекта.</span><span class="sxs-lookup"><span data-stu-id="4204e-152">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="4204e-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="4204e-153">For example:</span></span>

```console
dotnet publish c:/webs/web1
```

<span data-ttu-id="4204e-154">Для создания и публикации веб-приложения выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="4204e-154">Run the following commands to create and publish a web app:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="4204e-155">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="4204e-155">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```console
dotnet new mvc
dotnet publish
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="4204e-156">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="4204e-156">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```console
dotnet new mvc
dotnet restore
dotnet publish
```

---

<span data-ttu-id="4204e-157">Команда `dotnet publish` выдает результаты следующего вида.</span><span class="sxs-lookup"><span data-stu-id="4204e-157">The `dotnet publish` produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version 15.3.409.57025 for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp2.0\publish\
```

<span data-ttu-id="4204e-158">Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="4204e-158">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="4204e-159">Значение параметра `$(Configuration)` по умолчанию — Debug (Отладка).</span><span class="sxs-lookup"><span data-stu-id="4204e-159">The default for `$(Configuration)` is Debug.</span></span> <span data-ttu-id="4204e-160">В приведенном выше примере `<TargetFramework>` — это `netcoreapp2.0`.</span><span class="sxs-lookup"><span data-stu-id="4204e-160">In the sample above, the `<TargetFramework>` is `netcoreapp2.0`.</span></span>

<span data-ttu-id="4204e-161">`dotnet publish -h` отображает справку по публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-161">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="4204e-162">Следующая команда определяет сборку `Release` и папку для публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-162">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:/MyWebs/test
```

<span data-ttu-id="4204e-163">`dotnet publish` Команда вызывает MSBuild, который вызывает `Publish` цели.</span><span class="sxs-lookup"><span data-stu-id="4204e-163">The `dotnet publish` command calls MSBuild which invokes the `Publish` target.</span></span> <span data-ttu-id="4204e-164">Любые параметры, передаваемые в `dotnet publish` передаются в MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4204e-164">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="4204e-165">Параметр `-c` сопоставляется со свойством `Configuration` MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4204e-165">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="4204e-166">Параметр `-o` сопоставляется со свойством `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="4204e-166">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="4204e-167">Свойства MSBuild могут передаваться с помощью любого из следующих форматов:</span><span class="sxs-lookup"><span data-stu-id="4204e-167">MSBuild properties can be passed using either of the following formats:</span></span>

* ` p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="4204e-168">Следующая команда публикует сборку `Release` в общую сетевую папку.</span><span class="sxs-lookup"><span data-stu-id="4204e-168">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="4204e-169">Общая сетевая папка указывается с помощью символов косой черты (*//r8/*) и работает на всех поддерживаемых платформах .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4204e-169">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="4204e-170">Убедитесь, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="4204e-170">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="4204e-171">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="4204e-171">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="4204e-172">Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="4204e-172">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="4204e-173">Профили публикации</span><span class="sxs-lookup"><span data-stu-id="4204e-173">Publish profiles</span></span>

<span data-ttu-id="4204e-174">В этом разделе для создания профилей публикации используется Visual Studio 2017 и более поздних версий.</span><span class="sxs-lookup"><span data-stu-id="4204e-174">This section uses Visual Studio 2017 and higher to create publishing profiles.</span></span> <span data-ttu-id="4204e-175">После создания публикации из Visual Studio или в командной строке доступна.</span><span class="sxs-lookup"><span data-stu-id="4204e-175">Once created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="4204e-176">Профили публикации позволяют упростить процесс публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-176">Publish profiles can simplify the publishing process.</span></span> <span data-ttu-id="4204e-177">Несколько профилей публикации может существовать.</span><span class="sxs-lookup"><span data-stu-id="4204e-177">Multiple publish profiles can exist.</span></span> <span data-ttu-id="4204e-178">Чтобы создать профиль публикации в Visual Studio, щелкните правой кнопкой мыши проект в обозревателе решений и выберите **публикации**.</span><span class="sxs-lookup"><span data-stu-id="4204e-178">To create a publish profile in Visual Studio, right-click on the project in Solution Explorer and select **Publish**.</span></span> <span data-ttu-id="4204e-179">Можно также выбрать **публикации \<имя проекта >** из меню "Построение".</span><span class="sxs-lookup"><span data-stu-id="4204e-179">Alternatively, select **Publish \<project name>** from the build menu.</span></span> <span data-ttu-id="4204e-180">Откроется вкладка **Публикации** на странице свойств приложения.</span><span class="sxs-lookup"><span data-stu-id="4204e-180">The **Publish** tab of the application capacities page is displayed.</span></span> <span data-ttu-id="4204e-181">Если проект не содержит профиль публикации, откроется следующая страница.</span><span class="sxs-lookup"><span data-stu-id="4204e-181">If the project doesn't contain a publish profile, the following page is displayed:</span></span>

![Выбрана вкладка публикации страницы емкости приложения отображение Azure, IIS, FTB, папка с Azure.](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="4204e-184">Если выбран раздел **Папка**, кнопка **Опубликовать** создает профиль публикации в папку и выполняет публикацию.</span><span class="sxs-lookup"><span data-stu-id="4204e-184">When **Folder** is selected, the **Publish** button creates a folder publish profile and publishes.</span></span>

![Вкладка **Публикация** на странице свойств приложения с разделами "Azure", "IIS, FTB" и "Папка"](visual-studio-publish-profiles/_static/pub1.png)

<span data-ttu-id="4204e-186">После создания профиля публикации **публикации** изменений и выберите **Создание нового профиля** для создания нового профиля.</span><span class="sxs-lookup"><span data-stu-id="4204e-186">Once a publish profile is created, the **Publish** tab changes, and select **Create new profile** to create a new profile.</span></span>

![Вкладка **Публикация** на странице свойств приложения с созданным на предыдущем шаге профилем FolderProfile и ссылкой "Создать профиль".](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="4204e-188">Мастер публикации поддерживает следующие целевые объекты публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-188">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="4204e-189">Служба приложений Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4204e-189">Microsoft Azure App Service</span></span>
* <span data-ttu-id="4204e-190">IIS, FTP и т. д (для любого веб-сервера)</span><span class="sxs-lookup"><span data-stu-id="4204e-190">IIS, FTP, etc (for any web server)</span></span>
* <span data-ttu-id="4204e-191">Папка</span><span class="sxs-lookup"><span data-stu-id="4204e-191">Folder</span></span>
* <span data-ttu-id="4204e-192">Импорт профиля (допускает Импорт профиля).</span><span class="sxs-lookup"><span data-stu-id="4204e-192">Import profile (allows profile import).</span></span>
* <span data-ttu-id="4204e-193">Виртуальные машины Microsoft Azure</span><span class="sxs-lookup"><span data-stu-id="4204e-193">Microsoft Azure Virtual Machines</span></span>

<span data-ttu-id="4204e-194">Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="4204e-194">See [What publishing options are right for me?](https://docs.microsoft.com/visualstudio/ide/not-in-toc/web-publish-options) for more information.</span></span>

<span data-ttu-id="4204e-195">При создании профиля публикации с помощью Visual Studio *свойства/PublishProfiles/\<имя публикации > .pubxml* создается файл MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4204e-195">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/\<publish name>.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="4204e-196">Это файл с расширением *.pubxml* представляет собой файл MSBuild, содержащий настройки конфигурации публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-196">This *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="4204e-197">Этот файл можно изменить для настройки процесса построения и публикации процесс.</span><span class="sxs-lookup"><span data-stu-id="4204e-197">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="4204e-198">Этот файл считывается процессом публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-198">This file is read by the publishing process.</span></span> <span data-ttu-id="4204e-199">`<LastUsedBuildConfiguration>`является специальной, так как он является глобальным и не должны быть в любом файле, который импортируется в сборке.</span><span class="sxs-lookup"><span data-stu-id="4204e-199">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="4204e-200">Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).</span><span class="sxs-lookup"><span data-stu-id="4204e-200">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more info.</span></span>
<span data-ttu-id="4204e-201">*.Pubxml* файл не следует возвращен в систему управления версиями, так как он зависит от *.user* файла.</span><span class="sxs-lookup"><span data-stu-id="4204e-201">The *.pubxml* file shouldn't be checked into source control because it depends on the *.user* file.</span></span> <span data-ttu-id="4204e-202">Файл *.user* ни в коем случае не должен возвращаться в систему управления версиями, так как может содержать конфиденциальные сведения и быть действительным только для одного пользователя и компьютера.</span><span class="sxs-lookup"><span data-stu-id="4204e-202">The *.user* file should never be checked into source control because it can contain sensitive information and it's only valid for one user and machine.</span></span>

<span data-ttu-id="4204e-203">Конфиденциальные данные (такие как пароль публикации) шифруются на уровне пользователя или компьютера и хранятся в файле *Properties/PublishProfiles/\<имя публикации>.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="4204e-203">Sensitive information (like the publish password) is encrypted on a per user/machine level and stored in the *Properties/PublishProfiles/\<publish name>.pubxml.user* file.</span></span> <span data-ttu-id="4204e-204">Так как этот файл может содержать конфиденциальные данные, он **не** должен возвращаться в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="4204e-204">Because this file can contain sensitive information, it should **not** be checked into source control.</span></span>

<span data-ttu-id="4204e-205">Общие сведения о том, как опубликовать веб-приложения ASP.NET Core. в разделе [узла и развернуть](index.md).</span><span class="sxs-lookup"><span data-stu-id="4204e-205">For an overview of how to publish a web app on ASP.NET Core see [Host and deploy](index.md).</span></span> <span data-ttu-id="4204e-206">[Размещать и развертывать](index.md) является проекта с открытым кодом на https://github.com/aspnet/websdk.</span><span class="sxs-lookup"><span data-stu-id="4204e-206">[Host and deploy](index.md) is an open source project at https://github.com/aspnet/websdk.</span></span>

<span data-ttu-id="4204e-207">`dotnet publish`можно использовать папку MSDeploy, и [KUDU](https://github.com/projectkudu/kudu/wiki) профили публикации:</span><span class="sxs-lookup"><span data-stu-id="4204e-207">`dotnet publish` can use folder, MSDeploy, and [KUDU](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>
 
<span data-ttu-id="4204e-208">Папка (кросс-платформенное):`dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span><span class="sxs-lookup"><span data-stu-id="4204e-208">Folder (works cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>`</span></span>

<span data-ttu-id="4204e-209">MSDeploy (в настоящее время работает только в windows, поскольку MSDeploy не кросс платформенных):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span><span class="sxs-lookup"><span data-stu-id="4204e-209">MSDeploy (currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>`</span></span>

<span data-ttu-id="4204e-210">MSDeploy пакета (в настоящее время работает только в windows, поскольку MSDeploy не кросс платформенных):`dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span><span class="sxs-lookup"><span data-stu-id="4204e-210">MSDeploy package(currently this only works in windows since MSDeploy isn't cross-platform): `dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>`</span></span>

<span data-ttu-id="4204e-211">В примерах выше **не** передачи `deployonbuild` для `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="4204e-211">In the preceeding samples, **don't** pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="4204e-212">Дополнительные сведения см. в разделе [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="4204e-212">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="4204e-213">`dotnet publish` поддерживает API KUDU для публикации в Azure с любой платформы.</span><span class="sxs-lookup"><span data-stu-id="4204e-213">`dotnet publish` supports KUDU apis to publish to Azure from any platform.</span></span> <span data-ttu-id="4204e-214">Visual Studio опубликовать поддерживает KUDU API-интерфейсов, но поддерживается websdk для перекрестного plat публикации в Azure.</span><span class="sxs-lookup"><span data-stu-id="4204e-214">Visual Studio publish does support the KUDU APIs but it's supported by websdk for cross plat publish to Azure.</span></span>

<span data-ttu-id="4204e-215">Добавьте профиль публикации в папку *Properties/PublishProfiles* со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="4204e-215">Add a publish profile to *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="4204e-216">Выполните следующую команду моментально настройки публикации содержимого и его публикация в Azure с помощью API-интерфейсы KUDU:</span><span class="sxs-lookup"><span data-stu-id="4204e-216">Running the following command zips up the publish contents and publish it to Azure using the KUDU APIs:</span></span>

`dotnet publish /p:PublishProfile=Azure /p:Configuration=Release`

<span data-ttu-id="4204e-217">Для работы с профилем публикации задайте следующие свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4204e-217">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile=<Publish profile name>`

<span data-ttu-id="4204e-218">При публикации с использованием профиля с именем *FolderProfile*, можно выполнить одну из приведенных ниже команд:</span><span class="sxs-lookup"><span data-stu-id="4204e-218">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="4204e-219">При вызове `dotnet build`, он вызывает `msbuild` для выполнения сборки и процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-219">When invoking `dotnet build`, it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="4204e-220">Вызов `dotnet build` или `msbuild` эквивалентно по существу при передаче в папке профиля.</span><span class="sxs-lookup"><span data-stu-id="4204e-220">Calling `dotnet build` or `msbuild` is essentially equivalent when passing in a folder profile.</span></span> <span data-ttu-id="4204e-221">При вызове MSBuild непосредственно в Windows, .NET Framework версии MSBuild используется.</span><span class="sxs-lookup"><span data-stu-id="4204e-221">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="4204e-222">В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows.</span><span class="sxs-lookup"><span data-stu-id="4204e-222">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="4204e-223">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="4204e-223">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="4204e-224">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows).</span><span class="sxs-lookup"><span data-stu-id="4204e-224">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="4204e-225">Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.</span><span class="sxs-lookup"><span data-stu-id="4204e-225">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="4204e-226">Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:</span><span class="sxs-lookup"><span data-stu-id="4204e-226">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="4204e-227">Обратите внимание на то, что параметр `<LastUsedBuildConfiguration>` имеет значение `Release`.</span><span class="sxs-lookup"><span data-stu-id="4204e-227">Note `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="4204e-228">При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-228">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="4204e-229">`<LastUsedBuildConfiguration>` Свойство конфигурации является особенным и не должен переопределяться из импортированного файла MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4204e-229">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="4204e-230">Это свойство можно переопределить с помощью командной строки.</span><span class="sxs-lookup"><span data-stu-id="4204e-230">This property can be overridden from the command line.</span></span> <span data-ttu-id="4204e-231">Пример:</span><span class="sxs-lookup"><span data-stu-id="4204e-231">For example:</span></span>

`dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="4204e-232">С использованием MSBuild</span><span class="sxs-lookup"><span data-stu-id="4204e-232">Using MSBuild:</span></span>

`msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="4204e-233">Публикация конечной точки MSDeploy из командной строки</span><span class="sxs-lookup"><span data-stu-id="4204e-233">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="4204e-234">Как упоминалось ранее, публикация может выполняться с помощью `dotnet publish` или `msbuild` команды.</span><span class="sxs-lookup"><span data-stu-id="4204e-234">As previously mentioned, publishing can be accomplished using `dotnet publish` or the `msbuild` command.</span></span> <span data-ttu-id="4204e-235">Команда `dotnet publish` выполняется в контексте .NET Core.</span><span class="sxs-lookup"><span data-stu-id="4204e-235">`dotnet publish` runs in the context of .NET Core.</span></span> <span data-ttu-id="4204e-236">`msbuild` Команды требуется платформа .NET framework и поэтому ограничена среды Windows.</span><span class="sxs-lookup"><span data-stu-id="4204e-236">The `msbuild` command requires .NET framework, and is therefore limited to Windows environments.</span></span>

<span data-ttu-id="4204e-237">Простейший способ публикации с помощью MSDeploy — это сначала создать профиль публикации в Visual Studio 2017, а затем применить этот профиль из командной строки.</span><span class="sxs-lookup"><span data-stu-id="4204e-237">The easiest way to publish with MSDeploy is to first create a publish profile in Visual Studio 2017 and use the profile from the command line.</span></span>

<span data-ttu-id="4204e-238">В следующем примере создается веб-приложение ASP.NET Core (с помощью `dotnet new mvc`), и профиль публикации Azure добавляется с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4204e-238">In the following sample, an ASP.NET Core web app is created (using `dotnet new mvc`), and an Azure publish profile is added with Visual Studio.</span></span>

<span data-ttu-id="4204e-239">Запустите `msbuild` из **Командная строка разработчика для VS 2017 г**.</span><span class="sxs-lookup"><span data-stu-id="4204e-239">Run `msbuild` from a **Developer Command Prompt for VS 2017**.</span></span> <span data-ttu-id="4204e-240">Командная строка разработчика содержит правильные *msbuild.exe* в пути с некоторым набором переменных MSBuild.</span><span class="sxs-lookup"><span data-stu-id="4204e-240">The Developer Command Prompt has the correct *msbuild.exe* in its path with some MSBuild variables set.</span></span>

<span data-ttu-id="4204e-241">MSBuild использует следующий синтаксис.</span><span class="sxs-lookup"><span data-stu-id="4204e-241">MSBuild uses the following syntax:</span></span>

`msbuild <path-to-project-file> /p:DeployOnBuild=true /p:PublishProfile=<Publish Profile> /p:Username=<USERNAME> /p:Password=<PASSWORD>`

<span data-ttu-id="4204e-242">Получить `Password` из  *\<имя публикации >. PublishSettings* файл.</span><span class="sxs-lookup"><span data-stu-id="4204e-242">Get the `Password` from the *\<Publish name>.PublishSettings* file.</span></span> <span data-ttu-id="4204e-243">Загрузить *. PublishSettings* файл либо из:</span><span class="sxs-lookup"><span data-stu-id="4204e-243">Download the *.PublishSettings* file from either:</span></span>

* <span data-ttu-id="4204e-244">Обозреватель решений: Щелкните правой кнопкой мыши на веб-приложения и выберите **загрузить профиль публикации**.</span><span class="sxs-lookup"><span data-stu-id="4204e-244">Solution Explorer: Right-click on the Web App and select **Download Publish Profile**.</span></span>
* <span data-ttu-id="4204e-245">Портал управления Azure: Выберите **получение профиля публикации** из колонки веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="4204e-245">The Azure Management Portal: Select **Get publish profile** from the Web App blade.</span></span>

<span data-ttu-id="4204e-246">`Username` можно найти в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-246">`Username` can be found in the publish profile.</span></span>

<span data-ttu-id="4204e-247">В следующем примере используется профиль публикации "Web11112 - Web Deploy".</span><span class="sxs-lookup"><span data-stu-id="4204e-247">The following sample uses the "Web11112 - Web Deploy" publish profile:</span></span>

```console
msbuild "C:\Webs\Web1\Web1.csproj" /p:DeployOnBuild=true
 /p:PublishProfile="Web11112 - Web Deploy"  /p:Username="$Web11112"
 /p:Password="<password removed>"
```

## <a name="excluding-files"></a><span data-ttu-id="4204e-248">Исключение файлов</span><span class="sxs-lookup"><span data-stu-id="4204e-248">Excluding files</span></span>

<span data-ttu-id="4204e-249">При публикации веб-приложений ASP.NET Core включаются артефакты сборки и содержимое папки *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="4204e-249">When publishing ASP.NET Core web apps, the build artifacts and contents of the *wwwroot* folder are included.</span></span> <span data-ttu-id="4204e-250">`msbuild` поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="4204e-250">`msbuild` supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="4204e-251">Например, следующая `<Content>` элемент разметки исключает весь текст (*.txt*) файлов из *wwwroot/content* и всех ее вложенных папок.</span><span class="sxs-lookup"><span data-stu-id="4204e-251">For example, the following `<Content>` element markup excludes all text (*.txt*) files from the *wwwroot/content* folder and all its subfolders.</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="4204e-252">Показанная выше разметка может быть добавлена в профиль публикации или в файл *.csproj*.</span><span class="sxs-lookup"><span data-stu-id="4204e-252">The markup above can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="4204e-253">При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="4204e-253">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="4204e-254">Представленная ниже разметка элемента `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot/content*.</span><span class="sxs-lookup"><span data-stu-id="4204e-254">The following `<MsDeploySkipRules>` element markup exludes all files from the *wwwroot/content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="4204e-255">`<MsDeploySkipRules>`не удалять *пропустить* целевых объектов с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="4204e-255">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="4204e-256">`<Content>`целевые файлы и папки будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="4204e-256">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="4204e-257">Например предположим, что развернутого веб-приложения имели следующие файлы:</span><span class="sxs-lookup"><span data-stu-id="4204e-257">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="4204e-258">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4204e-258">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="4204e-259">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4204e-259">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="4204e-260">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="4204e-260">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="4204e-261">Если следующие `<MsDeploySkipRules>` добавляется разметки, эти файлы не удалены на сайте развертывания.</span><span class="sxs-lookup"><span data-stu-id="4204e-261">If the following `<MsDeploySkipRules>` markup is added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="4204e-262">`<MsDeploySkipRules>` Предотвращает разметки, показанный выше *пропущен* файлы из процесса depoyed, но не удаляет эти файлы, как только они развернуты.</span><span class="sxs-lookup"><span data-stu-id="4204e-262">The `<MsDeploySkipRules>` markup shown above prevents the *skipped* files from being depoyed but won't delete those files once they're deployed.</span></span>

<span data-ttu-id="4204e-263">Следующие `<Content>` разметки удаляет целевых файлов на сайте развертывания:</span><span class="sxs-lookup"><span data-stu-id="4204e-263">The following `<Content>` markup deletes the targeted files at the deployment site:</span></span>

``` xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="4204e-264">С помощью развертывания из командной строки с `<Content>` разметки выше результатов в выходные данные, аналогичные следующим:</span><span class="sxs-lookup"><span data-stu-id="4204e-264">Using command-line deployment with the `<Content>` markup above results in output similar to the following:</span></span>

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

## <a name="including-files"></a><span data-ttu-id="4204e-265">Включение файлов</span><span class="sxs-lookup"><span data-stu-id="4204e-265">Including files</span></span>

<span data-ttu-id="4204e-266">Включает следующую разметку *изображения* папку вне каталога проекта к *wwwroot/images* папку сайта публикации:</span><span class="sxs-lookup"><span data-stu-id="4204e-266">The following markup includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site:</span></span>

``` xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="4204e-267">Показанная выше разметка может быть добавлена в файл *.csproj* или в профиль публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-267">The markup can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="4204e-268">Если добавляется в *.csproj* файл, он включен в каждый профиль публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="4204e-268">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

<span data-ttu-id="4204e-269">Разметка, выделенная в приведенном ниже примере, показывает, как.</span><span class="sxs-lookup"><span data-stu-id="4204e-269">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="4204e-270">Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="4204e-270">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="4204e-271">Исключить папку *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="4204e-271">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="4204e-272">Исключить файл *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="4204e-272">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="4204e-273">Дополнительные примеры развертывания см. в файле [WebSDK Readme](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="4204e-273">See the [WebSDK Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

### <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="4204e-274">Выполнение целевого объекта до или после публикации</span><span class="sxs-lookup"><span data-stu-id="4204e-274">Run a target before or after publishing</span></span>

<span data-ttu-id="4204e-275">Встроенная `BeforePublish` и `AfterPublish` целевых объектов можно использовать для выполнения целевого объекта до или после назначения публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-275">The built-in `BeforePublish` and `AfterPublish` targets can be used to execute a target before or after the publish target.</span></span> <span data-ttu-id="4204e-276">В профиль публикации можно добавить показанную ниже разметку — в этом случае сообщения будут добавлены в вывод консоли до или после публикации.</span><span class="sxs-lookup"><span data-stu-id="4204e-276">The following markup can be added to the publish profile to log messages to the console output before and after publishing:</span></span>

``` xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="the-kudu-service"></a><span data-ttu-id="4204e-277">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="4204e-277">The Kudu service</span></span>

<span data-ttu-id="4204e-278">Чтобы просмотреть файлы в службе приложений Azure развертывание веб-приложения, используйте [Kudu службы](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="4204e-278">To view the files in the an Azure Apps Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="4204e-279">Добавление `scm` маркера к имени веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="4204e-279">Append the `scm` token to the name of the web app.</span></span> <span data-ttu-id="4204e-280">Пример:</span><span class="sxs-lookup"><span data-stu-id="4204e-280">For example:</span></span>

| <span data-ttu-id="4204e-281">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="4204e-281">URL</span></span>                                    | <span data-ttu-id="4204e-282">Результат</span><span class="sxs-lookup"><span data-stu-id="4204e-282">Result</span></span>      |
| -------------------------------------- | ----------- |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="4204e-283">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="4204e-283">Web App</span></span>     |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="4204e-284">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="4204e-284">Kudu sevice</span></span> |

<span data-ttu-id="4204e-285">Для просмотра, редактирования, удаления и добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="4204e-285">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view/edit/delete/add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4204e-286">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4204e-286">Additional resources</span></span>

* <span data-ttu-id="4204e-287">[Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.</span><span class="sxs-lookup"><span data-stu-id="4204e-287">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="4204e-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): проблемы с файлами и особенности запросов для развертывания.</span><span class="sxs-lookup"><span data-stu-id="4204e-288">[https://github.com/aspnet/websdk](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
