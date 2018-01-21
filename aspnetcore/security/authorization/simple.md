---
title: "Простой авторизации"
author: rick-anderson
description: "В этом документе объясняется, как использовать атрибут авторизации для ограничения доступа к ASP.NET Core контроллеров и действий."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/simple
ms.openlocfilehash: f1d5671785da815f2f4fcf5bef1352f4c9e62877
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="simple-authorization"></a>Простой авторизации

<a name="security-authorization-simple"></a>

Авторизация в MVC контролируется `AuthorizeAttribute` атрибута и его различные параметры. В самом простом случае применение `AuthorizeAttribute` контроллер или действие позволяет ограничить доступ к контроллеру или действие, любой прошедший проверку пользователь.

Например, следующий код ограничивает доступ к `AccountController` всем авторизованным пользователям.

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

Если требуется применять проверку подлинности для действия, а не контроллера применить `AuthorizeAttribute` атрибут само действие:

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

Теперь только прошедшие проверку подлинности пользователи могут обращаться к `Logout` функции.

Можно также использовать `AllowAnonymousAttribute` атрибут, чтобы разрешить доступ без проверки подлинности пользователей для отдельных операций. Пример:

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

Это позволит только прошедшие проверку подлинности пользователи `AccountController`, за исключением `Login` действия, который доступен всем пользователям, независимо от их состояния, прошедшего проверку подлинности или не прошедшие проверку подлинности и анонимный.

>[!WARNING]
> `[AllowAnonymous]`пропускает все инструкции авторизации. Если применить объединение `[AllowAnonymous]` , а также `[Authorize]` атрибута, а затем авторизовать атрибуты всегда будет игнорироваться. Например, если применить `[AllowAnonymous]` на контроллере уровне любой `[Authorize]` атрибуты на одном контроллере или на любое действие в ней будут игнорироваться.
