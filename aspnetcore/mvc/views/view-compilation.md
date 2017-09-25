---
title: "Компиляция представления Razor и предварительной компиляции"
author: rick-anderson
description: "Ссылки документов о том, как включить компиляции представления MVC Razor и предварительной компиляции в приложениях ASP.NET Core."
keywords: "ASP.NET Core компиляции представления Razor, предварительная компиляция Razor, предварительной компиляции Razor"
ms.author: riande
manager: wpickett
ms.date: 08/16/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: bfee2e5e8f71c99465be79589a77f0e173097b23
ms.sourcegitcommit: 8005eb4051e568d88ee58d48424f39916052e6e2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/24/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a><span data-ttu-id="e829c-104">Компиляция представления Razor и предварительной компиляции в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e829c-104">Razor view compilation and precompilation in ASP.NET Core</span></span>

<span data-ttu-id="e829c-105">Автор: [Рик Андерсон](https://twitter.com/RickAndMSFT) (Rick Anderson)</span><span class="sxs-lookup"><span data-stu-id="e829c-105">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="e829c-106">Представлений Razor компилируются во время выполнения при вызове представления.</span><span class="sxs-lookup"><span data-stu-id="e829c-106">Razor views are compiled at runtime when the view is invoked.</span></span> <span data-ttu-id="e829c-107">ASP.NET Core 1.1.0 и более поздней версии можно при необходимости компиляции представлений Razor и развертываются с приложением &mdash; этот процесс называется предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="e829c-107">ASP.NET Core 1.1.0 and higher can optionally compile Razor views and deploy them with the app &mdash; a process known as precompilation.</span></span> <span data-ttu-id="e829c-108">Предварительная компиляция включает шаблоны проектов ASP.NET Core 2.x по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="e829c-108">The ASP.NET Core 2.x project templates enable precompilation by default.</span></span>

> [!NOTE]
> <span data-ttu-id="e829c-109">Предварительная компиляция представления Razor в настоящее время недоступна, при выполнении [автономное развертывание (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) в ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="e829c-109">Razor view precompilation is currently unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span> <span data-ttu-id="e829c-110">Компонент будет доступен для SCD, когда освобождает 2.1.</span><span class="sxs-lookup"><span data-stu-id="e829c-110">The feature will be available for SCDs when 2.1 releases.</span></span> <span data-ttu-id="e829c-111">Дополнительные сведения см. в разделе [представление компиляция завершается с ошибкой при перекрестной компиляции для Linux в Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span><span class="sxs-lookup"><span data-stu-id="e829c-111">For more information, see [View compilation fails when cross-compiling for Linux on Windows](https://github.com/aspnet/MvcPrecompilation/issues/102).</span></span>

<span data-ttu-id="e829c-112">Замечания по предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="e829c-112">Precompilation considerations:</span></span>

* <span data-ttu-id="e829c-113">Предкомпиляция представлений приведет к более мелкие опубликованный пакет и уменьшение времени запуска.</span><span class="sxs-lookup"><span data-stu-id="e829c-113">Precompiling views results in a smaller published bundle and faster startup time.</span></span>
* <span data-ttu-id="e829c-114">После предкомпиляция представлений не может редактировать файлы Razor.</span><span class="sxs-lookup"><span data-stu-id="e829c-114">You can't edit Razor files after you precompile views.</span></span> <span data-ttu-id="e829c-115">Редактируемого представления не будет присутствовать в опубликованный пакет.</span><span class="sxs-lookup"><span data-stu-id="e829c-115">The edited views won't be present in the published bundle.</span></span> 

<span data-ttu-id="e829c-116">Развертывание предварительно скомпилированного представления:</span><span class="sxs-lookup"><span data-stu-id="e829c-116">To deploy precompiled views:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="e829c-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="e829c-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

<span data-ttu-id="e829c-118">Если проект предназначен для платформы .NET Framework, включить ссылку пакета `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span><span class="sxs-lookup"><span data-stu-id="e829c-118">If your project targets .NET Framework, include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`:</span></span>

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

<span data-ttu-id="e829c-119">Если проект предназначен для .NET Core, изменения не требуются.</span><span class="sxs-lookup"><span data-stu-id="e829c-119">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="e829c-120">Шаблоны проектов ASP.NET Core 2.x неявно задать `MvcRazorCompileOnPublish` для `true` по умолчанию, что означает этот узел можно безопасно удалить из *.csproj* файла.</span><span class="sxs-lookup"><span data-stu-id="e829c-120">The ASP.NET Core 2.x project templates implicitly set `MvcRazorCompileOnPublish` to `true` by default, which means this node can be safely removed from the *.csproj* file.</span></span> <span data-ttu-id="e829c-121">При желании быть явными, нет никакого вреда параметр `MvcRazorCompileOnPublish` свойства `true`.</span><span class="sxs-lookup"><span data-stu-id="e829c-121">If you prefer to be explicit, there's no harm in setting the `MvcRazorCompileOnPublish` property to `true`.</span></span> <span data-ttu-id="e829c-122">Следующие *.csproj* образец иллюстрирует этот параметр:</span><span class="sxs-lookup"><span data-stu-id="e829c-122">The following *.csproj* sample highlights this setting:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="e829c-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="e829c-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

<span data-ttu-id="e829c-124">Задать `MvcRazorCompileOnPublish` для `true`и включать ссылку на пакет `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span><span class="sxs-lookup"><span data-stu-id="e829c-124">Set `MvcRazorCompileOnPublish` to `true`, and include a package reference to `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`.</span></span> <span data-ttu-id="e829c-125">Следующие *.csproj* образец иллюстрирует следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="e829c-125">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---
