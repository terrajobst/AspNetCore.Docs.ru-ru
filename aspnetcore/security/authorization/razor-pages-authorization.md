---
title: Соглашения об авторизации Razor Pages в ASP.NET Core
author: guardrex
description: Узнайте, как управлять доступом к страницам с соглашениями, авторизовать пользователей и Разрешить анонимные пользователи для доступа к страницам или папкам страниц.
ms.author: riande
ms.custom: mvc
ms.date: 10/27/2017
uid: security/authorization/razor-pages-authorization
ms.openlocfilehash: d3ecb41765da912df68aeb829350d27e4d087e3a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837038"
---
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Соглашения об авторизации Razor Pages в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Для управления доступом в приложение Razor Pages рекомендуется использовать соглашения об авторизации во время запуска. Эти соглашения позволяют авторизовать пользователей и разрешить анонимным пользователям доступа к отдельным страницам или папкам страниц. Применение соглашений, описываемых в этом разделе, автоматически [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/samples) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

Пример приложения использует [проверки подлинности файла Cookie без ASP.NET Core Identity](xref:security/authentication/cookie). Учетная запись пользователя для гипотетической пользователя Родригез Мария встроен в приложение. Использовать имя пользователя по электронной почте "maria.rodriguez@contoso.com" и любой пароль для входа пользователя. Пользователь проходит проверку подлинности в `AuthenticateUser` метод в *Pages/Account/Login.cshtml.cs* файла. В реальный пример пользователь может пройти проверку подлинности в базе данных. Чтобы использовать удостоверение ASP.NET Core, следуйте указаниям в [Общие сведения об Identity в ASP.NET Core](xref:security/authentication/identity) раздела. Основные понятия и примеры, приведенные в этом разделе в равной мере применимы к приложениям, использующим удостоверения ASP.NET Core.

## <a name="require-authorization-to-access-a-page"></a>Требовать авторизации для доступа к странице

Используйте [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.

[AuthorizePage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.

::: moniker range=">= aspnetcore-2.1"

> [!NOTE]
> `AuthorizeFilter` Могут применяться к классу модели страницы с `[Authorize]` атрибут фильтра. Дополнительные сведения см. в разделе [атрибут фильтра Authorize](xref:razor-pages/filter#authorize-filter-attribute).

::: moniker-end

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требовать авторизации для доступа к папке страниц

Используйте [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.

[AuthorizeFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.

::: moniker range=">= aspnetcore-2.1"

## <a name="require-authorization-to-access-an-area-page"></a>Требовать авторизации для доступа к странице области

Используйте [AuthorizeAreaPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) к странице области по указанному пути:

```csharp
options.Conventions.AuthorizeAreaPage("Identity", "/Manage/Accounts");
```

Имя страницы — это путь к файлу без расширения относительно корневого каталога страниц для заданной области. Например, имя страницы для файла *Areas/Identity/Pages/Manage/Accounts.cshtml* — *учетных записей иуправление/*.

[AuthorizeAreaPage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareapage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaPage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) доступен, если необходимо указать политику авторизации.

## <a name="require-authorization-to-access-a-folder-of-areas"></a>Требовать авторизации для доступа к папке областей

Используйте [AuthorizeAreaFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) ко всем областям в папке по указанному пути:

```csharp
options.Conventions.AuthorizeAreaFolder("Identity", "/Manage");
```

Путь к папке — путь к папке относительно корневого каталога страниц для заданной области. Например, путь к папке для файлов в разделе *области/Identity/страниц и управление/* — *и Администрирование*.

[AuthorizeAreaFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizeareafolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeAreaFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_System_String_) доступен, если необходимо указать политику авторизации.

::: moniker-end

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

Используйте [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на страницу по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь является путь обработчик представлений, который представляет корневой Razor Pages относительный путь без расширения и содержащая только косые черты.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

Используйте [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) соглашение с помощью [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) ко всем страницам в папке по указанному пути:

[!code-csharp[](razor-pages-authorization/samples/2.x/AuthorizationSample/Startup.cs?name=snippet1&highlight=2,7)]

Указанный путь является путь обработчик представлений, который представляет относительный путь к корневой Razor Pages.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Обратите внимание на объединение авторизованным и анонимный доступ

Вполне указать, что папка страниц требуют авторизации и укажите, что страницы в этой папке разрешает анонимный доступ:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Но не наоборот. Невозможно объявить папку страниц для анонимного доступа и задание страницы в пределах для авторизации:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Требуется выполнять авторизацию на странице "закрытый" не будет работать, поскольку при как `AllowAnonymousFilter` и `AuthorizeFilter` применения фильтров к странице `AllowAnonymousFilter` wins, а также управляет доступом.

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Пользовательские поставщики моделей маршрутов и страниц Razor Pages](xref:razor-pages/razor-pages-conventions)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) класса
