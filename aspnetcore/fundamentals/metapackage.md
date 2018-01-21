---
title: "Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x и более поздних версий"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage включает все поддерживаемые пакеты ASP.NET Core и Entity Framework Core, вместе с их зависимости."
ms.author: riande
manager: wpickett
ms.date: 09/20/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 8a44ee7ebb7e6b0112000429f1f080bceb7dc895
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="fd50e-103">Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="fd50e-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="fd50e-104">Для этой функции требуется ASP.NET Core 2.x нацеливания .NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="fd50e-104">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="fd50e-105">Метапакет [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) для ASP.NET Core включает:</span><span class="sxs-lookup"><span data-stu-id="fd50e-105">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="fd50e-106">все пакеты, поддерживаемые командой ASP.NET Core;</span><span class="sxs-lookup"><span data-stu-id="fd50e-106">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="fd50e-107">все пакеты, поддерживаемые Entity Framework Core;</span><span class="sxs-lookup"><span data-stu-id="fd50e-107">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="fd50e-108">внутренние и сторонние зависимости, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="fd50e-108">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="fd50e-109">Все возможности ASP.NET Core 2.x и Entity Framework Core 2.x включаются в `Microsoft.AspNetCore.All` пакета.</span><span class="sxs-lookup"><span data-stu-id="fd50e-109">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="fd50e-110">Шаблоны проектов по умолчанию использовать этот пакет.</span><span class="sxs-lookup"><span data-stu-id="fd50e-110">The default project templates use this package.</span></span>

<span data-ttu-id="fd50e-111">Номер версии `Microsoft.AspNetCore.All` metapackage представляет версию ASP.NET Core и версии Entity Framework Core (выравнивание с версией .NET Core).</span><span class="sxs-lookup"><span data-stu-id="fd50e-111">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="fd50e-112">Приложения, использующие `Microsoft.AspNetCore.All` metapackage автоматически воспользоваться преимуществами [хранилище времени выполнения .NET Core](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span><span class="sxs-lookup"><span data-stu-id="fd50e-112">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](https://docs.microsoft.com/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="fd50e-113">Хранилище среда выполнения содержит все ресурсы среды выполнения, необходимые для запуска ASP.NET Core 2.x приложений.</span><span class="sxs-lookup"><span data-stu-id="fd50e-113">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="fd50e-114">При использовании `Microsoft.AspNetCore.All` metapackage, **не** активы из упоминаемой пакетов ASP.NET Core NuGet развертываются с приложением &mdash; хранилище среды выполнения .NET Core содержит эти ресурсы.</span><span class="sxs-lookup"><span data-stu-id="fd50e-114">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="fd50e-115">Ресурсы в хранилище среды выполнения компилируются для сокращения времени запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="fd50e-115">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="fd50e-116">Процесс усечения пакета можно использовать для удаления пакетов, которые не используются.</span><span class="sxs-lookup"><span data-stu-id="fd50e-116">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="fd50e-117">Обрезанная пакеты исключаются в выходных данных для опубликованного приложения.</span><span class="sxs-lookup"><span data-stu-id="fd50e-117">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="fd50e-118">Следующие *.csproj* ссылки на файлы `Microsoft.AspNetCore.All` metapackage для ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="fd50e-118">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]
