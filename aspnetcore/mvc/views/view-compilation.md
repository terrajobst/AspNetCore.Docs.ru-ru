---
title: Компиляция и предварительная компиляция файлов Razor в ASP.NET Core
author: rick-anderson
description: Узнайте о преимуществах предварительной компиляции файлов Razor и о том, как это сделать в приложении ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/17/2018
uid: mvc/views/view-compilation
ms.openlocfilehash: 9355d467ca819ea8c6292963b31367ad5ca36d55
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938541"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="d7744-103">Компиляция файлов Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7744-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="d7744-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="d7744-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="d7744-105">Файл Razor компилируется в среде выполнения при вызове связанного представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d7744-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="d7744-106">Публикация файла Razor во время сборки не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="d7744-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="d7744-107">При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="d7744-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="d7744-108">Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d7744-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="d7744-109">Публикация файла Razor во время сборки не поддерживается.</span><span class="sxs-lookup"><span data-stu-id="d7744-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="d7744-110">При необходимости файлы Razor можно компилировать во время публикации и развертывать вместе с приложением &mdash; для этого используется средство предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="d7744-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d7744-111">Файл Razor компилируется в среде выполнения при вызове связанной страницы Razor или представления MVC.</span><span class="sxs-lookup"><span data-stu-id="d7744-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="d7744-112">Файлы Razor компилируются и во время сборки, и во время публикации с помощью [пакета SDK для Razor](xref:razor-pages/sdk).</span><span class="sxs-lookup"><span data-stu-id="d7744-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>
::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="d7744-113">Особенности предварительной компиляции</span><span class="sxs-lookup"><span data-stu-id="d7744-113">Precompilation considerations</span></span>

<span data-ttu-id="d7744-114">Ниже перечислены побочные эффекты предварительной компиляции файлов Razor:</span><span class="sxs-lookup"><span data-stu-id="d7744-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="d7744-115">уменьшение размера публикуемого пакета;</span><span class="sxs-lookup"><span data-stu-id="d7744-115">A smaller published bundle</span></span>
* <span data-ttu-id="d7744-116">уменьшение времени запуска сервера;</span><span class="sxs-lookup"><span data-stu-id="d7744-116">A faster startup time</span></span>
* <span data-ttu-id="d7744-117">отсутствие возможности редактирования файлов Razor по причине отсутствия связанного содержимого в публикуемом пакете.</span><span class="sxs-lookup"><span data-stu-id="d7744-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="d7744-118">Развертывание предварительно скомпилированных файлов</span><span class="sxs-lookup"><span data-stu-id="d7744-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="d7744-119">Компиляция файлов Razor во время сборки и публикации включена по умолчанию с помощью пакета SDK для Razor.</span><span class="sxs-lookup"><span data-stu-id="d7744-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="d7744-120">Редактирование файлов Razor после их обновления поддерживается во время сборки.</span><span class="sxs-lookup"><span data-stu-id="d7744-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="d7744-121">По умолчанию только скомпилированный файл *Views.dll*, а не *CSHTML*-файлы, развертываются вместе с приложением.</span><span class="sxs-lookup"><span data-stu-id="d7744-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7744-122">Пакет SDK для Razor применяется только в том случае, если в файле проекта не заданы свойства предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="d7744-122">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="d7744-123">Например, если в *CSPROJ*-файле для свойства `MvcRazorCompileOnPublish` установлено значение `true`, пакет SDK для Razor будет отключен.</span><span class="sxs-lookup"><span data-stu-id="d7744-123">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-2.0"
<span data-ttu-id="d7744-124">Если проект ориентирован на платформу .NET Framework, установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="d7744-124">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="d7744-125">Если проект нацелен на .NET Core, никаких изменений не требуется.</span><span class="sxs-lookup"><span data-stu-id="d7744-125">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="d7744-126">В шаблонах проектов ASP.NET Core 2.x для свойства `MvcRazorCompileOnPublish` по умолчанию неявно задано значение `true`.</span><span class="sxs-lookup"><span data-stu-id="d7744-126">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="d7744-127">Следовательно, этот элемент можно безопасно удалить из *CSPROJ*-файла.</span><span class="sxs-lookup"><span data-stu-id="d7744-127">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d7744-128">Предварительная компиляция файлов Razor недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="d7744-128">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>
::: moniker-end

::: moniker range="= aspnetcore-1.1"
<span data-ttu-id="d7744-129">Задайте для свойства `MvcRazorCompileOnPublish` значение `true`и установите пакет NuGet [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/).</span><span class="sxs-lookup"><span data-stu-id="d7744-129">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="d7744-130">Это показано в следующем примере *CSPROJ*:</span><span class="sxs-lookup"><span data-stu-id="d7744-130">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]
::: moniker-end

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="d7744-131">Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="d7744-131">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="d7744-132">Например, выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="d7744-132">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="d7744-133">В случае успешного выполнения компиляции создается файл *<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные файлы Razor.</span><span class="sxs-lookup"><span data-stu-id="d7744-133">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="d7744-134">Например, следующий снимок экрана показывает содержимое файла *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="d7744-134">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)
::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="d7744-136">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d7744-136">Additional resources</span></span>

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
