---
title: Авторизация на основе политик в ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать и использовать обработчики политик авторизации для применения требований к авторизации в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: security/authorization/policies
ms.openlocfilehash: eeb5ddd63ef8177325b35e5a666aa5e9ab047057
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652978"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="03f2f-103">Авторизация на основе политик в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="03f2f-103">Policy-based authorization in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="03f2f-104">В процессе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) используются требования, обработчик требований и предварительно настроенная политика.</span><span class="sxs-lookup"><span data-stu-id="03f2f-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="03f2f-105">Эти стандартные блоки поддерживают выражение оценки авторизации в коде.</span><span class="sxs-lookup"><span data-stu-id="03f2f-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="03f2f-106">Результатом является расширенная, повторно используемая, тестируемая структура авторизации.</span><span class="sxs-lookup"><span data-stu-id="03f2f-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="03f2f-107">Политика авторизации состоит из одного или нескольких требований.</span><span class="sxs-lookup"><span data-stu-id="03f2f-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="03f2f-108">Он регистрируется как часть конфигурации службы авторизации в методе `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03f2f-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53, 58)]

<span data-ttu-id="03f2f-109">В предыдущем примере создается политика "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="03f2f-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="03f2f-110">Он имеет одно требование&mdash;, которое имеет минимальный возраст, который предоставляется в качестве параметра для требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="03f2f-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="03f2f-111">IAuthorizationService</span></span> 

<span data-ttu-id="03f2f-112">Основная служба, определяющая успешность авторизации, <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="03f2f-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="03f2f-113">В приведенном выше коде показаны два метода [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="03f2f-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="03f2f-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> — это служба маркеров без методов, а также механизм отслеживания успешности авторизации.</span><span class="sxs-lookup"><span data-stu-id="03f2f-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="03f2f-115">Каждый <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> отвечает за проверку соблюдения требований:</span><span class="sxs-lookup"><span data-stu-id="03f2f-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="03f2f-116">Класс <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> используется обработчиком для обозначения того, выполнены ли требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="03f2f-117">В следующем коде показаны упрощенная реализация службы авторизации по умолчанию (и заметки с комментариями):</span><span class="sxs-lookup"><span data-stu-id="03f2f-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="03f2f-118">В следующем коде показан типичный `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03f2f-118">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddControllersWithViews();
    services.AddRazorPages();
}
```

<span data-ttu-id="03f2f-119">Для авторизации используйте <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> или `[Authorize(Policy = "Something")]`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="03f2f-120">Применение политик к контроллерам MVC</span><span class="sxs-lookup"><span data-stu-id="03f2f-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="03f2f-121">Если вы используете Razor Pages, см. раздел [применение политик к Razor Pages](#applying-policies-to-razor-pages) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="03f2f-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="03f2f-122">Политики применяются к контроллерам с помощью атрибута `[Authorize]` с именем политики.</span><span class="sxs-lookup"><span data-stu-id="03f2f-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="03f2f-123">Пример:</span><span class="sxs-lookup"><span data-stu-id="03f2f-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="03f2f-124">Применение политик к Razor Pages</span><span class="sxs-lookup"><span data-stu-id="03f2f-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="03f2f-125">Политики применяются к Razor Pages с помощью атрибута `[Authorize]` с именем политики.</span><span class="sxs-lookup"><span data-stu-id="03f2f-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="03f2f-126">Пример:</span><span class="sxs-lookup"><span data-stu-id="03f2f-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="03f2f-127">Политики также можно применить к Razor Pages с помощью соглашения об [авторизации](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="03f2f-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="03f2f-128">Требования</span><span class="sxs-lookup"><span data-stu-id="03f2f-128">Requirements</span></span>

