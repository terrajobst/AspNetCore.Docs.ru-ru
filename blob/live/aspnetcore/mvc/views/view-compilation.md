---
title: "Компиляция представления Razor и предварительной компиляции"
author: rick-anderson
description: "Ссылки документов о том, как включить компиляции представления MVC Razor и предварительной компиляции в приложениях ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 12/13/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 87989455c2fb6b5a922c7fb6133aa3e8cef42c88
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="15ca7-103">Компиляция представления Razor и предварительной компиляции в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15ca7-103">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="15ca7-104">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="15ca7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="15ca7-105">Представлений Razor компилируются во время выполнения при вызове представления.</span><span class="sxs-lookup"><span data-stu-id="15ca7-105">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="15ca7-106">ASP.NET Core 1.1.0 и более поздней версии можно при необходимости компиляции представлений Razor и развертываются с приложением&mdash;этот процесс называется предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="15ca7-106">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app&mdash;a process known as precompilation.</span></span> <span data-ttu-id="15ca7-107">Предварительная компиляция включает шаблоны проектов ASP.NET Core 2.x по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="15ca7-107">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="15ca7-108">Предварительная компиляция представления Razor в настоящее время недоступна, при выполнении [автономное развертывание (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="15ca7-108">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="15ca7-109">Компонент будет доступен для SCD, когда освобождает 2.1.</span><span class="sxs-lookup"><span data-stu-id="15ca7-109">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="15ca7-110">Дополнительные сведения см. в разделе [представление компиляция завершается с ошибкой при перекрестной компиляции для Linux в Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="15ca7-110">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="15ca7-111">Замечания по предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="15ca7-111">Precompilation considerations:</span></span>

* <span data-ttu-id="15ca7-112">Предкомпиляция представлений приведет к более мелкие опубликованный пакет и уменьшение времени запуска.</span><span class="sxs-lookup"><span data-stu-id="15ca7-112">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="15ca7-113">После предкомпиляция представлений не может редактировать файлы Razor.</span><span class="sxs-lookup"><span data-stu-id="15ca7-113">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="15ca7-114">Редактируемого представления не будет присутствовать в опубликованный пакет.</span><span class="sxs-lookup"><span data-stu-id="15ca7-114">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="15ca7-115">Развертывание предварительно скомпилированного представления:</span><span class="sxs-lookup"><span data-stu-id="15ca7-115">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="15ca7-116">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="15ca7-116">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="15ca7-117">Если проект предназначен для платформы .NET Framework, включить ссылку пакета [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span><span class="sxs-lookup"><span data-stu-id="15ca7-117">If your project targets .NET Framework, include a package reference to [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="15ca7-118">Если проект предназначен для .NET Core, изменения не требуются.</span><span class="sxs-lookup"><span data-stu-id="15ca7-118">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="15ca7-119">Шаблоны проектов ASP.NET Core 2.x неявно задать `MvcRazorCompileOnPublish` для `true` по умолчанию, что означает этот узел можно безопасно удалить из *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="15ca7-119">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="15ca7-120">При желании быть явными, нет никакого вреда параметр `MvcRazorCompileOnPublish` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="15ca7-120">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="15ca7-121">Следующие *.csproj* образец иллюстрирует этот параметр:</span><span class="sxs-lookup"><span data-stu-id="15ca7-121">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="15ca7-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="15ca7-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="15ca7-123">Задать `MvcRazorCompileOnPublish` для `true`и включать ссылку на пакет `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="15ca7-123">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="15ca7-124">Следующие *.csproj* образец иллюстрирует следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="15ca7-124">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

<span data-ttu-id="15ca7-125">Подготовка приложения для [развертывания зависит от framework](/dotnet/core/deploying/#framework-dependent-deployments-fdd) , выполнив команду, подобную следующей, в корне проекта:</span><span class="sxs-lookup"><span data-stu-id="15ca7-125">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) by executing a command such as the following at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="15ca7-126">Объект *< имя_проекта >. PrecompiledViews.dll* файл, содержащий скомпилированный представлений Razor, создается при успешном завершении предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="15ca7-126">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor views, is produced when precompilation succeeds.</span></span> <span data-ttu-id="15ca7-127">Например, на следующем снимке экрана показано содержимое *Index.cshtml* внутри *WebApplication1.PrecompiledViews.dll*:</span><span class="sxs-lookup"><span data-stu-id="15ca7-127">For example, the screenshot below depicts the contents of *Index.cshtml* inside of *WebApplication1.PrecompiledViews.dll*:</span></span>

![Представлений Razor внутри библиотеки DLL](view-compilation/_static/razor-views-in-dll.png)
