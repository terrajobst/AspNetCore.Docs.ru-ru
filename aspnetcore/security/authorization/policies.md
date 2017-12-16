---
title: "Пользовательская авторизация на основе политик в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как создавать и использовать обработчики настраиваемой авторизации политики для реализации требования к проверке подлинности в приложении ASP.NET Core."
keywords: "ASP.NET Core авторизации, настраиваемые политики, политика авторизации"
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 280dd72b75e39546061d8455931f597f50c829fe
ms.sourcegitcommit: f1436107b4c022b26f5235dddef103cec5aa6bff
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/15/2017
---
# <a name="custom-policy-based-authorization"></a><span data-ttu-id="b8f79-104">Пользовательская авторизация на основе политик</span><span class="sxs-lookup"><span data-stu-id="b8f79-104">Custom policy-based authorization</span></span>

<span data-ttu-id="b8f79-105">В системе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) использовать требования, требования обработчик и предварительно настроенных политик.</span><span class="sxs-lookup"><span data-stu-id="b8f79-105">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="b8f79-106">Выражение вычисления авторизации поддерживают эти блоки в коде.</span><span class="sxs-lookup"><span data-stu-id="b8f79-106">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="b8f79-107">Результат представляет собой структуру богатый многократно используемых и тестируемых авторизации.</span><span class="sxs-lookup"><span data-stu-id="b8f79-107">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="b8f79-108">Политика авторизации состоит из одного или нескольких требования.</span><span class="sxs-lookup"><span data-stu-id="b8f79-108">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="b8f79-109">Он зарегистрирован в процессе настройки службы авторизации, `ConfigureServices` метод `Startup` класса:</span><span class="sxs-lookup"><span data-stu-id="b8f79-109">It's registered as part of the authorization service configuration, in the `ConfigureServices` method of the `Startup` class:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="b8f79-110">В предыдущем примере создается политика «AtLeast21».</span><span class="sxs-lookup"><span data-stu-id="b8f79-110">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="b8f79-111">Он имеет один требование, что минимальный возраст, который показан как параметр с требованием.</span><span class="sxs-lookup"><span data-stu-id="b8f79-111">It has a single requirement, that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="b8f79-112">Политики применяются с помощью `[Authorize]` атрибут с именем политики.</span><span class="sxs-lookup"><span data-stu-id="b8f79-112">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="b8f79-113">Пример:</span><span class="sxs-lookup"><span data-stu-id="b8f79-113">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="b8f79-114">Требования</span><span class="sxs-lookup"><span data-stu-id="b8f79-114">Requirements</span></span>

<span data-ttu-id="b8f79-115">Требования к авторизации — это коллекция данных параметров, которые можно использовать политику для оценки текущего участника-пользователя.</span><span class="sxs-lookup"><span data-stu-id="b8f79-115">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="b8f79-116">В политике «AtLeast21» требуется один параметр&mdash;минимальный возраст.</span><span class="sxs-lookup"><span data-stu-id="b8f79-116">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="b8f79-117">Реализует требование `IAuthorizationRequirement`, которая является интерфейсом пустой маркер.</span><span class="sxs-lookup"><span data-stu-id="b8f79-117">A requirement implements `IAuthorizationRequirement`, which is an empty marker interface.</span></span> <span data-ttu-id="b8f79-118">Параметризованные минимально допустимый возраст может быть реализован следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b8f79-118">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> <span data-ttu-id="b8f79-119">Это требование не должны иметь данные или свойства.</span><span class="sxs-lookup"><span data-stu-id="b8f79-119">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="b8f79-120">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="b8f79-120">Authorization handlers</span></span>

<span data-ttu-id="b8f79-121">Обработчик авторизации отвечает за вычисление свойств является обязательным.</span><span class="sxs-lookup"><span data-stu-id="b8f79-121">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="b8f79-122">Обработчик авторизации оценивает требования, к указанному `AuthorizationHandlerContext` для определения, разрешен доступ.</span><span class="sxs-lookup"><span data-stu-id="b8f79-122">The authorization handler evaluates the requirements against a provided `AuthorizationHandlerContext` to determine if access is allowed.</span></span> <span data-ttu-id="b8f79-123">Это требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="b8f79-123">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="b8f79-124">Наследовать обработчики `AuthorizationHandler<T>`, где `T` требуется обработать.</span><span class="sxs-lookup"><span data-stu-id="b8f79-124">Handlers inherit `AuthorizationHandler<T>`, where `T` is the requirement to be handled.</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="b8f79-125">Минимальный возраст обработчик может выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b8f79-125">The minimum age handler might look like this:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="b8f79-126">Предыдущий код определяет, обладает ли текущий пользователь основной Дата рождения утверждений, который выдал известного и надежного издателя.</span><span class="sxs-lookup"><span data-stu-id="b8f79-126">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="b8f79-127">Авторизация может произойти при утверждении отсутствует, в этом случае возвращается Завершенная задача.</span><span class="sxs-lookup"><span data-stu-id="b8f79-127">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="b8f79-128">Если утверждение присутствует, вычисляется возраста пользователя.</span><span class="sxs-lookup"><span data-stu-id="b8f79-128">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="b8f79-129">Если пользователь не отвечает минимальный возраст определяется требование, авторизация была успешно выполнена.</span><span class="sxs-lookup"><span data-stu-id="b8f79-129">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="b8f79-130">При успешном выполнении авторизации `context.Succeed` вызывается удовлетворяет требованию как параметр.</span><span class="sxs-lookup"><span data-stu-id="b8f79-130">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as a parameter.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="b8f79-131">Регистрации обработчика</span><span class="sxs-lookup"><span data-stu-id="b8f79-131">Handler registration</span></span>

