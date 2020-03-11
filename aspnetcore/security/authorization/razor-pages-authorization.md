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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Razor Pages соглашений об авторизации в ASP.NET Core

::: moniker range=">= aspnetcore-3.0"

Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске. Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц. Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie). Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core. Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Требовать авторизацию для доступа к странице

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> можно применить к классу модели страницы с помощью атрибута фильтра `[Authorize]`. Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требовать авторизацию для доступа к папке страниц

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Требовать авторизацию для доступа к странице области

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу области по указанному пути:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области. Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Требовать авторизацию для доступа к папке областей

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> во все области в папке по указанному пути:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области. Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Примечание о комбинировании авторизации и анонимного доступа

Допустимо указать, что для папки страниц требуется авторизация, а затем указать, что страница в этой папке разрешает анонимный доступ:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Обратная, однако, недопустима. Нельзя объявить папку страниц для анонимного доступа, а затем указать страницу в этой папке, для которой требуется авторизация:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Не удается выполнить авторизацию на частной странице. Когда к странице применяются как <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, так <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> имеет приоритет и управляет доступом.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске. Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц. Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie). Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core. Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Требовать авторизацию для доступа к странице

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> можно применить к классу модели страницы с помощью атрибута фильтра `[Authorize]`. Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требовать авторизацию для доступа к папке страниц

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Требовать авторизацию для доступа к странице области

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу области по указанному пути:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области. Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Требовать авторизацию для доступа к папке областей

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> во все области в папке по указанному пути:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области. Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте [перегрузку аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

Используйте соглашение <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>, чтобы добавить <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Примечание о комбинировании авторизации и анонимного доступа

Допустимо указать, что папка страниц, для которых требуется авторизация, а не указывает, что страница в этой папке разрешает анонимный доступ:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Обратная, однако, недопустима. Нельзя объявить папку страниц для анонимного доступа, а затем указать страницу в этой папке, для которой требуется авторизация:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Не удается выполнить авторизацию на частной странице. Когда к странице применяются как <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>, так <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter>, <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> имеет приоритет и управляет доступом.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
