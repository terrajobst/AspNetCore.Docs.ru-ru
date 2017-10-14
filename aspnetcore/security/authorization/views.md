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
ms.openlocfilehash: 58cafcfdc7946e82d1e0ea5de95e0e497b1b6bcf
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="view-based-authorization"></a><span data-ttu-id="02358-103">Авторизация на основе представления</span><span class="sxs-lookup"><span data-stu-id="02358-103">View Based Authorization</span></span>

<a name="security-authorization-views"></a>

<span data-ttu-id="02358-104">Часто разработчик необходимость отображения, скрытия или изменения пользовательского интерфейса на основе идентификатора текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="02358-104">Often a developer will want to show, hide or otherwise modify a UI based on the current user identity.</span></span> <span data-ttu-id="02358-105">Служба авторизации в представлениях MVC через доступна [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="02358-105">You can access the authorization service within MVC views via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection).</span></span> <span data-ttu-id="02358-106">Для добавления службы авторизации в использование представления Razor `@inject` директив, например `@inject IAuthorizationService AuthorizationService` (требуется `@using Microsoft.AspNetCore.Authorization`).</span><span class="sxs-lookup"><span data-stu-id="02358-106">To inject the authorization service into a Razor view use the `@inject` directive, for example `@inject IAuthorizationService AuthorizationService` (requires `@using Microsoft.AspNetCore.Authorization`).</span></span> <span data-ttu-id="02358-107">Если требуется служба авторизации в каждом представлении поместите `@inject` директив в `_ViewImports.cshtml` файла в `Views` каталога.</span><span class="sxs-lookup"><span data-stu-id="02358-107">If you want the authorization service in every view then place the `@inject` directive into the `_ViewImports.cshtml` file in the `Views` directory.</span></span> <span data-ttu-id="02358-108">Дополнительные сведения о внедрение зависимостей в представления. в разделе [внедрение зависимостей в представления](../../mvc/views/dependency-injection.md).</span><span class="sxs-lookup"><span data-stu-id="02358-108">For more information on dependency injection into views see [Dependency injection into views](../../mvc/views/dependency-injection.md).</span></span>

<span data-ttu-id="02358-109">Как только вы внедрили службы авторизации его использовать путем вызова `AuthorizeAsync` метод в точно так же, как проводится проверка во время [авторизации на основе ресурсов](resourcebased.md#security-authorization-resource-based-imperative).</span><span class="sxs-lookup"><span data-stu-id="02358-109">Once you have injected the authorization service you use it by calling the `AuthorizeAsync` method in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative).</span></span>

```cshtml
@if (await AuthorizationService.AuthorizeAsync(User, "PolicyName"))
   {
       <p>This paragraph is displayed because you fulfilled PolicyName.</p>
   }
   ```

<span data-ttu-id="02358-110">В некоторых случаях ресурс будет представление модели и может вызывать `AuthorizeAsync` в точно так же, как проводится проверка во время [авторизации на основе ресурсов](resourcebased.md#security-authorization-resource-based-imperative);</span><span class="sxs-lookup"><span data-stu-id="02358-110">In some cases the resource will be your view model, and you can call `AuthorizeAsync` in exactly the same way as you would check during [resource based authorization](resourcebased.md#security-authorization-resource-based-imperative);</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="02358-111">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="02358-111">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```cshtml
   @if ((await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit)).Succeeded)
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="02358-112">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="02358-112">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

```cshtml
   @if (await AuthorizationService.AuthorizeAsync(User, Model, Operations.Edit))
   {
       <p><a class="btn btn-default" role="button"
           href="@Url.Action("Edit", "Document", new { id = Model.Id })">Edit</a></p>
   }
   ```
---

<span data-ttu-id="02358-113">Здесь вы увидите, что модель передается как авторизации ресурсов необходимо учитывать.</span><span class="sxs-lookup"><span data-stu-id="02358-113">Here you can see the model is passed as the resource authorization should take into consideration.</span></span>

>[!WARNING]
><span data-ttu-id="02358-114">Не следует полагаться на отображение и скрытие частей пользовательского интерфейса в качестве единственного метода авторизации.</span><span class="sxs-lookup"><span data-stu-id="02358-114">Do not rely on showing or hiding parts of your UI as your only authorization method.</span></span> <span data-ttu-id="02358-115">Скрытие пользовательский Интерфейс элемента не означает, что пользователь не может получить доступ к.</span><span class="sxs-lookup"><span data-stu-id="02358-115">Hiding a UI element does not mean a user cannot access it.</span></span> <span data-ttu-id="02358-116">Пользователь также необходимо авторизовать в коде контроллера.</span><span class="sxs-lookup"><span data-stu-id="02358-116">You must also authorize the user within your controller code.</span></span>
