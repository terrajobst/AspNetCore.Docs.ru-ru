---
title: Простой авторизации в ASP.NET Core
author: rick-anderson
description: Узнайте, как использовать атрибут авторизации для ограничения доступа к ASP.NET Core контроллеров и действий.
ms.author: riande
ms.date: 10/14/2016
uid: security/authorization/simple
ms.openlocfilehash: 6409def0508b855d3d2a4a1f4d3a3d15bfe5dd32
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961127"
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

> [!WARNING]
> `[AllowAnonymous]` пропускает все инструкции авторизации. При сочетании `[AllowAnonymous]` , а также `[Authorize]` атрибута `[Authorize]` атрибуты игнорируются. Например, если применить `[AllowAnonymous]` на уровне контроллера любой `[Authorize]` атрибутов на одном контроллере (или на любое действие в ней) игнорируется.
