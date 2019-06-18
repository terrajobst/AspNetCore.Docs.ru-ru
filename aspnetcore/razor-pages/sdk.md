---
title: Пакет SDK для Razor в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/18/2019
uid: razor-pages/sdk
ms.openlocfilehash: fa69e4840377e0c1c8291c7ba9305a27bd3e6b82
ms.sourcegitcommit: 516f166c5f7cec54edf3d9c71e6e2ba53fb3b0e5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67196361"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="5b95c-103">Пакет SDK для Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5b95c-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="5b95c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="5b95c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="5b95c-105">Обзор</span><span class="sxs-lookup"><span data-stu-id="5b95c-105">Overview</span></span>

<span data-ttu-id="5b95c-106">[!INCLUDE[](~/includes/2.1-SDK.md)] Включает в себя `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="5b95c-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="5b95c-107">Пакет SDK для Razor:</span><span class="sxs-lookup"><span data-stu-id="5b95c-107">The Razor SDK:</span></span>

* <span data-ttu-id="5b95c-108">Стандартизирует процесс создания, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor), для проектов на основе MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="5b95c-108">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="5b95c-109">Включает набор предопределенных целевых объектов, свойств и элементов, которые позволяют настраивать параметры компиляции файлов Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="5b95c-110">Пакет SDK для Razor включает `<Content>` элемент с `Include` атрибут `**\*.cshtml` маски.</span><span class="sxs-lookup"><span data-stu-id="5b95c-110">The Razor SDK includes a `<Content>` element with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="5b95c-111">Совпадающие файлы публикуются.</span><span class="sxs-lookup"><span data-stu-id="5b95c-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="5b95c-112">Пакет SDK для Razor включает `<Content>` элементов при помощи `Include` атрибуты значения `**\*.cshtml` и `**\*.razor` шаблонов глобализации.</span><span class="sxs-lookup"><span data-stu-id="5b95c-112">The Razor SDK includes `<Content>` elements with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="5b95c-113">Совпадающие файлы публикуются.</span><span class="sxs-lookup"><span data-stu-id="5b95c-113">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="5b95c-114">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="5b95c-114">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="5b95c-115">Используйте пакет SDK для Razor</span><span class="sxs-lookup"><span data-stu-id="5b95c-115">Use the Razor SDK</span></span>

<span data-ttu-id="5b95c-116">Большинство веб-приложений не требуется явно ссылаться на пакет SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-116">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="5b95c-117">Использование пакета SDK для Razor для создания библиотеки классов с представлениями Razor или страницами Razor:</span><span class="sxs-lookup"><span data-stu-id="5b95c-117">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="5b95c-118">Используйте `Microsoft.NET.Sdk.Razor` вместо `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="5b95c-118">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="5b95c-119">Как правило, ссылки на пакет `Microsoft.AspNetCore.Mvc` требуется для получения дополнительные зависимости, необходимые для создания и компиляции Razor Pages и представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-119">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="5b95c-120">Как минимум проект следует добавить ссылки на пакеты для:</span><span class="sxs-lookup"><span data-stu-id="5b95c-120">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="5b95c-121">`Microsoft.AspNetCore.Razor.Design` Пакет содержит задачи компиляции Razor и целевых объектов для проекта.</span><span class="sxs-lookup"><span data-stu-id="5b95c-121">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="5b95c-122">Предыдущие пакеты включены в `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-122">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="5b95c-123">В следующей разметке показан файл проекта, который использует пакет SDK для Razor для создания файлов Razor для приложения ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="5b95c-123">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="5b95c-124">`Microsoft.AspNetCore.Razor.Design` И `Microsoft.AspNetCore.Mvc.Razor.Extensions` пакеты входят в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="5b95c-124">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="5b95c-125">Тем не менее тем меньше версии `Microsoft.AspNetCore.App` ссылки на пакет предоставляет метапакет к приложению, которое не включает последнюю версию `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-125">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="5b95c-126">Проекты должны ссылаться на согласованность версий `Microsoft.AspNetCore.Razor.Design` (или `Microsoft.AspNetCore.Mvc`), чтобы включаются последние исправления во время сборки для Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-126">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="5b95c-127">Дополнительные сведения см. в разделе [проблема GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="5b95c-127">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="5b95c-128">Свойства</span><span class="sxs-lookup"><span data-stu-id="5b95c-128">Properties</span></span>

<span data-ttu-id="5b95c-129">Следующие свойства управляют поведением пакета SDK для Razor в ходе создания проекта:</span><span class="sxs-lookup"><span data-stu-id="5b95c-129">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="5b95c-130">`RazorCompileOnBuild` &ndash; Когда `true`, компилирует и сборка Razor как часть сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="5b95c-130">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="5b95c-131">По умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-131">Defaults to `true`.</span></span>
* <span data-ttu-id="5b95c-132">`RazorCompileOnPublish` &ndash; Когда `true`, компиляция и сборка Razor как часть публикации проекта.</span><span class="sxs-lookup"><span data-stu-id="5b95c-132">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="5b95c-133">По умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-133">Defaults to `true`.</span></span>

<span data-ttu-id="5b95c-134">Свойства и элементы в следующей таблице используются для настройки входных и выходных данных в пакет SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-134">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="5b95c-135">Элементы</span><span class="sxs-lookup"><span data-stu-id="5b95c-135">Items</span></span> | <span data-ttu-id="5b95c-136">Описание</span><span class="sxs-lookup"><span data-stu-id="5b95c-136">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="5b95c-137">Элементы (файлы *.cshtml*), которые являются входными данными для целей создания кода.</span><span class="sxs-lookup"><span data-stu-id="5b95c-137">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="5b95c-138">Элементы Item ( *.cs* файлы), являются входными значениями для целевых сред компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-138">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="5b95c-139">Используйте этот `ItemGroup` чтобы указать дополнительные файлы, компилируемые в сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-139">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="5b95c-140">Элементы, используемые для создания атрибутов для сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-140">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="5b95c-141">Пример:</span><span class="sxs-lookup"><span data-stu-id="5b95c-141">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="5b95c-142">Элемент элементы добавляются в виде внедренных ресурсов на созданную сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-142">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="5b95c-143">Свойство</span><span class="sxs-lookup"><span data-stu-id="5b95c-143">Property</span></span> | <span data-ttu-id="5b95c-144">Описание</span><span class="sxs-lookup"><span data-stu-id="5b95c-144">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="5b95c-145">Имя файла (без расширения) для сборки, созданной Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-145">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="5b95c-146">Выходной каталог Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-146">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="5b95c-147">Используется для определения набора инструментов для построения сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-147">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="5b95c-148">Допустимые значения: `Implicit`, `RazorSDK` и `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-148">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="5b95c-149">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="5b95c-149">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="5b95c-150">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-150">Default is `true`.</span></span> <span data-ttu-id="5b95c-151">Когда `true`, включает в себя *web.config*, *.json*, и *.cshtml* файлы содержимого в проекте.</span><span class="sxs-lookup"><span data-stu-id="5b95c-151">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="5b95c-152">Когда я ссылаюсь через `Microsoft.NET.Sdk.Web`, файлы в разделе *wwwroot* и файлы конфигурации, также будут включены.</span><span class="sxs-lookup"><span data-stu-id="5b95c-152">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="5b95c-153">При значении `true` включает файлы *.cshtml* из элементов `Content` в элементы `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-153">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="5b95c-154">Когда `true`, приводит к возникновению ошибки *.cs* файл, содержащий атрибуты, указанные фильтром `RazorAssemblyAttribute` и файл в выходных данных компиляции.</span><span class="sxs-lookup"><span data-stu-id="5b95c-154">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="5b95c-155">При значении `true` добавляет стандартный набор атрибутов сборки в `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-155">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="5b95c-156">Когда `true`, копии `RazorGenerate` элементы ( *.cshtml*) файлы в каталог публикации.</span><span class="sxs-lookup"><span data-stu-id="5b95c-156">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="5b95c-157">Как правило файлы Razor не требуются для опубликованного приложения, если они участвуют в компиляции во время сборки или во время публикации.</span><span class="sxs-lookup"><span data-stu-id="5b95c-157">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="5b95c-158">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-158">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="5b95c-159">При значении `true` копирует элементы базовой сборки в каталог публикации.</span><span class="sxs-lookup"><span data-stu-id="5b95c-159">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="5b95c-160">Как правило ссылочные сборки не являются обязательными для опубликованного приложения случае во время сборки или публикации во время компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-160">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="5b95c-161">Значение `true` Если опубликованного приложения требует компиляции во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="5b95c-161">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="5b95c-162">Например, значение равно `true` Если приложение изменяет *.cshtml* файлы во время выполнения или использует внедренные представления.</span><span class="sxs-lookup"><span data-stu-id="5b95c-162">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="5b95c-163">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-163">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="5b95c-164">Когда `true`, все элементы содержимого Razor ( *.cshtml* файлы) отмечены для включения в создаваемый пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="5b95c-164">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="5b95c-165">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-165">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="5b95c-166">При значении `true` добавляет элементы RazorGenerate ( *.cshtml*) в виде внедренных файлов к создаваемой сборке Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-166">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="5b95c-167">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-167">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="5b95c-168">При значении `true` использует серверный процесс постоянной сборки для разгрузки работы по созданию кода.</span><span class="sxs-lookup"><span data-stu-id="5b95c-168">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="5b95c-169">По умолчанию используется значение `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="5b95c-169">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="5b95c-170">Дополнительные сведения о свойствах см. в статье [MSBuild Properties](/visualstudio/msbuild/msbuild-properties) (Свойства MSBuild).</span><span class="sxs-lookup"><span data-stu-id="5b95c-170">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="5b95c-171">Целевые объекты</span><span class="sxs-lookup"><span data-stu-id="5b95c-171">Targets</span></span>

