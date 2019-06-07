---
title: Пакет SDK для Razor в ASP.NET Core
author: Rick-Anderson
description: Сведения о применении в ASP.NET Core функции Razor Pages, которая делает создание кодов сценариев для страниц проще и эффективнее по сравнению с MVC.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 06/05/2019
uid: razor-pages/sdk
ms.openlocfilehash: 8c4e882af93b043afaa0bcf86fd1583405f84be9
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750179"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="4f67c-103">Пакет SDK для Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f67c-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="4f67c-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="4f67c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="4f67c-105">[!INCLUDE[](~/includes/2.1-SDK.md)] Включает в себя `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span><span class="sxs-lookup"><span data-stu-id="4f67c-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="4f67c-106">Пакет SDK для Razor:</span><span class="sxs-lookup"><span data-stu-id="4f67c-106">The Razor SDK:</span></span>

* <span data-ttu-id="4f67c-107">Стандартизирует процесс создания, упаковки и публикации проектов, содержащих файлы [Razor](xref:mvc/views/razor), для проектов на основе MVC ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f67c-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="4f67c-108">Включает набор предопределенных целевых объектов, свойств и элементов, которые позволяют настраивать параметры компиляции файлов Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f67c-109">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4f67c-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="4f67c-110">Использование пакета SDK для Razor</span><span class="sxs-lookup"><span data-stu-id="4f67c-110">Using the Razor SDK</span></span>

<span data-ttu-id="4f67c-111">Большинство веб-приложений не требуется явно ссылаться на пакет SDK Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-111">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="4f67c-112">Использование пакета SDK для Razor для создания библиотеки классов с представлениями Razor или страницами Razor:</span><span class="sxs-lookup"><span data-stu-id="4f67c-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="4f67c-113">Используйте `Microsoft.NET.Sdk.Razor` вместо `Microsoft.NET.Sdk`:</span><span class="sxs-lookup"><span data-stu-id="4f67c-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* <span data-ttu-id="4f67c-114">Как правило, ссылки на пакет `Microsoft.AspNetCore.Mvc` требуется для получения дополнительные зависимости, необходимые для создания и компиляции Razor Pages и представлениями Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-114">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="4f67c-115">Как минимум проект следует добавить ссылки на пакеты для:</span><span class="sxs-lookup"><span data-stu-id="4f67c-115">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="4f67c-116">`Microsoft.AspNetCore.Razor.Design` Пакет содержит задачи компиляции Razor и целевых объектов для проекта.</span><span class="sxs-lookup"><span data-stu-id="4f67c-116">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="4f67c-117">Предыдущие пакеты включены в `Microsoft.AspNetCore.Mvc`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-117">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="4f67c-118">В следующей разметке показан файл проекта, который использует пакет SDK для Razor для создания файлов Razor для приложения ASP.NET Core Razor Pages:</span><span class="sxs-lookup"><span data-stu-id="4f67c-118">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="4f67c-119">`Microsoft.AspNetCore.Razor.Design` И `Microsoft.AspNetCore.Mvc.Razor.Extensions` пакеты входят в [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app).</span><span class="sxs-lookup"><span data-stu-id="4f67c-119">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="4f67c-120">Тем не менее тем меньше версии `Microsoft.AspNetCore.App` ссылки на пакет предоставляет метапакет к приложению, которое не включает последнюю версию `Microsoft.AspNetCore.Razor.Design`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-120">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="4f67c-121">Проекты должны ссылаться на согласованность версий `Microsoft.AspNetCore.Razor.Design` (или `Microsoft.AspNetCore.Mvc`), чтобы включаются последние исправления во время сборки для Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-121">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="4f67c-122">Дополнительные сведения см. в разделе [проблема GitHub](https://github.com/aspnet/Razor/issues/2553).</span><span class="sxs-lookup"><span data-stu-id="4f67c-122">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="4f67c-123">Свойства</span><span class="sxs-lookup"><span data-stu-id="4f67c-123">Properties</span></span>

