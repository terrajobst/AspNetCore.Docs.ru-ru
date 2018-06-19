---
title: Авторизация на основе утверждений в ASP.NET Core
author: rick-anderson
description: Дополнительные сведения о добавлении проверки утверждений для авторизации в приложении ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/claims
ms.openlocfilehash: 2464f8cac720dcf5de02f2679e9450e8b77de3ee
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/20/2018
ms.locfileid: "34336308"
---
# <a name="claims-based-authorization-in-aspnet-core"></a><span data-ttu-id="d2c7d-103">Авторизация на основе утверждений в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d2c7d-103">Claims-based authorization in ASP.NET Core</span></span>

<a name="security-authorization-claims-based"></a>

<span data-ttu-id="d2c7d-104">При создании удостоверение может быть присвоено одно или несколько утверждений, выданных доверенной стороной.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-104">When an identity is created it may be assigned one or more claims issued by a trusted party.</span></span> <span data-ttu-id="d2c7d-105">Утверждения, является пара имя-значение, представляющее какие субъекта, можно сделать, не какие субъекта.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-105">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span> <span data-ttu-id="d2c7d-106">Например имеется водительское удостоверение, выданный центром локального определяющим лицензии.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-106">For example, you may have a driver's license, issued by a local driving license authority.</span></span> <span data-ttu-id="d2c7d-107">Ваше водительское удостоверение есть дата рождения.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-107">Your driver's license has your date of birth on it.</span></span> <span data-ttu-id="d2c7d-108">В этом случае будет имя утверждения `DateOfBirth`, значение утверждения будет дата рождения, например `8th June 1970` и издатель будет определяющим центра лицензии.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-108">In this case the claim name would be `DateOfBirth`, the claim value would be your date of birth, for example `8th June 1970` and the issuer would be the driving license authority.</span></span> <span data-ttu-id="d2c7d-109">Авторизация на основе утверждений, в самом простом случае проверяет значение утверждения и разрешает доступ к ресурсу на основе этого значения.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-109">Claims based authorization, at its simplest, checks the value of a claim and allows access to a resource based upon that value.</span></span> <span data-ttu-id="d2c7d-110">Например, если требуется доступ к сообщество ночь процесс авторизации может быть:</span><span class="sxs-lookup"><span data-stu-id="d2c7d-110">For example if you want access to a night club the authorization process might be:</span></span>

<span data-ttu-id="d2c7d-111">Сотрудник отдела безопасности дверца будет возвращаться значение даты рождения утверждения и ли они доверять издателя (определяющим центра лицензии) перед предоставлением доступа.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-111">The door security officer would evaluate the value of your date of birth claim and whether they trust the issuer (the driving license authority) before granting you access.</span></span>

<span data-ttu-id="d2c7d-112">Удостоверение может содержать несколько утверждений с несколькими значениями и может содержать несколько утверждений того же типа.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-112">An identity can contain multiple claims with multiple values and can contain multiple claims of the same type.</span></span>

## <a name="adding-claims-checks"></a><span data-ttu-id="d2c7d-113">Добавление утверждений проверки</span><span class="sxs-lookup"><span data-stu-id="d2c7d-113">Adding claims checks</span></span>

<span data-ttu-id="d2c7d-114">Утверждения проверки авторизации на основе декларативного — разработчик внедряет их в свой код от контроллера или действия в контроллере, указав утверждения, которые текущий пользователь должен обладать и при необходимости выполнять значение утверждения для доступа к Запрошенный ресурс.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-114">Claim based authorization checks are declarative - the developer embeds them within their code, against a controller or an action within a controller, specifying claims which the current user must possess, and optionally the value the claim must hold to access the requested resource.</span></span> <span data-ttu-id="d2c7d-115">Утверждения, что требования, на основе политики, разработчику необходимо построить и зарегистрировать программные требования утверждений политики.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-115">Claims requirements are policy based, the developer must build and register a policy expressing the claims requirements.</span></span>

<span data-ttu-id="d2c7d-116">Простейший тип утверждения политики выглядит на наличие утверждения и не проверяет значение.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-116">The simplest type of claim policy looks for the presence of a claim and doesn't check the value.</span></span>

<span data-ttu-id="d2c7d-117">Сначала необходимо для создания и регистрации политики.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-117">First you need to build and register the policy.</span></span> <span data-ttu-id="d2c7d-118">Это происходит в процессе настройки службы авторизации, которой обычно принимает участие в `ConfigureServices()` в ваш *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-118">This takes place as part of the Authorization service configuration, which normally takes part in `ConfigureServices()` in your *Startup.cs* file.</span></span>

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

<span data-ttu-id="d2c7d-119">В этом случае `EmployeeOnly` политика проверяет наличие `EmployeeNumber` утверждения для текущего удостоверения.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-119">In this case the `EmployeeOnly` policy checks for the presence of an `EmployeeNumber` claim on the current identity.</span></span>

