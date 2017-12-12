---
title: "Авторизация на основе представления в ASP.NET Core MVC"
author: rick-anderson
description: "В этом документе показано, как внедрить и использовать службы авторизации внутри представления ASP.NET Core Razor."
keywords: "ASP.NET Core, авторизации, IAuthorizationService, Razor авторизации"
ms.author: riande
manager: wpickett
ms.date: 10/30/2017
ms.topic: article
ms.assetid: 24ce40d8-9b83-4bae-9d4c-a66350fcc8f8
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/views
ms.openlocfilehash: 756431f398c29376ab0ecd6c4f4d1db4f8022b0b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="view-based-authorization"></a>Авторизация на основе представления

Часто разработчику для отображения, скрытия или изменения пользовательского интерфейса на основе идентификатора текущего пользователя. Служба авторизации в представлениях MVC через доступна [внедрения зависимостей](xref:fundamentals/dependency-injection#fundamentals-dependency-injection). Чтобы внедрить службы авторизации в представления Razor, используйте `@inject` директиву:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Служба авторизации в каждом представлении, установите `@inject` директив в *_ViewImports.cshtml* файл *представления* каталога. Дополнительные сведения см. в разделе [внедрение зависимостей в представления](xref:mvc/views/dependency-injection).

Использовать для вызова службы подставляемого авторизации `AuthorizeAsync` точно таким же образом проводится проверка во время [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

В некоторых случаях ресурс будет модели представления. Вызвать `AuthorizeAsync` точно таким же образом проводится проверка во время [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

В приведенном выше коде модели передается в качестве ресурса, которое должен выполнить вычисления политики во внимание.

> [!WARNING]
> Не полагайтесь на переключение видимости видимость элементов пользовательского интерфейса приложения как проверку единственным авторизации. Скрытие элемента пользовательского интерфейса не могут помешать полностью доступа к связанному контроллеру действия. Например рассмотрим кнопку в предыдущем фрагменте кода. Пользователь может вызвать `Edit` является URL-адрес метода действия, если он или она знает относительного ресурса */Document/Edit/1*. По этой причине `Edit` метод действия должен выполнить собственную проверку авторизации.
