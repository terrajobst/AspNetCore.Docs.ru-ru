---
title: Авторизация на основе представления в ASP.NET Core MVC
author: rick-anderson
description: В этом документе показано, как внедрить и использовать службы авторизации внутри представления ASP.NET Core Razor.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 927e510d68f269d8335b5a7c8592621219a90965
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/30/2018
ms.locfileid: "39342540"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a>Авторизация на основе представления в ASP.NET Core MVC

Часто разработчику необходимо отображение, скрытие или изменять пользовательский Интерфейс на основе текущего удостоверения пользователя. Служба авторизации в представлениях MVC с помощью доступна [внедрения зависимостей](xref:fundamentals/dependency-injection). Чтобы внедрить службы авторизации в представлении Razor, используйте `@inject` директивы:

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

Если требуется, чтобы служба авторизации в каждом представлении, поместите `@inject` директив в *_ViewImports.cshtml* файл *представления* каталога. Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).

Использование внедренного авторизации службы для вызова `AuthorizeAsync` точно таким же образом проводится проверка во время [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):

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

В приведенном выше коде модель передается в качестве ресурсов, которые необходимо выполнить вычисления политики во внимание.

> [!WARNING]
> Не полагайтесь на переключение видимости видимость элементов пользовательского интерфейса приложения, что и проверку исключительно авторизации. Скрытие элемента пользовательского интерфейса могут не полностью запретить доступ для связанного действия контроллера. Например рассмотрим кнопку в предыдущем фрагменте кода. Пользователь может вызвать `Edit` является URL-адрес метода действия, если он знает относительного ресурса */Document/Edit/1*. По этой причине `Edit` метод действия должен выполнять собственную проверку авторизации.
