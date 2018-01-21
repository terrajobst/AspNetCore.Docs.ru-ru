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
# <a name="razor-pages-authorization-conventions-in-aspnet-core"></a>Правила авторизации страниц Razor в ASP.NET Core

Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)

Для использования при запуске правила авторизации — один из способов управления доступом в приложении страниц Razor. Эти правила позволяют авторизацию пользователей и разрешить анонимным пользователям доступ к отдельным страницам или папкам страниц. Соглашения, описанные в этом разделе, автоматически применяются [фильтры авторизации](xref:mvc/controllers/filters#authorization-filters) для управления доступом.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/razor-pages-authorization/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="require-authorization-to-access-a-page"></a>Требуется разрешение на доступ к странице

Используйте [AuthorizePage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на страницу по указанному пути:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,4)]

Указанный путь — это путь обработчик представлений, который является корневой страниц Razor, относительный путь без расширения и содержащий только косые черты.

[AuthorizePage перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizepage#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizePage_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.

## <a name="require-authorization-to-access-a-folder-of-pages"></a>Требуется разрешение на доступ к папке страниц

Используйте [AuthorizeFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AuthorizeFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.authorizefilter) на все страницы в папку по указанному пути:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,5)]

Указанный путь является путь обработчика представлений, который страниц Razor относительный путь от корня.

[AuthorizeFolder перегрузка](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.authorizefolder#Microsoft_Extensions_DependencyInjection_PageConventionCollectionExtensions_AuthorizeFolder_Microsoft_AspNetCore_Mvc_ApplicationModels_PageConventionCollection_System_String_System_String_) доступен, если необходимо указать политику авторизации.

## <a name="allow-anonymous-access-to-a-page"></a>Разрешить анонимный доступ к странице

Используйте [AllowAnonymousToPage](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustopage) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) страницу по указанному пути:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,6)]

Указанный путь — это путь обработчик представлений, который является корневой страниц Razor, относительный путь без расширения и содержащий только косые черты.

## <a name="allow-anonymous-access-to-a-folder-of-pages"></a>Разрешить анонимный доступ к папке страниц

Используйте [AllowAnonymousToFolder](/dotnet/api/microsoft.extensions.dependencyinjection.pageconventioncollectionextensions.allowanonymoustofolder) соглашение через [AddRazorPagesOptions](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.addrazorpagesoptions) добавление [AllowAnonymousFilter](/dotnet/api/microsoft.aspnetcore.mvc.authorization.allowanonymousfilter) на все страницы в папку по указанному пути:

[!code-csharp[Main](razor-pages-authorization/sample/Startup.cs?name=snippet1&highlight=2,7)]

Указанный путь является путь обработчика представлений, который страниц Razor относительный путь от корня.

## <a name="note-on-combining-authorized-and-anonymous-access"></a>Обратите внимание на объединение авторизованные и анонимного доступа

Вполне указать, что папка страниц требуют наличия авторизации и указать, что страницы в этой папке разрешает анонимный доступ:

```csharp
// This works.
.AuthorizeFolder("/Private").AllowAnonymousToPage("/Private/Public")
```

Обратное, однако не является true. Не удается объявить папку страниц для анонимного доступа и указать страницу в для авторизации:

```csharp
// This doesn't work!
.AllowAnonymousToFolder("/Public").AuthorizePage("/Public/Private") 
```

Требующие авторизации на странице «закрытый» не будет работать, так как при как `AllowAnonymousFilter` и `AuthorizeFilter` фильтры применяются к странице `AllowAnonymousFilter` wins и управление доступом.

## <a name="see-also"></a>См. также

* [Пользовательские поставщики моделей маршрутов и страниц Razor Pages](xref:mvc/razor-pages/razor-pages-convention-features)
* [PageConventionCollection](/dotnet/api/microsoft.aspnetcore.mvc.applicationmodels.pageconventioncollection) класса
