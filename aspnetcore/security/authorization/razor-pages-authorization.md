---
title: Razor Pages соглашений об авторизации в ASP.NET Core
author: guardrex
description: Узнайте, как управлять доступом к страницам с помощью соглашений, которые разрешают пользователей, и разрешить анонимным пользователям доступ к страницам или папкам страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 08/12/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: e0102ff64921a83f0330acb6f5d9bfd90f64ca7a
ms.sourcegitcommit: 89fcc6cb3e12790dca2b8b62f86609bed6335be9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/13/2019
ms.locfileid: "68994037"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="979ca-103">Razor Pages соглашений об авторизации в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="979ca-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="979ca-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="979ca-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="979ca-105">Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске.</span><span class="sxs-lookup"><span data-stu-id="979ca-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="979ca-106">Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="979ca-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="979ca-107">Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="979ca-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="979ca-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="979ca-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="979ca-109">Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="979ca-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="979ca-110">Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="979ca-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="979ca-111">Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям <xref:security/authentication/identity>в статье.</span><span class="sxs-lookup"><span data-stu-id="979ca-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="979ca-112">Требовать авторизацию для доступа к странице</span><span class="sxs-lookup"><span data-stu-id="979ca-112">Require authorization to access a page</span></span>

<span data-ttu-id="979ca-113"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:</span><span class="sxs-lookup"><span data-stu-id="979ca-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="979ca-114">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="979ca-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="979ca-115">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="979ca-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="979ca-116">Можно применить к классу модели страницы `[Authorize]` с помощью атрибута Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="979ca-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="979ca-117">Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="979ca-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="979ca-118">Требовать авторизацию для доступа к папке страниц</span><span class="sxs-lookup"><span data-stu-id="979ca-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="979ca-119"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*></span><span class="sxs-lookup"><span data-stu-id="979ca-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="979ca-120">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="979ca-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="979ca-121">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="979ca-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="979ca-122">Требовать авторизацию для доступа к странице области</span><span class="sxs-lookup"><span data-stu-id="979ca-122">Require authorization to access an area page</span></span>

<span data-ttu-id="979ca-123"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитьнастраницуобластипо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:</span><span class="sxs-lookup"><span data-stu-id="979ca-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="979ca-124">Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="979ca-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="979ca-125">Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.</span><span class="sxs-lookup"><span data-stu-id="979ca-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="979ca-126">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="979ca-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="979ca-127">Требовать авторизацию для доступа к папке областей</span><span class="sxs-lookup"><span data-stu-id="979ca-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="979ca-128"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем областям в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*></span><span class="sxs-lookup"><span data-stu-id="979ca-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="979ca-129">Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="979ca-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="979ca-130">Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.</span><span class="sxs-lookup"><span data-stu-id="979ca-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="979ca-131">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="979ca-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="979ca-132">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="979ca-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="979ca-133"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:</span><span class="sxs-lookup"><span data-stu-id="979ca-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="979ca-134">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="979ca-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="979ca-135">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="979ca-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="979ca-136"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*></span><span class="sxs-lookup"><span data-stu-id="979ca-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="979ca-137">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="979ca-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="979ca-138">Примечание о комбинировании авторизации и анонимного доступа</span><span class="sxs-lookup"><span data-stu-id="979ca-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="979ca-139">Допустимо указать, что папка страниц, для которых требуется авторизация, а не указывает, что страница в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="979ca-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="979ca-140">Обратная, однако, недопустима.</span><span class="sxs-lookup"><span data-stu-id="979ca-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="979ca-141">Нельзя объявить папку страниц для анонимного доступа, а затем указать страницу в этой папке, для которой требуется авторизация:</span><span class="sxs-lookup"><span data-stu-id="979ca-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="979ca-142">Не удается выполнить авторизацию на частной странице.</span><span class="sxs-lookup"><span data-stu-id="979ca-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="979ca-143">Если к странице применяются <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> и и, и, управление имеет приоритет и управляет доступом. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter></span><span class="sxs-lookup"><span data-stu-id="979ca-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="979ca-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="979ca-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="979ca-145">Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске.</span><span class="sxs-lookup"><span data-stu-id="979ca-145">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="979ca-146">Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="979ca-146">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="979ca-147">Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="979ca-147">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="979ca-148">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="979ca-148">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="979ca-149">Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="979ca-149">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="979ca-150">Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="979ca-150">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="979ca-151">Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям <xref:security/authentication/identity>в статье.</span><span class="sxs-lookup"><span data-stu-id="979ca-151">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="979ca-152">Требовать авторизацию для доступа к странице</span><span class="sxs-lookup"><span data-stu-id="979ca-152">Require authorization to access a page</span></span>

