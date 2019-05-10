---
title: Авторизация на основе представления в ASP.NET Core MVC
author: rick-anderson
description: В этом документе показано, как внедрить и использовать службы авторизации внутри представления ASP.NET Core Razor.
ms.author: riande
ms.date: 10/30/2017
uid: security/authorization/views
ms.openlocfilehash: e497c41d4dca29fed8733f18cf727804e3f06d8c
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892061"
---
# <a name="view-based-authorization-in-aspnet-core-mvc"></a><span data-ttu-id="a6d08-103">Авторизация на основе представления в ASP.NET Core MVC</span><span class="sxs-lookup"><span data-stu-id="a6d08-103">View-based authorization in ASP.NET Core MVC</span></span>

<span data-ttu-id="a6d08-104">Часто разработчику необходимо отображение, скрытие или изменять пользовательский Интерфейс на основе текущего удостоверения пользователя.</span><span class="sxs-lookup"><span data-stu-id="a6d08-104">A developer often wants to show, hide, or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="a6d08-105">Служба авторизации в представлениях MVC с помощью доступна [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a6d08-105">You can access the authorization service within MVC views via [dependency injection](xref:fundamentals/dependency-injection).</span></span> <span data-ttu-id="a6d08-106">Чтобы внедрить службы авторизации в представлении Razor, используйте `@inject` директивы:</span><span class="sxs-lookup"><span data-stu-id="a6d08-106">To inject the authorization service into a Razor view, use the `@inject` directive:</span></span>

```cshtml
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService
```

<span data-ttu-id="a6d08-107">Если требуется, чтобы служба авторизации в каждом представлении, поместите `@inject` директив в *_ViewImports.cshtml* файл *представления* каталога.</span><span class="sxs-lookup"><span data-stu-id="a6d08-107">If you want the authorization service in every view, place the `@inject` directive into the *_ViewImports.cshtml* file of the *Views* directory.</span></span> <span data-ttu-id="a6d08-108">Дополнительные сведения: [Внедрение зависимостей в представления](xref:mvc/views/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a6d08-108">For more information, see [Dependency injection into views](xref:mvc/views/dependency-injection).</span></span>

<span data-ttu-id="a6d08-109">Использование внедренного авторизации службы для вызова `AuthorizeAsync` точно таким же образом проводится проверка во время [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="a6d08-109">Use the injected authorization service to invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6d08-110">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6d08-110">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, "PolicyName")).Succeeded)
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6d08-111">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6d08-111">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
{
    <p>This paragraph is displayed because you fulfilled PolicyName.</p>
}
```

---

<span data-ttu-id="a6d08-112">В некоторых случаях ресурс будет модели представления.</span><span class="sxs-lookup"><span data-stu-id="a6d08-112">In some cases, the resource will be your view model.</span></span> <span data-ttu-id="a6d08-113">Вызвать `AuthorizeAsync` точно таким же образом проводится проверка во время [авторизации на основе ресурсов](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span><span class="sxs-lookup"><span data-stu-id="a6d08-113">Invoke `AuthorizeAsync` in exactly the same way you would check during [resource-based authorization](xref:security/authorization/resourcebased#security-authorization-resource-based-imperative):</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="a6d08-114">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="a6d08-114">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
@if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="a6d08-115">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="a6d08-115">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
{
    <p><a class="btn btn-default" role="button"
        href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
}
```

---

<span data-ttu-id="a6d08-116">В приведенном выше коде модель передается в качестве ресурсов, которые необходимо выполнить вычисления политики во внимание.</span><span class="sxs-lookup"><span data-stu-id="a6d08-116">In the preceding code, the model is passed as a resource the policy evaluation should take into consideration.</span></span>

> [!WARNING]
> <span data-ttu-id="a6d08-117">Не полагайтесь на переключение видимости видимость элементов пользовательского интерфейса приложения, что и проверку исключительно авторизации.</span><span class="sxs-lookup"><span data-stu-id="a6d08-117">Don't rely on toggling visibility of your app's UI elements as the sole authorization check.</span></span> <span data-ttu-id="a6d08-118">Скрытие элемента пользовательского интерфейса могут не полностью запретить доступ для связанного действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="a6d08-118">Hiding a UI element may not completely prevent access to its associated controller action.</span></span> <span data-ttu-id="a6d08-119">Например рассмотрим кнопку в предыдущем фрагменте кода.</span><span class="sxs-lookup"><span data-stu-id="a6d08-119">For example, consider the button in the preceding code snippet.</span></span> <span data-ttu-id="a6d08-120">Пользователь может вызвать `Edit` является URL-адрес метода действия, если он знает относительного ресурса */Document/Edit/1*.</span><span class="sxs-lookup"><span data-stu-id="a6d08-120">A user can invoke the `Edit` action method if he or she knows the relative resource URL is */Document/Edit/1*.</span></span> <span data-ttu-id="a6d08-121">По этой причине `Edit` метод действия должен выполнять собственную проверку авторизации.</span><span class="sxs-lookup"><span data-stu-id="a6d08-121">For this reason, the `Edit` action method should perform its own authorization check.</span></span>
