---
title: "Пользовательская авторизация на основе политик"
author: rick-anderson
description: "В этом документе описывается создание и использование обработчиков политику настраиваемой авторизации в приложении ASP.NET Core."
keywords: "ASP.NET Core авторизации, настраиваемые политики, политика авторизации"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 0281d054204a11acc2cf11cf5fca23a8f70aad8e
ms.sourcegitcommit: 037d3900f739dbaa2ba14158e3d7dc81478952ad
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/01/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="9f0a8-104">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="9f0a8-104">Custom policy-based authorization</span></span>

<a name="security-authorization-policies-based"></a>

<span data-ttu-id="9f0a8-105">В системе [роли авторизации](roles.md) и [утверждений авторизации](claims.md) сделать использование требования, обработчик требование и предварительно настроенных политик.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-105">Underneath the covers, the [role authorization](roles.md) and [claims authorization](claims.md) make use of a requirement, a handler for the requirement, and a pre-configured policy.</span></span> <span data-ttu-id="9f0a8-106">Эти блоки позволяют представить оценок авторизации кода, что обеспечивает более широкие, для повторного использования, и структура легко тестируемых авторизации.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-106">These building blocks allow you to express authorization evaluations in code, allowing for a richer, reusable, and easily testable authorization structure.</span></span>

<span data-ttu-id="9f0a8-107">Политика авторизации состоит из одного или нескольких требований и регистрируется при запуске приложения в процессе настройки службы авторизации, в `ConfigureServices` в *файла Startup.cs* файл.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-107">An authorization policy is made up of one or more requirements and registered at application startup as part of the Authorization service configuration, in `ConfigureServices` in the *Startup.cs* file.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

<span data-ttu-id="9f0a8-108">Здесь вы увидите, что политики «Over21» создается один требование, что минимальный срок действия которого передается как параметр с требованием.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-108">Here you can see an "Over21" policy is created with a single requirement, that of a minimum age, which is passed as a parameter to the requirement.</span></span>

<span data-ttu-id="9f0a8-109">Политики применяются с помощью `Authorize` атрибут, указав имя политики, например;</span><span class="sxs-lookup"><span data-stu-id="9f0a8-109">Policies are applied using the `Authorize` attribute by specifying the policy name, for example;</span></span>

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a><span data-ttu-id="9f0a8-110">Требования</span><span class="sxs-lookup"><span data-stu-id="9f0a8-110">Requirements</span></span>

