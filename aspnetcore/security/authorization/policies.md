---
title: "Пользовательская авторизация на основе политик в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как создавать и использовать обработчики настраиваемой авторизации политики для реализации требования к проверке подлинности в приложении ASP.NET Core."
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/policies
ms.openlocfilehash: 0eb5451828a51771d9388c2db610ede6231ced51
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="74eab-103">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="74eab-103">Custom policy-based authorization</span></span>

<span data-ttu-id="74eab-104">В системе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) использовать требования, требования обработчик и предварительно настроенных политик.</span><span class="sxs-lookup"><span data-stu-id="74eab-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="74eab-105">Выражение вычисления авторизации поддерживают эти блоки в коде.</span><span class="sxs-lookup"><span data-stu-id="74eab-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="74eab-106">Результат представляет собой структуру богатый многократно используемых и тестируемых авторизации.</span><span class="sxs-lookup"><span data-stu-id="74eab-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="74eab-107">Политика авторизации состоит из одного или нескольких требования.</span><span class="sxs-lookup"><span data-stu-id="74eab-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="74eab-108">Он зарегистрирован в процессе настройки службы авторизации, `ConfigureServices` метод `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="74eab-108">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="74eab-109">В предыдущем примере создается политика «AtLeast21».</span><span class="sxs-lookup"><span data-stu-id="74eab-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="74eab-110">Он имеет один требование, что минимальный возраст, который показан как параметр с требованием.</span><span class="sxs-lookup"><span data-stu-id="74eab-110">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="74eab-111">Политики применяются с помощью `[Authorize]` атрибут с именем политики.</span><span class="sxs-lookup"><span data-stu-id="74eab-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="74eab-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="74eab-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="74eab-113">Требования</span><span class="sxs-lookup"><span data-stu-id="74eab-113">Requirements</span></span>

<span data-ttu-id="74eab-114">Требования к авторизации — это коллекция данных параметров, которые можно использовать политику для оценки текущего участника-пользователя.</span><span class="sxs-lookup"><span data-stu-id="74eab-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="74eab-115">В политике «AtLeast21» требуется один параметр&mdash;минимальный возраст.</span><span class="sxs-lookup"><span data-stu-id="74eab-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="74eab-116">Реализует требование `IAuthorizationRequirement`, которая является интерфейсом пустой маркер.</span><span class="sxs-lookup"><span data-stu-id="74eab-116">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="74eab-117">Параметризованные минимально допустимый возраст может быть реализован следующим образом:</span><span class="sxs-lookup"><span data-stu-id="74eab-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="74eab-118">Это требование не должны иметь данные или свойства.</span><span class="sxs-lookup"><span data-stu-id="74eab-118">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="74eab-119">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="74eab-119">Authorization handlers</span></span>

<span data-ttu-id="74eab-120">Обработчик авторизации отвечает за вычисление свойств является обязательным.</span><span class="sxs-lookup"><span data-stu-id="74eab-120">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="74eab-121">Обработчик авторизации оценивает требования, к указанному `AuthorizationHandlerContext` для определения, разрешен доступ.</span><span class="sxs-lookup"><span data-stu-id="74eab-121">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="74eab-122">Это требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="74eab-122">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="74eab-123">Наследовать обработчики `AuthorizationHandler<T>`, где `T` требуется обработать.</span><span class="sxs-lookup"><span data-stu-id="74eab-123">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="74eab-124">Минимальный возраст обработчик может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="74eab-124">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="74eab-125">Предыдущий код определяет, обладает ли текущий пользователь основной Дата рождения утверждений, который выдал известного и надежного издателя.</span><span class="sxs-lookup"><span data-stu-id="74eab-125">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="74eab-126">Авторизация может произойти при утверждении отсутствует, в этом случае возвращается Завершенная задача.</span><span class="sxs-lookup"><span data-stu-id="74eab-126">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="74eab-127">Если утверждение присутствует, вычисляется возраста пользователя.</span><span class="sxs-lookup"><span data-stu-id="74eab-127">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="74eab-128">Если пользователь не отвечает минимальный возраст определяется требование, авторизация была успешно выполнена.</span><span class="sxs-lookup"><span data-stu-id="74eab-128">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="74eab-129">При успешном выполнении авторизации `context.Succeed` вызывается удовлетворяет требованию как параметр.</span><span class="sxs-lookup"><span data-stu-id="74eab-129">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="74eab-130">Регистрации обработчика</span><span class="sxs-lookup"><span data-stu-id="74eab-130">Handler registration</span></span>

<span data-ttu-id="74eab-131">Обработчики, зарегистрированные в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="74eab-131">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="74eab-132">Пример:</span><span class="sxs-lookup"><span data-stu-id="74eab-132">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="74eab-133">Каждый обработчик добавляется в коллекцию службы путем вызова `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="74eab-133">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="74eab-134">Что следует возвратить обработчик?</span><span class="sxs-lookup"><span data-stu-id="74eab-134">What should a handler return?</span></span>

