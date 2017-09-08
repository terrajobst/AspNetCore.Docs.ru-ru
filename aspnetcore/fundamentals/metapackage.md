---
title: "Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x и более поздних версий"
author: Rick-Anderson
description: "Microsoft.AspNetCore.All metapackage включает все поддерживаемые пакеты."
keywords: "Пакет ASP.NET Core, NuGet, Microsoft.AspNetCore.All, metapackage"
ms.author: riande
manager: wpickett
ms.date: 07/16/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/metapackage
ms.openlocfilehash: 255438a4ce36ce4978f8c8ee298388a25ac00d17
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
#<a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-2x"></a><span data-ttu-id="12fc7-104">Metapackage Microsoft.AspNetCore.All для ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="12fc7-104">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.x</span></span>

<span data-ttu-id="12fc7-105">Для этой функции требуется ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="12fc7-105">This feature requires ASP.NET Core 2.x.</span></span>

<span data-ttu-id="12fc7-106">[Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage для ASP.NET Core включает в себя:</span><span class="sxs-lookup"><span data-stu-id="12fc7-106">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="12fc7-107">Все поддерживаемые пакеты командой ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="12fc7-107">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="12fc7-108">Для всех поддерживаемых пакетов по Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="12fc7-108">All supported packages by the Entity Framework Core.</span></span> 
* <span data-ttu-id="12fc7-109">Зависимости внутренних и сторонних поставщиков, используемые ASP.NET Core и Entity Framework Core.</span><span class="sxs-lookup"><span data-stu-id="12fc7-109">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span> 

<span data-ttu-id="12fc7-110">Все возможности ASP.NET Core 2.x и Entity Framework Core 2.x включаются в `Microsoft.AspNetCore.All` пакета.</span><span class="sxs-lookup"><span data-stu-id="12fc7-110">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="12fc7-111">Шаблоны проектов по умолчанию использовать этот пакет.</span><span class="sxs-lookup"><span data-stu-id="12fc7-111">The default project templates use this package.</span></span>

<span data-ttu-id="12fc7-112">Номер версии `Microsoft.AspNetCore.All` metapackage представляет версию ASP.NET Core и версии Entity Framework Core (выравнивание с версией .NET Core).</span><span class="sxs-lookup"><span data-stu-id="12fc7-112">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version (aligned with the .NET Core version).</span></span>

<span data-ttu-id="12fc7-113">Приложения, использующие `Microsoft.AspNetCore.All` metapackage автоматически используют хранилище среды выполнения .NET Core.</span><span class="sxs-lookup"><span data-stu-id="12fc7-113">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the .NET Core Runtime Store.</span></span> <span data-ttu-id="12fc7-114">Хранилище среда выполнения содержит все ресурсы среды выполнения, необходимые для запуска ASP.NET Core 2.x приложений.</span><span class="sxs-lookup"><span data-stu-id="12fc7-114">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="12fc7-115">При использовании `Microsoft.AspNetCore.All` metapackage, **не** активы из упоминаемой пакетов ASP.NET Core NuGet развертываются с приложением &mdash; хранилище среды выполнения .NET Core содержит эти ресурсы.</span><span class="sxs-lookup"><span data-stu-id="12fc7-115">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="12fc7-116"><!-- todo add link to Runtime store -->Ресурсы в хранилище среды выполнения компилируются для сокращения времени запуска приложения.</span><span class="sxs-lookup"><span data-stu-id="12fc7-116"><!-- todo add link to Runtime store --> The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="12fc7-117">Процесс усечения пакета можно использовать для удаления пакетов, которые не используются.</span><span class="sxs-lookup"><span data-stu-id="12fc7-117">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="12fc7-118">Обрезанная пакеты исключаются в выходных данных для опубликованного приложения.</span><span class="sxs-lookup"><span data-stu-id="12fc7-118">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="12fc7-119">Следующие *.csproj* ссылки на файлы `Microsoft.AspNetCore.All` metapackage для ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="12fc7-119">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

<span data-ttu-id="12fc7-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span><span class="sxs-lookup"><span data-stu-id="12fc7-120">[!code-xml[Main](..\mvc\views\view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=9)]</span></span>