<span data-ttu-id="9f0a8-111">Требования к авторизации — это коллекция данных параметров, которые можно использовать политику для оценки текущего участника-пользователя.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-111">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="9f0a8-112">В политике минимальный возраст еще требуется один параметр, минимальный возраст.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-112">In our Minimum Age policy, the requirement we have is a single parameter, the minimum age.</span></span> <span data-ttu-id="9f0a8-113">Необходимо реализовать требование `IAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-113">A requirement must implement `IAuthorizationRequirement`.</span></span> <span data-ttu-id="9f0a8-114">Это пустой интерфейс-маркер.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-114">This is an empty, marker interface.</span></span> <span data-ttu-id="9f0a8-115">Параметризованные минимально допустимый возраст может быть реализовано следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9f0a8-115">A parameterized minimum age requirement might be implemented as follows;</span></span>

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

<span data-ttu-id="9f0a8-116">Это требование не должны иметь данные или свойства.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-116">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="9f0a8-117">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="9f0a8-117">Authorization handlers</span></span>

<span data-ttu-id="9f0a8-118">Обработчик авторизации отвечает за вычисление любой свойств является обязательным.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-118">An authorization handler is responsible for the evaluation of any properties of a requirement.</span></span> <span data-ttu-id="9f0a8-119">Обработчик авторизации необходимо их оценки относительно указанного `AuthorizationHandlerContext` решаете, разрешена ли авторизация.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-119">The  authorization handler must evaluate them against a provided `AuthorizationHandlerContext` to decide if authorization is allowed.</span></span> <span data-ttu-id="9f0a8-120">Это требование может иметь [несколько обработчиков](policies.md#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="9f0a8-120">A requirement can have [multiple handlers](policies.md#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="9f0a8-121">Обработчики должны наследовать `AuthorizationHandler<T>` там, где T — требование, обрабатывает его.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-121">Handlers must inherit `AuthorizationHandler<T>` where T is the requirement it handles.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="9f0a8-122">Минимальный возраст обработчик может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9f0a8-122">The minimum age handler might look like this:</span></span>

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="9f0a8-123">В приведенном выше коде сначала взглянуть на предмет основной текущий пользователь не имеет даты рождения утверждения, которой выполнялась, мы знаем, что издатель и доверия.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-123">In the code above, we first look to see if the current user principal has a date of birth claim which has been issued by an Issuer we know and trust.</span></span> <span data-ttu-id="9f0a8-124">Если утверждение отсутствует мы не удается авторизовать так мы возвращаем.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-124">If the claim is missing we can't authorize so we return.</span></span> <span data-ttu-id="9f0a8-125">Если у нас есть утверждение, мы выяснить, насколько стара пользователь является и, если они удовлетворяют минимальный возраст, передаваемые с требование затем авторизация прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-125">If we have a claim, we figure out how old the user is, and if they meet the minimum age passed in by the requirement then authorization has been successful.</span></span> <span data-ttu-id="9f0a8-126">После успешной авторизации мы называем `context.Succeed()` передав требование, которое было выполнено успешно, как параметр.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-126">Once authorization is successful we call `context.Succeed()` passing in the requirement that has been successful as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="9f0a8-127">Регистрации обработчика</span><span class="sxs-lookup"><span data-stu-id="9f0a8-127">Handler registration</span></span>
<span data-ttu-id="9f0a8-128">Обработчики должны быть зарегистрированы в коллекции служб во время настройки, например;</span><span class="sxs-lookup"><span data-stu-id="9f0a8-128">Handlers must be registered in the services collection during configuration, for example;</span></span>

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

<span data-ttu-id="9f0a8-129">Каждый обработчик добавляется в коллекцию служб с помощью `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` передав класс-обработчик.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-129">Each handler is added to the services collection by using `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` passing in your handler class.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="9f0a8-130">Что следует возвратить обработчик?</span><span class="sxs-lookup"><span data-stu-id="9f0a8-130">What should a handler return?</span></span>

<span data-ttu-id="9f0a8-131">Вы увидите на нашем [пример обработчика](policies.md#security-authorization-handler-example) , `Handle()` метод не имеет возвращаемого значения, так как мы указывают на успешное или неуспешное?</span><span class="sxs-lookup"><span data-stu-id="9f0a8-131">You can see in our [handler example](policies.md#security-authorization-handler-example) that the `Handle()` method has no return value, so how do we indicate success or failure?</span></span>

* <span data-ttu-id="9f0a8-132">Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требования, которые успешно проверен.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-132">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="9f0a8-133">Как правило, обработка ошибок, как другие обработчики для того же требование может быть успешным обработчик необязательно.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-133">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="9f0a8-134">Чтобы гарантировать сбоя, даже если другие обработчики для требования завершиться успешно, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-134">To guarantee failure even if other handlers for a requirement succeed, call `context.Fail`.</span></span>

<span data-ttu-id="9f0a8-135">Независимо от того, вызывается, внутри обработчиком все обработчики для требования будет вызываться при политики требует требование.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-135">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="9f0a8-136">Это позволяет требования с побочными эффектами, например ведение журнала, который всегда будет иметь место даже в том случае, если `context.Fail()` был вызван в другой обработчик.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-136">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="9f0a8-137">Зачем нужен несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="9f0a8-137">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="9f0a8-138">В случаях, когда нужно оценки на **или** основы реализовать несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-138">In cases where you want evaluation to be on an **OR** basis you implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="9f0a8-139">Например Корпорация Майкрософт имеет двери, которые только открытых ключей карты.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-139">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="9f0a8-140">Если оставить ключ-карта домашнего секретарь выводит временные наклейке и создает предпосылки для вас.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-140">If you leave your key card at home the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="9f0a8-141">В этом случае будет иметь одну потребность *EnterBuilding*, но несколько обработчиков, каждый из них один требование проверки.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-141">In this scenario you'd have a single requirement, *EnterBuilding*, but multiple handlers, each one examining a single requirement.</span></span>

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="9f0a8-142">Теперь, при условии, что оба обработчика [зарегистрированные](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) когда оценивается политика `EnterBuildingRequirement` после успешного либо обработчик оценки политики будет выполнена успешно.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-142">Now, assuming both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) when a policy evaluates the `EnterBuildingRequirement` if either handler succeeds the policy evaluation will succeed.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="9f0a8-143">Используя функцию для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="9f0a8-143">Using a func to fulfill a policy</span></span>

<span data-ttu-id="9f0a8-144">Возможны случаи где поступивший политику простой для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-144">There may be occasions where fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="9f0a8-145">Можно просто предоставить `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-145">It is possible to simply supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="9f0a8-146">Например, предыдущие `BadgeEntryHandler` можно переписать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="9f0a8-146">For example the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="9f0a8-147">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="9f0a8-147">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="9f0a8-148">`Handle` Метод необходимо реализовать в обработчике авторизации имеет два параметра `AuthorizationContext` и `Requirement` обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-148">The `Handle` method you must implement in an authorization handler has two parameters, an `AuthorizationContext` and the `Requirement` you are handling.</span></span> <span data-ttu-id="9f0a8-149">Платформы, например MVC или Jabbr добавьте любой объект `Resource` свойство `AuthorizationContext` пройти дополнительную информацию.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-149">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationContext` to pass through extra information.</span></span>

<span data-ttu-id="9f0a8-150">Например, MVC передает экземпляр `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` в свойство ресурса, который используется для доступа к HttpContext RouteData и все остальные MVC предоставляет.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-150">For example, MVC passes an instance of `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` in the resource property which is used to access HttpContext, RouteData and everything else MVC provides.</span></span>

<span data-ttu-id="9f0a8-151">Использование `Resource` свойство имеет определенную платформу.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-151">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="9f0a8-152">Используя информацию в `Resource` свойство ограничивает политик авторизации для определенной платформы.</span><span class="sxs-lookup"><span data-stu-id="9f0a8-152">Using information in the `Resource` property will limit your authorization policies to particular frameworks.</span></span> <span data-ttu-id="9f0a8-153">Необходимо привести `Resource` свойства с помощью `as` ключевое слово, а затем проверяет, имеет приведение успешно, чтобы ваш код не приводит к сбою с `InvalidCastExceptions` при выполнении на другие платформы;</span><span class="sxs-lookup"><span data-stu-id="9f0a8-153">You should cast the `Resource` property using the `as` keyword, and then check the cast has succeed to ensure your code doesn't crash with `InvalidCastExceptions` when run on other frameworks;</span></span>

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
