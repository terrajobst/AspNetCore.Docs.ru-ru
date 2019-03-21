---
title: Авторизация на основе политик в ASP.NET Core
author: rick-anderson
description: Узнайте, как создать и использовать обработчики политики авторизации для принудительного применения требований к авторизации в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: e72f15c28fb7b62c671dd6475cc323cacce42de6
ms.sourcegitcommit: 5f299daa7c8102d56a63b214b9a34cc4bc87bc42
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/19/2019
ms.locfileid: "58208325"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="288f7-103">Авторизация на основе политик в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="288f7-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="288f7-104">Принципы работы [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) требование, обработчик требование и предварительно настроенная политика.</span><span class="sxs-lookup"><span data-stu-id="288f7-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="288f7-105">Эти блоки поддерживают выражения вычислений авторизации в коде.</span><span class="sxs-lookup"><span data-stu-id="288f7-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="288f7-106">Результат представляет собой структуру авторизации более широкие, многократно используемых, пригодного для тестирования.</span><span class="sxs-lookup"><span data-stu-id="288f7-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="288f7-107">Политика авторизации состоит из одной или нескольким требованиям.</span><span class="sxs-lookup"><span data-stu-id="288f7-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="288f7-108">Он зарегистрирован как часть конфигурации службы авторизации, в `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="288f7-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="288f7-109">В приведенном выше примере создается с помощью политики «AtLeast21».</span><span class="sxs-lookup"><span data-stu-id="288f7-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="288f7-110">Он имеет одну потребность&mdash;с минимальным возрастом, который передается в качестве параметра с требованием.</span><span class="sxs-lookup"><span data-stu-id="288f7-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="288f7-111">Применение политик для контроллеров MVC</span><span class="sxs-lookup"><span data-stu-id="288f7-111">Applying policies to MVC controllers</span></span>

