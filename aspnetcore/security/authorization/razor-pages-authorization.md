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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Razor Pages соглашений об авторизации в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

::: moniker range=">= aspnetcore-3.0"

Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске. Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц. Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie). Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core. Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям <xref:security/authentication/identity>в статье.

## <a name="require-authorization-to-access-a-page"></a>Требовать авторизацию для доступа к странице

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Можно применить к классу модели страницы `[Authorize]` с помощью атрибута Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требовать авторизацию для доступа к папке страниц

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Требовать авторизацию для доступа к странице области

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитьнастраницуобластипо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области. Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Требовать авторизацию для доступа к папке областей

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем областям в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области. Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>

[!code-csharp[](razor-pages-authorization/samples/3.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

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

Не удается выполнить авторизацию на частной странице. Если к странице применяются <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> и и, и, управление имеет приоритет и управляет доступом. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Одним из способов управления доступом в приложении Razor Pages является использование соглашений об авторизации при запуске. Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц. Соглашения, описанные в этом разделе, автоматически применяют [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения использует [проверку подлинности файлов cookie без ASP.NET Core удостоверения](xref:security/authentication/cookie). Понятия и примеры, приведенные в этом разделе, применяются одинаково к приложениям, использующим удостоверение ASP.NET Core. Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям <xref:security/authentication/identity>в статье.

## <a name="require-authorization-to-access-a-page"></a>Требовать авторизацию для доступа к странице

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризепаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> Можно применить к классу модели страницы `[Authorize]` с помощью атрибута Filter. <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Дополнительные сведения см. в разделе [авторизация атрибута фильтра](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требовать авторизацию для доступа к папке страниц

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корнем относительно пути.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризефолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Требовать авторизацию для доступа к странице области

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с,чтобыдобавитьнастраницуобластипо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для указанной области. Например, имя страницы для файлов *Areas/Identity/Pages/Manage/Accounts. cshtml* — */манаже/аккаунтс*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареапаже](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Требовать авторизацию для доступа к папке областей

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> соглашение с, чтобы добавить ко всем областям в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Путь к папке — это путь к папке относительно корневого каталога страниц для указанной области. Например, путь к папке для файлов в разделе *Areas/Identity/Pages/Manage/* */манаже*.

Чтобы указать [политику авторизации](xref:security/authorization/policies), используйте перегрузку [аусоризеареафолдер](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с,чтобыдобавитькстраницепо<xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь является путем к обработчику представлений, который является Razor Pages корневым относительном путем без расширения и содержит только косую черту.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

<xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> Используйте <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> соглашение с, чтобы добавить ко всем страницам в папке по указанному пути: <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>

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

Не удается выполнить авторизацию на частной странице. Если к странице применяются <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> и и, и, управление имеет приоритет и управляет доступом. <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter>

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>

::: moniker-end
