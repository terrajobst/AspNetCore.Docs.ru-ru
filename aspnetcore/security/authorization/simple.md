---
title: Простой авторизации в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать атрибут авторизации для ограничения доступа к ASP.NET Core контроллеров и действий.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 3c5e9d5dfd65ded40c9828a666143c1868f5562f
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36272069"
---
# <a name="simple-authorization-in-aspnet-core"></a>Простой авторизации в ASP.NET Core

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

Можно также использовать `AllowAnonymous` атрибут, чтобы разрешить доступ без проверки подлинности пользователей для отдельных операций. Пример:

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
> `[AllowAnonymous]` пропускает все инструкции авторизации. Если применить объединение `[AllowAnonymous]` , а также `[Authorize]` атрибута, а затем авторизовать атрибуты всегда будет игнорироваться. Например, если применить `[AllowAnonymous]` на контроллере уровне любой `[Authorize]` атрибуты на одном контроллере или на любое действие в ней будут игнорироваться.