<span data-ttu-id="03f2f-129">Требование авторизации — это коллекция параметров данных, которую политика может использовать для вычисления текущего субъекта-пользователя.</span><span class="sxs-lookup"><span data-stu-id="03f2f-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="03f2f-130">В нашей политике "AtLeast21" требование относится к одному параметру&mdash;минимального срока.</span><span class="sxs-lookup"><span data-stu-id="03f2f-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="03f2f-131">Требование реализует [иаусоризатионрекуиремент](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), который является пустым интерфейсом маркера.</span><span class="sxs-lookup"><span data-stu-id="03f2f-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="03f2f-132">Требование к параметризованному минимальному возрасту можно реализовать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="03f2f-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="03f2f-133">Если политика авторизации содержит несколько требований к авторизации, все требования должны пройти, чтобы оценка политики прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="03f2f-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="03f2f-134">Иными словами, несколько требований к авторизации, добавленных в одну политику авторизации, обрабатываются **и** на основе.</span><span class="sxs-lookup"><span data-stu-id="03f2f-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="03f2f-135">Требование не обязательно должно иметь данные или свойства.</span><span class="sxs-lookup"><span data-stu-id="03f2f-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="03f2f-136">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="03f2f-136">Authorization handlers</span></span>

<span data-ttu-id="03f2f-137">Обработчик авторизации отвечает за оценку свойств требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="03f2f-138">Обработчик авторизации оценивает требования к предоставленному [аусоризатионхандлерконтекст](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) , чтобы определить, разрешен ли доступ.</span><span class="sxs-lookup"><span data-stu-id="03f2f-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="03f2f-139">Требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="03f2f-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="03f2f-140">Обработчик может наследовать [AuthorizationHandler\<трекуиремент >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` является требованием обработки.</span><span class="sxs-lookup"><span data-stu-id="03f2f-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="03f2f-141">Кроме того, обработчик может реализовать [иаусоризатионхандлер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) для обработки более чем одного типа требований.</span><span class="sxs-lookup"><span data-stu-id="03f2f-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="03f2f-142">Использование обработчика для одного требования</span><span class="sxs-lookup"><span data-stu-id="03f2f-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="03f2f-143">Ниже приведен пример связи «один к одному», в которой обработчик минимального возраста использует одно требование:</span><span class="sxs-lookup"><span data-stu-id="03f2f-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="03f2f-144">Приведенный выше код определяет, имеет ли текущий участник пользователя дату утверждения о рождении, выданного известным и надежным издателем.</span><span class="sxs-lookup"><span data-stu-id="03f2f-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="03f2f-145">Авторизация не может произойти, если утверждение отсутствует, в этом случае возвращается завершенная задача.</span><span class="sxs-lookup"><span data-stu-id="03f2f-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="03f2f-146">При наличии утверждения возраст пользователя вычисляется.</span><span class="sxs-lookup"><span data-stu-id="03f2f-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="03f2f-147">Если пользователь соответствует минимальному возрасту, заданному требованием, авторизация считается успешной.</span><span class="sxs-lookup"><span data-stu-id="03f2f-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="03f2f-148">После успешной авторизации `context.Succeed` вызывается с удовлетворенным требованием в качестве единственного параметра.</span><span class="sxs-lookup"><span data-stu-id="03f2f-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="03f2f-149">Использование обработчика для нескольких требований</span><span class="sxs-lookup"><span data-stu-id="03f2f-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="03f2f-150">Ниже приведен пример связи «один ко многим», в которой обработчик разрешений может управлять тремя различными типами требований:</span><span class="sxs-lookup"><span data-stu-id="03f2f-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="03f2f-151">Предыдущий код проходит [пендингрекуирементс](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойство, содержащее требования, не помеченные как неудачные.</span><span class="sxs-lookup"><span data-stu-id="03f2f-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="03f2f-152">Для `ReadPermission` требования пользователь должен быть либо владельцем, либо спонсором для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="03f2f-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="03f2f-153">В случае `EditPermission` или `DeletePermission` необходимо быть владельцем для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="03f2f-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="03f2f-154">Регистрация обработчика</span><span class="sxs-lookup"><span data-stu-id="03f2f-154">Handler registration</span></span>

<span data-ttu-id="03f2f-155">Обработчики регистрируются в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="03f2f-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="03f2f-156">Пример:</span><span class="sxs-lookup"><span data-stu-id="03f2f-156">For example:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=31-32,39-40,42-45, 53-55, 58)]

<span data-ttu-id="03f2f-157">Предыдущий код регистрирует `MinimumAgeHandler` как одноэлементный, вызывая `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="03f2f-158">Обработчики можно регистрировать с помощью любого встроенного [времени существования службы](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="03f2f-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="03f2f-159">Что должен возвращать обработчик?</span><span class="sxs-lookup"><span data-stu-id="03f2f-159">What should a handler return?</span></span>

<span data-ttu-id="03f2f-160">Обратите внимание, что метод `Handle` в [примере обработчика](#security-authorization-handler-example) не возвращает значения.</span><span class="sxs-lookup"><span data-stu-id="03f2f-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="03f2f-161">Как отображается состояние «успешно» или «сбой»?</span><span class="sxs-lookup"><span data-stu-id="03f2f-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="03f2f-162">Обработчик указывает на успешное выполнение путем вызова `context.Succeed(IAuthorizationRequirement requirement)`, передавая требование, которое прошло проверку.</span><span class="sxs-lookup"><span data-stu-id="03f2f-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="03f2f-163">Обработчику обычно не требуется обработка ошибок, так как другие обработчики для такого же требования могут быть выполнены удачно.</span><span class="sxs-lookup"><span data-stu-id="03f2f-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="03f2f-164">Чтобы гарантировать сбой, даже если другие обработчики требований выполняются успешно, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="03f2f-165">Если обработчик вызывает `context.Succeed` или `context.Fail`, все остальные обработчики все еще вызываются.</span><span class="sxs-lookup"><span data-stu-id="03f2f-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="03f2f-166">Это позволяет создавать побочные эффекты, например ведение журнала, которое выполняется, даже если другой обработчик прошел проверку или не прошел требование.</span><span class="sxs-lookup"><span data-stu-id="03f2f-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="03f2f-167">Если задано значение `false`, свойство [инвокехандлерсафтерфаилуре](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (доступно в ASP.NET Core 1,1 и более поздних версиях) сокращено при выполнении обработчиков при вызове `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="03f2f-168">`InvokeHandlersAfterFailure` значение по умолчанию равно `true`, в этом случае вызываются все обработчики.</span><span class="sxs-lookup"><span data-stu-id="03f2f-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="03f2f-169">Обработчики авторизации вызываются даже в случае сбоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="03f2f-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="03f2f-170">Зачем требуется несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="03f2f-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="03f2f-171">В случаях, когда требуется, чтобы оценка была в **или** на основе, реализуйте несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="03f2f-172">Например, в корпорации Майкрософт есть дверцы, которые открываются только с карточками ключей.</span><span class="sxs-lookup"><span data-stu-id="03f2f-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="03f2f-173">Если вы оставите свой ключ дома, секретарь выводит временную наклейку и открывает дверцу.</span><span class="sxs-lookup"><span data-stu-id="03f2f-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="03f2f-174">В этом сценарии у вас будет одно требование, *буилдинжентри*, но несколько обработчиков, каждый из которых проанализировать одно требование.</span><span class="sxs-lookup"><span data-stu-id="03f2f-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="03f2f-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="03f2f-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="03f2f-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="03f2f-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="03f2f-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="03f2f-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="03f2f-178">Убедитесь, что оба обработчика [зарегистрированы](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="03f2f-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="03f2f-179">Если какой-либо из обработчиков будет выполнен после того, как политика вычислит `BuildingEntryRequirement`, оценка политики будет выполнена.</span><span class="sxs-lookup"><span data-stu-id="03f2f-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="03f2f-180">Использование функции func для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="03f2f-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="03f2f-181">Могут возникнуть ситуации, в которых выполнение политики может быть простым для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="03f2f-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="03f2f-182">При настройке политики с помощью построителя `RequireAssertion` политики можно указать `Func<AuthorizationHandlerContext, bool>`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="03f2f-183">Например, Предыдущая `BadgeEntryHandler` может быть переписана следующим образом:</span><span class="sxs-lookup"><span data-stu-id="03f2f-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/3.0PoliciesAuthApp1/Startup.cs?range=42-43,47-53)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="03f2f-184">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="03f2f-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="03f2f-185">Метод `HandleRequirementAsync`, реализуемый в обработчике авторизации, имеет два параметра: `AuthorizationHandlerContext` и `TRequirement`, которые вы обрабатываете.</span><span class="sxs-lookup"><span data-stu-id="03f2f-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="03f2f-186">Платформы, такие как MVC или Жаббр, могут добавлять любой объект в свойство `Resource` `AuthorizationHandlerContext` для передачи дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="03f2f-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="03f2f-187">Например, MVC передает экземпляр [аусоризатионфилтерконтекст](/dotnet/api/?term=AuthorizationFilterContext) в свойство `Resource`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="03f2f-188">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и всем остальным, предоставляемым MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="03f2f-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="03f2f-189">Использование свойства `Resource` зависит от платформы.</span><span class="sxs-lookup"><span data-stu-id="03f2f-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="03f2f-190">Использование сведений в свойстве `Resource` ограничивает политики авторизации определенными платформами.</span><span class="sxs-lookup"><span data-stu-id="03f2f-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="03f2f-191">Необходимо привести свойство `Resource` с помощью ключевого слова `is`, а затем подтвердить успешное приведение, чтобы гарантировать, что код не завершится сбоем с `InvalidCastException` при запуске на других платформах:</span><span class="sxs-lookup"><span data-stu-id="03f2f-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end


::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="03f2f-192">В процессе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) используются требования, обработчик требований и предварительно настроенная политика.</span><span class="sxs-lookup"><span data-stu-id="03f2f-192">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="03f2f-193">Эти стандартные блоки поддерживают выражение оценки авторизации в коде.</span><span class="sxs-lookup"><span data-stu-id="03f2f-193">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="03f2f-194">Результатом является расширенная, повторно используемая, тестируемая структура авторизации.</span><span class="sxs-lookup"><span data-stu-id="03f2f-194">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="03f2f-195">Политика авторизации состоит из одного или нескольких требований.</span><span class="sxs-lookup"><span data-stu-id="03f2f-195">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="03f2f-196">Он регистрируется как часть конфигурации службы авторизации в методе `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03f2f-196">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="03f2f-197">В предыдущем примере создается политика "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="03f2f-197">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="03f2f-198">Он имеет одно требование&mdash;, которое имеет минимальный возраст, который предоставляется в качестве параметра для требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-198">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="03f2f-199">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="03f2f-199">IAuthorizationService</span></span> 

<span data-ttu-id="03f2f-200">Основная служба, определяющая успешность авторизации, <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span><span class="sxs-lookup"><span data-stu-id="03f2f-200">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="03f2f-201">В приведенном выше коде показаны два метода [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="03f2f-201">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="03f2f-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> — это служба маркеров без методов, а также механизм отслеживания успешности авторизации.</span><span class="sxs-lookup"><span data-stu-id="03f2f-202"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="03f2f-203">Каждый <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> отвечает за проверку соблюдения требований:</span><span class="sxs-lookup"><span data-stu-id="03f2f-203">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/dotnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

```csharp
/// <summary>
/// Classes implementing this interface are able to make a decision if authorization
/// is allowed.
/// </summary>
public interface IAuthorizationHandler
{
    /// <summary>
    /// Makes a decision if authorization is allowed.
    /// </summary>
    /// <param name="context">The authorization information.</param>
    Task HandleAsync(AuthorizationHandlerContext context);
}
```

<span data-ttu-id="03f2f-204">Класс <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> используется обработчиком для обозначения того, выполнены ли требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-204">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="03f2f-205">В следующем коде показаны упрощенная реализация службы авторизации по умолчанию (и заметки с комментариями):</span><span class="sxs-lookup"><span data-stu-id="03f2f-205">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

```csharp
public async Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user, 
             object resource, IEnumerable<IAuthorizationRequirement> requirements)
{
    // Create a tracking context from the authorization inputs.
    var authContext = _contextFactory.CreateContext(requirements, user, resource);

    // By default this returns an IEnumerable<IAuthorizationHandlers> from DI.
    var handlers = await _handlers.GetHandlersAsync(authContext);

    // Invoke all handlers.
    foreach (var handler in handlers)
    {
        await handler.HandleAsync(authContext);
    }

    // Check the context, by default success is when all requirements have been met.
    return _evaluator.Evaluate(authContext);
}
```

<span data-ttu-id="03f2f-206">В следующем коде показан типичный `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="03f2f-206">The following code shows a typical `ConfigureServices`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    // Add all of your handlers to DI.
    services.AddSingleton<IAuthorizationHandler, MyHandler1>();
    // MyHandler2, ...

    services.AddSingleton<IAuthorizationHandler, MyHandlerN>();

    // Configure your policies
    services.AddAuthorization(options =>
          options.AddPolicy("Something",
          policy => policy.RequireClaim("Permission", "CanViewPage", "CanViewAnything")));


    services.AddMvc().SetCompatibilityVersion(CompatibilityVersion.Version_2_2);
}
```

<span data-ttu-id="03f2f-207">Для авторизации используйте <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> или `[Authorize(Policy = "Something")]`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-207">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="03f2f-208">Применение политик к контроллерам MVC</span><span class="sxs-lookup"><span data-stu-id="03f2f-208">Applying policies to MVC controllers</span></span>

<span data-ttu-id="03f2f-209">Если вы используете Razor Pages, см. раздел [применение политик к Razor Pages](#applying-policies-to-razor-pages) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="03f2f-209">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="03f2f-210">Политики применяются к контроллерам с помощью атрибута `[Authorize]` с именем политики.</span><span class="sxs-lookup"><span data-stu-id="03f2f-210">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="03f2f-211">Пример:</span><span class="sxs-lookup"><span data-stu-id="03f2f-211">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="03f2f-212">Применение политик к Razor Pages</span><span class="sxs-lookup"><span data-stu-id="03f2f-212">Applying policies to Razor Pages</span></span>

<span data-ttu-id="03f2f-213">Политики применяются к Razor Pages с помощью атрибута `[Authorize]` с именем политики.</span><span class="sxs-lookup"><span data-stu-id="03f2f-213">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="03f2f-214">Пример:</span><span class="sxs-lookup"><span data-stu-id="03f2f-214">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="03f2f-215">Политики также можно применить к Razor Pages с помощью соглашения об [авторизации](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="03f2f-215">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="03f2f-216">Требования</span><span class="sxs-lookup"><span data-stu-id="03f2f-216">Requirements</span></span>

<span data-ttu-id="03f2f-217">Требование авторизации — это коллекция параметров данных, которую политика может использовать для вычисления текущего субъекта-пользователя.</span><span class="sxs-lookup"><span data-stu-id="03f2f-217">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="03f2f-218">В нашей политике "AtLeast21" требование относится к одному параметру&mdash;минимального срока.</span><span class="sxs-lookup"><span data-stu-id="03f2f-218">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="03f2f-219">Требование реализует [иаусоризатионрекуиремент](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), который является пустым интерфейсом маркера.</span><span class="sxs-lookup"><span data-stu-id="03f2f-219">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="03f2f-220">Требование к параметризованному минимальному возрасту можно реализовать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="03f2f-220">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="03f2f-221">Если политика авторизации содержит несколько требований к авторизации, все требования должны пройти, чтобы оценка политики прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="03f2f-221">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="03f2f-222">Иными словами, несколько требований к авторизации, добавленных в одну политику авторизации, обрабатываются **и** на основе.</span><span class="sxs-lookup"><span data-stu-id="03f2f-222">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="03f2f-223">Требование не обязательно должно иметь данные или свойства.</span><span class="sxs-lookup"><span data-stu-id="03f2f-223">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="03f2f-224">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="03f2f-224">Authorization handlers</span></span>

<span data-ttu-id="03f2f-225">Обработчик авторизации отвечает за оценку свойств требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-225">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="03f2f-226">Обработчик авторизации оценивает требования к предоставленному [аусоризатионхандлерконтекст](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) , чтобы определить, разрешен ли доступ.</span><span class="sxs-lookup"><span data-stu-id="03f2f-226">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="03f2f-227">Требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="03f2f-227">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="03f2f-228">Обработчик может наследовать [AuthorizationHandler\<трекуиремент >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` является требованием обработки.</span><span class="sxs-lookup"><span data-stu-id="03f2f-228">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="03f2f-229">Кроме того, обработчик может реализовать [иаусоризатионхандлер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) для обработки более чем одного типа требований.</span><span class="sxs-lookup"><span data-stu-id="03f2f-229">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="03f2f-230">Использование обработчика для одного требования</span><span class="sxs-lookup"><span data-stu-id="03f2f-230">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="03f2f-231">Ниже приведен пример связи «один к одному», в которой обработчик минимального возраста использует одно требование:</span><span class="sxs-lookup"><span data-stu-id="03f2f-231">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="03f2f-232">Приведенный выше код определяет, имеет ли текущий участник пользователя дату утверждения о рождении, выданного известным и надежным издателем.</span><span class="sxs-lookup"><span data-stu-id="03f2f-232">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="03f2f-233">Авторизация не может произойти, если утверждение отсутствует, в этом случае возвращается завершенная задача.</span><span class="sxs-lookup"><span data-stu-id="03f2f-233">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="03f2f-234">При наличии утверждения возраст пользователя вычисляется.</span><span class="sxs-lookup"><span data-stu-id="03f2f-234">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="03f2f-235">Если пользователь соответствует минимальному возрасту, заданному требованием, авторизация считается успешной.</span><span class="sxs-lookup"><span data-stu-id="03f2f-235">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="03f2f-236">После успешной авторизации `context.Succeed` вызывается с удовлетворенным требованием в качестве единственного параметра.</span><span class="sxs-lookup"><span data-stu-id="03f2f-236">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="03f2f-237">Использование обработчика для нескольких требований</span><span class="sxs-lookup"><span data-stu-id="03f2f-237">Use a handler for multiple requirements</span></span>

