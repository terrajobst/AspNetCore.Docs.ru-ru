---
title: Авторизация на основе политик в ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать и использовать обработчики политик авторизации для применения требований к авторизации в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: 60625944d4ba31da6b98bdf947991088dc75ed87
ms.sourcegitcommit: 7001657c00358b082734ba4273693b9b3ed35d2a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/31/2019
ms.locfileid: "68669972"
---
# <a name="policy-based-authorization-in-aspnet-core"></a><span data-ttu-id="fa5fd-103">Авторизация на основе политик в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fa5fd-103">Policy-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="fa5fd-104">В процессе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) используются требования, обработчик требований и предварительно настроенная политика.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-104">Underneath the covers, [role-based authorization](xref:security/authorization/roles) and [claims-based authorization](xref:security/authorization/claims) use a requirement, a requirement handler, and a pre-configured policy.</span></span> <span data-ttu-id="fa5fd-105">Эти стандартные блоки поддерживают выражение оценки авторизации в коде.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-105">These building blocks support the expression of authorization evaluations in code.</span></span> <span data-ttu-id="fa5fd-106">Результатом является расширенная, повторно используемая, тестируемая структура авторизации.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-106">The result is a richer, reusable, testable authorization structure.</span></span>

<span data-ttu-id="fa5fd-107">Политика авторизации состоит из одного или нескольких требований.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-107">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="fa5fd-108">Он регистрируется как часть конфигурации службы авторизации в `Startup.ConfigureServices` методе:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-108">It's registered as part of the authorization service configuration, in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

<span data-ttu-id="fa5fd-109">В предыдущем примере создается политика "AtLeast21".</span><span class="sxs-lookup"><span data-stu-id="fa5fd-109">In the preceding example, an "AtLeast21" policy is created.</span></span> <span data-ttu-id="fa5fd-110">Он имеет одно требование&mdash;с минимальным возрастом, которое предоставляется в качестве параметра для требования.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-110">It has a single requirement&mdash;that of a minimum age, which is supplied as a parameter to the requirement.</span></span>

## <a name="iauthorizationservice"></a><span data-ttu-id="fa5fd-111">IAuthorizationService</span><span class="sxs-lookup"><span data-stu-id="fa5fd-111">IAuthorizationService</span></span> 

<span data-ttu-id="fa5fd-112">Основная служба, которая определяет, успешно ли выполнена <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>авторизация:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-112">The primary service that determines if authorization is successful is <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService>:</span></span>

[!code-csharp[](policies/samples/stubs/copy_of_IAuthorizationService.cs?highlight=24-25,48-49&name=snippet)]

