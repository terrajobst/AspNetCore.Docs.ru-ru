---
title: "Авторизация на основе представления"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 82c0c7282de34e496f529d964f99121ae2805c5a
ms.sourcegitcommit: e3b1726cc04e80dc28464c35259edbd3bc39a438
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/12/2017
---
# <a name="view-based-authorization"></a>Авторизация на основе представления

<a name=security-authorization-views></a>

Часто разработчик необходимость отображения, скрытия или изменения пользовательского интерфейса на основе идентификатора текущего пользователя. Служба авторизации в представлениях MVC через доступна [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection). Для добавления службы авторизации в использование представления Razor `@inject` директив, например `@inject IAuthorizationService AuthorizationService` (требуется `@using Microsoft.AspNetCore.Authorization`). Если требуется служба авторизации в каждом представлении поместите `@inject` директив в `_ViewImports.cshtml` файла в `Views` каталога. Дополнительные сведения о внедрение зависимостей в представления. в разделе [внедрение зависимостей в представления](../../mvc/views/dependency-injection.md).

Как только вы внедрили службы авторизации его использовать путем вызова `AuthorizeAsync` метод в точно так же, как проводится проверка во время [авторизации на основе ресурсов](resourcebased.md#security-authorization-resource-based-imperative).

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

В некоторых случаях ресурс будет представление модели и может вызывать `AuthorizeAsync` в точно так же, как проводится проверка во время [авторизации на основе ресурсов](resourcebased.md#security-authorization-resource-based-imperative);

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

Здесь вы увидите, что модель передается как авторизации ресурсов необходимо учитывать.

>[!WARNING]
>Не следует полагаться на отображение и скрытие частей пользовательского интерфейса в качестве единственного метода авторизации. Скрытие пользовательский Интерфейс элемента не означает, что пользователь не может получить доступ к. Пользователь также необходимо авторизовать в коде контроллера.
