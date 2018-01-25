---
title: "Авторизация на основе утверждений"
author: rick-anderson
description: "В этом документе описывается добавление проверки утверждений для авторизации в приложении ASP.NET Core."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: 870bdf79abc2c94745ab5da6997a37ed0e4ea4e2
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="claims-based-authorization"></a><span data-ttu-id="06d7d-103">Авторизация на основе утверждений</span><span class="sxs-lookup"><span data-stu-id="06d7d-103">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="06d7d-104">При создании удостоверение может быть присвоено одно или несколько утверждений, выданных доверенной стороной.</span><span class="sxs-lookup"><span data-stu-id="06d7d-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="06d7d-105">Утверждение-это имя значение пара, которая представляет какие субъекта, можно сделать, не какие субъекта.</span><span class="sxs-lookup"><span data-stu-id="06d7d-105">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="06d7d-106">Например имеется водительское удостоверение, выданный центром локального определяющим лицензии.</span><span class="sxs-lookup"><span data-stu-id="06d7d-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="06d7d-107">Ваше водительское удостоверение есть дата рождения.</span><span class="sxs-lookup"><span data-stu-id="06d7d-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="06d7d-108">В этом случае будет имя утверждения `DateOfBirth`, значение утверждения будет дата рождения, например `8th June 1970` и издатель будет определяющим центра лицензии.</span><span class="sxs-lookup"><span data-stu-id="06d7d-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="06d7d-109">Авторизация на основе утверждений, в самом простом случае проверяет значение утверждения и разрешает доступ к ресурсу на основе этого значения.</span><span class="sxs-lookup"><span data-stu-id="06d7d-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="06d7d-110">Например, если требуется доступ к сообщество ночь процесс авторизации может быть:</span><span class="sxs-lookup"><span data-stu-id="06d7d-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="06d7d-111">Сотрудник отдела безопасности дверца будет возвращаться значение даты рождения утверждения и ли они доверять издателя (определяющим центра лицензии) перед предоставлением доступа.</span><span class="sxs-lookup"><span data-stu-id="06d7d-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="06d7d-112">Удостоверение может содержать несколько утверждений с несколькими значениями и может содержать несколько утверждений того же типа.</span><span class="sxs-lookup"><span data-stu-id="06d7d-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="06d7d-113">Добавление утверждений проверки</span><span class="sxs-lookup"><span data-stu-id="06d7d-113">Adding claims checks</span></span>

<span data-ttu-id="06d7d-114">Утверждения проверки авторизации на основе декларативного — разработчик внедряет их в свой код от контроллера или действия в контроллере, указав утверждения, которые текущий пользователь должен обладать и при необходимости выполнять значение утверждения для доступа к Запрошенный ресурс.</span><span class="sxs-lookup"><span data-stu-id="06d7d-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="06d7d-115">Утверждения, что требования, на основе политики, разработчику необходимо построить и зарегистрировать программные требования утверждений политики.</span><span class="sxs-lookup"><span data-stu-id="06d7d-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="06d7d-116">Простейший тип утверждения политики выглядит на наличие утверждения и не проверяет значение.</span><span class="sxs-lookup"><span data-stu-id="06d7d-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="06d7d-117">Сначала необходимо для создания и регистрации политики.</span><span class="sxs-lookup"><span data-stu-id="06d7d-117">First you need to build and register the policy.</span></span> <span data-ttu-id="06d7d-118">Это происходит в процессе настройки службы авторизации, которой обычно принимает участие в `ConfigureServices()` в ваш *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="06d7d-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("EmployeeOnly", policy => policy.RequireClaim("EmployeeNumber"));
    });
}
```

<span data-ttu-id="06d7d-119">В этом случае `EmployeeOnly` политика проверяет наличие `EmployeeNumber` утверждения для текущего удостоверения.</span><span class="sxs-lookup"><span data-stu-id="06d7d-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="06d7d-120">Затем примените политику с помощью `Policy` свойство `AuthorizeAttribute` атрибута, чтобы указать имя политики;</span><span class="sxs-lookup"><span data-stu-id="06d7d-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="06d7d-121">`AuthorizeAttribute` Атрибут может применяться к контроллеру целиком в данном экземпляре только политика сопоставления удостоверений будет разрешен доступ к любому действию на контроллере.</span><span class="sxs-lookup"><span data-stu-id="06d7d-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="06d7d-122">При наличии контроллера, который защищен службой `AuthorizeAttribute` атрибута, но хотите разрешить анонимный доступ для применения каких-либо действий `AllowAnonymousAttribute` атрибута.</span><span class="sxs-lookup"><span data-stu-id="06d7d-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }

    [AllowAnonymous]
    public ActionResult VacationPolicy()
    {
    }
}
```

<span data-ttu-id="06d7d-123">Большинство утверждений имеют значение.</span><span class="sxs-lookup"><span data-stu-id="06d7d-123">Most claims come with a value.</span></span> <span data-ttu-id="06d7d-124">При создании политики можно указать список допустимых значений.</span><span class="sxs-lookup"><span data-stu-id="06d7d-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="06d7d-125">Следующий пример бы только успешно для сотрудников, чей номер сотрудника был 1, 2, 3, 4 или 5.</span><span class="sxs-lookup"><span data-stu-id="06d7d-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Founders", policy =>
                          policy.RequireClaim("EmployeeNumber", "1", "2", "3", "4", "5"));
    });
}
```

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="06d7d-126">Несколько оценки политики</span><span class="sxs-lookup"><span data-stu-id="06d7d-126">Multiple Policy Evaluation</span></span>

<span data-ttu-id="06d7d-127">Если применяется несколько политик контроллер или действие, все политики необходимо передать перед предоставлением доступа.</span><span class="sxs-lookup"><span data-stu-id="06d7d-127">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="06d7d-128">Пример:</span><span class="sxs-lookup"><span data-stu-id="06d7d-128">For example:</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class SalaryController : Controller
{
    public ActionResult Payslip()
    {
    }

    [Authorize(Policy = "HumanResources")]
    public ActionResult UpdateSalary()
    {
    }
}
```

<span data-ttu-id="06d7d-129">В приведенном выше примере все удостоверения, что удовлетворяет `EmployeeOnly` политики можно получить доступ к `Payslip` действия, что эта политика применяется на контроллере.</span><span class="sxs-lookup"><span data-stu-id="06d7d-129">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="06d7d-130">Однако для вызова `UpdateSalary` действия должны выполняться удостоверение *оба* `EmployeeOnly` политики и `HumanResources` политики.</span><span class="sxs-lookup"><span data-stu-id="06d7d-130">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="06d7d-131">Если требуется более сложных политик, например переводить Дата рождения утверждения, вычисление age от него, а затем проверка возраст 21 или более ранних версий, а затем нужно написать [обработчики настраиваемой политики](policies.md).</span><span class="sxs-lookup"><span data-stu-id="06d7d-131">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
