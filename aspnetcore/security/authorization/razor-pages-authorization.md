---
title: Соглашения об авторизации Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как управлять доступом к страницам с соглашениями, авторизовать пользователей и Разрешить анонимные пользователи для доступа к страницам или папкам страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: 040d33eba7eaf7a3aece2eedcdef7343e52972af
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345513"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a><span data-ttu-id="11194-103">Соглашения об авторизации Razor Pages в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11194-103">Razor Pages authorization conventions in ASP.NET Core</span></span>

<span data-ttu-id="11194-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="11194-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="11194-105">Для управления доступом в приложение Razor Pages рекомендуется использовать соглашения об авторизации во время запуска.</span><span class="sxs-lookup"><span data-stu-id="11194-105">One way to control access in your Razor Pages app is to use authorization conventions at startup.</span></span> <span data-ttu-id="11194-106">Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступа к отдельным страницам или папкам страниц.</span><span class="sxs-lookup"><span data-stu-id="11194-106">These conventions allow you to authorize users and allow anonymous users to access individual pages or folders of pages.</span></span> <span data-ttu-id="11194-107">Применение соглашений, описываемых в этом разделе, автоматически [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.</span><span class="sxs-lookup"><span data-stu-id="11194-107">The conventions described in this topic automatically apply [authorization filters](xref:mvc/controllers/filters#authorization-filters) to control access.</span></span>

<span data-ttu-id="11194-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11194-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="11194-109">Пример приложения использует [проверки подлинности файла cookie без ASP.NET Core Identity](xref:security/authentication/cookie).</span><span class="sxs-lookup"><span data-stu-id="11194-109">The sample app uses [cookie authentication without ASP.NET Core Identity](xref:security/authentication/cookie).</span></span> <span data-ttu-id="11194-110">Основные понятия и примеры, приведенные в этом разделе в равной мере применимы к приложениям, использующим удостоверения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="11194-110">The concepts and examples shown in this topic apply equally to apps that use ASP.NET Core Identity.</span></span> <span data-ttu-id="11194-111">Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в <xref:security/authentication/identity>.</span><span class="sxs-lookup"><span data-stu-id="11194-111">To use ASP.NET Core Identity, follow the guidance in <xref:security/authentication/identity>.</span></span>

## <a name="require-authorization-to-access-a-page"></a><span data-ttu-id="11194-112">Требовать авторизации для доступа к странице</span><span class="sxs-lookup"><span data-stu-id="11194-112">Require authorization to access a page</span></span>

<span data-ttu-id="11194-113">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="11194-113">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

<span data-ttu-id="11194-114">Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.</span><span class="sxs-lookup"><span data-stu-id="11194-114">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

<span data-ttu-id="11194-115">Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizePage перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span><span class="sxs-lookup"><span data-stu-id="11194-115">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizePage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):</span></span>

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <span data-ttu-id="11194-116"><xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Могут применяться к классу модели страницы с `[Authorize]` атрибут фильтра.</span><span class="sxs-lookup"><span data-stu-id="11194-116">An <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> can be applied to a page model class with the `[Authorize]` filter attribute.</span></span> <span data-ttu-id="11194-117">Дополнительные сведения см. в разделе [атрибут фильтра Authorize](xref:razor-pages/filter#authorize-filter-attribute).</span><span class="sxs-lookup"><span data-stu-id="11194-117">For more information, see [Authorize filter attribute](xref:razor-pages/filter#authorize-filter-attribute).</span></span>

## <a name="require-authorization-to-access-a-folder-of-pages"></a><span data-ttu-id="11194-118">Требовать авторизации для доступа к папке страниц</span><span class="sxs-lookup"><span data-stu-id="11194-118">Require authorization to access a folder of pages</span></span>

<span data-ttu-id="11194-119">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="11194-119">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

<span data-ttu-id="11194-120">Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="11194-120">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

<span data-ttu-id="11194-121">Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizeFolder перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span><span class="sxs-lookup"><span data-stu-id="11194-121">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):</span></span>

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a><span data-ttu-id="11194-122">Требовать авторизации для доступа к странице области</span><span class="sxs-lookup"><span data-stu-id="11194-122">Require authorization to access an area page</span></span>

<span data-ttu-id="11194-123">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> к странице области по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="11194-123">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to the area page at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

<span data-ttu-id="11194-124">Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для заданной области.</span><span class="sxs-lookup"><span data-stu-id="11194-124">The page name is the path of the file without an extension relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="11194-125">Например, имя страницы для файла *Areas/Identity/Pages/Manage/Accounts.cshtml* — *учетных записей иуправление/*.</span><span class="sxs-lookup"><span data-stu-id="11194-125">For example, the page name for the file *Areas/Identity/Pages/Manage/Accounts.cshtml* is */Manage/Accounts*.</span></span>

<span data-ttu-id="11194-126">Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizeAreaPage перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span><span class="sxs-lookup"><span data-stu-id="11194-126">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaPage overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):</span></span>

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a><span data-ttu-id="11194-127">Требовать авторизации для доступа к папке областей</span><span class="sxs-lookup"><span data-stu-id="11194-127">Require authorization to access a folder of areas</span></span>