<span data-ttu-id="fa5fd-113">В приведенном выше коде показаны два метода [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span><span class="sxs-lookup"><span data-stu-id="fa5fd-113">The preceding code highlights the two methods of the [IAuthorizationService](https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationService.cs).</span></span>

<span data-ttu-id="fa5fd-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement>— это служба маркеров без методов, а также механизм отслеживания успешности авторизации.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-114"><xref:Microsoft.AspNetCore.Authorization.IAuthorizationRequirement> is a marker service with no methods, and the mechanism for tracking whether authorization is successful.</span></span>

<span data-ttu-id="fa5fd-115">Каждый <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> из них отвечает за проверку соблюдения требований:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-115">Each <xref:Microsoft.AspNetCore.Authorization.IAuthorizationHandler> is responsible for checking if requirements are met:</span></span>
<!--The following code is a copy/paste from 
https://github.com/aspnet/AspNetCore/blob/v2.2.4/src/Security/Authorization/Core/src/IAuthorizationHandler.cs -->

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

<span data-ttu-id="fa5fd-116">Этот <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> класс используется обработчиком для обозначения того, выполнены ли требования.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-116">The <xref:Microsoft.AspNetCore.Authorization.AuthorizationHandlerContext> class is what the handler uses to mark whether requirements have been met:</span></span>

```csharp
 context.Succeed(requirement)
```

<span data-ttu-id="fa5fd-117">В следующем коде показаны упрощенная реализация службы авторизации по умолчанию (и заметки с комментариями):</span><span class="sxs-lookup"><span data-stu-id="fa5fd-117">The following code shows the simplified (and annotated with comments) default implementation of the authorization service:</span></span>

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

<span data-ttu-id="fa5fd-118">В следующем коде показана типичная `ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-118">The following code shows a typical `ConfigureServices`:</span></span>

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

<span data-ttu-id="fa5fd-119">Используйте <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> или`[Authorize(Policy = "Something")]` для авторизации.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-119">Use <xref:Microsoft.AspNetCore.Authorization.IAuthorizationService> or `[Authorize(Policy = "Something")]` for authorization.</span></span>

## <a name="applying-policies-to-mvc-controllers"></a><span data-ttu-id="fa5fd-120">Применение политик к контроллерам MVC</span><span class="sxs-lookup"><span data-stu-id="fa5fd-120">Applying policies to MVC controllers</span></span>

<span data-ttu-id="fa5fd-121">Если вы используете Razor Pages, см. раздел [применение политик к Razor Pages](#applying-policies-to-razor-pages) в этом документе.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-121">If you're using Razor Pages, see [Applying policies to Razor Pages](#applying-policies-to-razor-pages) in this document.</span></span>

<span data-ttu-id="fa5fd-122">Политики применяются к контроллерам с помощью `[Authorize]` атрибута с именем политики.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-122">Policies are applied to controllers by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="fa5fd-123">Например:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-123">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a><span data-ttu-id="fa5fd-124">Применение политик к Razor Pages</span><span class="sxs-lookup"><span data-stu-id="fa5fd-124">Applying policies to Razor Pages</span></span>

<span data-ttu-id="fa5fd-125">Политики применяются к Razor Pages с помощью `[Authorize]` атрибута с именем политики.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-125">Policies are applied to Razor Pages by using the `[Authorize]` attribute with the policy name.</span></span> <span data-ttu-id="fa5fd-126">Например:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-126">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

<span data-ttu-id="fa5fd-127">Политики также можно применить к Razor Pages с помощью соглашения об [авторизации](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="fa5fd-127">Policies can also be applied to Razor Pages by using an [authorization convention](xref:security/authorization/razor-pages-authorization).</span></span>

## <a name="requirements"></a><span data-ttu-id="fa5fd-128">Требования</span><span class="sxs-lookup"><span data-stu-id="fa5fd-128">Requirements</span></span>

<span data-ttu-id="fa5fd-129">Требование авторизации — это коллекция параметров данных, которую политика может использовать для вычисления текущего субъекта-пользователя.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-129">An authorization requirement is a collection of data parameters that a policy can use to evaluate the current user principal.</span></span> <span data-ttu-id="fa5fd-130">В нашей политике "AtLeast21" требование — это единственный параметр&mdash;с минимальным возрастом.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-130">In our "AtLeast21" policy, the requirement is a single parameter&mdash;the minimum age.</span></span> <span data-ttu-id="fa5fd-131">Требование реализует [иаусоризатионрекуиремент](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), который является пустым интерфейсом маркера.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-131">A requirement implements [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), which is an empty marker interface.</span></span> <span data-ttu-id="fa5fd-132">Требование к параметризованному минимальному возрасту можно реализовать следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-132">A parameterized minimum age requirement could be implemented as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

<span data-ttu-id="fa5fd-133">Если политика авторизации содержит несколько требований к авторизации, все требования должны пройти, чтобы оценка политики прошла успешно.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-133">If an authorization policy contains multiple authorization requirements, all requirements must pass in order for the policy evaluation to succeed.</span></span> <span data-ttu-id="fa5fd-134">Иными словами, несколько требований к авторизации, добавленных в одну политику авторизации, обрабатываются **и** на основе.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-134">In other words, multiple authorization requirements added to a single authorization policy are treated on an **AND** basis.</span></span>

> [!NOTE]
> <span data-ttu-id="fa5fd-135">Требование не обязательно должно иметь данные или свойства.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-135">A requirement doesn't need to have data or properties.</span></span>

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a><span data-ttu-id="fa5fd-136">Обработчики авторизации</span><span class="sxs-lookup"><span data-stu-id="fa5fd-136">Authorization handlers</span></span>

<span data-ttu-id="fa5fd-137">Обработчик авторизации отвечает за оценку свойств требования.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-137">An authorization handler is responsible for the evaluation of a requirement's properties.</span></span> <span data-ttu-id="fa5fd-138">Обработчик авторизации оценивает требования к предоставленному [аусоризатионхандлерконтекст](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) , чтобы определить, разрешен ли доступ.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-138">The authorization handler evaluates the requirements against a provided [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) to determine if access is allowed.</span></span>

<span data-ttu-id="fa5fd-139">Требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers).</span><span class="sxs-lookup"><span data-stu-id="fa5fd-139">A requirement can have [multiple handlers](#security-authorization-policies-based-multiple-handlers).</span></span> <span data-ttu-id="fa5fd-140">Обработчик может наследовать [AuthorizationHandler\<трекуиремент >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` является требованием для обработки.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-140">A handler may inherit [AuthorizationHandler\<TRequirement>](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), where `TRequirement` is the requirement to be handled.</span></span> <span data-ttu-id="fa5fd-141">Кроме того, обработчик может реализовать [иаусоризатионхандлер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) для обработки более чем одного типа требований.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-141">Alternatively, a handler may implement [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) to handle more than one type of requirement.</span></span>

### <a name="use-a-handler-for-one-requirement"></a><span data-ttu-id="fa5fd-142">Использование обработчика для одного требования</span><span class="sxs-lookup"><span data-stu-id="fa5fd-142">Use a handler for one requirement</span></span>

<a name="security-authorization-handler-example"></a>

<span data-ttu-id="fa5fd-143">Ниже приведен пример связи «один к одному», в которой обработчик минимального возраста использует одно требование:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-143">The following is an example of a one-to-one relationship in which a minimum age handler utilizes a single requirement:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

<span data-ttu-id="fa5fd-144">Приведенный выше код определяет, имеет ли текущий участник пользователя дату утверждения о рождении, выданного известным и надежным издателем.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-144">The preceding code determines if the current user principal has a date of birth claim which has been issued by a known and trusted Issuer.</span></span> <span data-ttu-id="fa5fd-145">Авторизация не может произойти, если утверждение отсутствует, в этом случае возвращается завершенная задача.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-145">Authorization can't occur when the claim is missing, in which case a completed task is returned.</span></span> <span data-ttu-id="fa5fd-146">При наличии утверждения возраст пользователя вычисляется.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-146">When a claim is present, the user's age is calculated.</span></span> <span data-ttu-id="fa5fd-147">Если пользователь соответствует минимальному возрасту, заданному требованием, авторизация считается успешной.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-147">If the user meets the minimum age defined by the requirement, authorization is deemed successful.</span></span> <span data-ttu-id="fa5fd-148">Когда авторизация прошла `context.Succeed` успешно, вызывается с удовлетворенным требованием в качестве единственного параметра.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-148">When authorization is successful, `context.Succeed` is invoked with the satisfied requirement as its sole parameter.</span></span>

### <a name="use-a-handler-for-multiple-requirements"></a><span data-ttu-id="fa5fd-149">Использование обработчика для нескольких требований</span><span class="sxs-lookup"><span data-stu-id="fa5fd-149">Use a handler for multiple requirements</span></span>

<span data-ttu-id="fa5fd-150">Ниже приведен пример связи «один ко многим», в которой обработчик разрешений может управлять тремя различными типами требований:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-150">The following is an example of a one-to-many relationship in which a permission handler can handle three different types of requirements:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

<span data-ttu-id="fa5fd-151">Предыдущий код проходит по [пендингрекуирементс](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойству, содержащему требования, не помеченные как неудачные.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-151">The preceding code traverses [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;a property containing requirements not marked as successful.</span></span> <span data-ttu-id="fa5fd-152">Для получения `ReadPermission` требования пользователь должен быть владельцем или спонсором для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-152">For a `ReadPermission` requirement, the user must be either an owner or a sponsor to access the requested resource.</span></span> <span data-ttu-id="fa5fd-153">В случае `EditPermission` требования или `DeletePermission` ему необходимо быть владельцем для доступа к запрошенному ресурсу.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-153">In the case of an `EditPermission` or `DeletePermission` requirement, he or she must be an owner to access the requested resource.</span></span>

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a><span data-ttu-id="fa5fd-154">Регистрация обработчика</span><span class="sxs-lookup"><span data-stu-id="fa5fd-154">Handler registration</span></span>

<span data-ttu-id="fa5fd-155">Обработчики регистрируются в коллекции служб во время настройки.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-155">Handlers are registered in the services collection during configuration.</span></span> <span data-ttu-id="fa5fd-156">Например:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-156">For example:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

<span data-ttu-id="fa5fd-157">Предыдущий код регистрируется `MinimumAgeHandler` как одноэлементный, `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`вызывая.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-157">The preceding code registers `MinimumAgeHandler` as a singleton by invoking `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`.</span></span> <span data-ttu-id="fa5fd-158">Обработчики можно регистрировать с помощью любого встроенного [времени существования службы](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="fa5fd-158">Handlers can be registered using any of the built-in [service lifetimes](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

## <a name="what-should-a-handler-return"></a><span data-ttu-id="fa5fd-159">Что должен возвращать обработчик?</span><span class="sxs-lookup"><span data-stu-id="fa5fd-159">What should a handler return?</span></span>

<span data-ttu-id="fa5fd-160">Обратите внимание `Handle` , что метод в [примере обработчика](#security-authorization-handler-example) не возвращает значения.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-160">Note that the `Handle` method in the [handler example](#security-authorization-handler-example) returns no value.</span></span> <span data-ttu-id="fa5fd-161">Как отображается состояние «успешно» или «сбой»?</span><span class="sxs-lookup"><span data-stu-id="fa5fd-161">How is a status of either success or failure indicated?</span></span>

* <span data-ttu-id="fa5fd-162">Обработчик указывает на успешное выполнение `context.Succeed(IAuthorizationRequirement requirement)`путем вызова, передавая требование, которое прошло проверку.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-162">A handler indicates success by calling `context.Succeed(IAuthorizationRequirement requirement)`, passing the requirement that has been successfully validated.</span></span>

* <span data-ttu-id="fa5fd-163">Обработчику обычно не требуется обработка ошибок, так как другие обработчики для такого же требования могут быть выполнены удачно.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-163">A handler doesn't need to handle failures generally, as other handlers for the same requirement may succeed.</span></span>

* <span data-ttu-id="fa5fd-164">Чтобы гарантировать сбой, даже если другие обработчики требований выполняются успешно `context.Fail`, вызовите.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-164">To guarantee failure, even if other requirement handlers succeed, call `context.Fail`.</span></span>

<span data-ttu-id="fa5fd-165">Если обработчик вызывает `context.Succeed` или `context.Fail`, все остальные обработчики все еще вызываются.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-165">If a handler calls `context.Succeed` or `context.Fail`, all other handlers are still called.</span></span> <span data-ttu-id="fa5fd-166">Это позволяет создавать побочные эффекты, например ведение журнала, которое выполняется, даже если другой обработчик прошел проверку или не прошел требование.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-166">This allows requirements to produce side effects, such as logging, which takes place even if another handler has successfully validated or failed a requirement.</span></span> <span data-ttu-id="fa5fd-167">Если задано `false`значение, свойство [инвокехандлерсафтерфаилуре](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) (доступно в ASP.NET Core 1,1 и более поздних версиях) сокращено при `context.Fail` вызове обработчиков.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-167">When set to `false`, the [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) property (available in ASP.NET Core 1.1 and later) short-circuits the execution of handlers when `context.Fail` is called.</span></span> <span data-ttu-id="fa5fd-168">`InvokeHandlersAfterFailure`по умолчанию имеет значение ,вэтомслучаевызываютсявсеобработчики.`true`</span><span class="sxs-lookup"><span data-stu-id="fa5fd-168">`InvokeHandlersAfterFailure` defaults to `true`, in which case all handlers are called.</span></span>

> [!NOTE]
> <span data-ttu-id="fa5fd-169">Обработчики авторизации вызываются даже в случае сбоя проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-169">Authorization handlers are called even if authentication fails.</span></span>

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a><span data-ttu-id="fa5fd-170">Зачем требуется несколько обработчиков для требования?</span><span class="sxs-lookup"><span data-stu-id="fa5fd-170">Why would I want multiple handlers for a requirement?</span></span>

<span data-ttu-id="fa5fd-171">В случаях, когда требуется, чтобы оценка была в **или** на основе, реализуйте несколько обработчиков для одного требования.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-171">In cases where you want evaluation to be on an **OR** basis, implement multiple handlers for a single requirement.</span></span> <span data-ttu-id="fa5fd-172">Например, в корпорации Майкрософт есть дверцы, которые открываются только с карточками ключей.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-172">For example, Microsoft has doors which only open with key cards.</span></span> <span data-ttu-id="fa5fd-173">Если вы оставите свой ключ дома, секретарь выводит временную наклейку и открывает дверцу.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-173">If you leave your key card at home, the receptionist prints a temporary sticker and opens the door for you.</span></span> <span data-ttu-id="fa5fd-174">В этом сценарии у вас будет одно требование, *буилдинжентри*, но несколько обработчиков, каждый из которых проанализировать одно требование.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-174">In this scenario, you'd have a single requirement, *BuildingEntry*, but multiple handlers, each one examining a single requirement.</span></span>

<span data-ttu-id="fa5fd-175">*BuildingEntryRequirement.cs*</span><span class="sxs-lookup"><span data-stu-id="fa5fd-175">*BuildingEntryRequirement.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

<span data-ttu-id="fa5fd-176">*BadgeEntryHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fa5fd-176">*BadgeEntryHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

<span data-ttu-id="fa5fd-177">*TemporaryStickerHandler.cs*</span><span class="sxs-lookup"><span data-stu-id="fa5fd-177">*TemporaryStickerHandler.cs*</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

<span data-ttu-id="fa5fd-178">Убедитесь, что оба обработчика [зарегистрированы](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span><span class="sxs-lookup"><span data-stu-id="fa5fd-178">Ensure that both handlers are [registered](xref:security/authorization/policies#security-authorization-policies-based-handler-registration).</span></span> <span data-ttu-id="fa5fd-179">Если любой из обработчиков будет `BuildingEntryRequirement`выполнен, когда политика оценивает, то Вычисление политики будет завершено.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-179">If either handler succeeds when a policy evaluates the `BuildingEntryRequirement`, the policy evaluation succeeds.</span></span>

## <a name="using-a-func-to-fulfill-a-policy"></a><span data-ttu-id="fa5fd-180">Использование функции func для выполнения политики</span><span class="sxs-lookup"><span data-stu-id="fa5fd-180">Using a func to fulfill a policy</span></span>

<span data-ttu-id="fa5fd-181">Могут возникнуть ситуации, в которых выполнение политики может быть простым для выражения в коде.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-181">There may be situations in which fulfilling a policy is simple to express in code.</span></span> <span data-ttu-id="fa5fd-182">`Func<AuthorizationHandlerContext, bool>` При настройке политики `RequireAssertion` с помощью построителя политик можно указать.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-182">It's possible to supply a `Func<AuthorizationHandlerContext, bool>` when configuring your policy with the `RequireAssertion` policy builder.</span></span>

<span data-ttu-id="fa5fd-183">Например, Предыдущая `BadgeEntryHandler` возможность может быть переписана следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-183">For example, the previous `BadgeEntryHandler` could be rewritten as follows:</span></span>

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a><span data-ttu-id="fa5fd-184">Доступ к контексту запроса MVC в обработчиках</span><span class="sxs-lookup"><span data-stu-id="fa5fd-184">Accessing MVC request context in handlers</span></span>

<span data-ttu-id="fa5fd-185">Метод, реализуемый в обработчике авторизации, имеет два параметра `AuthorizationHandlerContext` : и `TRequirement` обрабатываемый объект. `HandleRequirementAsync`</span><span class="sxs-lookup"><span data-stu-id="fa5fd-185">The `HandleRequirementAsync` method you implement in an authorization handler has two parameters: an `AuthorizationHandlerContext` and the `TRequirement` you are handling.</span></span> <span data-ttu-id="fa5fd-186">Платформы, такие как MVC или жаббр, могут добавлять любой объект в `Resource` свойство `AuthorizationHandlerContext` в для передачи дополнительной информации.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-186">Frameworks such as MVC or Jabbr are free to add any object to the `Resource` property on the `AuthorizationHandlerContext` to pass extra information.</span></span>

<span data-ttu-id="fa5fd-187">Например, MVC передает экземпляр [аусоризатионфилтерконтекст](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-187">For example, MVC passes an instance of [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) in the `Resource` property.</span></span> <span data-ttu-id="fa5fd-188">Это свойство предоставляет доступ к `HttpContext`, `RouteData`и всем остальным, предоставляемым MVC и Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-188">This property provides access to `HttpContext`, `RouteData`, and everything else provided by MVC and Razor Pages.</span></span>

<span data-ttu-id="fa5fd-189">Использование `Resource` свойства зависит от платформы.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-189">The use of the `Resource` property is framework specific.</span></span> <span data-ttu-id="fa5fd-190">Использование сведений в `Resource` свойстве ограничивает политики авторизации определенными платформами.</span><span class="sxs-lookup"><span data-stu-id="fa5fd-190">Using information in the `Resource` property limits your authorization policies to particular frameworks.</span></span> <span data-ttu-id="fa5fd-191">Необходимо привести `Resource` свойство `is` с помощью ключевого слова, а затем подтвердить успешное приведение, чтобы гарантировать, что код не будет `InvalidCastException` завершаться с ошибкой при выполнении в других платформах:</span><span class="sxs-lookup"><span data-stu-id="fa5fd-191">You should cast the `Resource` property using the `is` keyword, and then confirm the cast has succeeded to ensure your code doesn't crash with an `InvalidCastException` when run on other frameworks:</span></span>

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
