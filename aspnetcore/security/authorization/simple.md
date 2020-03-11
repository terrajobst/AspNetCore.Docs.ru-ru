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
# <a name="simple-authorization-in-aspnet-core"></a>Простая Авторизация в ASP.NET Core

<a name="security-authorization-simple"></a>

Авторизация в MVC контролируется с помощью атрибута `AuthorizeAttribute` и его различных параметров. В самом простом случае применение атрибута `AuthorizeAttribute` к контроллеру или действию ограничивает доступ к контроллеру или действию для любого пользователя, прошедшего проверку подлинности.

Например, следующий код ограничивает доступ к `AccountController` любому пользователю, прошедшему проверку подлинности.

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

Если вы хотите применить авторизацию к действию, а не к контроллеру, примените атрибут `AuthorizeAttribute` к самому действию:

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

Теперь только пользователи, прошедшие проверку подлинности, могут получить доступ к функции `Logout`.

Кроме того, можно использовать атрибут `AllowAnonymous`, чтобы разрешить пользователям, не прошедшим проверку подлинности, доступ к отдельным действиям. Пример:

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

Это позволит только пользователям, прошедшим проверку подлинности, `AccountController`, за исключением действия `Login`, доступного всем, независимо от их состояния, прошедшего проверку подлинности или без проверки подлинности.

> [!WARNING]
> `[AllowAnonymous]` обходит все инструкции авторизации. При объединении `[AllowAnonymous]` и любого `[Authorize]` атрибута атрибуты `[Authorize]` игнорируются. Например, при применении `[AllowAnonymous]` на уровне контроллера все атрибуты `[Authorize]` на том же контроллере (или в любом действии в нем) игнорируются.