<span data-ttu-id="979ca-153"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:</span><span class="sxs-lookup"><span data-stu-id="979ca-153">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="979ca-154">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="979ca-154">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="979ca-155">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="979ca-155">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="979ca-156">Можно применить к классу модели страницы `[Authorize]` с помощью атрибута Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter></span><span class="sxs-lookup"><span data-stu-id="979ca-156">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="979ca-157">Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="979ca-157">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="979ca-158">Требовать авторизацию для доступа к папке страниц</span><span class="sxs-lookup"><span data-stu-id="979ca-158">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="979ca-159"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*></span><span class="sxs-lookup"><span data-stu-id="979ca-159">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="979ca-160">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="979ca-160">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="979ca-161">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="979ca-161">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="979ca-162">Требовать авторизацию для доступа к странице области</span><span class="sxs-lookup"><span data-stu-id="979ca-162">Require authorization to access an area page</span></span>

<span data-ttu-id="979ca-163"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитьнастраницуобластипо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:</span><span class="sxs-lookup"><span data-stu-id="979ca-163">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="979ca-164">Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="979ca-164">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="979ca-165">Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.</span><span class="sxs-lookup"><span data-stu-id="979ca-165">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="979ca-166">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="979ca-166">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="979ca-167">Требовать авторизацию для доступа к папке областей</span><span class="sxs-lookup"><span data-stu-id="979ca-167">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="979ca-168"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем областям в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*></span><span class="sxs-lookup"><span data-stu-id="979ca-168">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="979ca-169">Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области.</span><span class="sxs-lookup"><span data-stu-id="979ca-169">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="979ca-170">Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.</span><span class="sxs-lookup"><span data-stu-id="979ca-170">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="979ca-171">Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="979ca-171">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="979ca-172">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="979ca-172">Allow anonymous access to a page</span></span>

<span data-ttu-id="979ca-173"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:</span><span class="sxs-lookup"><span data-stu-id="979ca-173">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="979ca-174">Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.</span><span class="sxs-lookup"><span data-stu-id="979ca-174">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="979ca-175">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="979ca-175">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="979ca-176"><xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*></span><span class="sxs-lookup"><span data-stu-id="979ca-176">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="979ca-177">Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.</span><span class="sxs-lookup"><span data-stu-id="979ca-177">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="979ca-178">Примечание о комбинировании авторизации и анонимного доступа</span><span class="sxs-lookup"><span data-stu-id="979ca-178">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="979ca-179">Допустимо указать, что папка страниц, для которых требуется авторизация, а не указывает, что страница в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="979ca-179">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="979ca-180">Обратная, однако, недопустима.</span><span class="sxs-lookup"><span data-stu-id="979ca-180">The reverse, however, isn't valid.</span></span> <span data-ttu-id="979ca-181">Нельзя объявить папку страниц для анонимного доступа, а затем указать страницу в этой папке, для которой требуется авторизация:</span><span class="sxs-lookup"><span data-stu-id="979ca-181">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="979ca-182">Не удается выполнить авторизацию на частной странице.</span><span class="sxs-lookup"><span data-stu-id="979ca-182">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="979ca-183">Если к странице применяются <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> и и, и, управление имеет приоритет и управляет доступом. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter></span><span class="sxs-lookup"><span data-stu-id="979ca-183">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="979ca-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="979ca-184">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