<span data-ttu-id="4f67c-124">Следующие свойства управляют поведением пакета SDK для Razor в ходе создания проекта:</span><span class="sxs-lookup"><span data-stu-id="4f67c-124">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="4f67c-125">`RazorCompileOnBuild` &ndash; Когда `true`, компилирует и сборка Razor как часть сборки проекта.</span><span class="sxs-lookup"><span data-stu-id="4f67c-125">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="4f67c-126">По умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-126">Defaults to `true`.</span></span>
* <span data-ttu-id="4f67c-127">`RazorCompileOnPublish` &ndash; Когда `true`, компиляция и сборка Razor как часть публикации проекта.</span><span class="sxs-lookup"><span data-stu-id="4f67c-127">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="4f67c-128">По умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-128">Defaults to `true`.</span></span>

<span data-ttu-id="4f67c-129">Свойства и элементы в следующей таблице используются для настройки входных и выходных данных в пакет SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-129">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="4f67c-130">Элементы</span><span class="sxs-lookup"><span data-stu-id="4f67c-130">Items</span></span> | <span data-ttu-id="4f67c-131">Описание</span><span class="sxs-lookup"><span data-stu-id="4f67c-131">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="4f67c-132">Элементы (файлы *.cshtml*), которые являются входными данными для целей создания кода.</span><span class="sxs-lookup"><span data-stu-id="4f67c-132">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="4f67c-133">Элементы Item ( *.cs* файлы), являются входными значениями для целевых сред компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-133">Item elements (*.cs* files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="4f67c-134">Используйте этот элемент ItemGroup, чтобы указать дополнительные файлы для компиляции в сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-134">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="4f67c-135">Элементы, используемые для создания атрибутов для сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-135">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="4f67c-136">Пример:</span><span class="sxs-lookup"><span data-stu-id="4f67c-136">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="4f67c-137">Элемент элементы добавляются в виде внедренных ресурсов на созданную сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-137">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="4f67c-138">Свойство</span><span class="sxs-lookup"><span data-stu-id="4f67c-138">Property</span></span> | <span data-ttu-id="4f67c-139">Описание</span><span class="sxs-lookup"><span data-stu-id="4f67c-139">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="4f67c-140">Имя файла (без расширения) для сборки, созданной Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="4f67c-141">Выходной каталог Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-141">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="4f67c-142">Используется для определения набора инструментов для построения сборки Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-142">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="4f67c-143">Допустимые значения: `Implicit`, `RazorSDK` и `PrecompilationTool`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-143">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="4f67c-144">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="4f67c-144">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="4f67c-145">Значение по умолчанию — `true`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-145">Default is `true`.</span></span> <span data-ttu-id="4f67c-146">Когда `true`, включает в себя *web.config*, *.json*, и *.cshtml* файлы содержимого в проекте.</span><span class="sxs-lookup"><span data-stu-id="4f67c-146">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="4f67c-147">Когда я ссылаюсь через `Microsoft.NET.Sdk.Web`, файлы в разделе *wwwroot* и файлы конфигурации, также будут включены.</span><span class="sxs-lookup"><span data-stu-id="4f67c-147">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="4f67c-148">При значении `true` включает файлы *.cshtml* из элементов `Content` в элементы `RazorGenerate`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-148">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="4f67c-149">Когда `true`, приводит к возникновению ошибки *.cs* файл, содержащий атрибуты, указанные фильтром `RazorAssemblyAttribute` и файл в выходных данных компиляции.</span><span class="sxs-lookup"><span data-stu-id="4f67c-149">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="4f67c-150">При значении `true` добавляет стандартный набор атрибутов сборки в `RazorAssemblyAttribute`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-150">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="4f67c-151">Когда `true`, копии `RazorGenerate` элементы ( *.cshtml*) файлы в каталог публикации.</span><span class="sxs-lookup"><span data-stu-id="4f67c-151">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="4f67c-152">Как правило файлы Razor не требуются для опубликованного приложения, если они участвуют в компиляции во время сборки или во время публикации.</span><span class="sxs-lookup"><span data-stu-id="4f67c-152">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="4f67c-153">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-153">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="4f67c-154">При значении `true` копирует элементы базовой сборки в каталог публикации.</span><span class="sxs-lookup"><span data-stu-id="4f67c-154">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="4f67c-155">Как правило ссылочные сборки не являются обязательными для опубликованного приложения случае во время сборки или публикации во время компиляции Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-155">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="4f67c-156">Значение `true` Если опубликованного приложения требует компиляции во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="4f67c-156">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="4f67c-157">Например, значение равно `true` Если приложение изменяет *.cshtml* файлы во время выполнения или использует внедренные представления.</span><span class="sxs-lookup"><span data-stu-id="4f67c-157">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="4f67c-158">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-158">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="4f67c-159">Когда `true`, все элементы содержимого Razor ( *.cshtml* файлы) отмечены для включения в создаваемый пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="4f67c-159">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="4f67c-160">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-160">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="4f67c-161">При значении `true` добавляет элементы RazorGenerate ( *.cshtml*) в виде внедренных файлов к создаваемой сборке Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-161">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="4f67c-162">По умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-162">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="4f67c-163">При значении `true` использует серверный процесс постоянной сборки для разгрузки работы по созданию кода.</span><span class="sxs-lookup"><span data-stu-id="4f67c-163">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="4f67c-164">По умолчанию используется значение `UseSharedCompilation`.</span><span class="sxs-lookup"><span data-stu-id="4f67c-164">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="4f67c-165">Дополнительные сведения о свойствах см. в статье [MSBuild Properties](/visualstudio/msbuild/msbuild-properties) (Свойства MSBuild).</span><span class="sxs-lookup"><span data-stu-id="4f67c-165">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="4f67c-166">Целевые объекты</span><span class="sxs-lookup"><span data-stu-id="4f67c-166">Targets</span></span>

<span data-ttu-id="4f67c-167">Пакет SDK для Razor определяет два основных целевых объекта:</span><span class="sxs-lookup"><span data-stu-id="4f67c-167">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="4f67c-168">`RazorGenerate` &ndash; Код создает *.cs* файлов из `RazorGenerate` элементы item.</span><span class="sxs-lookup"><span data-stu-id="4f67c-168">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="4f67c-169">Используйте свойство `RazorGenerateDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="4f67c-169">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="4f67c-170">`RazorCompile` &ndash; Компилирует созданный *.cs* файлы в сборку Razor.</span><span class="sxs-lookup"><span data-stu-id="4f67c-170">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="4f67c-171">Используйте `RazorCompileDependsOn`, чтобы указать дополнительные целевые объекты, которые могут выполняться до или после этого целевого объекта.</span><span class="sxs-lookup"><span data-stu-id="4f67c-171">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="4f67c-172">Компиляция среды выполнения представлений Razor</span><span class="sxs-lookup"><span data-stu-id="4f67c-172">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="4f67c-173">По умолчанию пакет SDK для Razor не публикует базовые сборки, необходимые для компиляции среды выполнения.</span><span class="sxs-lookup"><span data-stu-id="4f67c-173">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="4f67c-174">Это приведет к сбою компиляции, если модель приложения зависит от компиляции среды выполнения &mdash; например, приложение использует внедренные представления или меняет представления после публикации.</span><span class="sxs-lookup"><span data-stu-id="4f67c-174">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="4f67c-175">Установите для `CopyRefAssembliesToPublishDirectory` значение `true`, чтобы продолжить публикацию базовых сборок.</span><span class="sxs-lookup"><span data-stu-id="4f67c-175">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="4f67c-176">Для веб-приложения, убедитесь в приложение предназначено для `Microsoft.NET.Sdk.Web` пакета SDK.</span><span class="sxs-lookup"><span data-stu-id="4f67c-176">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="4f67c-177">Версия языка Razor</span><span class="sxs-lookup"><span data-stu-id="4f67c-177">Razor language version</span></span>

<span data-ttu-id="4f67c-178">При нацеливании `Microsoft.NET.Sdk.Web` SDK, версию языка Razor выводится из приложения целевой версии платформы.</span><span class="sxs-lookup"><span data-stu-id="4f67c-178">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="4f67c-179">Для проектов, предназначенных для `Microsoft.NET.Sdk.Razor` SDK или версии в тех редких случаях, что приложению требуется другой версии языка Razor выводимого значения, можно настроить, задав `<RazorLangVersion>` свойство в файле проекта приложения:</span><span class="sxs-lookup"><span data-stu-id="4f67c-179">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```
