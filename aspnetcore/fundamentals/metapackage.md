---
title: Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.x и более поздних версий
author: Rick-Anderson
description: Метапакет Microsoft.AspNetCore.All включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core, а также их зависимости.
manager: wpickett
monikerRange: = aspnetcore-2.0
ms.author: riande
ms.date: 09/20/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/metapackage
ms.openlocfilehash: 4c11f15e659565325bfe8b8d91188b62177b251d
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="b87e6-103">Метапакет Microsoft.AspNetCore.All для ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b87e6-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="b87e6-104">Для этой функции нужен ASP.NET Core 2.x, нацеленный на .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="b87e6-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="b87e6-105">Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:</span><span class="sxs-lookup"><span data-stu-id="b87e6-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="b87e6-106">все пакеты, поддерживаемые командой ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="b87e6-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="b87e6-107">все пакеты, поддерживаемые Entity Framework Core;</span><span class="sxs-lookup"><span data-stu-id="b87e6-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="b87e6-108">внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="b87e6-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="b87e6-109">В пакет `Microsoft.AspNetCore.All` входят все компоненты ASP.NET Core 2.x и Entity Framework Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="b87e6-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="b87e6-110">Этот пакет по умолчанию используется для шаблонов проектов, предназначенных для ASP.NET Core 2.0.</span><span class="sxs-lookup"><span data-stu-id="b87e6-110">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="b87e6-111">Номер версии метапакета `Microsoft.AspNetCore.All` представляет версию ASP.NET Core и версию Entity Framework Core (соответствующую версии .NET Core).</span><span class="sxs-lookup"><span data-stu-id="b87e6-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="b87e6-112">Приложения, использующие метапакет `Microsoft.AspNetCore.All`, автоматически получают все преимущества [хранилища среды выполнения .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="b87e6-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="b87e6-113">Это хранилище содержит все ресурсы среды выполнения, необходимые для запуска приложений ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="b87e6-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="b87e6-114">При использовании метапакета `Microsoft.AspNetCore.All` с приложением не развертываются **никакие** ресурсы из указанных по ссылке пакетов NuGet ASP.NET Core &mdash;, хранилище среды выполнения .NET Core уже содержит эти ресурсы.</span><span class="sxs-lookup"><span data-stu-id="b87e6-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="b87e6-115">Для сокращения времени запуска приложения ресурсы в хранилище среды выполнения подвергаются предварительной компиляции.</span><span class="sxs-lookup"><span data-stu-id="b87e6-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="b87e6-116">Для удаления неиспользуемых пакетов можно воспользоваться процессом усечения пакета.</span><span class="sxs-lookup"><span data-stu-id="b87e6-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="b87e6-117">Усеченные пакеты исключаются из выходных данных опубликованного приложения.</span><span class="sxs-lookup"><span data-stu-id="b87e6-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="b87e6-118">Следующий файл *CSPROJ* ссылается на метапакет `Microsoft.AspNetCore.All` для ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="b87e6-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](../mvc/views/view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=9)]
