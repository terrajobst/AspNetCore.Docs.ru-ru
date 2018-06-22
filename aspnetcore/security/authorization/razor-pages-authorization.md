---
title: Правила авторизации страниц Razor в ASP.NET Core
author: guardrex
description: Узнайте, как для управления доступом к страницам с соглашениями, авторизацию пользователей и Разрешить анонимные пользователи для доступа к страницам или папкам страниц.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 8856520bf43f2f62cc12c7e883485babdb43fb3e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272679"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="41c7a-103">Правила авторизации страниц Razor в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="41c7a-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="41c7a-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="41c7a-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="41c7a-105">Для использования при запуске правила авторизации — один из способов управления доступом в приложении страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="41c7a-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="41c7a-106">Эти правила позволяют авторизацию пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="41c7a-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="41c7a-107">Соглашения, описанные в этом разделе, автоматически применяются [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="41c7a-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="41c7a-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="41c7a-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="41c7a-109">В образце приложения используется [файла Cookie проверки подлинности без ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="41c7a-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="41c7a-110">Учетная запись пользователя для гипотетического пользователя Rodriguez Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="41c7a-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="41c7a-111">Используйте имя электронной почты "maria.rodriguez@contoso.com" и все пароли для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="41c7a-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="41c7a-112">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="41c7a-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="41c7a-113">В реальном примере пользователь будет пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="41c7a-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="41c7a-114">Чтобы использовать ASP.NET Core Identity, следуйте указаниям в [введение в ASP.NET Core удостоверения](xref:security/authentication/identity) раздела.</span><span class="sxs-lookup"><span data-stu-id="41c7a-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="41c7a-115">Концепции и примеры, приведенные в этом разделе также применяются к приложениям, использующим ASP.NET Core Identity.</span><span class="sxs-lookup"><span data-stu-id="41c7a-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="41c7a-116">Требуется разрешение на доступ к странице</span><span class="sxs-lookup"><span data-stu-id="41c7a-116">Require authorization to access a page</span></span>

<span data-ttu-id="41c7a-117">Используйте [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="41c7a-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="41c7a-118">Указанный путь — это путь обработчик представлений, который является корневой страниц Razor, относительный путь без расширения и содержащий только косые черты.</span><span class="sxs-lookup"><span data-stu-id="41c7a-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="41c7a-119">[AuthorizePage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="41c7a-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="41c7a-120">`AuthorizeFilter` Может применяться к странице класс модели с `[Authorize]` атрибут фильтра.</span><span class="sxs-lookup"><span data-stu-id="41c7a-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="41c7a-121">Дополнительные сведения см. в разделе [атрибут фильтра авторизовать](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="41c7a-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="41c7a-122">Требуется разрешение на доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="41c7a-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="41c7a-123">Используйте [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на все страницы в папку по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="41c7a-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="41c7a-124">Указанный путь является путь обработчика представлений, который страниц Razor относительный путь от корня.</span><span class="sxs-lookup"><span data-stu-id="41c7a-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="41c7a-125">[AuthorizeFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="41c7a-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="41c7a-126">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="41c7a-126">Allow anonymous access to a page</span></span>

<span data-ttu-id="41c7a-127">Используйте [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="41c7a-127">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="41c7a-128">Указанный путь — это путь обработчик представлений, который является корневой страниц Razor, относительный путь без расширения и содержащий только косые черты.</span><span class="sxs-lookup"><span data-stu-id="41c7a-128">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="41c7a-129">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="41c7a-129">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="41c7a-130">Используйте [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на все страницы в папку по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="41c7a-130">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="41c7a-131">Указанный путь является путь обработчика представлений, который страниц Razor относительный путь от корня.</span><span class="sxs-lookup"><span data-stu-id="41c7a-131">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="41c7a-132">Обратите внимание на объединение авторизованные и анонимного доступа</span><span class="sxs-lookup"><span data-stu-id="41c7a-132">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="41c7a-133">Вполне указать, что папка страниц требуют наличия авторизации и указать, что страницы в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="41c7a-133">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="41c7a-134">Обратное, однако не является true.</span><span class="sxs-lookup"><span data-stu-id="41c7a-134">The reverse, however, isn't true.</span></span> <span data-ttu-id="41c7a-135">Не удается объявить папку страниц для анонимного доступа и указать страницу в для авторизации:</span><span class="sxs-lookup"><span data-stu-id="41c7a-135">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="41c7a-136">Требующие авторизации на странице «закрытый» не будет работать, так как при как `AllowAnonymousFilter` и `AuthorizeFilter` фильтры применяются к странице `AllowAnonymousFilter` wins и управление доступом.</span><span class="sxs-lookup"><span data-stu-id="41c7a-136">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="41c7a-137">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="41c7a-137">Additional resources</span></span>

* [<span data-ttu-id="41c7a-138">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="41c7a-138">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="41c7a-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) класса</span><span class="sxs-lookup"><span data-stu-id="41c7a-139">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
