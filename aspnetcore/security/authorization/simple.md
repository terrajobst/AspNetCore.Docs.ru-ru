---
title: "Простой авторизации"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 391bcaad-205f-43e4-badc-fa592d6f79f3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: 91f402b1cbf73e212418d197a8a7230ce22e9e1d
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="simple-authorization"></a><span data-ttu-id="0b730-103">Простой авторизации</span><span class="sxs-lookup"><span data-stu-id="0b730-103">Simple Authorization</span></span>

<a name="security-authorization-simple"></a>

<span data-ttu-id="0b730-104">Авторизация в MVC контролируется `AuthorizeAttribute` атрибута и его различные параметры.</span><span class="sxs-lookup"><span data-stu-id="0b730-104">Authorization in MVC is controlled through the `AuthorizeAttribute` attribute and its various parameters.</span></span> <span data-ttu-id="0b730-105">В простейшей применение `AuthorizeAttribute` контроллер или действие позволяет ограничить доступ к контроллеру или действие, любой прошедший проверку пользователь.</span><span class="sxs-lookup"><span data-stu-id="0b730-105">At its simplest applying the `AuthorizeAttribute` attribute to a controller or action limits access to the controller or action to any authenticated user.</span></span>

<span data-ttu-id="0b730-106">Например, следующий код ограничивает доступ к `AccountController` всем авторизованным пользователям.</span><span class="sxs-lookup"><span data-stu-id="0b730-106">For example, the following code limits access to the `AccountController` to any authenticated user.</span></span>

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

<span data-ttu-id="0b730-107">Если требуется применять проверку подлинности для действия, а не контроллера просто применить `AuthorizeAttribute` атрибут действие;</span><span class="sxs-lookup"><span data-stu-id="0b730-107">If you want to apply authorization to an action rather than the controller simply apply the `AuthorizeAttribute` attribute to the action itself;</span></span>

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

<span data-ttu-id="0b730-108">Теперь только прошедшие проверку подлинности пользователи могут получить доступ к функция logout.</span><span class="sxs-lookup"><span data-stu-id="0b730-108">Now only authenticated users can access the logout function.</span></span>

<span data-ttu-id="0b730-109">Можно также использовать `AllowAnonymousAttribute` атрибут, чтобы разрешить доступ без проверки подлинности пользователей на отдельные действия; например</span><span class="sxs-lookup"><span data-stu-id="0b730-109">You can also use the `AllowAnonymousAttribute` attribute to allow access by non-authenticated users to individual actions; for example</span></span>

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

<span data-ttu-id="0b730-110">Это позволит только прошедшие проверку подлинности пользователи `AccountController`, за исключением `Login` действия, который доступен всем пользователям, независимо от их состояния, прошедшего проверку подлинности или не прошедшие проверку подлинности и анонимный.</span><span class="sxs-lookup"><span data-stu-id="0b730-110">This would allow only authenticated users to the `AccountController`, except for the `Login` action, which is accessible by everyone, regardless of their authenticated or unauthenticated / anonymous status.</span></span>

>[!WARNING]
> <span data-ttu-id="0b730-111">`[AllowAnonymous]`пропускает все инструкции авторизации.</span><span class="sxs-lookup"><span data-stu-id="0b730-111">`[AllowAnonymous]` bypasses all authorization statements.</span></span> <span data-ttu-id="0b730-112">Если применить объединение `[AllowAnonymous]` , а также `[Authorize]` атрибута, а затем авторизовать атрибуты всегда будет игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="0b730-112">If you apply combine `[AllowAnonymous]` and any `[Authorize]` attribute then the Authorize attributes will always be ignored.</span></span> <span data-ttu-id="0b730-113">Например, если применить `[AllowAnonymous]` на контроллере уровне любой `[Authorize]` атрибуты на одном контроллере или на любое действие в ней будут игнорироваться.</span><span class="sxs-lookup"><span data-stu-id="0b730-113">For example if you apply `[AllowAnonymous]` at the controller level any `[Authorize]` attributes on the same controller, or on any action within it will be ignored.</span></span>
