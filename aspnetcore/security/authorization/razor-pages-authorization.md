---
title: Соглашения об авторизации Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как управлять доступом к страницам с соглашениями, авторизовать пользователей и Разрешить анонимные пользователи для доступа к страницам или папкам страниц.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/03/2019
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: ff061f96f30cd893b903403de760a172c924cf06
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895421"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Соглашения об авторизации Razor Pages в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Для управления доступом в приложение Razor Pages рекомендуется использовать соглашения об авторизации во время запуска. Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступа к отдельным страницам или папкам страниц. Применение соглашений, описываемых в этом разделе, автоматически [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:index#how-to-download-a-sample))

Пример приложения использует [проверки подлинности файла cookie без ASP.NET Core Identity](xref:security/authentication/cookie). Основные понятия и примеры, приведенные в этом разделе в равной мере применимы к приложениям, использующим удостоверения ASP.NET Core. Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в <xref:security/authentication/identity>.

## <a name="require-authorization-to-access-a-page"></a>Требовать авторизации для доступа к странице

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.

Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizePage перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizePage*):

```csharp
options.Conventions.AuthorizePage("/Contact", "AtLeast21");
```

> [!NOTE]
> <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> Могут применяться к классу модели страницы с `[Authorize]` атрибут фильтра. Дополнительные сведения см. в разделе [атрибут фильтра Authorize](xref:razor-pages/filter#authorize-filter-attribute).

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требовать авторизации для доступа к папке страниц

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.

Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizeFolder перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeFolder*):

```csharp
options.Conventions.AuthorizeFolder("/Private", "AtLeast21");
```

## <a name="require-authorization-to-access-an-area-page"></a>Требовать авторизации для доступа к странице области

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> к странице области по указанному пути:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для заданной области. Например, имя страницы для файла *Areas/Identity/Pages/Manage/Accounts.cshtml* — *учетных записей иуправление/*.

Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizeAreaPage перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaPage*):

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts", "AtLeast21");
```

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Требовать авторизации для доступа к папке областей

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> ко всем областям в папке по указанному пути:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Путь к папке — путь к папке относительно корневого каталога страниц для заданной области. Например, путь к папке для файлов в разделе *области/Identity/страниц и управление/* — *и Администрирование*.

Чтобы указать [политики авторизации](xref:security/authorization/policies), использовать [AuthorizeAreaFolder перегрузка](xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AuthorizeAreaFolder*):

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage", "AtLeast21");
```

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToPage*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

Используйте <xref:Microsoft.Extensions.DependencyInjection.PageConventionCollectionExtensions.AllowAnonymousToFolder*> соглашение с помощью <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> добавление <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Обратите внимание на объединение авторизованным и анонимный доступ

Можно указать, что является папкой для страниц, требующих авторизации и не указать, что страницы в этой папке разрешает анонимный доступ:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Тем не менее, аннулирование, является недопустимой. Нельзя объявить папку страниц для анонимного доступа и затем указать страницу в этой папке, требующему проверки подлинности:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private")
```

Требовать авторизации на странице "закрытый" завершается сбоем. Когда как <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> и <xref:Microsoft.AspNetCore.Mvc.Authorization.AuthorizeFilter> применяются к странице <xref:Microsoft.AspNetCore.Mvc.Authorization.AllowAnonymousFilter> имеет более высокий приоритет, а также управляет доступом.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:razor-pages/razor-pages-conventions>
* <xref:Microsoft.AspNetCore.Mvc.ApplicationModels.PageConventionCollection>
