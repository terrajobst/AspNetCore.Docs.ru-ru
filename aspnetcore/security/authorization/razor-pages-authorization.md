---
title: Razor Pages соглашений об авторизации в ASP.NET Core
author: rick-anderson
description: Узнайте, как управлять доступом к страницам с помощью соглашений, которые разрешают пользователей, и разрешить анонимным пользователям доступ к страницам или папкам страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 00fc487c6ac802f213bcf83994ecc2b1a1468589
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653116"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="b1213-103">Razor Pages соглашений об авторизации в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="b1213-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b1213-104">Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске.</span><span class="sxs-lookup"><span data-stu-id="b1213-104">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="b1213-105">Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="b1213-105">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="b1213-106">Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="b1213-106">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="b1213-107">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b1213-107">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b1213-108">Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="b1213-108">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="b1213-109">Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1213-109">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="b1213-110">Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="b1213-110">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="b1213-111">Требовать авторизацию для доступа к странице</span><span class="sxs-lookup"><span data-stu-id="b1213-111">Require authorization to access a page</span></span>

<span data-ttu-id="b1213-112">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-112">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="b1213-113">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="b1213-113">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="b1213-114">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="b1213-114">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="b1213-115"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> можно применить к классу модели страницы с помощью атрибута фильтра `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="b1213-115">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="b1213-116">Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="b1213-116">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="b1213-117">Требовать авторизацию для доступа к папке страниц</span><span class="sxs-lookup"><span data-stu-id="b1213-117">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="b1213-118">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-118">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="b1213-119">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="b1213-119">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="b1213-120">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="b1213-120">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="b1213-121">Требовать авторизацию для доступа к странице области</span><span class="sxs-lookup"><span data-stu-id="b1213-121">Require authorization to access an area page</span></span>

<span data-ttu-id="b1213-122">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу области по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-122">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="b1213-123">Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="b1213-123">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="b1213-124">Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.</span><span class="sxs-lookup"><span data-stu-id="b1213-124">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="b1213-125">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="b1213-125">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="b1213-126">Требовать авторизацию для доступа к папке областей</span><span class="sxs-lookup"><span data-stu-id="b1213-126">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="b1213-127">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> во все области в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-127">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="b1213-128">Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="b1213-128">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="b1213-129">Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.</span><span class="sxs-lookup"><span data-stu-id="b1213-129">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="b1213-130">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="b1213-130">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="b1213-131">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="b1213-131">Allow anonymous access to a page</span></span>

<span data-ttu-id="b1213-132">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-132">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="b1213-133">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="b1213-133">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="b1213-134">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="b1213-134">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="b1213-135">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-135">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="b1213-136">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="b1213-136">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="b1213-137">Примечание о комбинировании авторизации и анонимного доступа</span><span class="sxs-lookup"><span data-stu-id="b1213-137">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="b1213-138">Допустимо указать, что для папки страниц требуется авторизация, а затем указать, что страница в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="b1213-138">It's valid to specify that a folder of pages requires authorization and then specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="b1213-139">Обратная, однако, недопустима.</span><span class="sxs-lookup"><span data-stu-id="b1213-139">The reverse, however, isn't valid.</span></span> <span data-ttu-id="b1213-140">Нельзя объявить папку страниц для анонимного доступа, а затем указать страницу в этой папке, для которой требуется авторизация:</span><span class="sxs-lookup"><span data-stu-id="b1213-140">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="b1213-141">Не удается выполнить авторизацию на частной странице.</span><span class="sxs-lookup"><span data-stu-id="b1213-141">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="b1213-142">Когда к странице применяются как <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, так <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> имеет приоритет и управляет доступом.</span><span class="sxs-lookup"><span data-stu-id="b1213-142">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1213-143">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b1213-143">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b1213-144">Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске.</span><span class="sxs-lookup"><span data-stu-id="b1213-144">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="b1213-145">Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="b1213-145">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="b1213-146">Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="b1213-146">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="b1213-147">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="b1213-147">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="b1213-148">Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="b1213-148">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="b1213-149">Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="b1213-149">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="b1213-150">Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="b1213-150">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="b1213-151">Требовать авторизацию для доступа к странице</span><span class="sxs-lookup"><span data-stu-id="b1213-151">Require authorization to access a page</span></span>

<span data-ttu-id="b1213-152">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-152">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="b1213-153">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="b1213-153">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="b1213-154">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="b1213-154">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="b1213-155"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> можно применить к классу модели страницы с помощью атрибута фильтра `[Authorize]`.</span><span class="sxs-lookup"><span data-stu-id="b1213-155">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="b1213-156">Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="b1213-156">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="b1213-157">Требовать авторизацию для доступа к папке страниц</span><span class="sxs-lookup"><span data-stu-id="b1213-157">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="b1213-158">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-158">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="b1213-159">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="b1213-159">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="b1213-160">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="b1213-160">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="b1213-161">Требовать авторизацию для доступа к странице области</span><span class="sxs-lookup"><span data-stu-id="b1213-161">Require authorization to access an area page</span></span>

<span data-ttu-id="b1213-162">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу области по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-162">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="b1213-163">Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="b1213-163">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="b1213-164">Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.</span><span class="sxs-lookup"><span data-stu-id="b1213-164">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="b1213-165">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="b1213-165">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="b1213-166">Требовать авторизацию для доступа к папке областей</span><span class="sxs-lookup"><span data-stu-id="b1213-166">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="b1213-167">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> во все области в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-167">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="b1213-168">Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="b1213-168">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="b1213-169">Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.</span><span class="sxs-lookup"><span data-stu-id="b1213-169">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="b1213-170">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="b1213-170">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="b1213-171">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="b1213-171">Allow anonymous access to a page</span></span>

<span data-ttu-id="b1213-172">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-172">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="b1213-173">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="b1213-173">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="b1213-174">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="b1213-174">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="b1213-175">Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="b1213-175">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="b1213-176">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="b1213-176">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="b1213-177">Примечание о комбинировании авторизации и анонимного доступа</span><span class="sxs-lookup"><span data-stu-id="b1213-177">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="b1213-178">Допустимо указать, что папка страниц, для которых требуется авторизация, а не указывает, что страница в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="b1213-178">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="b1213-179">Обратная, однако, недопустима.</span><span class="sxs-lookup"><span data-stu-id="b1213-179">The reverse, however, isn't valid.</span></span> <span data-ttu-id="b1213-180">Нельзя объявить папку страниц для анонимного доступа, а затем указать страницу в этой папке, для которой требуется авторизация:</span><span class="sxs-lookup"><span data-stu-id="b1213-180">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="b1213-181">Не удается выполнить авторизацию на частной странице.</span><span class="sxs-lookup"><span data-stu-id="b1213-181">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="b1213-182">Когда к странице применяются как <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, так <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> имеет приоритет и управляет доступом.</span><span class="sxs-lookup"><span data-stu-id="b1213-182">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="b1213-183">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="b1213-183">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