<span data-ttu-id="288f7-112">Если вы используете Razor Pages, см. в разделе [применения политик к Razor Pages](#applying-policies-to-razor-pages) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="288f7-112">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="288f7-113">Политики применяются к контроллерам с помощью `[Authorize]` атрибут с именем политики.</span><span class="sxs-lookup"><span data-stu-id="288f7-113">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="288f7-114">Пример:</span><span class="sxs-lookup"><span data-stu-id="288f7-114">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="288f7-115">Применение политик для Razor Pages</span><span class="sxs-lookup"><span data-stu-id="288f7-115">Applying policies to Razor Pages</span></span>

<span data-ttu-id="288f7-116">Политики применяются к Razor Pages с помощью `[Authorize]` атрибут с именем политики.</span><span class="sxs-lookup"><span data-stu-id="288f7-116">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="288f7-117">Пример:</span><span class="sxs-lookup"><span data-stu-id="288f7-117">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="288f7-118">Политики могут также применяться для Razor Pages с помощью [соглашение об авторизации](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="288f7-118">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="288f7-119">Требования</span><span class="sxs-lookup"><span data-stu-id="288f7-119">Requirements</span></span>

<span data-ttu-id="288f7-120">Потребность в авторизации является коллекцией данных параметров, которые можно использовать политику для оценки текущего участника-пользователя.</span><span class="sxs-lookup"><span data-stu-id="288f7-120">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="288f7-121">В политике «AtLeast21», это единственный параметр&mdash;минимальный возраст.</span><span class="sxs-lookup"><span data-stu-id="288f7-121">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="288f7-122">Реализует требования [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), который является интерфейсом пустой маркер.</span><span class="sxs-lookup"><span data-stu-id="288f7-122">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="288f7-123">Параметризованные минимальный возраст требование может быть реализован следующим образом:</span><span class="sxs-lookup"><span data-stu-id="288f7-123">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="288f7-124">Если политика авторизации содержит несколько требований к авторизации, всем требованиям необходимо передать в порядке, для успешного вычисления политики.</span><span class="sxs-lookup"><span data-stu-id="288f7-124">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="288f7-125">Другими словами, несколько требований к авторизации, добавляемый в единую политику авторизации, обрабатываются на **AND** основы.</span><span class="sxs-lookup"><span data-stu-id="288f7-125">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="288f7-126">Требования не должны иметь данных или свойства.</span><span class="sxs-lookup"><span data-stu-id="288f7-126">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="288f7-127">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="288f7-127">Authorization handlers</span></span>

<span data-ttu-id="288f7-128">Обработчик авторизации отвечает за вычисление свойств требование.</span><span class="sxs-lookup"><span data-stu-id="288f7-128">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="288f7-129">Обработки авторизации оценивает требования к предоставленному [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) для определения того, разрешен ли доступ.</span><span class="sxs-lookup"><span data-stu-id="288f7-129">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="288f7-130">Требования могут иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="288f7-130">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="288f7-131">Обработчик может наследовать [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` является требованием для обработки.</span><span class="sxs-lookup"><span data-stu-id="288f7-131">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="288f7-132">Кроме того, может реализовать обработчик [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) для обработки более чем один тип требования.</span><span class="sxs-lookup"><span data-stu-id="288f7-132">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="288f7-133">Используйте обработчик для одно требование</span><span class="sxs-lookup"><span data-stu-id="288f7-133">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="288f7-134">Ниже приведен пример взаимно-однозначной связи, в котором не менее обработчик использует единый требование:</span><span class="sxs-lookup"><span data-stu-id="288f7-134">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="288f7-135">Приведенный выше код определяет, обладает ли текущий пользователь участника дату рождения утверждения, который выдан известного и надежного издателя.</span><span class="sxs-lookup"><span data-stu-id="288f7-135">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="288f7-136">Авторизация не может произойти при утверждении отсутствует; в этом случае возвращается завершенной задачи.</span><span class="sxs-lookup"><span data-stu-id="288f7-136">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="288f7-137">Если заявка присутствует, вычисляется его возраст.</span><span class="sxs-lookup"><span data-stu-id="288f7-137">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="288f7-138">Если пользователь соответствует минимальный возраст определяется требование, авторизация была успешно выполнена.</span><span class="sxs-lookup"><span data-stu-id="288f7-138">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="288f7-139">При успешном выполнении авторизации `context.Succeed` вызывается удовлетворяет всем требованиям как единственного параметра.</span><span class="sxs-lookup"><span data-stu-id="288f7-139">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="288f7-140">Используйте обработчик для несколько требований</span><span class="sxs-lookup"><span data-stu-id="288f7-140">Use a handler for multiple requirements</span></span>

<span data-ttu-id="288f7-141">Ниже приведен пример отношения один ко многим, в котором разрешение обработчик может обрабатывать три различных типа требования:</span><span class="sxs-lookup"><span data-stu-id="288f7-141">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="288f7-142">Приведенный выше код проходит через [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойство, содержащее требованиям не помечен как об успешном.</span><span class="sxs-lookup"><span data-stu-id="288f7-142">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="288f7-143">Для `ReadPermission` требование, пользователь должен быть владельцем или спонсора для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="288f7-143">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="288f7-144">В случае использования `EditPermission` или `DeletePermission` требование, он должен быть владельцем, чтобы получить доступ к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="288f7-144">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="288f7-145">Регистрация обработчика</span><span class="sxs-lookup"><span data-stu-id="288f7-145">Handler registration</span></span>

<span data-ttu-id="288f7-146">Обработчики, регистрируются в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="288f7-146">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="288f7-147">Пример:</span><span class="sxs-lookup"><span data-stu-id="288f7-147">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="288f7-148">Приведенный выше код регистрирует `MinimumAgeHandler` как Singleton-класс путем вызова `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="288f7-148">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="288f7-149">Обработчики могут быть зарегистрированы с помощью любого из встроенных [службы времени существования](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="288f7-149">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="288f7-150">Что должен возвращать обработчик?</span><span class="sxs-lookup"><span data-stu-id="288f7-150">What should a handler return?</span></span>

<span data-ttu-id="288f7-151">Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений.</span><span class="sxs-lookup"><span data-stu-id="288f7-151">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="288f7-152">Как такое состояние об успешном или обнаруженное?</span><span class="sxs-lookup"><span data-stu-id="288f7-152">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="288f7-153">Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требование, проверка прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="288f7-153">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="288f7-154">Обработчику не нужно обрабатывать сбои в общем случае, как другие обработчики для требования может завершиться успешно.</span><span class="sxs-lookup"><span data-stu-id="288f7-154">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="288f7-155">Чтобы гарантировать сбоя, даже если другие обработчики требований, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="288f7-155">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="288f7-156">Если задано значение `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) запуск обработчиков игнорирует свойство (в ASP.NET Core 1.1 и более поздние версии) при `context.Fail` вызывается.</span><span class="sxs-lookup"><span data-stu-id="288f7-156">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="288f7-157">`InvokeHandlersAfterFailure` по умолчанию используется `true`, в этом случае вызываются все обработчики.</span><span class="sxs-lookup"><span data-stu-id="288f7-157">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span> <span data-ttu-id="288f7-158">Благодаря этому потребности для получения побочные эффекты, такие как ведение журнала, которые всегда выполняются даже в том случае, если `context.Fail` был вызван в другой обработчик.</span><span class="sxs-lookup"><span data-stu-id="288f7-158">This allows requirements to produce side effects, such as logging, which always take place even if `context.Fail` has been called in another handler.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="288f7-159">Зачем несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="288f7-159">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="288f7-160">В тех случаях, когда вычисление на **или** основы, реализовать несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="288f7-160">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="288f7-161">Например Корпорация Майкрософт двери, которые только открытых ключей карты.</span><span class="sxs-lookup"><span data-stu-id="288f7-161">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="288f7-162">Если ключ-карта забыть дома, секретарь выводится временный наклейку и открывает возможность для вас.</span><span class="sxs-lookup"><span data-stu-id="288f7-162">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="288f7-163">В этом случае будет иметь одну потребность, *BuildingEntry*, но несколько обработчиков, каждый из них действует только одно требование проверки.</span><span class="sxs-lookup"><span data-stu-id="288f7-163">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="288f7-164">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="288f7-164">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="288f7-165">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="288f7-165">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="288f7-166">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="288f7-166">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="288f7-167">Убедитесь, что оба обработчика [зарегистрирован](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="288f7-167">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="288f7-168">Если обработчик либо завершается успешно, если политика оценивает `BuildingEntryRequirement`, оценка политики завершается успешно.</span><span class="sxs-lookup"><span data-stu-id="288f7-168">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="288f7-169">С помощью func для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="288f7-169">Using a func to fulfill a policy</span></span>

<span data-ttu-id="288f7-170">Могут возникнуть ситуации, в какие выполнении политики прост для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="288f7-170">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="288f7-171">Это можно указать `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.</span><span class="sxs-lookup"><span data-stu-id="288f7-171">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="288f7-172">Например, предыдущий `BadgeEntryHandler` можно переписать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="288f7-172">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="288f7-173">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="288f7-173">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="288f7-174">`HandleRequirementAsync` Метод реализовать в обработчике авторизации имеет два параметра: `AuthorizationHandlerContext` и `TRequirement` обрабатываются.</span><span class="sxs-lookup"><span data-stu-id="288f7-174">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="288f7-175">Платформы, такие как MVC или Jabbr предоставляются бесплатно добавить любой объект `Resource` свойство `AuthorizationHandlerContext` для передачи дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="288f7-175">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="288f7-176">Например, MVC передает экземпляр [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство.</span><span class="sxs-lookup"><span data-stu-id="288f7-176">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="288f7-177">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и все, что предоставленный другим, MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="288f7-177">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="288f7-178">Использование `Resource` свойство является определенной платформы.</span><span class="sxs-lookup"><span data-stu-id="288f7-178">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="288f7-179">Используя информацию в `Resource` свойство ограничивает политик авторизации для конкретной платформы.</span><span class="sxs-lookup"><span data-stu-id="288f7-179">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="288f7-180">Следует привести `Resource` свойства с помощью `is` ключевое слово, а затем подтвердите приведение выполнено успешно, чтобы обеспечить отсутствие сбоев кода с `InvalidCastException` при запуске на другие платформы:</span><span class="sxs-lookup"><span data-stu-id="288f7-180">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
