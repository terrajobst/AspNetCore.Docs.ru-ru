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
ms.openlocfilehash: 3dfba8a492c5d592b1fa9c8893a0ec4b1a2e70b9
ms.sourcegitcommit: 8f4d4fad1ca27adf9e396f5c205c9875a3963664
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="role-based-authorization"></a><span data-ttu-id="a8c7f-103">Авторизация на основе ролей</span><span class="sxs-lookup"><span data-stu-id="a8c7f-103">Role based Authorization</span></span>

<a name="security-authorization-role-based"></a>

<span data-ttu-id="a8c7f-104">При создании удостоверения может принадлежать одной или нескольких ролей, например Трейси может принадлежать к роли администраторов и пользователей одновременным Скотт может относиться только к роли пользователя.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-104">When an identity is created it may belong to one or more roles, for example Tracy may belong to the Administrator and User roles whilst Scott may only belong to the user role.</span></span> <span data-ttu-id="a8c7f-105">Как эти роли создаются и управляются зависит от процесса авторизации резервного хранилища.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-105">How these roles are created and managed depends on the backing store of the authorization process.</span></span> <span data-ttu-id="a8c7f-106">Роли доступны разработчику через [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) свойство [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) класса.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-106">Roles are exposed to the developer through the [IsInRole](https://docs.microsoft.com/dotnet/api/system.security.principal.genericprincipal.isinrole) property on the [ClaimsPrincipal](https://docs.microsoft.com/dotnet/api/system.security.claims.claimsprincipal) class.</span></span>

## <a name="adding-role-checks"></a><span data-ttu-id="a8c7f-107">Добавление проверки роли</span><span class="sxs-lookup"><span data-stu-id="a8c7f-107">Adding role checks</span></span>

<span data-ttu-id="a8c7f-108">Декларативных проверок авторизации на основе ролей — разработчик внедряет их в свой код от контроллера или действия в контроллере, определения ролей, которые текущий пользователь должен быть членом для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-108">Role based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying roles which the current user must be a member of to access the requested resource.</span></span>

<span data-ttu-id="a8c7f-109">Например следующий код ограничить доступ ко всем действиям в `AdministrationController` для пользователей, которые являются членом `Administrator` группы.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-109">For example the following code would limit access to any actions on the `AdministrationController` to users who are a member of the `Administrator` group.</span></span>

```csharp
[Authorize(Roles = "Administrator")]
public class AdministrationController : Controller
{
}
```

<span data-ttu-id="a8c7f-110">Можно указать несколько ролей в виде списка с разделителями-запятыми;</span><span class="sxs-lookup"><span data-stu-id="a8c7f-110">You can specify multiple roles as a comma separated list;</span></span>

```csharp
[Authorize(Roles = "HRManager,Finance")]
public class SalaryController : Controller
{
}
```

<span data-ttu-id="a8c7f-111">Этот контроллер доступны только пользователями, которые являются членами объекта `HRManager` роли или `Finance` роли.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-111">This controller would be only accessible by users who are members of the `HRManager` role or the `Finance` role.</span></span>

<span data-ttu-id="a8c7f-112">Если применить несколько атрибутов, то пользователь должен быть членом всех ролей, указанных; Следующий пример требует, что пользователь должен быть членом как `PowerUser` и `ControlPanelUser` роли.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-112">If you apply multiple attributes then an accessing user must be a member of all the roles specified; the following sample requires that a user must be a member of both the `PowerUser` and `ControlPanelUser` role.</span></span>

```csharp
[Authorize(Roles = "PowerUser")]
[Authorize(Roles = "ControlPanelUser")]
public class ControlPanelController : Controller
{
}
```

<span data-ttu-id="a8c7f-113">Можно ограничить доступ, применив атрибуты авторизации дополнительных ролей на уровне действия.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-113">You can further limit access by applying additional role authorization attributes at the action level;</span></span>

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

<span data-ttu-id="a8c7f-114">В предыдущем коде фрагмент членах `Administrator` роли или `PowerUser` роли контроллера и `SetTime` действия, но только члены `Administrator` роли `ShutDown` действие.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-114">In the previous code snippet members of the `Administrator` role or the `PowerUser` role can access the controller and the `SetTime` action, but only members of the `Administrator` role can access the `ShutDown` action.</span></span>

<span data-ttu-id="a8c7f-115">Также можно заблокировать контроллера, но Разрешить анонимные, не прошедшие проверку подлинности для отдельных операций.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-115">You can also lock down a controller but allow anonymous, unauthenticated access to individual actions.</span></span>

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

## <a name="policy-based-role-checks"></a><span data-ttu-id="a8c7f-116">Проверки роли на основе политик</span><span class="sxs-lookup"><span data-stu-id="a8c7f-116">Policy based role checks</span></span>

<span data-ttu-id="a8c7f-117">Требования к роли может быть также выражен в новом синтаксисе политики с помощью которой разработчик регистрирует политики при запуске в процессе настройки службы авторизации.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-117">Role requirements can also be expressed using the new Policy syntax, where a developer registers a policy at startup as part of the Authorization service configuration.</span></span> <span data-ttu-id="a8c7f-118">Это обычно происходит в `ConfigureServices()` в ваш *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-118">This normally occurs in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="a8c7f-119">Политики применяются с помощью `Policy` свойство `AuthorizeAttribute` по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-119">Policies are applied using the `Policy` property on the `AuthorizeAttribute` attribute;</span></span>

```csharp
[Authorize(Policy = "RequireAdministratorRole")]
public IActionResult Shutdown()
{
    return View();
}
```

<span data-ttu-id="a8c7f-120">Если требуется указать несколько ролей, разрешенных требований, то их можно указать в качестве параметров для `RequireRole` метода.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-120">If you want to specify multiple allowed roles in a requirement then you can specify them as parameters to the `RequireRole` method;</span></span>

```csharp
options.AddPolicy("ElevatedRights", policy =>
                  policy.RequireRole("Administrator", "PowerUser", "BackupAdministrator"));
```

<span data-ttu-id="a8c7f-121">В этом примере разрешает пользователям, принадлежащим к `Administrator`, `PowerUser` или `BackupAdministrator` ролей.</span><span class="sxs-lookup"><span data-stu-id="a8c7f-121">This example authorizes users who belong to the `Administrator`, `PowerUser` or `BackupAdministrator` roles.</span></span>
