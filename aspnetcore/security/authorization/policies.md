---
title: Авторизация на основе политик в ASP.NET Core
author: rick-anderson
description: Узнайте, как создать и использовать обработчики политики авторизации для принудительного применения требований к авторизации в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: c2bc626b2dd341dda878a151def6b405884357d7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665406"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="bdbea-103">Авторизация на основе политик в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="bdbea-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="bdbea-104">Принципы работы [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) требование, обработчик требование и предварительно настроенная политика.</span><span class="sxs-lookup"><span data-stu-id="bdbea-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="bdbea-105">Эти блоки поддерживают выражения вычислений авторизации в коде.</span><span class="sxs-lookup"><span data-stu-id="bdbea-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="bdbea-106">Результат представляет собой структуру авторизации более широкие, многократно используемых, пригодного для тестирования.</span><span class="sxs-lookup"><span data-stu-id="bdbea-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="bdbea-107">Политика авторизации состоит из одной или нескольким требованиям.</span><span class="sxs-lookup"><span data-stu-id="bdbea-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="bdbea-108">Он зарегистрирован как часть конфигурации службы авторизации, в `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="bdbea-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

<span data-ttu-id="bdbea-109">В приведенном выше примере создается с помощью политики «AtLeast21».</span><span class="sxs-lookup"><span data-stu-id="bdbea-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="bdbea-110">Он имеет одну потребность&mdash;с минимальным возрастом, который передается в качестве параметра с требованием.</span><span class="sxs-lookup"><span data-stu-id="bdbea-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

<span data-ttu-id="bdbea-111">Политики применяются с помощью `[Authorize]` атрибут с именем политики.</span><span class="sxs-lookup"><span data-stu-id="bdbea-111">Policies are applied by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="bdbea-112">Пример:</span><span class="sxs-lookup"><span data-stu-id="bdbea-112">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a><span data-ttu-id="bdbea-113">Требования</span><span class="sxs-lookup"><span data-stu-id="bdbea-113">Requirements</span></span>

<span data-ttu-id="bdbea-114">Потребность в авторизации является коллекцией данных параметров, которые можно использовать политику для оценки текущего участника-пользователя.</span><span class="sxs-lookup"><span data-stu-id="bdbea-114">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="bdbea-115">В политике «AtLeast21», это единственный параметр&mdash;минимальный возраст.</span><span class="sxs-lookup"><span data-stu-id="bdbea-115">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="bdbea-116">Реализует требования [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), который является интерфейсом пустой маркер.</span><span class="sxs-lookup"><span data-stu-id="bdbea-116">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="bdbea-117">Параметризованные минимальный возраст требование может быть реализован следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bdbea-117">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="bdbea-118">Если политика авторизации содержит несколько требований к авторизации, всем требованиям необходимо передать в порядке, для успешного вычисления политики.</span><span class="sxs-lookup"><span data-stu-id="bdbea-118">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="bdbea-119">Другими словами, несколько требований к авторизации, добавляемый в единую политику авторизации, обрабатываются на **AND** основы.</span><span class="sxs-lookup"><span data-stu-id="bdbea-119">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="bdbea-120">Требования не должны иметь данных или свойства.</span><span class="sxs-lookup"><span data-stu-id="bdbea-120">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="bdbea-121">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="bdbea-121">Authorization handlers</span></span>

<span data-ttu-id="bdbea-122">Обработчик авторизации отвечает за вычисление свойств требование.</span><span class="sxs-lookup"><span data-stu-id="bdbea-122">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="bdbea-123">Обработки авторизации оценивает требования к предоставленному [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) для определения того, разрешен ли доступ.</span><span class="sxs-lookup"><span data-stu-id="bdbea-123">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="bdbea-124">Требования могут иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="bdbea-124">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="bdbea-125">Обработчик может наследовать [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` является требованием для обработки.</span><span class="sxs-lookup"><span data-stu-id="bdbea-125">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="bdbea-126">Кроме того, может реализовать обработчик [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) для обработки более чем один тип требования.</span><span class="sxs-lookup"><span data-stu-id="bdbea-126">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="bdbea-127">Используйте обработчик для одно требование</span><span class="sxs-lookup"><span data-stu-id="bdbea-127">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="bdbea-128">Ниже приведен пример взаимно-однозначной связи, в котором не менее обработчик использует единый требование:</span><span class="sxs-lookup"><span data-stu-id="bdbea-128">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="bdbea-129">Приведенный выше код определяет, обладает ли текущий пользователь участника дату рождения утверждения, который выдан известного и надежного издателя.</span><span class="sxs-lookup"><span data-stu-id="bdbea-129">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="bdbea-130">Авторизация не может произойти при утверждении отсутствует; в этом случае возвращается завершенной задачи.</span><span class="sxs-lookup"><span data-stu-id="bdbea-130">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="bdbea-131">Если заявка присутствует, вычисляется его возраст.</span><span class="sxs-lookup"><span data-stu-id="bdbea-131">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="bdbea-132">Если пользователь соответствует минимальный возраст определяется требование, авторизация была успешно выполнена.</span><span class="sxs-lookup"><span data-stu-id="bdbea-132">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="bdbea-133">При успешном выполнении авторизации `context.Succeed` вызывается удовлетворяет всем требованиям как единственного параметра.</span><span class="sxs-lookup"><span data-stu-id="bdbea-133">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="bdbea-134">Используйте обработчик для несколько требований</span><span class="sxs-lookup"><span data-stu-id="bdbea-134">Use a handler for multiple requirements</span></span>

