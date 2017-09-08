---
title: "Авторизация на основе ролей"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 5e014da1-8bc0-409b-951a-88b92c661fdf
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/roles
ms.openlocfilehash: d8dfcbb16ee7977d197b019c4e5e1b30fff17755
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="role-based-authorization"></a>Авторизация на основе ролей

<a name=security-authorization-role-based></a>

При создании удостоверения может принадлежать одной или нескольких ролей, например Трейси может принадлежать к роли администраторов и пользователей одновременным Скотт может относиться только к роли пользователя. Как эти роли создаются и управляются зависит от процесса авторизации резервного хранилища. Роли доступны разработчику через [IsInRole](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal.isinrole(v=vs.110).aspx) свойство [ClaimsPrincipal](https://msdn.microsoft.com/library/system.security.claims.claimsprincipal(v=vs.110).aspx) класса.

## <a name="adding-role-checks"></a>Добавление проверки роли

Декларативных проверок авторизации на основе ролей — разработчик внедряет их в свой код от контроллера или действия в контроллере, определения ролей, которые текущий пользователь должен быть членом для доступа к запрошенному ресурсу.

Например следующий код ограничить доступ ко всем действиям в `AdministrationController` для пользователей, которые являются членом `Administrator` группы.

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

Можно указать несколько ролей в виде списка с разделителями-запятыми;

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

Этот контроллер доступны только пользователями, которые являются членами объекта `HRManager` роли или `Finance` роли.

Если применить несколько атрибутов, то пользователь должен быть членом всех ролей, указанных; Следующий пример требует, что пользователь должен быть членом как `PowerUser` и `ControlPanelUser` роли.

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

Можно ограничить доступ, применив атрибуты авторизации дополнительных ролей на уровне действия.

```csharp
[Authorize(Roles = "Administrator, PowerUser")]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [Authorize(Roles = "Administrator")]
    public ActionResult ShutDown()
    {
    }
}
```

В предыдущем коде фрагмент членах `Administrator` роли или `PowerUser` роли контроллера и `SetTime` действия, но только члены `Administrator` роли `ShutDown` действие.

Также можно заблокировать контроллера, но Разрешить анонимные, не прошедшие проверку подлинности для отдельных операций.

```csharp
[Authorize]
public class ControlPanelController : Controller
{
    public ActionResult SetTime()
    {
    }

    [AllowAnonymous]
    public ActionResult Login()
    {
    }
}
```

<a name=security-authorization-role-policy></a>

## <a name="policy-based-role-checks"></a>Проверки роли на основе политик

Требования к роли может быть также выражен в новом синтаксисе политики с помощью которой разработчик регистрирует политики при запуске в процессе настройки службы авторизации. Это обычно происходит в `ConfigureServices()` в ваш *файла Startup.cs* файл.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("RequireAdministratorRole", policy => policy.RequireRole("Administrator"));
    });
}
```

Политики применяются с помощью `Policy` свойство `AuthorizeAttribute` по умолчанию.

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

Если требуется указать несколько ролей, разрешенных требований, то их можно указать в качестве параметров для `RequireRole` метода.

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

В этом примере разрешает пользователям, принадлежащим к `Administrator`, `PowerUser` или `BackupAdministrator` ролей.