<span data-ttu-id="b8f79-132">Обработчики, зарегистрированные в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="b8f79-132">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="b8f79-133">Пример:</span><span class="sxs-lookup"><span data-stu-id="b8f79-133">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="b8f79-134">Каждый обработчик добавляется в коллекцию службы путем вызова `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span><span class="sxs-lookup"><span data-stu-id="b8f79-134">Each handler is added to the services collection by invoking `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="b8f79-135">Что следует возвратить обработчик?</span><span class="sxs-lookup"><span data-stu-id="b8f79-135">What should a handler return?</span></span>

<span data-ttu-id="b8f79-136">Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений.</span><span class="sxs-lookup"><span data-stu-id="b8f79-136">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="b8f79-137">Как это состояние, успех или сбой?</span><span class="sxs-lookup"><span data-stu-id="b8f79-137">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="b8f79-138">Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требования, которые успешно проверен.</span><span class="sxs-lookup"><span data-stu-id="b8f79-138">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="b8f79-139">Как правило, обработка ошибок, как другие обработчики для того же требование может быть успешным обработчик необязательно.</span><span class="sxs-lookup"><span data-stu-id="b8f79-139">A handler does not need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="b8f79-140">Чтобы гарантировать сбоя, даже если другие обработчики требование успешно, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="b8f79-140">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="b8f79-141">Независимо от того, вызывается, внутри обработчиком все обработчики для требования будет вызываться при политики требует требование.</span><span class="sxs-lookup"><span data-stu-id="b8f79-141">Regardless of what you call inside your handler, all handlers for a requirement will be called when a policy requires the requirement.</span></span> <span data-ttu-id="b8f79-142">Это позволяет требования с побочными эффектами, например ведение журнала, который всегда будет иметь место даже в том случае, если `context.Fail()` был вызван в другой обработчик.</span><span class="sxs-lookup"><span data-stu-id="b8f79-142">This allows requirements to have side effects, such as logging, which will always take place even if `context.Fail()` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="b8f79-143">Зачем нужен несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="b8f79-143">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="b8f79-144">В случаях, когда нужно оценки на **или** основы, реализовать несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="b8f79-144">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="b8f79-145">Например Корпорация Майкрософт имеет двери, которые только открытых ключей карты.</span><span class="sxs-lookup"><span data-stu-id="b8f79-145">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="b8f79-146">Если оставить ключ-карта дома, секретарь выводит временные наклейке и создает предпосылки для вас.</span><span class="sxs-lookup"><span data-stu-id="b8f79-146">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="b8f79-147">В этом случае будет иметь одну потребность *BuildingEntry*, но несколько обработчиков, каждый из них один требование проверки.</span><span class="sxs-lookup"><span data-stu-id="b8f79-147">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="b8f79-148">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="b8f79-148">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="b8f79-149">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="b8f79-149">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="b8f79-150">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="b8f79-150">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="b8f79-151">Убедитесь, что оба обработчика [зарегистрированные](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="b8f79-151">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="b8f79-152">Если обработчик либо завершается успешно, если политика вычисляется `BuildingEntryRequirement`, прохождения проверки политики.</span><span class="sxs-lookup"><span data-stu-id="b8f79-152">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="b8f79-153">Используя функцию для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="b8f79-153">Using a func to fulfill a policy</span></span>

<span data-ttu-id="b8f79-154">Могут возникнуть ситуации, в какие выполнении политику простой для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="b8f79-154">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="b8f79-155">Можно указать `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.</span><span class="sxs-lookup"><span data-stu-id="b8f79-155">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="b8f79-156">Например, предыдущие `BadgeEntryHandler` можно переписать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b8f79-156">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="b8f79-157">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="b8f79-157">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="b8f79-158">`HandleRequirementAsync` Метод реализовать в обработчике авторизации имеет два параметра: `AuthorizationHandlerContext` и `TRequirement` обрабатывается.</span><span class="sxs-lookup"><span data-stu-id="b8f79-158">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="b8f79-159">Платформы, например MVC или Jabbr добавьте любой объект `Resource` свойство `AuthorizationHandlerContext` для передачи дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="b8f79-159">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="b8f79-160">Например, MVC передает экземпляр [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство.</span><span class="sxs-lookup"><span data-stu-id="b8f79-160">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="b8f79-161">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и все еще предоставляемых MVC и страниц Razor.</span><span class="sxs-lookup"><span data-stu-id="b8f79-161">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="b8f79-162">Использование `Resource` свойство имеет определенную платформу.</span><span class="sxs-lookup"><span data-stu-id="b8f79-162">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="b8f79-163">Используя информацию в `Resource` свойство ограничивает политик авторизации для определенной платформы.</span><span class="sxs-lookup"><span data-stu-id="b8f79-163">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="b8f79-164">Необходимо привести `Resource` свойства с помощью `as` ключевое слово и убедитесь, что приведение имеет успешно чтобы ваш код не приводит к сбою с `InvalidCastException` при выполнении на других платформах:</span><span class="sxs-lookup"><span data-stu-id="b8f79-164">You should cast the `Resource` property using the `as` keyword, and then confirm the cast has succeed to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
