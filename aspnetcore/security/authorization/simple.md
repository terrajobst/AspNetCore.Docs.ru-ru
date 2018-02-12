---
title: "Простой авторизации"
author: rick-anderson
description: "В этом документе объясняется, как использовать атрибут авторизации для ограничения доступа к ASP.NET Core контроллеров и действий."
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/simple
ms.openlocfilehash: 503ebc665efd460a85f49844ddc847eb12114308
ms.sourcegitcommit: b83a5f731a9c02bdb1cc1e3f9a8bf273eb5b33e0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/11/2018
---
# <a name="simple-authorization"></a><span data-ttu-id="2b2cf-103">Простой авторизации</span><span class="sxs-lookup"><span data-stu-id="2b2cf-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="2b2cf-104">Авторизация в MVC контролируется `AuthorizeAttribute` атрибута и его различные параметры.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="2b2cf-105">В самом простом случае применение `AuthorizeAttribute` контроллер или действие позволяет ограничить доступ к контроллеру или действие, любой прошедший проверку пользователь.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-105">At its simplest, applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="2b2cf-106">Например, следующий код ограничивает доступ к `AccountController` всем авторизованным пользователям.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="2b2cf-107">Если требуется применять проверку подлинности для действия, а не контроллера применить `AuthorizeAttribute` атрибут само действие:</span><span class="sxs-lookup"><span data-stu-id="2b2cf-107">If you want to apply authorization to an action rather than the controller, apply the `AuthorizeAttribute` attribute to the action itself:</span></span>

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

<span data-ttu-id="2b2cf-108">Теперь только прошедшие проверку подлинности пользователи могут обращаться к `Logout` функции.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-108">Now only authenticated users can access the `Logout` function.</span></span>

<span data-ttu-id="2b2cf-109">Можно также использовать `AllowAnonymous` атрибут, чтобы разрешить доступ без проверки подлинности пользователей для отдельных операций.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-109">You can also use the `AllowAnonymous` attribute to allow access by non-authenticated users to individual actions.</span></span> <span data-ttu-id="2b2cf-110">Пример:</span><span class="sxs-lookup"><span data-stu-id="2b2cf-110">For example:</span></span>

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

<span data-ttu-id="2b2cf-111">Это позволит только прошедшие проверку подлинности пользователи `AccountController`, за исключением `Login` действия, который доступен всем пользователям, независимо от их состояния, прошедшего проверку подлинности или не прошедшие проверку подлинности и анонимный.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-111">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="2b2cf-112">`[AllowAnonymous]`пропускает все инструкции авторизации.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-112">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="2b2cf-113">Если применить объединение `[AllowAnonymous]` , а также `[Authorize]` атрибута, а затем авторизовать атрибуты всегда будет игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-113">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="2b2cf-114">Например, если применить `[AllowAnonymous]` на контроллере уровне любой `[Authorize]` атрибуты на одном контроллере или на любое действие в ней будут игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="2b2cf-114">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