<span data-ttu-id="74eab-135">Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений.</span><span class="sxs-lookup"><span data-stu-id="74eab-135">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="74eab-136">Как это состояние, успех или сбой?</span><span class="sxs-lookup"><span data-stu-id="74eab-136">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="74eab-137">Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требования, которые успешно проверен.</span><span class="sxs-lookup"><span data-stu-id="74eab-137">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="74eab-138">Обработчик не требуется, как правило, обработка ошибок, как может быть успешным, другие обработчики для того же требования.</span><span class="sxs-lookup"><span data-stu-id="74eab-138">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="74eab-139">Чтобы гарантировать сбоя, даже если другие обработчики требование успешно, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="74eab-139">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="74eab-140">Независимо от того, вызывается, внутри обработчиком все обработчики для требования будет вызываться при политики требует требование.</span><span class="sxs-lookup"><span data-stu-id="74eab-140">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="74eab-141">Это позволяет требования с побочными эффектами, например ведение журнала, который всегда будет иметь место даже в том случае, если `context.Fail()` был вызван в другой обработчик.</span><span class="sxs-lookup"><span data-stu-id="74eab-141">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="74eab-142">Зачем нужен несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="74eab-142">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="74eab-143">В случаях, когда нужно оценки на **или** основы, реализовать несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="74eab-143">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="74eab-144">Например Корпорация Майкрософт имеет двери, которые только открытых ключей карты.</span><span class="sxs-lookup"><span data-stu-id="74eab-144">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="74eab-145">Если оставить ключ-карта дома, секретарь выводит временные наклейке и создает предпосылки для вас.</span><span class="sxs-lookup"><span data-stu-id="74eab-145">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="74eab-146">В этом случае будет иметь одну потребность *BuildingEntry*, но несколько обработчиков, каждый из них один требование проверки.</span><span class="sxs-lookup"><span data-stu-id="74eab-146">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="74eab-147">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="74eab-147">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="74eab-148">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="74eab-148">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="74eab-149">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="74eab-149">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="74eab-150">Убедитесь, что оба обработчика [зарегистрированные](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="74eab-150">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="74eab-151">Если обработчик либо завершается успешно, если политика вычисляется `BuildingEntryRequirement`, прохождения проверки политики.</span><span class="sxs-lookup"><span data-stu-id="74eab-151">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="74eab-152">Используя функцию для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="74eab-152">Using a func to fulfill a policy</span></span>

<span data-ttu-id="74eab-153">Могут возникнуть ситуации, в какие выполнении политику простой для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="74eab-153">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="74eab-154">Можно указать `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.</span><span class="sxs-lookup"><span data-stu-id="74eab-154">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="74eab-155">Например, предыдущие `BadgeEntryHandler` можно переписать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="74eab-155">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="74eab-156">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="74eab-156">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="74eab-157">`HandleRequirementAsync` Метод реализовать в обработчике авторизации имеет два параметра: `AuthorizationHandlerContext` и `TRequirement` обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="74eab-157">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="74eab-158">Платформы, например MVC или Jabbr добавьте любой объект `Resource` свойство `AuthorizationHandlerContext` для передачи дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="74eab-158">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="74eab-159">Например, MVC передает экземпляр [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство.</span><span class="sxs-lookup"><span data-stu-id="74eab-159">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="74eab-160">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и все еще предоставляемых MVC и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="74eab-160">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="74eab-161">Использование `Resource` свойство имеет определенную платформу.</span><span class="sxs-lookup"><span data-stu-id="74eab-161">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="74eab-162">Используя информацию в `Resource` свойство ограничивает политик авторизации для определенной платформы.</span><span class="sxs-lookup"><span data-stu-id="74eab-162">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="74eab-163">Необходимо привести `Resource` свойства с помощью `as` ключевое слово и убедитесь, что приведение имеет успешно чтобы ваш код не приводит к сбою с `InvalidCastException` при выполнении на других платформах:</span><span class="sxs-lookup"><span data-stu-id="74eab-163">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
