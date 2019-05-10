---
title: Простая авторизация в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать атрибут Authorize для ограничения доступа к ASP.NET Core контроллеров и действий.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897681"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="565e4-103">Простая авторизация в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="565e4-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="565e4-104">Авторизация в MVC осуществляется с помощью `AuthorizeAttribute` атрибут и его различные параметры.</span><span class="sxs-lookup"><span data-stu-id="565e4-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="565e4-105">В простейшем случае применение `AuthorizeAttribute` атрибута контроллера или действия ограничения доступа к контроллеру или действие, чтобы любой прошедший проверку пользователь.</span><span class="sxs-lookup"><span data-stu-id="565e4-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="565e4-106">Например, следующий код ограничивает доступ к `AccountController` всем авторизованным пользователям.</span><span class="sxs-lookup"><span data-stu-id="565e4-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="565e4-107">Если вы хотите применить авторизации действие, а не на контроллере, примените `AuthorizeAttribute` атрибут у самого действия:</span><span class="sxs-lookup"><span data-stu-id="565e4-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

```csharp
public class AccountController : Controller
{
   public ActionResult Login()
   {
   }

   [Authorize]
   public ActionResult Logout()
   {
   }
}
```

<span data-ttu-id="565e4-108">Теперь доступны только аутентифицированным пользователям `Logout` функции.</span><span class="sxs-lookup"><span data-stu-id="565e4-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="565e4-109">Можно также использовать `AllowAnonymous` атрибут, чтобы разрешить доступ без проверки подлинности пользователей, к отдельным действиям.</span><span class="sxs-lookup"><span data-stu-id="565e4-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="565e4-110">Пример:</span><span class="sxs-lookup"><span data-stu-id="565e4-110">For example:</span></span>

```csharp
[Authorize]
public class AccountController : Controller
{
    [AllowAnonymous]
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

<span data-ttu-id="565e4-111">Это позволит только пользователям, прошедшим проверку `AccountController`, за исключением `Login` действие, которое доступно всем пользователям, независимо от их состояния, прошедшего проверку подлинности или не прошедшие проверку подлинности / анонимный.</span><span class="sxs-lookup"><span data-stu-id="565e4-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="565e4-112">`[AllowAnonymous]` обходит все инструкции авторизации.</span><span class="sxs-lookup"><span data-stu-id="565e4-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="565e4-113">Если объединить `[AllowAnonymous]` , а также `[Authorize]` атрибут, `[Authorize]` атрибуты учитываются.</span><span class="sxs-lookup"><span data-stu-id="565e4-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="565e4-114">Например, если применить `[AllowAnonymous]` на уровне контроллера, любой `[Authorize]` атрибуты на одном контроллере (или на любое действие в нем) учитывается.</span><span class="sxs-lookup"><span data-stu-id="565e4-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
