---
title: Компиляция и предварительная компиляция файлов Razor в ASP.NET Core
author: rick-anderson
description: Узнайте о преимуществах предварительной компиляции файлов Razor и о том, как это сделать в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248190"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="f6526-103">Компиляция файлов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f6526-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="f6526-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f6526-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="f6526-105">Файл Razor компилируется в среде выполнения при вызове связанного представления MVC.</span><span class="sxs-lookup"><span data-stu-id="f6526-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="f6526-106">Публикация файла Razor во время сборки не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="f6526-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="f6526-107">При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="f6526-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f6526-108">Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC.</span><span class="sxs-lookup"><span data-stu-id="f6526-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="f6526-109">Публикация файла Razor во время сборки не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="f6526-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="f6526-110">При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="f6526-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f6526-111">Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC.</span><span class="sxs-lookup"><span data-stu-id="f6526-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="f6526-112">Файлы Razor компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f6526-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="f6526-113">Особенности предварительной компиляции</span><span class="sxs-lookup"><span data-stu-id="f6526-113">Precompilation considerations</span></span>

<span data-ttu-id="f6526-114">Ниже перечислены побочные эффекты предварительной компиляции файлов Razor:</span><span class="sxs-lookup"><span data-stu-id="f6526-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="f6526-115">уменьшение размера публикуемого пакета;</span><span class="sxs-lookup"><span data-stu-id="f6526-115">A smaller published bundle</span></span>
* <span data-ttu-id="f6526-116">уменьшение времени запуска сервера;</span><span class="sxs-lookup"><span data-stu-id="f6526-116">A faster startup time</span></span>
* <span data-ttu-id="f6526-117">отсутствие возможности редактирования файлов Razor по причине отсутствия связанного содержимого в публикуемом пакете.</span><span class="sxs-lookup"><span data-stu-id="f6526-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="f6526-118">Развертывание предварительно скомпилированных файлов</span><span class="sxs-lookup"><span data-stu-id="f6526-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="f6526-119">Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="f6526-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="f6526-120">Редактирование файлов Razor после их обновления поддерживается во время сборки.</span><span class="sxs-lookup"><span data-stu-id="f6526-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="f6526-121">По умолчанию только скомпилированный файл *Views.dll*, а не *CSHTML*-файлы, развертываются вместе с приложением.</span><span class="sxs-lookup"><span data-stu-id="f6526-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6526-122">Средство предварительной компиляции будет удалено в ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="f6526-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="f6526-123">Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f6526-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="f6526-124">Пакет SDK для Razor применяется только в том случае, если в файле проекта не заданы свойства предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="f6526-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="f6526-125">Например, если в *CSPROJ*-файле для свойства `MvcRazorCompileOnPublish` установлено значение `true`, пакет SDK для Razor будет отключен.</span><span class="sxs-lookup"><span data-stu-id="f6526-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="f6526-126">Если проект ориентирован на платформу .NET Framework, установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="f6526-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="f6526-127">Если проект нацелен на .NET Core, никаких изменений не требуется.</span><span class="sxs-lookup"><span data-stu-id="f6526-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="f6526-128">В шаблонах проектов ASP.NET Core 2.x для свойства `MvcRazorCompileOnPublish` по умолчанию неявно задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="f6526-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="f6526-129">Следовательно, этот элемент можно безопасно удалить из *CSPROJ*-файла.</span><span class="sxs-lookup"><span data-stu-id="f6526-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f6526-130">Средство предварительной компиляции будет удалено в ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="f6526-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="f6526-131">Мы советуем перейти на [пакет SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="f6526-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="f6526-132">Предварительная компиляция файлов Razor недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f6526-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="f6526-133">Задайте для свойства `MvcRazorCompileOnPublish` значение `true`и установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="f6526-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="f6526-134">Это показано в следующем примере *CSPROJ*:</span><span class="sxs-lookup"><span data-stu-id="f6526-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="f6526-135">Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="f6526-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="f6526-136">Например, выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="f6526-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="f6526-137">В случае успешного выполнения компиляции создается файл *<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные файлы Razor.</span><span class="sxs-lookup"><span data-stu-id="f6526-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="f6526-138">Например, следующий снимок экрана показывает содержимое файла *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="f6526-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a><span data-ttu-id="f6526-140">Перекомпилирование файлов Razor при изменении</span><span class="sxs-lookup"><span data-stu-id="f6526-140">Recompile Razor files on change</span></span>

<span data-ttu-id="f6526-141"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> возвращает или задает значение, определяющее, выполняется ли повторная компиляция и обновление файлов Razor (представления Razor и Razor Pages) при изменении файлов на диске.</span><span class="sxs-lookup"><span data-stu-id="f6526-141">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="f6526-142">Если задано значение `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) отслеживает изменения в файлах Razor в настроенных экземплярах <xref:Microsoft.Extensions.FileProviders.IFileProvider>.</span><span class="sxs-lookup"><span data-stu-id="f6526-142">When set to `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) watches for changes to Razor files in configured <xref:Microsoft.Extensions.FileProviders.IFileProvider> instances.</span></span>

<span data-ttu-id="f6526-143">Значение по умолчанию — `true` для:</span><span class="sxs-lookup"><span data-stu-id="f6526-143">The default value is `true` for:</span></span>

* <span data-ttu-id="f6526-144">приложений ASP.NET Core 2.1 или более ранней версии;</span><span class="sxs-lookup"><span data-stu-id="f6526-144">ASP.NET Core 2.1 or earlier apps.</span></span>
* <span data-ttu-id="f6526-145">приложений ASP.NET Core 2.2 или более поздней версии в среде разработки.</span><span class="sxs-lookup"><span data-stu-id="f6526-145">ASP.NET Core 2.2 or later apps in the Development environment.</span></span>

<span data-ttu-id="f6526-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> связан с параметром совместимости и может вести себя по-разному в зависимости от настроенной версии совместимости для приложения.</span><span class="sxs-lookup"><span data-stu-id="f6526-146"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is associated with a compatibility switch and can provide different behavior depending on the configured compatibility version for the app.</span></span> <span data-ttu-id="f6526-147">Настройте приложения, задав для <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> приоритет над значением, которое содержится в версии совместимости приложения.</span><span class="sxs-lookup"><span data-stu-id="f6526-147">Configuring the app by setting <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> takes precedence over the value implied by the app's compatibility version.</span></span>

<span data-ttu-id="f6526-148">Если версия совместимости приложения имеет значение <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> или более ранее, то для <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> задается значение `true` (если не задано явно).</span><span class="sxs-lookup"><span data-stu-id="f6526-148">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `true` unless explicitly configured.</span></span>

<span data-ttu-id="f6526-149">Если версия совместимости приложения имеет значение <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> или более позднее, то для <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> задается значение `false` (если не используется среда разработки или значение не задано явным образом).</span><span class="sxs-lookup"><span data-stu-id="f6526-149">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later, <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is set to `false` unless the environment is Development or the value is explicitly configured.</span></span>

<span data-ttu-id="f6526-150">Рекомендации и примеры настройки версии совместимости приложения см. в статье <xref:mvc/compatibility-version>.</span><span class="sxs-lookup"><span data-stu-id="f6526-150">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f6526-151">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f6526-151">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
