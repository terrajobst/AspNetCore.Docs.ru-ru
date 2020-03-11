---
title: Простая Авторизация в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать атрибут авторизации для ограничения доступа к ASP.NET Core контроллерам и действиям.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78653560"
---
# <a name="simple-authorization-in-aspnet-core"></a><span data-ttu-id="be818-103">Простая Авторизация в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="be818-103">Simple authorization in ASP.NET Core</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="be818-104">Авторизация в MVC контролируется с помощью атрибута `AuthorizeAttribute` и его различных параметров.</span><span class="sxs-lookup"><span data-stu-id="be818-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="be818-105">В самом простом случае применение атрибута `AuthorizeAttribute` к контроллеру или действию ограничивает доступ к контроллеру или действию для любого пользователя, прошедшего проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="be818-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="be818-106">Например, следующий код ограничивает доступ к `AccountController` любому пользователю, прошедшему проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="be818-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="be818-107">Если вы хотите применить авторизацию к действию, а не к контроллеру, примените атрибут `AuthorizeAttribute` к самому действию:</span><span class="sxs-lookup"><span data-stu-id="be818-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="be818-108">Теперь только пользователи, прошедшие проверку подлинности, могут получить доступ к функции `Logout`.</span><span class="sxs-lookup"><span data-stu-id="be818-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="be818-109">Кроме того, можно использовать атрибут `AllowAnonymous`, чтобы разрешить пользователям, не прошедшим проверку подлинности, доступ к отдельным действиям.</span><span class="sxs-lookup"><span data-stu-id="be818-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="be818-110">Пример:</span><span class="sxs-lookup"><span data-stu-id="be818-110">For example:</span></span>

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

<span data-ttu-id="be818-111">Это позволит только пользователям, прошедшим проверку подлинности, `AccountController`, за исключением действия `Login`, доступного всем, независимо от их состояния, прошедшего проверку подлинности или без проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="be818-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

> [!WARNING]
> <span data-ttu-id="be818-112">`[AllowAnonymous]` обходит все инструкции авторизации.</span><span class="sxs-lookup"><span data-stu-id="be818-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="be818-113">При объединении `[AllowAnonymous]` и любого `[Authorize]` атрибута атрибуты `[Authorize]` игнорируются.</span><span class="sxs-lookup"><span data-stu-id="be818-113">If you combine `[AllowAnonymous]` and any `[Authorize]` attribute, the `[Authorize]` attributes are ignored.</span></span> <span data-ttu-id="be818-114">Например, при применении `[AllowAnonymous]` на уровне контроллера все атрибуты `[Authorize]` на том же контроллере (или в любом действии в нем) игнорируются.</span><span class="sxs-lookup"><span data-stu-id="be818-114">For example if you apply `[AllowAnonymous]` at the controller level, any `[Authorize]` attributes on the same controller (or on any action within it) is ignored.</span></span>