<span data-ttu-id="5b95c-172">Пакет SDK для Razor определяет два основных целевых объекта:</span><span class="sxs-lookup"><span data-stu-id="5b95c-172">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="5b95c-173">`RazorGenerate` &ndash; Код создает *.cs* файлов из `RazorGenerate` элементы item.</span><span class="sxs-lookup"><span data-stu-id="5b95c-173">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="5b95c-174">Используйте свойство `RazorGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="5b95c-174">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="5b95c-175">`RazorCompile` &ndash; Компилирует созданный *.cs* файлы в сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="5b95c-175">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="5b95c-176">Используйте `RazorCompileDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="5b95c-176">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="5b95c-177">Компиляция среды выполнения представлений Razor</span><span class="sxs-lookup"><span data-stu-id="5b95c-177">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="5b95c-178">По умолчанию пакет SDK для Razor не публикует базовые сборки, необходимые для компиляции среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="5b95c-178">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="5b95c-179">Это приведет к сбою компиляции, если модель приложения зависит от компиляции среды выполнения &mdash; например, приложение использует внедренные представления или меняет представления после публикации.</span><span class="sxs-lookup"><span data-stu-id="5b95c-179">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="5b95c-180">Установите для `CopyRefAssembliesToPublishDirectory` значение `true`, чтобы продолжить публикацию базовых сборок.</span><span class="sxs-lookup"><span data-stu-id="5b95c-180">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="5b95c-181">Для веб-приложения, убедитесь в приложение предназначено для `Microsoft.NET.Sdk.Web` пакета SDK.</span><span class="sxs-lookup"><span data-stu-id="5b95c-181">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="5b95c-182">Версия языка Razor</span><span class="sxs-lookup"><span data-stu-id="5b95c-182">Razor language version</span></span>

<span data-ttu-id="5b95c-183">При нацеливании `Microsoft.NET.Sdk.Web` SDK, версию языка Razor выводится из приложения целевой версии платформы.</span><span class="sxs-lookup"><span data-stu-id="5b95c-183">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="5b95c-184">Для проектов, предназначенных для `Microsoft.NET.Sdk.Razor` SDK или версии в тех редких случаях, что приложению требуется другой версии языка Razor выводимого значения, можно настроить, задав `<RazorLangVersion>` свойство в файле проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="5b95c-184">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

## <a name="additional-resources"></a><span data-ttu-id="5b95c-185">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5b95c-185">Additional resources</span></span>

* [<span data-ttu-id="5b95c-186">Дополнения к формату CSPROJ для .NET Core</span><span class="sxs-lookup"><span data-stu-id="5b95c-186">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="5b95c-187">Общие элементы проектов MSBuild</span><span class="sxs-lookup"><span data-stu-id="5b95c-187">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
