---
title: Авторизация на основе ролей в ASP.NET Core
author: rick-anderson
description: Узнайте, как ограничить доступ ASP.NET Core контроллера и действия, передача роли к атрибуту Authorize.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/roles
ms.openlocfilehash: f1e7209cae1e2a58ad536547d655dd744ca0d3f7
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/03/2018
ms.locfileid: "32740040"
---
# <a name="role-based-authorization-in-aspnet-core"></a><span data-ttu-id="64f8e-103">Авторизация на основе ролей в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="64f8e-103">Role-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="64f8e-104">При создании удостоверения может принадлежать одной или нескольких ролей.</span><span class="sxs-lookup"><span data-stu-id="64f8e-104">When an identity is created it may belong to one or more roles.</span></span> <span data-ttu-id="64f8e-105">Например Трейси может принадлежать роли администраторов и пользователей в процессе Скотт может относиться только к роли пользователя.</span><span class="sxs-lookup"><span data-stu-id="64f8e-105">For example, Tracy may belong to the Administrator and User roles whilst Scott may only belong to the User role.</span></span> <span data-ttu-id="64f8e-106">Как эти роли создаются и управляются зависит от процесса авторизации резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="64f8e-106">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="64f8e-107">Роли доступны разработчику через [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) метод [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) класса.</span><span class="sxs-lookup"><span data-stu-id="64f8e-107">Roles are exposed to the developer through the [IsInRole](/dotnet/api/system.security.principal.genericprincipal.isinrole) method on the [ClaimsPrincipal](/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="64f8e-108">Добавление проверки роли</span><span class="sxs-lookup"><span data-stu-id="64f8e-108">Adding role checks</span></span>

<span data-ttu-id="64f8e-109">Декларативных проверок авторизации на основе ролей&mdash;разработчик внедряет их в свой код от контроллера или действия в контроллере, определения ролей, которые текущий пользователь должен быть членом для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="64f8e-109">Role-based authorization checks are declarative&mdash;the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="64f8e-110">Например, следующий код ограничивает доступ к действиям на `AdministrationController` для пользователей, которые являются членом `Administrator` роли:</span><span class="sxs-lookup"><span data-stu-id="64f8e-110">For example, the following code limits access to any actions on the `AdministrationController` to users who are a member of the `Administrator` role:</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="64f8e-111">Можно указать несколько ролей в виде списка с разделителями-запятыми:</span><span class="sxs-lookup"><span data-stu-id="64f8e-111">You can specify multiple roles as a comma separated list:</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="64f8e-112">Этот контроллер доступны только пользователями, которые являются членами объекта `HRManager` роли или `Finance` роли.</span><span class="sxs-lookup"><span data-stu-id="64f8e-112">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="64f8e-113">Если применить несколько атрибутов, то пользователь должен быть членом всех ролей, указанных; Следующий пример требует, что пользователь должен быть членом как `PowerUser` и `ControlPanelUser` роли.</span><span class="sxs-lookup"><span data-stu-id="64f8e-113">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="64f8e-114">Можно ограничить доступ, применив атрибуты авторизации дополнительных ролей на уровне действия:</span><span class="sxs-lookup"><span data-stu-id="64f8e-114">You can further limit access by applying additional role authorization attributes at the action level:</span></span>

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

<span data-ttu-id="64f8e-115">В предыдущем коде фрагмент членах `Administrator` роли или `PowerUser` роли контроллера и `SetTime` действия, но только члены `Administrator` роли `ShutDown` действие.</span><span class="sxs-lookup"><span data-stu-id="64f8e-115">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="64f8e-116">Также можно заблокировать контроллера, но Разрешить анонимные, не прошедшие проверку подлинности для отдельных операций.</span><span class="sxs-lookup"><span data-stu-id="64f8e-116">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

<a name="security-authorization-role-policy"></a>

## <a name="policy-based-role-checks"></a><span data-ttu-id="64f8e-117">Проверки роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="64f8e-117">Policy based role checks</span></span>

<span data-ttu-id="64f8e-118">Требования к роли может быть также выражен в новом синтаксисе политики с помощью которой разработчик регистрирует политики при запуске в процессе настройки службы авторизации.</span><span class="sxs-lookup"><span data-stu-id="64f8e-118">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="64f8e-119">Это обычно происходит в `ConfigureServices()` в ваш *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="64f8e-119">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="64f8e-120">Политики применяются с помощью `Policy` свойство `AuthorizeAttribute` атрибута:</span><span class="sxs-lookup"><span data-stu-id="64f8e-120">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute:</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="64f8e-121">Если требуется указать несколько ролей, разрешенных требований, то их можно указать в качестве параметров для `RequireRole` метод:</span><span class="sxs-lookup"><span data-stu-id="64f8e-121">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method:</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="64f8e-122">В этом примере разрешает пользователям, принадлежащим к `Administrator`, `PowerUser` или `BackupAdministrator` ролей.</span><span class="sxs-lookup"><span data-stu-id="64f8e-122">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
