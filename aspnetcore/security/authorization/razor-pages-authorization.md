---
title: "Правила авторизации страниц Razor в ASP.NET Core"
author: guardrex
description: "Узнайте, как для управления доступом к страницам с соглашения во время запуска, авторизацию пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц."
ms.author: riande
manager: wpickett
ms.date: 10/27/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 72b558816e687c30d0c60f2fd85227d0d803219b
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="a43e0-103">Правила авторизации страниц Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a43e0-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="a43e0-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a43e0-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a43e0-105">Для использования при запуске правила авторизации — один из способов управления доступом в приложении страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="a43e0-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="a43e0-106">Эти правила позволяют авторизацию пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="a43e0-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="a43e0-107">Соглашения, описанные в этом разделе, автоматически применяются [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="a43e0-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="a43e0-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a43e0-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="a43e0-109">Требуется разрешение на доступ к странице</span><span class="sxs-lookup"><span data-stu-id="a43e0-109">Require authorization to access a page</span></span>

<span data-ttu-id="a43e0-110">Используйте [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="a43e0-110">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="a43e0-111">Указанный путь — это путь обработчик представлений, который является корневой страниц Razor, относительный путь без расширения и содержащий только косые черты.</span><span class="sxs-lookup"><span data-stu-id="a43e0-111">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="a43e0-112">[AuthorizePage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="a43e0-112">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="a43e0-113">Требуется разрешение на доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="a43e0-113">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="a43e0-114">Используйте [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на все страницы в папку по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="a43e0-114">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="a43e0-115">Указанный путь является путь обработчика представлений, который страниц Razor относительный путь от корня.</span><span class="sxs-lookup"><span data-stu-id="a43e0-115">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="a43e0-116">[AuthorizeFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="a43e0-116">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="a43e0-117">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="a43e0-117">Allow anonymous access to a page</span></span>

<span data-ttu-id="a43e0-118">Используйте [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="a43e0-118">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="a43e0-119">Указанный путь — это путь обработчик представлений, который является корневой страниц Razor, относительный путь без расширения и содержащий только косые черты.</span><span class="sxs-lookup"><span data-stu-id="a43e0-119">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="a43e0-120">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="a43e0-120">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="a43e0-121">Используйте [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на все страницы в папку по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="a43e0-121">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="a43e0-122">Указанный путь является путь обработчика представлений, который страниц Razor относительный путь от корня.</span><span class="sxs-lookup"><span data-stu-id="a43e0-122">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="a43e0-123">Обратите внимание на объединение авторизованные и анонимного доступа</span><span class="sxs-lookup"><span data-stu-id="a43e0-123">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="a43e0-124">Вполне указать, что папка страниц требуют наличия авторизации и указать, что страницы в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="a43e0-124">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="a43e0-125">Обратное, однако не является true.</span><span class="sxs-lookup"><span data-stu-id="a43e0-125">The reverse, however, isn't true.</span></span> <span data-ttu-id="a43e0-126">Не удается объявить папку страниц для анонимного доступа и указать страницу в для авторизации:</span><span class="sxs-lookup"><span data-stu-id="a43e0-126">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="a43e0-127">Требующие авторизации на странице «закрытый» не будет работать, так как при как `AllowAnonymousFilter` и `AuthorizeFilter` фильтры применяются к странице `AllowAnonymousFilter` wins и управление доступом.</span><span class="sxs-lookup"><span data-stu-id="a43e0-127">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="see-also"></a><span data-ttu-id="a43e0-128">См. также</span><span class="sxs-lookup"><span data-stu-id="a43e0-128">See also</span></span>

* [<span data-ttu-id="a43e0-129">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="a43e0-129">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* <span data-ttu-id="a43e0-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) класса</span><span class="sxs-lookup"><span data-stu-id="a43e0-130">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