<span data-ttu-id="03f2f-238">Ниже приведен пример связи «один ко многим», в которой обработчик разрешений может управлять тремя различными типами требований:</span><span class="sxs-lookup"><span data-stu-id="03f2f-238">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="03f2f-239">Предыдущий код проходит [пендингрекуирементс](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойство, содержащее требования, не помеченные как неудачные.</span><span class="sxs-lookup"><span data-stu-id="03f2f-239">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="03f2f-240">Для `ReadPermission` требования пользователь должен быть либо владельцем, либо спонсором для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="03f2f-240">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="03f2f-241">В случае `EditPermission` или `DeletePermission` необходимо быть владельцем для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="03f2f-241">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="03f2f-242">Регистрация обработчика</span><span class="sxs-lookup"><span data-stu-id="03f2f-242">Handler registration</span></span>

<span data-ttu-id="03f2f-243">Обработчики регистрируются в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="03f2f-243">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="03f2f-244">Пример:</span><span class="sxs-lookup"><span data-stu-id="03f2f-244">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="03f2f-245">Предыдущий код регистрирует `MinimumAgeHandler` как одноэлементный, вызывая `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-245">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="03f2f-246">Обработчики можно регистрировать с помощью любого встроенного [времени существования службы](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="03f2f-246">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="03f2f-247">Что должен возвращать обработчик?</span><span class="sxs-lookup"><span data-stu-id="03f2f-247">What should a handler return?</span></span>

<span data-ttu-id="03f2f-248">Обратите внимание, что метод `Handle` в [примере обработчика](#security-authorization-handler-example) не возвращает значения.</span><span class="sxs-lookup"><span data-stu-id="03f2f-248">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="03f2f-249">Как отображается состояние «успешно» или «сбой»?</span><span class="sxs-lookup"><span data-stu-id="03f2f-249">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="03f2f-250">Обработчик указывает на успешное выполнение путем вызова `context.Succeed(IAuthorizationRequirement requirement)`, передавая требование, которое прошло проверку.</span><span class="sxs-lookup"><span data-stu-id="03f2f-250">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="03f2f-251">Обработчику обычно не требуется обработка ошибок, так как другие обработчики для такого же требования могут быть выполнены удачно.</span><span class="sxs-lookup"><span data-stu-id="03f2f-251">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="03f2f-252">Чтобы гарантировать сбой, даже если другие обработчики требований выполняются успешно, вызовите `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-252">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="03f2f-253">Если обработчик вызывает `context.Succeed` или `context.Fail`, все остальные обработчики все еще вызываются.</span><span class="sxs-lookup"><span data-stu-id="03f2f-253">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="03f2f-254">Это позволяет создавать побочные эффекты, например ведение журнала, которое выполняется, даже если другой обработчик прошел проверку или не прошел требование.</span><span class="sxs-lookup"><span data-stu-id="03f2f-254">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="03f2f-255">Если задано значение `false`, свойство [инвокехандлерсафтерфаилуре](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (доступно в ASP.NET Core 1,1 и более поздних версиях) сокращено при выполнении обработчиков при вызове `context.Fail`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-255">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="03f2f-256">`InvokeHandlersAfterFailure` значение по умолчанию равно `true`, в этом случае вызываются все обработчики.</span><span class="sxs-lookup"><span data-stu-id="03f2f-256">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="03f2f-257">Обработчики авторизации вызываются даже в случае сбоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="03f2f-257">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="03f2f-258">Зачем требуется несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="03f2f-258">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="03f2f-259">В случаях, когда требуется, чтобы оценка была в **или** на основе, реализуйте несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="03f2f-259">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="03f2f-260">Например, в корпорации Майкрософт есть дверцы, которые открываются только с карточками ключей.</span><span class="sxs-lookup"><span data-stu-id="03f2f-260">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="03f2f-261">Если вы оставите свой ключ дома, секретарь выводит временную наклейку и открывает дверцу.</span><span class="sxs-lookup"><span data-stu-id="03f2f-261">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="03f2f-262">В этом сценарии у вас будет одно требование, *буилдинжентри*, но несколько обработчиков, каждый из которых проанализировать одно требование.</span><span class="sxs-lookup"><span data-stu-id="03f2f-262">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="03f2f-263">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="03f2f-263">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="03f2f-264">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="03f2f-264">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="03f2f-265">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="03f2f-265">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="03f2f-266">Убедитесь, что оба обработчика [зарегистрированы](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="03f2f-266">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="03f2f-267">Если какой-либо из обработчиков будет выполнен после того, как политика вычислит `BuildingEntryRequirement`, оценка политики будет выполнена.</span><span class="sxs-lookup"><span data-stu-id="03f2f-267">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="03f2f-268">Использование функции func для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="03f2f-268">Using a func to fulfill a policy</span></span>

<span data-ttu-id="03f2f-269">Могут возникнуть ситуации, в которых выполнение политики может быть простым для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="03f2f-269">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="03f2f-270">При настройке политики с помощью построителя `RequireAssertion` политики можно указать `Func<AuthorizationHandlerContext, bool>`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-270">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="03f2f-271">Например, Предыдущая `BadgeEntryHandler` может быть переписана следующим образом:</span><span class="sxs-lookup"><span data-stu-id="03f2f-271">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="03f2f-272">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="03f2f-272">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="03f2f-273">Метод `HandleRequirementAsync`, реализуемый в обработчике авторизации, имеет два параметра: `AuthorizationHandlerContext` и `TRequirement`, которые вы обрабатываете.</span><span class="sxs-lookup"><span data-stu-id="03f2f-273">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="03f2f-274">Платформы, такие как MVC или Жаббр, могут добавлять любой объект в свойство `Resource` `AuthorizationHandlerContext` для передачи дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="03f2f-274">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="03f2f-275">Например, MVC передает экземпляр [аусоризатионфилтерконтекст](/dotnet/api/?term=AuthorizationFilterContext) в свойство `Resource`.</span><span class="sxs-lookup"><span data-stu-id="03f2f-275">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="03f2f-276">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и всем остальным, предоставляемым MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="03f2f-276">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="03f2f-277">Использование свойства `Resource` зависит от платформы.</span><span class="sxs-lookup"><span data-stu-id="03f2f-277">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="03f2f-278">Использование сведений в свойстве `Resource` ограничивает политики авторизации определенными платформами.</span><span class="sxs-lookup"><span data-stu-id="03f2f-278">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="03f2f-279">Необходимо привести свойство `Resource` с помощью ключевого слова `is`, а затем подтвердить успешное приведение, чтобы гарантировать, что код не завершится сбоем с `InvalidCastException` при запуске на других платформах:</span><span class="sxs-lookup"><span data-stu-id="03f2f-279">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```

::: moniker-end