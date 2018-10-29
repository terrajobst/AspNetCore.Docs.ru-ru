---
title: Соглашения об авторизации Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как управлять доступом к страницам с соглашениями, авторизовать пользователей и Разрешить анонимные пользователи для доступа к страницам или папкам страниц.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 675dc8aa4bf00bb21981cc892a09a4acd0d53c15
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207268"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="cc822-103">Соглашения об авторизации Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="cc822-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="cc822-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="cc822-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="cc822-105">Для управления доступом в приложение Razor Pages рекомендуется использовать соглашения об авторизации во время запуска.</span><span class="sxs-lookup"><span data-stu-id="cc822-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="cc822-106">Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступа к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="cc822-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="cc822-107">Применение соглашений, описываемых в этом разделе, автоматически [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="cc822-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="cc822-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="cc822-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="cc822-109">Пример приложения использует [проверки подлинности файла Cookie без ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="cc822-109">The sample app uses [Cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="cc822-110">Учетная запись пользователя для гипотетической пользователя Родригез Мария встроен в приложение.</span><span class="sxs-lookup"><span data-stu-id="cc822-110">The user account for the hypothetical user, Maria Rodriguez, is hardcoded into the app.</span></span> <span data-ttu-id="cc822-111">Использовать имя пользователя по электронной почте "maria.rodriguez@contoso.com" и любой пароль для входа пользователя.</span><span class="sxs-lookup"><span data-stu-id="cc822-111">Use the Email username "maria.rodriguez@contoso.com" and any password to sign in the user.</span></span> <span data-ttu-id="cc822-112">Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла.</span><span class="sxs-lookup"><span data-stu-id="cc822-112">The user is authenticated in the `AuthenticateUser` method in the *Pages/Account/Login.cshtml.cs* file.</span></span> <span data-ttu-id="cc822-113">В реальный пример пользователь может пройти проверку подлинности в базе данных.</span><span class="sxs-lookup"><span data-stu-id="cc822-113">In a real-world example, the user would be authenticated against a database.</span></span> <span data-ttu-id="cc822-114">Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в [Общие сведения об Identity в ASP.NET Core](xref:security/authentication/identity) раздела.</span><span class="sxs-lookup"><span data-stu-id="cc822-114">To use ASP.NET Core Identity, follow the guidance in the [Introduction to Identity on ASP.NET Core](xref:security/authentication/identity) topic.</span></span> <span data-ttu-id="cc822-115">Основные понятия и примеры, приведенные в этом разделе в равной мере применимы к приложениям, использующим удостоверения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="cc822-115">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="cc822-116">Требовать авторизации для доступа к странице</span><span class="sxs-lookup"><span data-stu-id="cc822-116">Require authorization to access a page</span></span>

<span data-ttu-id="cc822-117">Используйте [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="cc822-117">Use the [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="cc822-118">Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.</span><span class="sxs-lookup"><span data-stu-id="cc822-118">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="cc822-119">[AuthorizePage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="cc822-119">An [AuthorizePage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> <span data-ttu-id="cc822-120">`AuthorizeFilter` Могут применяться к классу модели страницы с `[Authorize]` атрибут фильтра.</span><span class="sxs-lookup"><span data-stu-id="cc822-120">An `AuthorizeFilter` can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="cc822-121">Дополнительные сведения см. в разделе [атрибут фильтра Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="cc822-121">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="cc822-122">Требовать авторизации для доступа к папке страниц</span><span class="sxs-lookup"><span data-stu-id="cc822-122">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="cc822-123">Используйте [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="cc822-123">Use the [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="cc822-124">Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc822-124">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="cc822-125">[AuthorizeFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="cc822-125">An [AuthorizeFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="cc822-126">Требовать авторизации для доступа к странице области</span><span class="sxs-lookup"><span data-stu-id="cc822-126">Require authorization to access an area page</span></span>

<span data-ttu-id="cc822-127">Используйте [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) к странице области по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="cc822-127">Use the [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="cc822-128">Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для заданной области.</span><span class="sxs-lookup"><span data-stu-id="cc822-128">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="cc822-129">Например, имя страницы для файла *Areas/Identity/Pages/Manage/Accounts.cshtml* — *учетных записей иуправление/*.</span><span class="sxs-lookup"><span data-stu-id="cc822-129">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="cc822-130">[AuthorizeAreaPage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="cc822-130">An [AuthorizeAreaPage overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="cc822-131">Требовать авторизации для доступа к папке областей</span><span class="sxs-lookup"><span data-stu-id="cc822-131">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="cc822-132">Используйте [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ко всем областям в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="cc822-132">Use the [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="cc822-133">Путь к папке — путь к папке относительно корневого каталога страниц для заданной области.</span><span class="sxs-lookup"><span data-stu-id="cc822-133">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="cc822-134">Например, путь к папке для файлов в разделе *области/Identity/страниц и управление/* — *и Администрирование*.</span><span class="sxs-lookup"><span data-stu-id="cc822-134">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="cc822-135">[AuthorizeAreaFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) доступен, если необходимо указать политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="cc822-135">An [AuthorizeAreaFolder overload](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) is available if you need to specify an authorization policy.</span></span>

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="cc822-136">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="cc822-136">Allow anonymous access to a page</span></span>

<span data-ttu-id="cc822-137">Используйте [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="cc822-137">Use the [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="cc822-138">Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.</span><span class="sxs-lookup"><span data-stu-id="cc822-138">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="cc822-139">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="cc822-139">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="cc822-140">Используйте [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="cc822-140">Use the [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) convention via [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) to add an [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="cc822-141">Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="cc822-141">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="cc822-142">Обратите внимание на объединение авторизованным и анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="cc822-142">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="cc822-143">Вполне указать, что папка страниц требуют авторизации и укажите, что страницы в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="cc822-143">It's perfectly valid to specify that a folder of pages require authorization and specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="cc822-144">Но не наоборот.</span><span class="sxs-lookup"><span data-stu-id="cc822-144">The reverse, however, isn't true.</span></span> <span data-ttu-id="cc822-145">Невозможно объявить папку страниц для анонимного доступа и задание страницы в пределах для авторизации:</span><span class="sxs-lookup"><span data-stu-id="cc822-145">You can't declare a folder of pages for anonymous access and specify a page within for authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

<span data-ttu-id="cc822-146">Требуется выполнять авторизацию на странице "закрытый" не будет работать, поскольку при как `AllowAnonymousFilter` и `AuthorizeFilter` применения фильтров к странице `AllowAnonymousFilter` wins, а также управляет доступом.</span><span class="sxs-lookup"><span data-stu-id="cc822-146">Requiring authorization on the Private page won't work because when both the `AllowAnonymousFilter` and `AuthorizeFilter` filters are applied to the page, the `AllowAnonymousFilter` wins and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="cc822-147">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cc822-147">Additional resources</span></span>

* [<span data-ttu-id="cc822-148">Пользовательские поставщики моделей маршрутов и страниц Razor Pages</span><span class="sxs-lookup"><span data-stu-id="cc822-148">Razor Pages custom route and page model providers</span></span>](xref:razor-pages/razor-pages-conventions)
* <span data-ttu-id="cc822-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) класса</span><span class="sxs-lookup"><span data-stu-id="cc822-149">[PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) class</span></span>
