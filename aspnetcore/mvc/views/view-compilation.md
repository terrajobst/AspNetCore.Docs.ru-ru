---
title: Компиляция и предварительная компиляция представлений Razor в ASP.NET Core
author: rick-anderson
description: Узнайте, как включить компиляцию и предварительную компиляцию представлений MVC Razor в приложениях ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="f95b4-103">Компиляция и предварительная компиляция представлений Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f95b4-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="f95b4-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="f95b4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f95b4-105">Представления Razor компилируются во время выполнения при вызове представления.</span><span class="sxs-lookup"><span data-stu-id="f95b4-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="f95b4-106">ASP.NET Core 1.1.0 и более поздние версии позволяют при необходимости компилировать представления Razor отдельно и развертывать их с приложением &mdash; (процесс, называемый "предварительной компиляцией").</span><span class="sxs-lookup"><span data-stu-id="f95b4-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="f95b4-107">Шаблоны проектов ASP.NET Core 2.x поддерживают предварительную компиляцию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f95b4-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f95b4-108">Предварительная компиляция представлений Razor сейчас недоступна при выполнении [автономного развертывания](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="f95b4-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="f95b4-109">Эта функция станет доступна для автономных развертываний в версии 2.1.</span><span class="sxs-lookup"><span data-stu-id="f95b4-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="f95b4-110">Дополнительные сведения: [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102) (Сбой компиляции представления при перекрестной компиляции для Linux в Windows).</span><span class="sxs-lookup"><span data-stu-id="f95b4-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="f95b4-111">Особенности предварительной компиляции:</span><span class="sxs-lookup"><span data-stu-id="f95b4-111">Precompilation considerations:</span></span>

* <span data-ttu-id="f95b4-112">Предварительная компиляция представлений уменьшает размер публикуемого пакета и сокращает время запуска.</span><span class="sxs-lookup"><span data-stu-id="f95b4-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="f95b4-113">После предварительной компиляции представлений редактировать файлы Razor невозможно.</span><span class="sxs-lookup"><span data-stu-id="f95b4-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="f95b4-114">Отредактированные представления не попадут в опубликованный пакет.</span><span class="sxs-lookup"><span data-stu-id="f95b4-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="f95b4-115">Развертывание предварительно скомпилированных представлений осуществляется следующим образом.</span><span class="sxs-lookup"><span data-stu-id="f95b4-115">To deploy precompiled views:</span></span>

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="f95b4-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="f95b4-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)
<span data-ttu-id="f95b4-117">Если проект нацелен на платформу .NET Framework, включите ссылку на пакет в [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="f95b4-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="f95b4-118">Если проект нацелен на .NET Core, никаких изменений не требуется.</span><span class="sxs-lookup"><span data-stu-id="f95b4-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="f95b4-119">Шаблоны проектов ASP.NET Core 2.x неявно задают свойству `MvcRazorCompileOnPublish` значение `true` по умолчанию. Это означает, что этот узел можно безопасно удалить из файла *CSPROJ*.</span><span class="sxs-lookup"><span data-stu-id="f95b4-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="f95b4-120">Если вы предпочитаете работать явным образом, задание свойству `MvcRazorCompileOnPublish` значения `true` не принесет никакого вреда.</span><span class="sxs-lookup"><span data-stu-id="f95b4-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="f95b4-121">Это показано в следующем примере *CSPROJ*:</span><span class="sxs-lookup"><span data-stu-id="f95b4-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="f95b4-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="f95b4-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)
<span data-ttu-id="f95b4-123">Задайте свойству `MvcRazorCompileOnPublish` значение `true` и включите ссылку на пакет в `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="f95b4-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="f95b4-124">Это показано в следующем примере *CSPROJ*:</span><span class="sxs-lookup"><span data-stu-id="f95b4-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

<span data-ttu-id="f95b4-125">Подготовьте приложение к [развертыванию в зависимости от платформы](/dotnet/core/deploying/#framework-dependent-deployments-fdd) с помощью [команды публикации .NET Core CLI](/dotnet/core/tools/dotnet-publish).</span><span class="sxs-lookup"><span data-stu-id="f95b4-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="f95b4-126">Например, выполните следующую команду в корневом элементе проекта:</span><span class="sxs-lookup"><span data-stu-id="f95b4-126">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="f95b4-127">При успешной компиляции создается файл *<имя_проекта>.PrecompiledViews.dll*, содержащий скомпилированные представления Razor.</span><span class="sxs-lookup"><span data-stu-id="f95b4-127">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="f95b4-128">Например, следующий снимок экрана показывает содержимое *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="f95b4-128">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Представления Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)
