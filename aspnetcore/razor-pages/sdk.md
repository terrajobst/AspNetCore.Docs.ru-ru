---
title: Пакет SDK для Razor в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: 1dc001c7c5fe320629835e06fe6db7fadabff94d
ms.sourcegitcommit: 6189b0ced9c115248c6ede02efcd0b29d31f2115
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/23/2019
ms.locfileid: "69985395"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="8c8be-103">Пакет SDK для Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8c8be-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="8c8be-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="8c8be-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="8c8be-105">Обзор</span><span class="sxs-lookup"><span data-stu-id="8c8be-105">Overview</span></span>

<span data-ttu-id="8c8be-106">[!INCLUDE[](~/includes/2.1-SDK.md)] Включает в себя `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="8c8be-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="8c8be-107">Пакет SDK для Razor:</span><span class="sxs-lookup"><span data-stu-id="8c8be-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
* <span data-ttu-id="8c8be-108">Стандартизирует процесс создания, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor), для проектов на основе MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="8c8be-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="8c8be-109">Включает набор предопределенных целевых объектов, свойств и элементов, которые позволяют настраивать параметры компиляции файлов Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
* <span data-ttu-id="8c8be-110">требуется для сборки, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor) для ASP.NET Core проектов на основе MVC или проектов [блазор](xref:blazor/index)</span><span class="sxs-lookup"><span data-stu-id="8c8be-110">is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects, or [Blazor](xref:blazor/index) projects</span></span>
* <span data-ttu-id="8c8be-111">Включает набор предопределенных целевых объектов, свойств и элементов, позволяющих настраивать компиляцию файлов Razor (. cshtml или. Razor).</span><span class="sxs-lookup"><span data-stu-id="8c8be-111">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (.cshtml or .razor) files.</span></span>
::: moniker-end


::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="8c8be-112">Пакет SDK для Razor включает `<Content>` элемент `Include` с атрибутом, `**\*.cshtml` установленным в шаблон глобализации.</span><span class="sxs-lookup"><span data-stu-id="8c8be-112">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="8c8be-113">Публикуются соответствующие файлы.</span><span class="sxs-lookup"><span data-stu-id="8c8be-113">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8c8be-114">Пакет SDK для Razor `<Content>` включает элементы `Include` с атрибутами, `**\*.cshtml` заданными шаблонами и `**\*.razor` глобализации.</span><span class="sxs-lookup"><span data-stu-id="8c8be-114">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="8c8be-115">Публикуются соответствующие файлы.</span><span class="sxs-lookup"><span data-stu-id="8c8be-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="8c8be-116">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="8c8be-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="8c8be-117">Использование пакета SDK для Razor</span><span class="sxs-lookup"><span data-stu-id="8c8be-117">Use the Razor SDK</span></span>

<span data-ttu-id="8c8be-118">Большинство веб-приложений не требуется явно ссылаться на пакет SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"
<span data-ttu-id="8c8be-119">Использование пакета SDK для Razor для создания библиотеки классов с представлениями Razor или страницами Razor:</span><span class="sxs-lookup"><span data-stu-id="8c8be-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="8c8be-120">Используйте `Microsoft.NET.Sdk.Razor` вместо `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="8c8be-120">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="8c8be-121">Как правило, ссылки на пакет `Microsoft.AspNetCore.Mvc` требуется для получения дополнительные зависимости, необходимые для создания и компиляции Razor Pages и представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-121">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="8c8be-122">Как минимум проект следует добавить ссылки на пакеты для:</span><span class="sxs-lookup"><span data-stu-id="8c8be-122">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="8c8be-123">`Microsoft.AspNetCore.Razor.Design` Пакет содержит задачи компиляции Razor и целевых объектов для проекта.</span><span class="sxs-lookup"><span data-stu-id="8c8be-123">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="8c8be-124">Предыдущие пакеты включены в `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-124">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="8c8be-125">В следующей разметке показан файл проекта, который использует пакет SDK для Razor для создания файлов Razor для приложения ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="8c8be-125">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="8c8be-126">Чтобы использовать пакет SDK для Razor для построения библиотек классов, содержащих представления Razor или Razor Pages мы рекомендуем начать с шаблона проекта библиотеки классов Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-126">To use the Razor SDK to build class libraries containing Razor views or Razor Pages we recommend starting with the Razor Class Library project template.</span></span> <span data-ttu-id="8c8be-127">В библиотеке классов Razor, используемой для сборки файлов блазор (. Razor), по меньшей мере требуется ссылка на `Microsoft.AspNetCore.Components` пакет.</span><span class="sxs-lookup"><span data-stu-id="8c8be-127">A Razor class library that is used to build Blazor (.razor) files will at minimum require a reference to the `Microsoft.AspNetCore.Components` package.</span></span> <span data-ttu-id="8c8be-128">Библиотека классов Razor, которая используется для построения представлений Razor или страниц (CSHTML-файлов), требует выбора целевой платформы `netcoreapp3.0` или более поздней версии и `FrameworkReference` иметь `Microsoft.AspNetCore.App`в.</span><span class="sxs-lookup"><span data-stu-id="8c8be-128">A Razor class library that is used to build Razor views or pages (.cshtml files), will require targeting `netcoreapp3.0` or newer and have a `FrameworkReference` to `Microsoft.AspNetCore.App`.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="8c8be-129">`Microsoft.AspNetCore.Razor.Design` И `Microsoft.AspNetCore.Mvc.Razor.Extensions` пакеты входят в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="8c8be-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="8c8be-130">Тем не менее тем меньше версии `Microsoft.AspNetCore.App` ссылки на пакет предоставляет метапакет к приложению, которое не включает последнюю версию `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="8c8be-131">Проекты должны ссылаться на согласованность версий `Microsoft.AspNetCore.Razor.Design` (или `Microsoft.AspNetCore.Mvc`), чтобы включаются последние исправления во время сборки для Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="8c8be-132">Дополнительные сведения см. в разделе [проблема GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="8c8be-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="8c8be-133">Свойства</span><span class="sxs-lookup"><span data-stu-id="8c8be-133">Properties</span></span>

<span data-ttu-id="8c8be-134">Следующие свойства управляют поведением пакета SDK для Razor в ходе создания проекта:</span><span class="sxs-lookup"><span data-stu-id="8c8be-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="8c8be-135">`RazorCompileOnBuild` &ndash; Когда `true`, компилирует и сборка Razor как часть сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="8c8be-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="8c8be-136">По умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-136">Defaults to `true`.</span></span>
* <span data-ttu-id="8c8be-137">`RazorCompileOnPublish` &ndash; Когда `true`, компиляция и сборка Razor как часть публикации проекта.</span><span class="sxs-lookup"><span data-stu-id="8c8be-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="8c8be-138">По умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-138">Defaults to `true`.</span></span>

<span data-ttu-id="8c8be-139">Свойства и элементы в следующей таблице используются для настройки входных и выходных данных в пакет SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"
> [!WARNING]
<span data-ttu-id="8c8be-140">Начиная с ASP.NET Core 3,0, представления MVC или Razor Pages не будут обслуживаться по умолчанию `RazorCompileOnBuild` , `RazorCompileOnPublish` если или отключены.</span><span class="sxs-lookup"><span data-stu-id="8c8be-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages will not be served by default if `RazorCompileOnBuild` or `RazorCompileOnPublish` is disabled.</span></span> <span data-ttu-id="8c8be-141">Приложения должны добавить явную ссылку на `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` пакет, чтобы добавить поддержку компиляции среды выполнения, если они используют компиляцию времени выполнения для обработки файлов CSHTML.</span><span class="sxs-lookup"><span data-stu-id="8c8be-141">Applications must add an explicit reference to `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` package to add support for runtime compilation if they rely on runtime compilation to process cshtml files.</span></span>
::: moniker-end


| <span data-ttu-id="8c8be-142">Элементы</span><span class="sxs-lookup"><span data-stu-id="8c8be-142">Items</span></span> | <span data-ttu-id="8c8be-143">Описание</span><span class="sxs-lookup"><span data-stu-id="8c8be-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="8c8be-144">Элементы Item (*CSHTML* -файлы), которые являются входными данными для создания кода.</span><span class="sxs-lookup"><span data-stu-id="8c8be-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="8c8be-145">Элементы элементов (*Razor* -файлы), которые являются входными данными для создания кода компонента.</span><span class="sxs-lookup"><span data-stu-id="8c8be-145">Item elements (*.razor* files) that are inputs to Component code generation.</span></span>
| `RazorCompile` | <span data-ttu-id="8c8be-146">Элементы Item (*CS* -файлы), входные в целевые объекты компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="8c8be-147">Используйте этот `ItemGroup` параметр, чтобы указать дополнительные файлы для компиляции в сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="8c8be-148">Элементы, используемые для создания атрибутов для сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="8c8be-149">Пример:</span><span class="sxs-lookup"><span data-stu-id="8c8be-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="8c8be-150">Элемент элементы добавляются в виде внедренных ресурсов на созданную сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="8c8be-151">Свойство</span><span class="sxs-lookup"><span data-stu-id="8c8be-151">Property</span></span> | <span data-ttu-id="8c8be-152">Описание</span><span class="sxs-lookup"><span data-stu-id="8c8be-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="8c8be-153">Имя файла (без расширения) для сборки, созданной Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="8c8be-154">Выходной каталог Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="8c8be-155">Используется для определения набора инструментов для построения сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="8c8be-156">Допустимые значения: `Implicit`, `RazorSDK` и `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="8c8be-157">енабледефаултконтентитемс</span><span class="sxs-lookup"><span data-stu-id="8c8be-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="8c8be-158">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-158">Default is `true`.</span></span> <span data-ttu-id="8c8be-159">Когда `true`, включает файлы *Web. config*, *. JSON*и *. cshtml* в качестве содержимого в проекте.</span><span class="sxs-lookup"><span data-stu-id="8c8be-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="8c8be-160">Когда я ссылаюсь через `Microsoft.NET.Sdk.Web`, файлы в разделе *wwwroot* и файлы конфигурации, также будут включены.</span><span class="sxs-lookup"><span data-stu-id="8c8be-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="8c8be-161">При значении `true` включает файлы *.cshtml* из элементов `Content` в элементы `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="8c8be-162">Когда `true`, приводит к возникновению ошибки *.cs* файл, содержащий атрибуты, указанные фильтром `RazorAssemblyAttribute` и файл в выходных данных компиляции.</span><span class="sxs-lookup"><span data-stu-id="8c8be-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="8c8be-163">При значении `true` добавляет стандартный набор атрибутов сборки в `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="8c8be-164">Когда `true`, копии `RazorGenerate` элементы ( *.cshtml*) файлы в каталог публикации.</span><span class="sxs-lookup"><span data-stu-id="8c8be-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="8c8be-165">Как правило файлы Razor не требуются для опубликованного приложения, если они участвуют в компиляции во время сборки или во время публикации.</span><span class="sxs-lookup"><span data-stu-id="8c8be-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="8c8be-166">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="8c8be-167">При значении `true` копирует элементы базовой сборки в каталог публикации.</span><span class="sxs-lookup"><span data-stu-id="8c8be-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="8c8be-168">Как правило ссылочные сборки не являются обязательными для опубликованного приложения случае во время сборки или публикации во время компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="8c8be-169">Значение `true` Если опубликованного приложения требует компиляции во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="8c8be-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="8c8be-170">Например, значение равно `true` Если приложение изменяет *.cshtml* файлы во время выполнения или использует внедренные представления.</span><span class="sxs-lookup"><span data-stu-id="8c8be-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="8c8be-171">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="8c8be-172">Когда `true`, все элементы содержимого Razor ( *.cshtml* файлы) отмечены для включения в создаваемый пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="8c8be-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="8c8be-173">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="8c8be-174">При значении `true` добавляет элементы RazorGenerate ( *.cshtml*) в виде внедренных файлов к создаваемой сборке Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="8c8be-175">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="8c8be-176">При значении `true` использует серверный процесс постоянной сборки для разгрузки работы по созданию кода.</span><span class="sxs-lookup"><span data-stu-id="8c8be-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="8c8be-177">По умолчанию используется значение `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="8c8be-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="8c8be-178">Когда `true`пакет SDK создает дополнительные атрибуты, используемые MVC во время выполнения, для выполнения обнаружения частей приложения.</span><span class="sxs-lookup"><span data-stu-id="8c8be-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="8c8be-179">Дополнительные сведения о свойствах см. в статье [MSBuild Properties](/visualstudio/msbuild/msbuild-properties) (Свойства MSBuild).</span><span class="sxs-lookup"><span data-stu-id="8c8be-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="8c8be-180">Целевые объекты</span><span class="sxs-lookup"><span data-stu-id="8c8be-180">Targets</span></span>