<span data-ttu-id="bdbea-135">Ниже приведен пример отношения один ко многим, в котором разрешение обработчик может обрабатывать три различных типа требования:</span><span class="sxs-lookup"><span data-stu-id="bdbea-135">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="bdbea-136">Приведенный выше код проходит через [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойство, содержащее требованиям не помечен как об успешном.</span><span class="sxs-lookup"><span data-stu-id="bdbea-136">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="bdbea-137">Для `ReadPermission` требование, пользователь должен быть владельцем или спонсора для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="bdbea-137">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="bdbea-138">В случае использования `EditPermission` или `DeletePermission` требование, он должен быть владельцем, чтобы получить доступ к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="bdbea-138">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="bdbea-139">Регистрация обработчика</span><span class="sxs-lookup"><span data-stu-id="bdbea-139">Handler registration</span></span>

<span data-ttu-id="bdbea-140">Обработчики, регистрируются в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="bdbea-140">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="bdbea-141">Пример:</span><span class="sxs-lookup"><span data-stu-id="bdbea-141">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

<span data-ttu-id="bdbea-142">Приведенный выше код регистрирует `MinimumAgeHandler` как Singleton-класс путем вызова `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="bdbea-142">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="bdbea-143">Обработчики могут быть зарегистрированы с помощью любого из встроенных [службы времени существования](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="bdbea-143">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="bdbea-144">Что должен возвращать обработчик?</span><span class="sxs-lookup"><span data-stu-id="bdbea-144">What should a handler return?</span></span>

<span data-ttu-id="bdbea-145">Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений.</span><span class="sxs-lookup"><span data-stu-id="bdbea-145">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="bdbea-146">Как такое состояние об успешном или обнаруженное?</span><span class="sxs-lookup"><span data-stu-id="bdbea-146">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="bdbea-147">Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требование, проверка прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="bdbea-147">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="bdbea-148">Обработчику не нужно обрабатывать сбои в общем случае, как другие обработчики для требования может завершиться успешно.</span><span class="sxs-lookup"><span data-stu-id="bdbea-148">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="bdbea-149">Чтобы гарантировать сбоя, даже если другие обработчики требований, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="bdbea-149">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="bdbea-150">Если задано значение `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) запуск обработчиков игнорирует свойство (в ASP.NET Core 1.1 и более поздние версии) при `context.Fail` вызывается.</span><span class="sxs-lookup"><span data-stu-id="bdbea-150">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="bdbea-151">`InvokeHandlersAfterFailure` по умолчанию используется `true`, в этом случае вызываются все обработчики.</span><span class="sxs-lookup"><span data-stu-id="bdbea-151">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="bdbea-152">Благодаря этому потребности для получения побочные эффекты, такие как ведение журнала, которые всегда выполняются даже в том случае, если `context.Fail` был вызван в другой обработчик.</span><span class="sxs-lookup"><span data-stu-id="bdbea-152">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="bdbea-153">Зачем несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="bdbea-153">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="bdbea-154">В тех случаях, когда вычисление на **или** основы, реализовать несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="bdbea-154">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="bdbea-155">Например Корпорация Майкрософт двери, которые только открытых ключей карты.</span><span class="sxs-lookup"><span data-stu-id="bdbea-155">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="bdbea-156">Если ключ-карта забыть дома, секретарь выводится временный наклейку и открывает возможность для вас.</span><span class="sxs-lookup"><span data-stu-id="bdbea-156">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="bdbea-157">В этом случае будет иметь одну потребность, *BuildingEntry*, но несколько обработчиков, каждый из них действует только одно требование проверки.</span><span class="sxs-lookup"><span data-stu-id="bdbea-157">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="bdbea-158">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="bdbea-158">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="bdbea-159">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="bdbea-159">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="bdbea-160">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="bdbea-160">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="bdbea-161">Убедитесь, что оба обработчика [зарегистрирован](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="bdbea-161">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="bdbea-162">Если обработчик либо завершается успешно, если политика оценивает `BuildingEntryRequirement`, оценка политики завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="bdbea-162">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="bdbea-163">С помощью func для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="bdbea-163">Using a func to fulfill a policy</span></span>

<span data-ttu-id="bdbea-164">Могут возникнуть ситуации, в какие выполнении политики прост для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="bdbea-164">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="bdbea-165">Это можно указать `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.</span><span class="sxs-lookup"><span data-stu-id="bdbea-165">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="bdbea-166">Например, предыдущий `BadgeEntryHandler` можно переписать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bdbea-166">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="bdbea-167">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="bdbea-167">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="bdbea-168">`HandleRequirementAsync` Метод реализовать в обработчике авторизации имеет два параметра: `AuthorizationHandlerContext` и `TRequirement` обрабатываются.</span><span class="sxs-lookup"><span data-stu-id="bdbea-168">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="bdbea-169">Платформы, такие как MVC или Jabbr предоставляются бесплатно добавить любой объект `Resource` свойство `AuthorizationHandlerContext` для передачи дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="bdbea-169">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="bdbea-170">Например, MVC передает экземпляр [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство.</span><span class="sxs-lookup"><span data-stu-id="bdbea-170">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="bdbea-171">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и все, что предоставленный другим, MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="bdbea-171">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="bdbea-172">Использование `Resource` свойство является определенной платформы.</span><span class="sxs-lookup"><span data-stu-id="bdbea-172">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="bdbea-173">Используя информацию в `Resource` свойство ограничивает политик авторизации для конкретной платформы.</span><span class="sxs-lookup"><span data-stu-id="bdbea-173">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="bdbea-174">Следует привести `Resource` свойства с помощью `is` ключевое слово, а затем подтвердите приведение выполнено успешно, чтобы обеспечить отсутствие сбоев кода с `InvalidCastException` при запуске на другие платформы:</span><span class="sxs-lookup"><span data-stu-id="bdbea-174">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