<span data-ttu-id="d2c7d-120">Затем примените политику с помощью `Policy` свойство `AuthorizeAttribute` атрибута, чтобы указать имя политики;</span><span class="sxs-lookup"><span data-stu-id="d2c7d-120">You then apply the policy using the `Policy` property on the `AuthorizeAttribute` attribute to specify the policy name;</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public IActionResult VacationBalance()
{
    return View();
}
```

<span data-ttu-id="d2c7d-121">`AuthorizeAttribute` Атрибут может применяться к контроллеру целиком в данном экземпляре только политика сопоставления удостоверений будет разрешен доступ к любому действию на контроллере.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-121">The `AuthorizeAttribute` attribute can be applied to an entire controller, in this instance only identities matching the policy will be allowed access to any Action on the controller.</span></span>

```csharp
[Authorize(Policy = "EmployeeOnly")]
public class VacationController : Controller
{
    public ActionResult VacationBalance()
    {
    }
}
```

<span data-ttu-id="d2c7d-122">При наличии контроллера, который защищен службой `AuthorizeAttribute` атрибута, но хотите разрешить анонимный доступ для применения каких-либо действий `AllowAnonymousAttribute` атрибута.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-122">If you have a controller that's protected by the `AuthorizeAttribute` attribute, but want to allow anonymous access to particular actions you apply the `AllowAnonymousAttribute` attribute.</span></span>

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

<span data-ttu-id="d2c7d-123">Большинство утверждений имеют значение.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-123">Most claims come with a value.</span></span> <span data-ttu-id="d2c7d-124">При создании политики можно указать список допустимых значений.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-124">You can specify a list of allowed values when creating the policy.</span></span> <span data-ttu-id="d2c7d-125">Следующий пример бы только успешно для сотрудников, чей номер сотрудника был 1, 2, 3, 4 или 5.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-125">The following example would only succeed for employees whose employee number was 1, 2, 3, 4 or 5.</span></span>

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

### <a name="add-a-generic-claim-check"></a><span data-ttu-id="d2c7d-126">Добавьте проверку универсального утверждения</span><span class="sxs-lookup"><span data-stu-id="d2c7d-126">Add a generic claim check</span></span>

<span data-ttu-id="d2c7d-127">Если значение утверждения не одно значение или преобразование не требуется, используйте [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span><span class="sxs-lookup"><span data-stu-id="d2c7d-127">If the claim value isn't a single value or a transformation is required, use [RequireAssertion](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicybuilder.requireassertion).</span></span> <span data-ttu-id="d2c7d-128">Дополнительные сведения см. в разделе [с помощью func для выполнения политики](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span><span class="sxs-lookup"><span data-stu-id="d2c7d-128">For more information, see [Using a func to fulfill a policy](xref:security/authorization/policies#using-a-func-to-fulfill-a-policy).</span></span>

## <a name="multiple-policy-evaluation"></a><span data-ttu-id="d2c7d-129">Несколько оценки политики</span><span class="sxs-lookup"><span data-stu-id="d2c7d-129">Multiple Policy Evaluation</span></span>

<span data-ttu-id="d2c7d-130">Если применяется несколько политик контроллер или действие, все политики необходимо передать перед предоставлением доступа.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-130">If you apply multiple policies to a controller or action, then all policies must pass before access is granted.</span></span> <span data-ttu-id="d2c7d-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="d2c7d-131">For example:</span></span>

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

<span data-ttu-id="d2c7d-132">В приведенном выше примере все удостоверения, что удовлетворяет `EmployeeOnly` политики можно получить доступ к `Payslip` действия, что эта политика применяется на контроллере.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-132">In the above example any identity which fulfills the `EmployeeOnly` policy can access the `Payslip` action as that policy is enforced on the controller.</span></span> <span data-ttu-id="d2c7d-133">Однако для вызова `UpdateSalary` действия должны выполняться удостоверение *оба* `EmployeeOnly` политики и `HumanResources` политики.</span><span class="sxs-lookup"><span data-stu-id="d2c7d-133">However in order to call the `UpdateSalary` action the identity must fulfill *both* the `EmployeeOnly` policy and the `HumanResources` policy.</span></span>

<span data-ttu-id="d2c7d-134">Если требуется более сложных политик, например переводить Дата рождения утверждения, вычисление age от него, а затем проверка возраст 21 или более ранних версий, а затем нужно написать [обработчики настраиваемой политики](xref:security/authorization/policies).</span><span class="sxs-lookup"><span data-stu-id="d2c7d-134">If you want more complicated policies, such as taking a date of birth claim, calculating an age from it then checking the age is 21 or older then you need to write [custom policy handlers](xref:security/authorization/policies).</span></span>