<span data-ttu-id="8c8be-181">Пакет SDK для Razor определяет два основных целевых объекта:</span><span class="sxs-lookup"><span data-stu-id="8c8be-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="8c8be-182">`RazorGenerate` &ndash; Код создает *.cs* файлов из `RazorGenerate` элементы item.</span><span class="sxs-lookup"><span data-stu-id="8c8be-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="8c8be-183">Используйте свойство `RazorGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="8c8be-183">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="8c8be-184">`RazorCompile` &ndash; Компилирует созданный *.cs* файлы в сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="8c8be-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="8c8be-185">Используйте `RazorCompileDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="8c8be-185">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="8c8be-186">`RazorComponentGenerate`Код создает файлы *. CS* для `RazorComponent` элементов Item. &ndash;</span><span class="sxs-lookup"><span data-stu-id="8c8be-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="8c8be-187">Используйте свойство `RazorComponentGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="8c8be-187">Use `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="8c8be-188">Компиляция среды выполнения представлений Razor</span><span class="sxs-lookup"><span data-stu-id="8c8be-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="8c8be-189">По умолчанию пакет SDK для Razor не публикует базовые сборки, необходимые для компиляции среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="8c8be-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="8c8be-190">Это приведет к сбою компиляции, если модель приложения зависит от компиляции среды выполнения &mdash; например, приложение использует внедренные представления или меняет представления после публикации.</span><span class="sxs-lookup"><span data-stu-id="8c8be-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="8c8be-191">Установите для `CopyRefAssembliesToPublishDirectory` значение `true`, чтобы продолжить публикацию базовых сборок.</span><span class="sxs-lookup"><span data-stu-id="8c8be-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="8c8be-192">Для веб-приложения, убедитесь в приложение предназначено для `Microsoft.NET.Sdk.Web` пакета SDK.</span><span class="sxs-lookup"><span data-stu-id="8c8be-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="8c8be-193">Версия языка Razor</span><span class="sxs-lookup"><span data-stu-id="8c8be-193">Razor language version</span></span>

