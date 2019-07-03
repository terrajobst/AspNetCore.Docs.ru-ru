---
title: Профили публикации Visual Studio для развертывания приложений ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать профили публикации в Visual Studio и применять их для управления развертыванием приложений ASP.NET Core в разных целевых объектах.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 06/18/2019
uid: host-and-deploy/visual-studio-publish-profiles
ms.openlocfilehash: ac243a3898553b2e14a6c15d311afaf62f112a24
ms.sourcegitcommit: a1283d486ac1dcedfc7ea302e1cc882833e2c515
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67207815"
---
# <a name="visual-studio-publish-profiles-for-aspnet-core-app-deployment"></a><span data-ttu-id="ab165-103">Профили публикации Visual Studio для развертывания приложений ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ab165-103">Visual Studio publish profiles for ASP.NET Core app deployment</span></span>

<span data-ttu-id="ab165-104">Авторы: [Саид Ибрагим Хашими](https://github.com/sayedihashimi) (Sayed Ibrahim Hashimi) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ab165-104">By [Sayed Ibrahim Hashimi](https://github.com/sayedihashimi) and [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ab165-105">Этот документ посвящен использованию Visual Studio 2017 или более поздней версии для создания и применения профилей публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-105">This document focuses on using Visual Studio 2017 or later to create and use publish profiles.</span></span> <span data-ttu-id="ab165-106">Профили публикации, созданные с помощью Visual Studio, можно применять в MSBuild и Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab165-106">The publish profiles created with Visual Studio can be run from MSBuild and Visual Studio.</span></span> <span data-ttu-id="ab165-107">Инструкции по публикации в Azure см. в статье [Публикация веб-приложения ASP.NET Core в службе приложений Azure с помощью Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs).</span><span class="sxs-lookup"><span data-stu-id="ab165-107">See [Publish an ASP.NET Core web app to Azure App Service using Visual Studio](xref:tutorials/publish-to-azure-webapp-using-vs) for instructions on publishing to Azure.</span></span>

<span data-ttu-id="ab165-108">Команда `dotnet new mvc` создает файл проекта со следующим элементом `<Project>` на верхнем уровне:</span><span class="sxs-lookup"><span data-stu-id="ab165-108">The `dotnet new mvc` command produces a project file containing the following top-level `<Project>` element:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
    <!-- omitted for brevity -->
</Project>
```

<span data-ttu-id="ab165-109">В описанном выше элементе `<Project>` атрибут `Sdk` импортирует [свойства](/visualstudio/msbuild/msbuild-properties) и [целевые объекты](/visualstudio/msbuild/msbuild-targets) для MSBuild из файлов *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\ SDK.props* и *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, соответственно.</span><span class="sxs-lookup"><span data-stu-id="ab165-109">The preceding `<Project>` element's `Sdk` attribute imports the MSBuild [properties](/visualstudio/msbuild/msbuild-properties) and [targets](/visualstudio/msbuild/msbuild-targets) from *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.props* and *$(MSBuildSDKsPath)\Microsoft.NET.Sdk.Web\Sdk\Sdk.targets*, respectively.</span></span> <span data-ttu-id="ab165-110">По умолчанию `$(MSBuildSDKsPath)` (в Visual Studio 2019 Enterprise) находится в папке *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks*.</span><span class="sxs-lookup"><span data-stu-id="ab165-110">The default location for `$(MSBuildSDKsPath)` (with Visual Studio 2019 Enterprise) is the *%programfiles(x86)%\Microsoft Visual Studio\2019\Enterprise\MSBuild\Sdks* folder.</span></span>

<span data-ttu-id="ab165-111">`Microsoft.NET.Sdk.Web` (Веб-пакет SDK) зависит от других пакетов SDK, в том числе `Microsoft.NET.Sdk` (пакет SDK для .NET Core) и `Microsoft.NET.Sdk.Razor` ([пакет SDK для Razor](xref:razor-pages/sdk)).</span><span class="sxs-lookup"><span data-stu-id="ab165-111">`Microsoft.NET.Sdk.Web` (Web SDK) depends on other SDKs, including `Microsoft.NET.Sdk` (.NET Core SDK) and `Microsoft.NET.Sdk.Razor` ([Razor SDK](xref:razor-pages/sdk)).</span></span> <span data-ttu-id="ab165-112">Импортируются свойства и целевые объекты MSBuild, связанные с каждым из зависимых пакетов SDK.</span><span class="sxs-lookup"><span data-stu-id="ab165-112">The MSBuild properties and targets associated with each dependent SDK are imported.</span></span> <span data-ttu-id="ab165-113">Целевые объекты публикации импортируют подходящий набор целевых объектов в зависимости от используемых методов публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-113">Publish targets import the appropriate set of targets based on the publish method used.</span></span>

<span data-ttu-id="ab165-114">Когда MSBuild или Visual Studio загружает проект, выполняются следующие обобщенные действия:</span><span class="sxs-lookup"><span data-stu-id="ab165-114">When MSBuild or Visual Studio loads a project, the following high-level actions occur:</span></span>

* <span data-ttu-id="ab165-115">сборка проекта;</span><span class="sxs-lookup"><span data-stu-id="ab165-115">Build project</span></span>
* <span data-ttu-id="ab165-116">вычисление файлов для публикации;</span><span class="sxs-lookup"><span data-stu-id="ab165-116">Compute files to publish</span></span>
* <span data-ttu-id="ab165-117">публикация файлов в месте назначения;</span><span class="sxs-lookup"><span data-stu-id="ab165-117">Publish files to destination</span></span>

## <a name="compute-project-items"></a><span data-ttu-id="ab165-118">вычисление элементов проекта.</span><span class="sxs-lookup"><span data-stu-id="ab165-118">Compute project items</span></span>

<span data-ttu-id="ab165-119">После загрузки проекта вычисляются [элементы (файлы) проекта MSBuild](/visualstudio/msbuild/common-msbuild-project-items).</span><span class="sxs-lookup"><span data-stu-id="ab165-119">When the project is loaded, the [MSBuild project items](/visualstudio/msbuild/common-msbuild-project-items) (files) are computed.</span></span> <span data-ttu-id="ab165-120">Порядок обработки файла определяется типом элемента.</span><span class="sxs-lookup"><span data-stu-id="ab165-120">The item type determines how the file is processed.</span></span> <span data-ttu-id="ab165-121">По умолчанию файлы с расширением *.cs* включаются в список элементов `Compile`.</span><span class="sxs-lookup"><span data-stu-id="ab165-121">By default, *.cs* files are included in the `Compile` item list.</span></span> <span data-ttu-id="ab165-122">Файлы в списке элементов `Compile` компилируются.</span><span class="sxs-lookup"><span data-stu-id="ab165-122">Files in the `Compile` item list are compiled.</span></span>

<span data-ttu-id="ab165-123">Список элементов `Content` содержит файлы, предназначенные для публикации, а также результаты сборки.</span><span class="sxs-lookup"><span data-stu-id="ab165-123">The `Content` item list contains files that are published in addition to the build outputs.</span></span> <span data-ttu-id="ab165-124">По умолчанию в список элементов `Content` включаются файлы, соответствующие шаблонам `wwwroot\**`, `**\*.config` и `**\*.json`.</span><span class="sxs-lookup"><span data-stu-id="ab165-124">By default, files matching the patterns `wwwroot\**`, `**\*.config`, and `**\*.json` are included in the `Content` item list.</span></span> <span data-ttu-id="ab165-125">Например, [Стандартная маска](https://gruntjs.com/configuring-tasks#globbing-patterns) `wwwroot\**` соответствует всем файлам в папке *wwwroot* **и** всех ее подпапках.</span><span class="sxs-lookup"><span data-stu-id="ab165-125">For example, the `wwwroot\**` [globbing pattern](https://gruntjs.com/configuring-tasks#globbing-patterns) matches all files in the *wwwroot* folder **and** its subfolders.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="ab165-126">Веб-пакет SDK импортирует [пакет SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="ab165-126">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="ab165-127">Это означает, что в список элементов `Content` дополнительно включаются файлы, соответствующие шаблонам `**\*.cshtml` и `**\*.razor`.</span><span class="sxs-lookup"><span data-stu-id="ab165-127">As a result, files matching the patterns `**\*.cshtml` and `**\*.razor` are also included in the `Content` item list.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="ab165-128">Веб-пакет SDK импортирует [пакет SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="ab165-128">The Web SDK imports the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="ab165-129">Это означает, что в список элементов `Content` дополнительно включаются файлы, соответствующие шаблону `**\*.cshtml`.</span><span class="sxs-lookup"><span data-stu-id="ab165-129">As a result, files matching the `**\*.cshtml` pattern are also included in the `Content` item list.</span></span>

::: moniker-end

<span data-ttu-id="ab165-130">Чтобы явным образом добавить файл в список публикации, поместите его в *CSPROJ*-файл, как показано в разделе [включения файлов](#include-files).</span><span class="sxs-lookup"><span data-stu-id="ab165-130">To explicitly add a file to the publish list, add the file directly in the *.csproj* file as shown in the [Include Files](#include-files) section.</span></span>

<span data-ttu-id="ab165-131">При нажатии кнопки **Публикация** в Visual Studio или при публикации из командной строки происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="ab165-131">When selecting the **Publish** button in Visual Studio or when publishing from the command line:</span></span>

* <span data-ttu-id="ab165-132">Вычисляются свойства и (или) элементы (файлы, требующие сборки).</span><span class="sxs-lookup"><span data-stu-id="ab165-132">The properties/items are computed (the files that are needed to build).</span></span>
* <span data-ttu-id="ab165-133">**Только Visual Studio**: пакеты NuGet восстанавливаются.</span><span class="sxs-lookup"><span data-stu-id="ab165-133">**Visual Studio only**: NuGet packages are restored.</span></span> <span data-ttu-id="ab165-134">(Восстановление выполняется пользователем в интерфейсе командной строки.)</span><span class="sxs-lookup"><span data-stu-id="ab165-134">(Restore needs to be explicit by the user on the CLI.)</span></span>
* <span data-ttu-id="ab165-135">Выполняется сборка проекта.</span><span class="sxs-lookup"><span data-stu-id="ab165-135">The project builds.</span></span>
* <span data-ttu-id="ab165-136">Вычисляются публикуемые элементы (файлы, требующие публикации).</span><span class="sxs-lookup"><span data-stu-id="ab165-136">The publish items are computed (the files that are needed to publish).</span></span>
* <span data-ttu-id="ab165-137">Публикуется проект (вычисляемые файлы копируются в место назначения публикации.)</span><span class="sxs-lookup"><span data-stu-id="ab165-137">The project is published (the computed files are copied to the publish destination).</span></span>

<span data-ttu-id="ab165-138">Когда проект ASP.NET Core ссылается на `Microsoft.NET.Sdk.Web` в файле проекта, файл *app_offline.htm* помещается в корневой каталог веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="ab165-138">When an ASP.NET Core project references `Microsoft.NET.Sdk.Web` in the project file, an *app_offline.htm* file is placed at the root of the web app directory.</span></span> <span data-ttu-id="ab165-139">Если файл присутствует, модуль ASP.NET Core корректно завершает работу приложения и обслуживает файл *app_offline.htm* во время развертывания.</span><span class="sxs-lookup"><span data-stu-id="ab165-139">When the file is present, the ASP.NET Core Module gracefully shuts down the app and serves the *app_offline.htm* file during the deployment.</span></span> <span data-ttu-id="ab165-140">Дополнительные сведения см. в разделе [Справочник по конфигурации модуля ASP.NET Core](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span><span class="sxs-lookup"><span data-stu-id="ab165-140">For more information, see the [ASP.NET Core Module configuration reference](xref:host-and-deploy/aspnet-core-module#app_offlinehtm).</span></span>

## <a name="basic-command-line-publishing"></a><span data-ttu-id="ab165-141">Простая публикация из командной строки</span><span class="sxs-lookup"><span data-stu-id="ab165-141">Basic command-line publishing</span></span>

<span data-ttu-id="ab165-142">Публикация из командной строки работает на всех платформах, поддерживаемых .NET Core, и не требует наличия Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab165-142">Command-line publishing works on all .NET Core-supported platforms and doesn't require Visual Studio.</span></span> <span data-ttu-id="ab165-143">В приведенных ниже примерах команда [dotnet publish](/dotnet/core/tools/dotnet-publish) выполняется из папки проекта (где хранится *CSPROJ*-файл).</span><span class="sxs-lookup"><span data-stu-id="ab165-143">In the following samples, the [dotnet publish](/dotnet/core/tools/dotnet-publish) command is run from the project directory (which contains the *.csproj* file).</span></span> <span data-ttu-id="ab165-144">Если вы не находитесь в папке проекта, укажите путь к файлу проекта явным образом.</span><span class="sxs-lookup"><span data-stu-id="ab165-144">If not in the project folder, explicitly pass in the project file path.</span></span> <span data-ttu-id="ab165-145">Например:</span><span class="sxs-lookup"><span data-stu-id="ab165-145">For example:</span></span>

```console
dotnet publish C:\Webs\Web1
```

<span data-ttu-id="ab165-146">Для создания и публикации веб-приложения выполните следующие команды.</span><span class="sxs-lookup"><span data-stu-id="ab165-146">Run the following commands to create and publish a web app:</span></span>

```console
dotnet new mvc
dotnet publish
```

<span data-ttu-id="ab165-147">Выходные данные команды [dotnet publish](/dotnet/core/tools/dotnet-publish) выглядят примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="ab165-147">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command produces output similar to the following:</span></span>

```console
C:\Webs\Web1>dotnet publish
Microsoft (R) Build Engine version {version} for .NET Core
Copyright (C) Microsoft Corporation. All rights reserved.

  Restore completed in 36.81 ms for C:\Webs\Web1\Web1.csproj.
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\Web1.Views.dll
  Web1 -> C:\Webs\Web1\bin\Debug\netcoreapp{X.Y}\publish\
```

<span data-ttu-id="ab165-148">Папка для публикации по умолчанию — `bin\$(Configuration)\netcoreapp<version>\publish`.</span><span class="sxs-lookup"><span data-stu-id="ab165-148">The default publish folder is `bin\$(Configuration)\netcoreapp<version>\publish`.</span></span> <span data-ttu-id="ab165-149">Для параметра `$(Configuration)` по умолчанию подставляется значение *Debug*.</span><span class="sxs-lookup"><span data-stu-id="ab165-149">The default for `$(Configuration)` is *Debug*.</span></span> <span data-ttu-id="ab165-150">В предыдущем примере `<TargetFramework>` имеет значение `netcoreapp{X.Y}`.</span><span class="sxs-lookup"><span data-stu-id="ab165-150">In the preceding sample, the `<TargetFramework>` is `netcoreapp{X.Y}`.</span></span>

<span data-ttu-id="ab165-151">`dotnet publish -h` отображает справку по публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-151">`dotnet publish -h` displays help information for publish.</span></span>

<span data-ttu-id="ab165-152">Следующая команда определяет сборку `Release` и папку для публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-152">The following command specifies a `Release` build and the publishing directory:</span></span>

```console
dotnet publish -c Release -o C:\MyWebs\test
```

<span data-ttu-id="ab165-153">Команда [dotnet publish](/dotnet/core/tools/dotnet-publish) вызывает MSBuild, что в свою очередь вызывает целевой объект `Publish`.</span><span class="sxs-lookup"><span data-stu-id="ab165-153">The [dotnet publish](/dotnet/core/tools/dotnet-publish) command calls MSBuild, which invokes the `Publish` target.</span></span> <span data-ttu-id="ab165-154">Все параметры, передаваемые в `dotnet publish`, передаются далее в MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ab165-154">Any parameters passed to `dotnet publish` are passed to MSBuild.</span></span> <span data-ttu-id="ab165-155">Параметр `-c` сопоставляется со свойством `Configuration` MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ab165-155">The `-c` parameter maps to the `Configuration` MSBuild property.</span></span> <span data-ttu-id="ab165-156">Параметр `-o` сопоставляется со свойством `OutputPath`.</span><span class="sxs-lookup"><span data-stu-id="ab165-156">The `-o` parameter maps to `OutputPath`.</span></span>

<span data-ttu-id="ab165-157">Свойства MSBuild можно передавать в любом из следующих форматов.</span><span class="sxs-lookup"><span data-stu-id="ab165-157">MSBuild properties can be passed using either of the following formats:</span></span>

* `p:<NAME>=<VALUE>`
* `/p:<NAME>=<VALUE>`

<span data-ttu-id="ab165-158">Следующая команда публикует сборку `Release` в общую сетевую папку.</span><span class="sxs-lookup"><span data-stu-id="ab165-158">The following command publishes a `Release` build to a network share:</span></span>

`dotnet publish -c Release /p:PublishDir=//r8/release/AdminWeb`

<span data-ttu-id="ab165-159">Общая сетевая папка указывается с помощью символов косой черты ( *//r8/* ) и работает на всех поддерживаемых платформах .NET Core.</span><span class="sxs-lookup"><span data-stu-id="ab165-159">The network share is specified with forward slashes (*//r8/*) and works on all .NET Core supported platforms.</span></span>

<span data-ttu-id="ab165-160">Убедитесь, что приложение, публикуемое для развертывания, не запущено.</span><span class="sxs-lookup"><span data-stu-id="ab165-160">Confirm that the published app for deployment isn't running.</span></span> <span data-ttu-id="ab165-161">Во время выполнения приложения файлы в папке *publish* блокируются.</span><span class="sxs-lookup"><span data-stu-id="ab165-161">Files in the *publish* folder are locked when the app is running.</span></span> <span data-ttu-id="ab165-162">Заблокированные файлы скопировать нельзя, так что развертывание в этом случае не произойдет.</span><span class="sxs-lookup"><span data-stu-id="ab165-162">Deployment can't occur because locked files can't be copied.</span></span>

## <a name="publish-profiles"></a><span data-ttu-id="ab165-163">Профили публикации</span><span class="sxs-lookup"><span data-stu-id="ab165-163">Publish profiles</span></span>

<span data-ttu-id="ab165-164">В этом разделе для создания профиля публикации используется Visual Studio 2017 или более поздней версии.</span><span class="sxs-lookup"><span data-stu-id="ab165-164">This section uses Visual Studio 2017 or later to create a publishing profile.</span></span> <span data-ttu-id="ab165-165">После создания профиля публикацию можно выполнять из Visual Studio или из командной строки.</span><span class="sxs-lookup"><span data-stu-id="ab165-165">Once the profile is created, publishing from Visual Studio or the command line is available.</span></span>

<span data-ttu-id="ab165-166">Профили публикации упрощают процесс публикации. Вы можете создать любое количество профилей.</span><span class="sxs-lookup"><span data-stu-id="ab165-166">Publish profiles can simplify the publishing process, and any number of profiles can exist.</span></span> <span data-ttu-id="ab165-167">Создайте профиль публикации в Visual Studio, выбрав один из следующих методов:</span><span class="sxs-lookup"><span data-stu-id="ab165-167">Create a publish profile in Visual Studio by choosing one of the following paths:</span></span>

* <span data-ttu-id="ab165-168">Щелкните правой кнопкой мыши проект в **обозревателе решений** и выберите **Опубликовать**.</span><span class="sxs-lookup"><span data-stu-id="ab165-168">Right-click the project in **Solution Explorer** and select **Publish**.</span></span>
* <span data-ttu-id="ab165-169">Выберите команду **Опубликовать {имя проекта}** в меню **Сборка**.</span><span class="sxs-lookup"><span data-stu-id="ab165-169">Select **Publish {PROJECT NAME}** from the **Build** menu.</span></span>

<span data-ttu-id="ab165-170">Откроется вкладка **Публикация** на странице свойств приложения.</span><span class="sxs-lookup"><span data-stu-id="ab165-170">The **Publish** tab of the app capacities page is displayed.</span></span> <span data-ttu-id="ab165-171">Если в проекте нет профиля публикации, откроется следующая страница:</span><span class="sxs-lookup"><span data-stu-id="ab165-171">If the project lacks a publish profile, the following page is displayed:</span></span>

![Вкладка "Публикация" на странице свойств приложения](visual-studio-publish-profiles/_static/az.png)

<span data-ttu-id="ab165-173">Выберите **Папка** и укажите путь к папке, в которой будут храниться опубликованные ресурсы.</span><span class="sxs-lookup"><span data-stu-id="ab165-173">When **Folder** is selected, specify a folder path to store the published assets.</span></span> <span data-ttu-id="ab165-174">По умолчанию используется папка *bin\Release\PublishOutput*.</span><span class="sxs-lookup"><span data-stu-id="ab165-174">The default folder is *bin\Release\PublishOutput*.</span></span> <span data-ttu-id="ab165-175">Нажмите кнопку **Создать профиль**, чтобы завершить процесс.</span><span class="sxs-lookup"><span data-stu-id="ab165-175">Click the **Create Profile** button to finish.</span></span>

<span data-ttu-id="ab165-176">Когда профиль публикации будет создан, изменения отразятся на вкладке **Публикация**.</span><span class="sxs-lookup"><span data-stu-id="ab165-176">Once a publish profile is created, the **Publish** tab changes.</span></span> <span data-ttu-id="ab165-177">Новый профиль появится в раскрывающемся списке.</span><span class="sxs-lookup"><span data-stu-id="ab165-177">The newly created profile appears in a drop-down list.</span></span> <span data-ttu-id="ab165-178">Щелкните **Создать новый профиль**, чтобы создать еще один профиль.</span><span class="sxs-lookup"><span data-stu-id="ab165-178">Click **Create new profile** to create another new profile.</span></span>

![Вкладка "Публикация" на странице свойств приложения с профилем FolderProfile](visual-studio-publish-profiles/_static/create_new.png)

<span data-ttu-id="ab165-180">Мастер публикации поддерживает следующие целевые объекты публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-180">The Publish wizard supports the following publish targets:</span></span>

* <span data-ttu-id="ab165-181">Служба приложений Azure</span><span class="sxs-lookup"><span data-stu-id="ab165-181">Azure App Service</span></span>
* <span data-ttu-id="ab165-182">Виртуальные машины Azure</span><span class="sxs-lookup"><span data-stu-id="ab165-182">Azure Virtual Machines</span></span>
* <span data-ttu-id="ab165-183">IIS, FTP и т. д. (для любого веб-сервера)</span><span class="sxs-lookup"><span data-stu-id="ab165-183">IIS, FTP, etc. (for any web server)</span></span>
* <span data-ttu-id="ab165-184">Папка</span><span class="sxs-lookup"><span data-stu-id="ab165-184">Folder</span></span>
* <span data-ttu-id="ab165-185">Профиль импорта</span><span class="sxs-lookup"><span data-stu-id="ab165-185">Import Profile</span></span>

<span data-ttu-id="ab165-186">Дополнительные сведения см. в статье [Выбор подходящих вариантов публикации](/visualstudio/ide/not-in-toc/web-publish-options).</span><span class="sxs-lookup"><span data-stu-id="ab165-186">For more information, see [What publishing options are right for me](/visualstudio/ide/not-in-toc/web-publish-options).</span></span>

<span data-ttu-id="ab165-187">При создании профиля публикации с помощью Visual Studio создается файл MSBuild *Properties/PublishProfiles/{имя_профиля}.pubxml*.</span><span class="sxs-lookup"><span data-stu-id="ab165-187">When creating a publish profile with Visual Studio, a *Properties/PublishProfiles/{PROFILE NAME}.pubxml* MSBuild file is created.</span></span> <span data-ttu-id="ab165-188">Этот файл MSBuild с расширением *.pubxml* содержит параметры конфигурации публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-188">The *.pubxml* file is a MSBuild file and contains publish configuration settings.</span></span> <span data-ttu-id="ab165-189">Вы можете изменить этот файл, чтобы настроить процесс сборки и публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-189">This file can be changed to customize the build and publish process.</span></span> <span data-ttu-id="ab165-190">Этот файл считывается процессом публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-190">This file is read by the publishing process.</span></span> <span data-ttu-id="ab165-191">Особое свойство `<LastUsedBuildConfiguration>` является глобальным и не должно присутствовать ни в одном файле, импортируемом в сборку.</span><span class="sxs-lookup"><span data-stu-id="ab165-191">`<LastUsedBuildConfiguration>` is special because it's a global property and shouldn't be in any file that's imported in the build.</span></span> <span data-ttu-id="ab165-192">Дополнительные сведения см. в статье [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) (MSBuild: настройка свойства конфигурации).</span><span class="sxs-lookup"><span data-stu-id="ab165-192">See [MSBuild: how to set the configuration property](http://sedodream.com/2012/10/27/MSBuildHowToSetTheConfigurationProperty.aspx) for more information.</span></span>

<span data-ttu-id="ab165-193">Если публикация выполняется в целевой объект Azure, файл с расширением *.pubxml* содержит идентификатор подписки Azure.</span><span class="sxs-lookup"><span data-stu-id="ab165-193">When publishing to an Azure target, the *.pubxml* file contains your Azure subscription identifier.</span></span> <span data-ttu-id="ab165-194">При таком типе целевого объекта мы не рекомендуем добавлять этот файл в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="ab165-194">With that target type, adding this file to source control is discouraged.</span></span> <span data-ttu-id="ab165-195">Если публикация выполняется в целевой объект, не относящийся к Azure, файл с расширением *.pubxml* можно безопасно вернуть в систему.</span><span class="sxs-lookup"><span data-stu-id="ab165-195">When publishing to a non-Azure target, it's safe to check in the *.pubxml* file.</span></span>

<span data-ttu-id="ab165-196">Конфиденциальные данные (например, пароль публикации) шифруются на уровне пользователя или компьютера.</span><span class="sxs-lookup"><span data-stu-id="ab165-196">Sensitive information (like the publish password) is encrypted on a per user/machine level.</span></span> <span data-ttu-id="ab165-197">Они сохраняются в файле *Properties/PublishProfiles/{имя_профиля}.pubxml.user*.</span><span class="sxs-lookup"><span data-stu-id="ab165-197">It's stored in the *Properties/PublishProfiles/{PROFILE NAME}.pubxml.user* file.</span></span> <span data-ttu-id="ab165-198">Так как этот файл может содержать конфиденциальные данные, его не следует возвращать в систему управления версиями.</span><span class="sxs-lookup"><span data-stu-id="ab165-198">Because this file can store sensitive information, it shouldn't be checked into source control.</span></span>

<span data-ttu-id="ab165-199">Общие сведения о публикации веб-приложений в ASP.NET Core см. в статье [Размещение и развертывание ASP.NET Core](xref:host-and-deploy/index).</span><span class="sxs-lookup"><span data-stu-id="ab165-199">For an overview of how to publish a web app on ASP.NET Core, see [Host and deploy](xref:host-and-deploy/index).</span></span> <span data-ttu-id="ab165-200">Открытый исходный код задач MSBuild и целевых объектов, необходимых для публикации приложения ASP.NET Core, размещен в [репозитории aspnet/websdk](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="ab165-200">The MSBuild tasks and targets necessary to publish an ASP.NET Core app are open-source at the [aspnet/websdk repository](https://github.com/aspnet/websdk).</span></span>

<span data-ttu-id="ab165-201">`dotnet publish` может использовать профили публикации папки, MSDeploy и [Kudu](https://github.com/projectkudu/kudu/wiki).</span><span class="sxs-lookup"><span data-stu-id="ab165-201">`dotnet publish` can use folder, MSDeploy, and [Kudu](https://github.com/projectkudu/kudu/wiki) publish profiles:</span></span>

<span data-ttu-id="ab165-202">Папка (работает на всех платформах):</span><span class="sxs-lookup"><span data-stu-id="ab165-202">Folder (works cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<FolderProfileName>
```

<span data-ttu-id="ab165-203">MSDeploy (сейчас работает только в Windows, так как MSDeploy не является кроссплатформенным):</span><span class="sxs-lookup"><span data-stu-id="ab165-203">MSDeploy (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployProfileName> /p:Password=<DeploymentPassword>
```

<span data-ttu-id="ab165-204">Пакет MSDeploy (сейчас работает только в Windows, так как MSDeploy не является кроссплатформенным):</span><span class="sxs-lookup"><span data-stu-id="ab165-204">MSDeploy package (currently this only works in Windows since MSDeploy isn't cross-platform):</span></span>

```console
dotnet publish WebApplication.csproj /p:PublishProfile=<MsDeployPackageProfileName>
```

<span data-ttu-id="ab165-205">В примерах выше не передавайте `deployonbuild` в `dotnet publish`.</span><span class="sxs-lookup"><span data-stu-id="ab165-205">In the preceding examples, don't pass `deployonbuild` to `dotnet publish`.</span></span>

<span data-ttu-id="ab165-206">Дополнительные сведения см. в [документации по Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span><span class="sxs-lookup"><span data-stu-id="ab165-206">For more information, see [Microsoft.NET.Sdk.Publish](https://github.com/aspnet/websdk#microsoftnetsdkpublish).</span></span>

<span data-ttu-id="ab165-207">`dotnet publish` поддерживает API-интерфейсы Kudu для публикации в Azure с любой платформы.</span><span class="sxs-lookup"><span data-stu-id="ab165-207">`dotnet publish` supports Kudu APIs to publish to Azure from any platform.</span></span> <span data-ttu-id="ab165-208">Публикация с помощью Visual Studio поддерживает API Kudu, но для кроссплатформенной публикации в Azure их поддерживает WebSDK.</span><span class="sxs-lookup"><span data-stu-id="ab165-208">Visual Studio publish supports the Kudu APIs, but it's supported by WebSDK for cross-platform publish to Azure.</span></span>

<span data-ttu-id="ab165-209">Добавьте в папку *Properties/PublishProfiles* профиль публикации со следующим содержимым:</span><span class="sxs-lookup"><span data-stu-id="ab165-209">Add a publish profile to the *Properties/PublishProfiles* folder with the following content:</span></span>

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

<span data-ttu-id="ab165-210">Выполните следующую команду, чтобы архивировать содержимое публикации и опубликовать его в Azure с помощью API-интерфейсов Kudu.</span><span class="sxs-lookup"><span data-stu-id="ab165-210">Run the following command to zip up the publish contents and publish it to Azure using the Kudu APIs:</span></span>

```console
dotnet publish /p:PublishProfile=Azure /p:Configuration=Release
```

<span data-ttu-id="ab165-211">Для работы с профилем публикации задайте следующие свойства MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ab165-211">Set the following MSBuild properties when using a publish profile:</span></span>

* `DeployOnBuild=true`
* `PublishProfile={PUBLISH PROFILE}`

<span data-ttu-id="ab165-212">Для публикации с использованием профиля *FolderProfile* вы можете выполнить одну из указанных ниже команд.</span><span class="sxs-lookup"><span data-stu-id="ab165-212">When publishing with a profile named *FolderProfile*, either of the commands below can be executed:</span></span>

* `dotnet build /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`
* `msbuild      /p:DeployOnBuild=true /p:PublishProfile=FolderProfile`

<span data-ttu-id="ab165-213">Команда [dotnet build](/dotnet/core/tools/dotnet-build) вызывает `msbuild` для сборки и публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-213">When invoking [dotnet build](/dotnet/core/tools/dotnet-build), it calls `msbuild` to run the build and publish process.</span></span> <span data-ttu-id="ab165-214">Вызовы `dotnet build` и `msbuild` эквивалентны, если они передаются в профиле папки.</span><span class="sxs-lookup"><span data-stu-id="ab165-214">Calling either `dotnet build` or `msbuild` is equivalent when passing in a folder profile.</span></span> <span data-ttu-id="ab165-215">При прямом вызове MSBuild на платформе Windows используется версия MSBuild для .NET Framework.</span><span class="sxs-lookup"><span data-stu-id="ab165-215">When calling MSBuild directly on Windows, the .NET Framework version of MSBuild is used.</span></span> <span data-ttu-id="ab165-216">В настоящее время MSDeploy поддерживает публикацию только на компьютерах Windows.</span><span class="sxs-lookup"><span data-stu-id="ab165-216">MSDeploy is currently limited to Windows machines for publishing.</span></span> <span data-ttu-id="ab165-217">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild, который использует в таких профилях MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="ab165-217">Calling `dotnet build` on a non-folder profile invokes MSBuild, and MSBuild uses MSDeploy on non-folder profiles.</span></span> <span data-ttu-id="ab165-218">Вызов `dotnet build` в других профилях, кроме профиля папки, вызывает MSBuild (с помощью MSDeploy) и завершается сбоем (даже при работе на платформе Windows).</span><span class="sxs-lookup"><span data-stu-id="ab165-218">Calling `dotnet build` on a non-folder profile invokes MSBuild (using MSDeploy) and results in a failure (even when running on a Windows platform).</span></span> <span data-ttu-id="ab165-219">Для публикации профилей, отличных от профиля папки, вызывайте MSBuild напрямую.</span><span class="sxs-lookup"><span data-stu-id="ab165-219">To publish with a non-folder profile, call MSBuild directly.</span></span>

<span data-ttu-id="ab165-220">Следующий профиль публикации папки был создан в Visual Studio и публикуется в общей сетевой папке:</span><span class="sxs-lookup"><span data-stu-id="ab165-220">The following folder publish profile was created with Visual Studio and publishes to a network share:</span></span>

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

<span data-ttu-id="ab165-221">В примерах выше `<LastUsedBuildConfiguration>` получает значение `Release`.</span><span class="sxs-lookup"><span data-stu-id="ab165-221">In the preceding example, `<LastUsedBuildConfiguration>` is set to `Release`.</span></span> <span data-ttu-id="ab165-222">При публикации из Visual Studio свойство конфигурации `<LastUsedBuildConfiguration>` получает значение, которые использовалось при запуске процесса публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-222">When publishing from Visual Studio, the `<LastUsedBuildConfiguration>` configuration property value is set using the value when the publish process is started.</span></span> <span data-ttu-id="ab165-223">Свойство конфигурации `<LastUsedBuildConfiguration>` имеет особое значение и его не следует переопределять в импортируемом файле MSBuild.</span><span class="sxs-lookup"><span data-stu-id="ab165-223">The `<LastUsedBuildConfiguration>` configuration property is special and shouldn't be overridden in an imported MSBuild file.</span></span> <span data-ttu-id="ab165-224">Это свойство можно переопределить из командной строки.</span><span class="sxs-lookup"><span data-stu-id="ab165-224">This property can be overridden from the command line.</span></span>

<span data-ttu-id="ab165-225">С использованием .NET Core CLI:</span><span class="sxs-lookup"><span data-stu-id="ab165-225">Using the .NET Core CLI:</span></span>

```console
dotnet build -c Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

<span data-ttu-id="ab165-226">С использованием MSBuild</span><span class="sxs-lookup"><span data-stu-id="ab165-226">Using MSBuild:</span></span>

```console
msbuild /p:Configuration=Release /p:DeployOnBuild=true /p:PublishProfile=FolderProfile
```

## <a name="publish-to-an-msdeploy-endpoint-from-the-command-line"></a><span data-ttu-id="ab165-227">Публикация конечной точки MSDeploy из командной строки</span><span class="sxs-lookup"><span data-stu-id="ab165-227">Publish to an MSDeploy endpoint from the command line</span></span>

<span data-ttu-id="ab165-228">В следующем примере используется веб-приложение ASP.NET Core, созданное с помощью Visual Studio, с именем *AzureWebApp*.</span><span class="sxs-lookup"><span data-stu-id="ab165-228">The following example uses an ASP.NET Core web app created by Visual Studio named *AzureWebApp*.</span></span> <span data-ttu-id="ab165-229">Профиль публикации приложений Azure добавляется с помощью Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab165-229">An Azure Apps publish profile is added with Visual Studio.</span></span> <span data-ttu-id="ab165-230">Дополнительные сведения о том, как создать профиль, см. в разделе [Профили публикации](#publish-profiles).</span><span class="sxs-lookup"><span data-stu-id="ab165-230">For more information on how to create a profile, see the [Publish profiles](#publish-profiles) section.</span></span>

<span data-ttu-id="ab165-231">Чтобы развернуть приложение с помощью профиля публикации, выполните команду `msbuild` из **командной строки разработчика** Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab165-231">To deploy the app using a publish profile, execute the `msbuild` command from a Visual Studio **Developer Command Prompt**.</span></span> <span data-ttu-id="ab165-232">Командная строка доступна в папке *Visual Studio* в меню **Пуск** на панели задач Windows.</span><span class="sxs-lookup"><span data-stu-id="ab165-232">The command prompt is available in the *Visual Studio* folder of the **Start** menu on the Windows taskbar.</span></span> <span data-ttu-id="ab165-233">Для удобства можно добавить командную строку в меню **Сервис** в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ab165-233">For easier access, you can add the command prompt to the **Tools** menu in Visual Studio.</span></span> <span data-ttu-id="ab165-234">Дополнительные сведения см. в статье [Командная строка разработчика для Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span><span class="sxs-lookup"><span data-stu-id="ab165-234">For more information, see [Developer Command Prompt for Visual Studio](/dotnet/framework/tools/developer-command-prompt-for-vs#run-the-command-prompt-from-inside-visual-studio).</span></span>

<span data-ttu-id="ab165-235">MSBuild использует следующий синтаксис команд.</span><span class="sxs-lookup"><span data-stu-id="ab165-235">MSBuild uses the following command syntax:</span></span>

```console
msbuild {PATH} 
    /p:DeployOnBuild=true 
    /p:PublishProfile={PROFILE} 
    /p:Username={USERNAME} 
    /p:Password={PASSWORD}
```

* <span data-ttu-id="ab165-236">{PATH} &ndash; путь к файлу проекта приложения.</span><span class="sxs-lookup"><span data-stu-id="ab165-236">{PATH} &ndash; Path to the app's project file.</span></span>
* <span data-ttu-id="ab165-237">{PROFILE} &ndash; имя профиля публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-237">{PROFILE} &ndash; Name of the publish profile.</span></span>
* <span data-ttu-id="ab165-238">{USERNAME} &ndash; имя пользователя MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="ab165-238">{USERNAME} &ndash; MSDeploy username.</span></span> <span data-ttu-id="ab165-239">{USERNAME} можно найти в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-239">The {USERNAME} can be found in the publish profile.</span></span>
* <span data-ttu-id="ab165-240">{PASSWORD} &ndash; пароль MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="ab165-240">{PASSWORD} &ndash; MSDeploy password.</span></span> <span data-ttu-id="ab165-241">Получите {PASSWORD} из файла *{PROFILE}.PublishSettings*.</span><span class="sxs-lookup"><span data-stu-id="ab165-241">Obtain the {PASSWORD} from the *{PROFILE}.PublishSettings* file.</span></span> <span data-ttu-id="ab165-242">Файл с расширением *.PublishSettings* можно скачать одним из следующих способов:</span><span class="sxs-lookup"><span data-stu-id="ab165-242">Download the *.PublishSettings* file from either:</span></span>
  * <span data-ttu-id="ab165-243">Обозреватель решений. Выберите **Представление** > **Cloud Explorer**.</span><span class="sxs-lookup"><span data-stu-id="ab165-243">Solution Explorer: Select **View** > **Cloud Explorer**.</span></span> <span data-ttu-id="ab165-244">Подключитесь к подписке Azure.</span><span class="sxs-lookup"><span data-stu-id="ab165-244">Connect with your Azure subscription.</span></span> <span data-ttu-id="ab165-245">Откройте **Службы приложений**.</span><span class="sxs-lookup"><span data-stu-id="ab165-245">Open **App Services**.</span></span> <span data-ttu-id="ab165-246">Щелкните правой кнопкой приложение.</span><span class="sxs-lookup"><span data-stu-id="ab165-246">Right-click the app.</span></span> <span data-ttu-id="ab165-247">Выберите **Загрузить профиль публикации**.</span><span class="sxs-lookup"><span data-stu-id="ab165-247">Select **Download Publish Profile**.</span></span>
  * <span data-ttu-id="ab165-248">Портал Azure: щелкните **Скачать профиль публикации** на панели **Обзор** веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="ab165-248">Azure portal: Select **Get publish profile** in the web app's **Overview** panel.</span></span>

<span data-ttu-id="ab165-249">В следующем примере используется профиль публикации с именем *AzureWebApp — веб-развертывание*:</span><span class="sxs-lookup"><span data-stu-id="ab165-249">The following example uses a publish profile named *AzureWebApp - Web Deploy*:</span></span>

```console
msbuild "AzureWebApp.csproj" 
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

<span data-ttu-id="ab165-250">Профиль публикации можно также использовать с командой .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) из командной строки Windows.</span><span class="sxs-lookup"><span data-stu-id="ab165-250">A publish profile can also be used with the .NET Core CLI [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command from a Windows command prompt:</span></span>

```console
dotnet msbuild "AzureWebApp.csproj"
    /p:DeployOnBuild=true 
    /p:PublishProfile="AzureWebApp - Web Deploy" 
    /p:Username="$AzureWebApp" 
    /p:Password=".........."
```

> [!NOTE]
> <span data-ttu-id="ab165-251">Команда [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) доступна на всех платформах и может компилировать приложения ASP.NET Core в macOS и Linux.</span><span class="sxs-lookup"><span data-stu-id="ab165-251">The [dotnet msbuild](/dotnet/core/tools/dotnet-msbuild) command is available cross-platform and can compile ASP.NET Core apps on macOS and Linux.</span></span> <span data-ttu-id="ab165-252">Однако MSBuild в macOS и Linux не может развертывать приложения в Azure или другой конечной точке MSDeploy.</span><span class="sxs-lookup"><span data-stu-id="ab165-252">However, MSBuild on macOS and Linux isn't capable of deploying an app to Azure or other MSDeploy endpoint.</span></span> <span data-ttu-id="ab165-253">MSDeploy доступен только в Windows.</span><span class="sxs-lookup"><span data-stu-id="ab165-253">MSDeploy is only available on Windows.</span></span>

## <a name="set-the-environment"></a><span data-ttu-id="ab165-254">Указание среды</span><span class="sxs-lookup"><span data-stu-id="ab165-254">Set the environment</span></span>

<span data-ttu-id="ab165-255">Включите свойство `<EnvironmentName>` в профиле публикации (*PUBXML*) или файле проекта, чтобы задать [среду](xref:fundamentals/environments) приложения:</span><span class="sxs-lookup"><span data-stu-id="ab165-255">Include the `<EnvironmentName>` property in the publish profile (*.pubxml*) or project file to set the app's [environment](xref:fundamentals/environments):</span></span>

```xml
<PropertyGroup>
  <EnvironmentName>Development</EnvironmentName>
</PropertyGroup>
```

<span data-ttu-id="ab165-256">Если вам нужно преобразовать *web.config* (например, задать переменные среды на основе конфигурации, профиля или среды), см. раздел <xref:host-and-deploy/iis/transform-webconfig>.</span><span class="sxs-lookup"><span data-stu-id="ab165-256">If you require *web.config* transformations (for example, setting environment variables based on the configuration, profile, or environment), see <xref:host-and-deploy/iis/transform-webconfig>.</span></span>

## <a name="exclude-files"></a><span data-ttu-id="ab165-257">Исключение файлов</span><span class="sxs-lookup"><span data-stu-id="ab165-257">Exclude files</span></span>

<span data-ttu-id="ab165-258">При публикации веб-приложений ASP.NET Core включаются следующие ресурсы:</span><span class="sxs-lookup"><span data-stu-id="ab165-258">When publishing ASP.NET Core web apps, the following assets are included:</span></span>

* <span data-ttu-id="ab165-259">Артефакты сборки</span><span class="sxs-lookup"><span data-stu-id="ab165-259">Build artifacts</span></span>
* <span data-ttu-id="ab165-260">Файлы и папки, соответствующие следующим стандартным маскам:</span><span class="sxs-lookup"><span data-stu-id="ab165-260">Folders and files matching the following globbing patterns:</span></span>
  * <span data-ttu-id="ab165-261">`**\*.config` (например, *web.config*);</span><span class="sxs-lookup"><span data-stu-id="ab165-261">`**\*.config` (for example, *web.config*)</span></span>
  * <span data-ttu-id="ab165-262">`**\*.json` (например, *appsettings.json*).</span><span class="sxs-lookup"><span data-stu-id="ab165-262">`**\*.json` (for example, *appsettings.json*)</span></span>
  * `wwwroot\**`

<span data-ttu-id="ab165-263">MSBuild поддерживает [стандартные маски](https://gruntjs.com/configuring-tasks#globbing-patterns).</span><span class="sxs-lookup"><span data-stu-id="ab165-263">MSBuild supports [globbing patterns](https://gruntjs.com/configuring-tasks#globbing-patterns).</span></span> <span data-ttu-id="ab165-264">Например, представленный ниже элемент `<Content>` запретит копирование текстовых файлов ( *.txt*) из папки *wwwroot\content* и всех ее подпапок.</span><span class="sxs-lookup"><span data-stu-id="ab165-264">For example, the following `<Content>` element suppresses the copying of text (*.txt*) files in the *wwwroot\content* folder and its subfolders:</span></span>

```xml
<ItemGroup>
  <Content Update="wwwroot/content/**/*.txt" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="ab165-265">Предложенную выше разметку можно добавить в профиль публикации или в файл *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="ab165-265">The preceding markup can be added to a publish profile or the *.csproj* file.</span></span> <span data-ttu-id="ab165-266">При добавлении в файл *.csproj* правило добавляется во все профили публикации в проекте.</span><span class="sxs-lookup"><span data-stu-id="ab165-266">When added to the *.csproj* file, the rule is added to all publish profiles in the project.</span></span>

<span data-ttu-id="ab165-267">Следующий элемент `<MsDeploySkipRules>` исключает все файлы из папки *wwwroot\content*.</span><span class="sxs-lookup"><span data-stu-id="ab165-267">The following `<MsDeploySkipRules>` element excludes all files from the *wwwroot\content* folder:</span></span>

```xml
<ItemGroup>
  <MsDeploySkipRules Include="CustomSkipFolder">
    <ObjectName>dirPath</ObjectName>
    <AbsolutePath>wwwroot\\content</AbsolutePath>
  </MsDeploySkipRules>
</ItemGroup>
```

<span data-ttu-id="ab165-268">Элемент `<MsDeploySkipRules>` не удаляет *пропускаемые* целевые объекты с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="ab165-268">`<MsDeploySkipRules>` won't delete the *skip* targets from the deployment site.</span></span> <span data-ttu-id="ab165-269">Целевые файлы и папки, указанные в элементе `<Content>`, будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="ab165-269">`<Content>` targeted files and folders are deleted from the deployment site.</span></span> <span data-ttu-id="ab165-270">Допустим, что в развернутом веб-приложении есть следующие файлы.</span><span class="sxs-lookup"><span data-stu-id="ab165-270">For example, suppose a deployed web app had the following files:</span></span>

* <span data-ttu-id="ab165-271">*Views/Home/About1.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ab165-271">*Views/Home/About1.cshtml*</span></span>
* <span data-ttu-id="ab165-272">*Views/Home/About2.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ab165-272">*Views/Home/About2.cshtml*</span></span>
* <span data-ttu-id="ab165-273">*Views/Home/About3.cshtml*</span><span class="sxs-lookup"><span data-stu-id="ab165-273">*Views/Home/About3.cshtml*</span></span>

<span data-ttu-id="ab165-274">Если вы добавите указанные ниже элементы `<MsDeploySkipRules>`, эти файлы не будут удалены с сайта развертывания.</span><span class="sxs-lookup"><span data-stu-id="ab165-274">If the following `<MsDeploySkipRules>` elements are added, those files wouldn't be deleted on the deployment site.</span></span>

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

<span data-ttu-id="ab165-275">Приведенные выше элементы `<MsDeploySkipRules>` запрещают развертывание *пропускаемых* файлов.</span><span class="sxs-lookup"><span data-stu-id="ab165-275">The preceding `<MsDeploySkipRules>` elements prevent the *skipped* files from being deployed.</span></span> <span data-ttu-id="ab165-276">Эти файлы не будут удалены, если они уже развернуты.</span><span class="sxs-lookup"><span data-stu-id="ab165-276">It won't delete those files once they're deployed.</span></span>

<span data-ttu-id="ab165-277">Следующий элемент `<Content>` удаляет целевые файлы на сайте развертывания.</span><span class="sxs-lookup"><span data-stu-id="ab165-277">The following `<Content>` element deletes the targeted files at the deployment site:</span></span>

```xml
<ItemGroup>
  <Content Update="Views/Home/About?.cshtml" CopyToPublishDirectory="Never" />
</ItemGroup>
```

<span data-ttu-id="ab165-278">Применение указанного выше элемента `<Content>` при развертывании из командной строки дает следующий результат:</span><span class="sxs-lookup"><span data-stu-id="ab165-278">Using command-line deployment with the preceding `<Content>` element yields the following output:</span></span>

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

## <a name="include-files"></a><span data-ttu-id="ab165-279">Включаемые файлы</span><span class="sxs-lookup"><span data-stu-id="ab165-279">Include files</span></span>

<span data-ttu-id="ab165-280">Следующий пример разметки:</span><span class="sxs-lookup"><span data-stu-id="ab165-280">The following markup:</span></span>

* <span data-ttu-id="ab165-281">включает папку *images*, расположенную за пределами папки проекта, в папку *wwwroot/images* на сайте публикации;</span><span class="sxs-lookup"><span data-stu-id="ab165-281">Includes an *images* folder outside the project directory to the *wwwroot/images* folder of the publish site.</span></span>
* <span data-ttu-id="ab165-282">может размещаться в файле *.csproj* или в профиле публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-282">Can be added to the *.csproj* file or the publish profile.</span></span> <span data-ttu-id="ab165-283">Если добавить ее в файл *CSPROJ*, она будет включаться в каждый профиль публикации этого проекта.</span><span class="sxs-lookup"><span data-stu-id="ab165-283">If it's added to the *.csproj* file, it's included in each publish profile in the project.</span></span>

```xml
<ItemGroup>
  <_CustomFiles Include="$(MSBuildProjectDirectory)/../images/**/*" />
  <DotnetPublishFiles Include="@(_CustomFiles)">
    <DestinationRelativePath>wwwroot/images/%(RecursiveDir)%(Filename)%(Extension)</DestinationRelativePath>
  </DotnetPublishFiles>
</ItemGroup>
```

<span data-ttu-id="ab165-284">Разметка, выделенная в приведенном ниже примере, показывает, как.</span><span class="sxs-lookup"><span data-stu-id="ab165-284">The following highlighted markup shows how to:</span></span>

* <span data-ttu-id="ab165-285">Скопировать файл из папки, не связанной с проектом, в папку *wwwroot*.</span><span class="sxs-lookup"><span data-stu-id="ab165-285">Copy a file from outside the project into the *wwwroot* folder.</span></span>
* <span data-ttu-id="ab165-286">Исключить папку *wwwroot\Content*.</span><span class="sxs-lookup"><span data-stu-id="ab165-286">Exclude the *wwwroot\Content* folder.</span></span>
* <span data-ttu-id="ab165-287">Исключить файл *Views\Home\About2.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="ab165-287">Exclude *Views\Home\About2.cshtml*.</span></span>

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

<span data-ttu-id="ab165-288">Дополнительные примеры развертывания см. в [файле Readme из репозитория веб-пакета SDK](https://github.com/aspnet/websdk).</span><span class="sxs-lookup"><span data-stu-id="ab165-288">See the [Web SDK repository Readme](https://github.com/aspnet/websdk) for more deployment samples.</span></span>

## <a name="run-a-target-before-or-after-publishing"></a><span data-ttu-id="ab165-289">Выполнение целевого объекта до или после публикации</span><span class="sxs-lookup"><span data-stu-id="ab165-289">Run a target before or after publishing</span></span>

<span data-ttu-id="ab165-290">Встроенные целевые объекты `BeforePublish` и `AfterPublish` позволяют выполнить целевой объект до или после его публикации.</span><span class="sxs-lookup"><span data-stu-id="ab165-290">The built-in `BeforePublish` and `AfterPublish` targets execute a target before or after the publish target.</span></span> <span data-ttu-id="ab165-291">Добавьте следующие элементы в профиль публикации, чтобы сохранять сообщения в консоли, возвращаемые до и после публикации:</span><span class="sxs-lookup"><span data-stu-id="ab165-291">Add the following elements to the publish profile to log console messages both before and after publishing:</span></span>

```xml
<Target Name="CustomActionsBeforePublish" BeforeTargets="BeforePublish">
    <Message Text="Inside BeforePublish" Importance="high" />
  </Target>
  <Target Name="CustomActionsAfterPublish" AfterTargets="AfterPublish">
    <Message Text="Inside AfterPublish" Importance="high" />
</Target>
```

## <a name="publish-to-a-server-using-an-untrusted-certificate"></a><span data-ttu-id="ab165-292">Публикация на сервере с использованием сертификата без доверия</span><span class="sxs-lookup"><span data-stu-id="ab165-292">Publish to a server using an untrusted certificate</span></span>

<span data-ttu-id="ab165-293">Добавьте свойство `<AllowUntrustedCertificate>` со значением `True` в профиль публикации:</span><span class="sxs-lookup"><span data-stu-id="ab165-293">Add the `<AllowUntrustedCertificate>` property with a value of `True` to the publish profile:</span></span>

```xml
<PropertyGroup>
  <AllowUntrustedCertificate>True</AllowUntrustedCertificate>
</PropertyGroup>
```

## <a name="the-kudu-service"></a><span data-ttu-id="ab165-294">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="ab165-294">The Kudu service</span></span>

<span data-ttu-id="ab165-295">Чтобы просмотреть файлы в развертывании веб-приложения службы приложений Azure, используйте [службу Kudu](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span><span class="sxs-lookup"><span data-stu-id="ab165-295">To view the files in an Azure App Service web app deployment, use the [Kudu service](https://github.com/projectkudu/kudu/wiki/Accessing-the-kudu-service).</span></span> <span data-ttu-id="ab165-296">Добавьте к имени веб-приложения маркер `scm`.</span><span class="sxs-lookup"><span data-stu-id="ab165-296">Append the `scm` token to the web app name.</span></span> <span data-ttu-id="ab165-297">Например:</span><span class="sxs-lookup"><span data-stu-id="ab165-297">For example:</span></span>

| <span data-ttu-id="ab165-298">URL-адрес</span><span class="sxs-lookup"><span data-stu-id="ab165-298">URL</span></span>                                    | <span data-ttu-id="ab165-299">Результат</span><span class="sxs-lookup"><span data-stu-id="ab165-299">Result</span></span>       |
| -------------------------------------- | ------------ |
| `http://mysite.azurewebsites.net/`     | <span data-ttu-id="ab165-300">Веб-приложение</span><span class="sxs-lookup"><span data-stu-id="ab165-300">Web App</span></span>      |
| `http://mysite.scm.azurewebsites.net/` | <span data-ttu-id="ab165-301">Служба Kudu</span><span class="sxs-lookup"><span data-stu-id="ab165-301">Kudu service</span></span> |

<span data-ttu-id="ab165-302">Для просмотра, редактирования, удаления и (или) добавления файлов выберите в меню пункт [Консоль отладки](https://github.com/projectkudu/kudu/wiki/Kudu-console).</span><span class="sxs-lookup"><span data-stu-id="ab165-302">Select the [Debug Console](https://github.com/projectkudu/kudu/wiki/Kudu-console) menu item to view, edit, delete, or add files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="ab165-303">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ab165-303">Additional resources</span></span>

* <span data-ttu-id="ab165-304">[Веб-развертывание](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) упрощает развертывание веб-приложений и веб-сайтов на серверах IIS.</span><span class="sxs-lookup"><span data-stu-id="ab165-304">[Web Deploy](https://www.iis.net/downloads/microsoft/web-deploy) (MSDeploy) simplifies deployment of web apps and websites to IIS servers.</span></span>
* <span data-ttu-id="ab165-305">[Репозиторий веб-пакета SDK на GitHub](https://github.com/aspnet/websdk/issues) проблемы с файлами и возможности запросов для развертывания.</span><span class="sxs-lookup"><span data-stu-id="ab165-305">[Web SDK GitHub repository](https://github.com/aspnet/websdk/issues): File issues and request features for deployment.</span></span>
* [<span data-ttu-id="ab165-306">Публикация веб-приложения ASP.NET в виртуальную машину Azure из Visual Studio</span><span class="sxs-lookup"><span data-stu-id="ab165-306">Publish an ASP.NET Web App to an Azure VM from Visual Studio</span></span>](/azure/virtual-machines/windows/publish-web-app-from-visual-studio)
* <xref:host-and-deploy/iis/transform-webconfig>
