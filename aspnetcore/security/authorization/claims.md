---
title: "Авторизация на основе утверждений"
author: rick-anderson
description: "В этом документе описывается добавление проверки утверждений для авторизации в приложении ASP.NET Core."
keywords: "ASP.NET Core, авторизации утверждений"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 737be5cd-3511-4f1c-b0ce-65403fb5eed3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/claims
ms.openlocfilehash: eebaddabdd360f34b6ff44e8f4f9f1f10fda6406
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
# <a name="claims-based-authorization"></a><span data-ttu-id="14ced-104">Авторизация на основе утверждений</span><span class="sxs-lookup"><span data-stu-id="14ced-104">Claims-Based Authorization</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="14ced-105">При создании удостоверение может быть присвоено одно или несколько утверждений, выданных доверенной стороной.</span><span class="sxs-lookup"><span data-stu-id="14ced-105">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="14ced-106">Утверждение-это имя значение пара, которая представляет какие субъекта, можно сделать, не какие субъекта.</span><span class="sxs-lookup"><span data-stu-id="14ced-106">A claim is name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="14ced-107">Например имеется водительское удостоверение, выданный центром локального определяющим лицензии.</span><span class="sxs-lookup"><span data-stu-id="14ced-107">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="14ced-108">Ваше водительское удостоверение есть дата рождения.</span><span class="sxs-lookup"><span data-stu-id="14ced-108">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="14ced-109">В этом случае будет имя утверждения `DateOfBirth`, значение утверждения будет дата рождения, например `8th June 1970` и издатель будет определяющим центра лицензии.</span><span class="sxs-lookup"><span data-stu-id="14ced-109">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="14ced-110">Авторизация на основе утверждений, в самом простом случае проверяет значение утверждения и разрешает доступ к ресурсу на основе этого значения.</span><span class="sxs-lookup"><span data-stu-id="14ced-110">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="14ced-111">Например, если требуется доступ к сообщество ночь процесс авторизации может быть:</span><span class="sxs-lookup"><span data-stu-id="14ced-111">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="14ced-112">Сотрудник отдела безопасности дверца будет возвращаться значение даты рождения утверждения и ли они доверять издателя (определяющим центра лицензии) перед предоставлением доступа.</span><span class="sxs-lookup"><span data-stu-id="14ced-112">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="14ced-113">Удостоверение может содержать несколько утверждений с несколькими значениями и может содержать несколько утверждений того же типа.</span><span class="sxs-lookup"><span data-stu-id="14ced-113">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="14ced-114">Добавление утверждений проверки</span><span class="sxs-lookup"><span data-stu-id="14ced-114">Adding claims checks</span></span>

<span data-ttu-id="14ced-115">Утверждения проверки авторизации на основе декларативного — разработчик внедряет их в свой код от контроллера или действия в контроллере, указав утверждения, которые текущий пользователь должен обладать и при необходимости выполнять значение утверждения для доступа к Запрошенный ресурс.</span><span class="sxs-lookup"><span data-stu-id="14ced-115">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="14ced-116">Утверждения, что требования, на основе политики, разработчику необходимо построить и зарегистрировать программные требования утверждений политики.</span><span class="sxs-lookup"><span data-stu-id="14ced-116">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="14ced-117">Простейший тип утверждения политики выглядит на наличие утверждения и не проверяет значение.</span><span class="sxs-lookup"><span data-stu-id="14ced-117">The simplest type of claim policy looks for the presence of a claim and does not check the value.</span></span>

<span data-ttu-id="14ced-118">Сначала необходимо для создания и регистрации политики.</span><span class="sxs-lookup"><span data-stu-id="14ced-118">First you need to build and register the policy.</span></span> <span data-ttu-id="14ced-119">Это происходит в процессе настройки службы авторизации, которой обычно принимает участие в `ConfigureServices()` в ваш *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="14ced-119">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="14ced-120">В этом случае `EmployeeOnly` политика проверяет наличие `EmployeeNumber` утверждения для текущего удостоверения.</span><span class="sxs-lookup"><span data-stu-id="14ced-120">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="14ced-121">Затем примените политику с помощью `Policy` свойство `AuthorizeAttribute` атрибута, чтобы указать имя политики;</span><span class="sxs-lookup"><span data-stu-id="14ced-121">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="14ced-122">`AuthorizeAttribute` Атрибут может применяться к контроллеру целиком в данном экземпляре только политика сопоставления удостоверений будет разрешен доступ к любому действию на контроллере.</span><span class="sxs-lookup"><span data-stu-id="14ced-122">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="14ced-123">При наличии контроллера, который защищен службой `AuthorizeAttribute` атрибута, но хотите разрешить анонимный доступ для применения каких-либо действий `AllowAnonymousAttribute` атрибута.</span><span class="sxs-lookup"><span data-stu-id="14ced-123">If you have a controller that is protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="14ced-124">Большинство утверждений имеют значение.</span><span class="sxs-lookup"><span data-stu-id="14ced-124">Most claims come with a value.</span></span> <span data-ttu-id="14ced-125">При создании политики можно указать список допустимых значений.</span><span class="sxs-lookup"><span data-stu-id="14ced-125">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="14ced-126">Следующий пример бы только успешно для сотрудников, чей номер сотрудника был 1, 2, 3, 4 или 5.</span><span class="sxs-lookup"><span data-stu-id="14ced-126">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="14ced-127">Несколько оценки политики</span><span class="sxs-lookup"><span data-stu-id="14ced-127">Multiple Policy Evaluation</span></span>

<span data-ttu-id="14ced-128">Если применяется несколько политик контроллер или действие, все политики необходимо передать перед предоставлением доступа.</span><span class="sxs-lookup"><span data-stu-id="14ced-128">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="14ced-129">Пример:</span><span class="sxs-lookup"><span data-stu-id="14ced-129">For example:</span></span>

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

<span data-ttu-id="14ced-130">В приведенном выше примере все удостоверения, что удовлетворяет `EmployeeOnly` политики можно получить доступ к `Payslip` действия, что эта политика применяется на контроллере.</span><span class="sxs-lookup"><span data-stu-id="14ced-130">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="14ced-131">Однако для вызова `UpdateSalary` действия должны выполняться удостоверение *оба* `EmployeeOnly` политики и `HumanResources` политики.</span><span class="sxs-lookup"><span data-stu-id="14ced-131">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="14ced-132">Если требуется более сложных политик, например переводить Дата рождения утверждения, вычисление age от него, а затем проверка возраст 21 или более ранних версий, а затем нужно написать [обработчики настраиваемой политики](policies.md).</span><span class="sxs-lookup"><span data-stu-id="14ced-132">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](policies.md).</span></span>