<span data-ttu-id="8c8be-194">При нацеливании `Microsoft.NET.Sdk.Web` на пакет SDK версия языка Razor выводится из целевой версии платформы приложения.</span><span class="sxs-lookup"><span data-stu-id="8c8be-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="8c8be-195">Для проектов, предназначенных `Microsoft.NET.Sdk.Razor` для пакета SDK, или в редких случаях, когда приложению требуется другая версия языка Razor, чем выводимое значение, можно настроить версию, `<RazorLangVersion>` задав свойство в файле проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="8c8be-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="8c8be-196">Языковая версия Razor тесно интегрирована с версией среды выполнения, для которой она была создана.</span><span class="sxs-lookup"><span data-stu-id="8c8be-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="8c8be-197">Нацеливание на языковую версию, не предназначенную для среды выполнения, не поддерживается и, скорее всего, приведет к ошибкам сборки.</span><span class="sxs-lookup"><span data-stu-id="8c8be-197">Targeting a language version that is not designed for the runtime is unsupported, and will likely produce build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c8be-198">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8c8be-198">Additional resources</span></span>

* [<span data-ttu-id="8c8be-199">Дополнения к формату CSPROJ для .NET Core</span><span class="sxs-lookup"><span data-stu-id="8c8be-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="8c8be-200">Общие элементы проектов MSBuild</span><span class="sxs-lookup"><span data-stu-id="8c8be-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