<span data-ttu-id="11194-128">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем областям в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="11194-128">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> to all of the areas in a folder at the specified path:</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

<span data-ttu-id="11194-129">Путь к папке — путь к папке относительно корневого каталога страниц для заданной области.</span><span class="sxs-lookup"><span data-stu-id="11194-129">The folder path is the path of the folder relative to the pages root directory for the specified area.</span></span> <span data-ttu-id="11194-130">Например, путь к папке для файлов в разделе *области/Identity/страниц и управление/* — *и Администрирование*.</span><span class="sxs-lookup"><span data-stu-id="11194-130">For example, the folder path for the files under *Areas/Identity/Pages/Manage/* is */Manage*.</span></span>

<span data-ttu-id="11194-131">Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizeAreaFolder перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span><span class="sxs-lookup"><span data-stu-id="11194-131">To specify an [authorization policy](xref:security/authorization/policies), use an [AuthorizeAreaFolder overload](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):</span></span>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a><span data-ttu-id="11194-132">Разрешить анонимный доступ к странице</span><span class="sxs-lookup"><span data-stu-id="11194-132">Allow anonymous access to a page</span></span>

<span data-ttu-id="11194-133">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> на страницу по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="11194-133">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to a page at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

<span data-ttu-id="11194-134">Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.</span><span class="sxs-lookup"><span data-stu-id="11194-134">The specified path is the View Engine path, which is the Razor Pages root relative path without an extension and containing only forward slashes.</span></span>

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a><span data-ttu-id="11194-135">Разрешить анонимный доступ к папке страниц</span><span class="sxs-lookup"><span data-stu-id="11194-135">Allow anonymous access to a folder of pages</span></span>

<span data-ttu-id="11194-136">Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ко всем страницам в папке по указанному пути:</span><span class="sxs-lookup"><span data-stu-id="11194-136">Use the <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> convention via <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> to add an <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> to all of the pages in a folder at the specified path:</span></span>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

<span data-ttu-id="11194-137">Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="11194-137">The specified path is the View Engine path, which is the Razor Pages root relative path.</span></span>

## <a name="note-on-combining-authorized-and-anonymous-access"></a><span data-ttu-id="11194-138">Обратите внимание на объединение авторизованным и анонимный доступ</span><span class="sxs-lookup"><span data-stu-id="11194-138">Note on combining authorized and anonymous access</span></span>

<span data-ttu-id="11194-139">Можно указать, что является папкой для страниц, требующих авторизации и не указать, что страницы в этой папке разрешает анонимный доступ:</span><span class="sxs-lookup"><span data-stu-id="11194-139">It's valid to specify that a folder of pages that require authorization and than specify that a page within that folder allows anonymous access:</span></span>

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

<span data-ttu-id="11194-140">Тем не менее, аннулирование, является недопустимой.</span><span class="sxs-lookup"><span data-stu-id="11194-140">The reverse, however, isn't valid.</span></span> <span data-ttu-id="11194-141">Нельзя объявить папку страниц для анонимного доступа и затем указать страницу в этой папке, требующему проверки подлинности:</span><span class="sxs-lookup"><span data-stu-id="11194-141">You can't declare a folder of pages for anonymous access and then specify a page within that folder that requires authorization:</span></span>

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

<span data-ttu-id="11194-142">Требовать авторизации на странице "закрытый" завершается сбоем.</span><span class="sxs-lookup"><span data-stu-id="11194-142">Requiring authorization on the Private page fails.</span></span> <span data-ttu-id="11194-143">Когда как <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> и <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> применяются к странице <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> имеет более высокий приоритет, а также управляет доступом.</span><span class="sxs-lookup"><span data-stu-id="11194-143">When both the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> and <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> are applied to the page, the <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> takes precedence and controls access.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="11194-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="11194-144">Additional resources</span></span>

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
