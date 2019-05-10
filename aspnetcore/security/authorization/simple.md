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
# <a name="simple-authorization-in-aspnet-core"></a>Простая авторизация в ASP.NET Core

<a name="security-authorization-simple"></a>

Авторизация в MVC осуществляется с помощью `AuthorizeAttribute` атрибут и его различные параметры. В простейшем случае применение `AuthorizeAttribute` атрибута контроллера или действия ограничения доступа к контроллеру или действие, чтобы любой прошедший проверку пользователь.

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

Если вы хотите применить авторизации действие, а не на контроллере, примените `AuthorizeAttribute` атрибут у самого действия:

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

Теперь доступны только аутентифицированным пользователям `Logout` функции.

Можно также использовать `AllowAnonymous` атрибут, чтобы разрешить доступ без проверки подлинности пользователей, к отдельным действиям. Пример:

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

Это позволит только пользователям, прошедшим проверку `AccountController`, за исключением `Login` действие, которое доступно всем пользователям, независимо от их состояния, прошедшего проверку подлинности или не прошедшие проверку подлинности / анонимный.

> [!WARNING]
> `[AllowAnonymous]` обходит все инструкции авторизации. Если объединить `[AllowAnonymous]` , а также `[Authorize]` атрибут, `[Authorize]` атрибуты учитываются. Например, если применить `[AllowAnonymous]` на уровне контроллера, любой `[Authorize]` атрибуты на одном контроллере (или на любое действие в нем) учитывается.
