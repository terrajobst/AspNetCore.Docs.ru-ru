---
title: Пользовательская авторизация политики поставщиков в ASP.NET Core
author: mjrousos
description: Сведения об использовании пользовательских IAuthorizationPolicyProvider в приложении ASP.NET Core для динамического создания политик авторизации.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 218d7a495655598046671093c0cfe7b9622aca5e
ms.sourcegitcommit: 931b6a2d7eb28a0f1295e8a95690b8c4c5f58477
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/28/2018
ms.locfileid: "37077606"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="67a8f-103">Настраиваемые поставщики политики авторизации, используя IAuthorizationPolicyProvider в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="67a8f-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="67a8f-104">По [Майк Rousos](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="67a8f-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="67a8f-105">Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies), политики регистрируются путем вызова `AuthorizationOptions.AddPolicy` как часть конфигурации авторизации службы.</span><span class="sxs-lookup"><span data-stu-id="67a8f-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="67a8f-106">В некоторых сценариях, могут остаться возможно (или желательно) для регистрации всех политик авторизации, таким образом.</span><span class="sxs-lookup"><span data-stu-id="67a8f-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="67a8f-107">В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` для управления, как передаются политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="67a8f-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="67a8f-108">Примеры сценариев, где пользовательское [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) может оказаться полезным включить:</span><span class="sxs-lookup"><span data-stu-id="67a8f-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="67a8f-109">С помощью внешней службы для обеспечения оценки политики.</span><span class="sxs-lookup"><span data-stu-id="67a8f-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="67a8f-110">С помощью широкий диапазон политики (для номера разных мест или возрасте, например), поэтому не имеет смысла добавление каждой политики авторизации отдельных с `AuthorizationOptions.AddPolicy` вызова.</span><span class="sxs-lookup"><span data-stu-id="67a8f-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="67a8f-111">Создание политик во время выполнения на основе информации из внешнего источника данных (например, база данных), или динамически определить требования к проверке подлинности посредством другого механизма.</span><span class="sxs-lookup"><span data-stu-id="67a8f-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

## <a name="customizing-policy-retrieval"></a><span data-ttu-id="67a8f-112">Настройка политики извлечения</span><span class="sxs-lookup"><span data-stu-id="67a8f-112">Customizing policy retrieval</span></span>

<span data-ttu-id="67a8f-113">Приложения ASP.NET Core используется реализация `IAuthorizationPolicyProvider` интерфейс для получения политики авторизации.</span><span class="sxs-lookup"><span data-stu-id="67a8f-113">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="67a8f-114">По умолчанию [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется.</span><span class="sxs-lookup"><span data-stu-id="67a8f-114">By default, [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="67a8f-115">`DefaultAuthorizationPolicyProvider` Возвращает политики из `AuthorizationOptions` в `IServiceCollection.AddAuthorization` вызова.</span><span class="sxs-lookup"><span data-stu-id="67a8f-115">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="67a8f-116">Это поведение можно настроить путем регистрации другой `IAuthorizationPolicyProvider` реализации в приложении [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера.</span><span class="sxs-lookup"><span data-stu-id="67a8f-116">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="67a8f-117">`IAuthorizationPolicyProvider` Интерфейс содержит два API:</span><span class="sxs-lookup"><span data-stu-id="67a8f-117">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="67a8f-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) возвращает политику авторизации для данного имени.</span><span class="sxs-lookup"><span data-stu-id="67a8f-118">[GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="67a8f-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) возвращает политику авторизации по умолчанию (политику, используемую для `[Authorize]` атрибутов без политика указана).</span><span class="sxs-lookup"><span data-stu-id="67a8f-119">[GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="67a8f-120">Реализуя эти два API-интерфейсы, можно настроить, каким образом предоставляются политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="67a8f-120">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="67a8f-121">Параметризованные авторизовать пример атрибута</span><span class="sxs-lookup"><span data-stu-id="67a8f-121">Parameterized authorize attribute example</span></span>

<span data-ttu-id="67a8f-122">Один из сценариев где `IAuthorizationPolicyProvider` полезно Включение пользовательских `[Authorize]` атрибуты которого требования зависят от параметра.</span><span class="sxs-lookup"><span data-stu-id="67a8f-122">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="67a8f-123">Например, в [авторизации на основе политики](xref:security/authorization/policies) документации, на основе возраста («AtLeast21») политики использовался в качестве образца.</span><span class="sxs-lookup"><span data-stu-id="67a8f-123">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="67a8f-124">Если другой контроллер действия в приложении должны быть доступны пользователям *другой* возраста, бывает полезно иметь много различных политик на основе возраста.</span><span class="sxs-lookup"><span data-stu-id="67a8f-124">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="67a8f-125">Вместо регистрации всех различные возрастную политики необходимые приложения в `AuthorizationOptions`, можно создать политики динамически с пользовательским `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="67a8f-125">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="67a8f-126">Чтобы с помощью политики, проще, можно снабдить действия с атрибутом настраиваемой авторизации как `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="67a8f-126">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="67a8f-127">Атрибуты настраиваемой авторизации</span><span class="sxs-lookup"><span data-stu-id="67a8f-127">Custom Authorization Attributes</span></span>

<span data-ttu-id="67a8f-128">Политики авторизации идентифицируются по их именам.</span><span class="sxs-lookup"><span data-stu-id="67a8f-128">Authorization policies are identified by their names.</span></span> <span data-ttu-id="67a8f-129">Пользовательский `MinimumAgeAuthorizeAttribute` описанные ранее, необходимо сопоставить аргументов в строку, которая может использоваться для получения соответствующей политики авторизации.</span><span class="sxs-lookup"><span data-stu-id="67a8f-129">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="67a8f-130">Это можно сделать путем наследования от `AuthorizeAttribute` , делая `Age` свойство wrap `AuthorizeAttribute.Policy` свойство.</span><span class="sxs-lookup"><span data-stu-id="67a8f-130">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```CSharp
internal class MinimumAgeAuthorizeAttribute : AuthorizeAttribute
{
    const string POLICY_PREFIX = "MinimumAge";

    public MinimumAgeAuthorizeAttribute(int age) => Age = age;

    // Get or set the Age property by manipulating the underlying Policy property
    public int Age
    {
        get
        {
            if (int.TryParse(Policy.Substring(POLICY_PREFIX.Length), out var age))
            {
                return age;
            }
            return default(int);
        }
        set
        {
            Policy = $"{POLICY_PREFIX}{value.ToString()}";
        }
    }
}
```

<span data-ttu-id="67a8f-131">Этот тип атрибута имеет `Policy` строки на основе жестко префикса (`"MinimumAge"`) и является целым числом, переданный через конструктор.</span><span class="sxs-lookup"><span data-stu-id="67a8f-131">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="67a8f-132">Его можно применить к действиям так же, как другие `Authorize` атрибуты, за исключением того, что он принимает целое число как параметр.</span><span class="sxs-lookup"><span data-stu-id="67a8f-132">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="67a8f-133">Пользовательские IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="67a8f-133">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="67a8f-134">Пользовательский `MinimumAgeAuthorizeAttribute` упрощает политики авторизации запроса для любой минимальный срок действия требуемого.</span><span class="sxs-lookup"><span data-stu-id="67a8f-134">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="67a8f-135">Далее задачи для решения убедиться, что политики авторизации доступны для всех этих различных возрастов.</span><span class="sxs-lookup"><span data-stu-id="67a8f-135">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="67a8f-136">Это место, куда `IAuthorizationPolicyProvider` полезно.</span><span class="sxs-lookup"><span data-stu-id="67a8f-136">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="67a8f-137">При использовании `MinimumAgeAuthorizationAttribute`, имена политик авторизации будет использовать этот подход `"MinimumAge" + Age`, поэтому пользовательский `IAuthorizationPolicyProvider` должен создать политики авторизации с:</span><span class="sxs-lookup"><span data-stu-id="67a8f-137">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="67a8f-138">При синтаксическом анализе возраст с имя политики.</span><span class="sxs-lookup"><span data-stu-id="67a8f-138">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="67a8f-139">С помощью `AuthorizationPolicyBuilder` для создания нового `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="67a8f-139">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="67a8f-140">Добавление требований в политику на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="67a8f-140">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="67a8f-141">Можно использовать в других сценариях `RequireClaim`, `RequireRole`, или `RequireUserName` вместо него.</span><span class="sxs-lookup"><span data-stu-id="67a8f-141">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```CSharp
internal class MinimumAgePolicyProvider : IAuthorizationPolicyProvider
{
    const string POLICY_PREFIX = "MinimumAge";

    // Policies are looked up by string name, so expect 'parameters' (like age)
    // to be embedded in the policy names. This is abstracted away from developers
    // by the more strongly-typed attributes derived from AuthorizeAttribute
    // (like [MinimumAgeAuthorize()] in this sample)
    public Task<AuthorizationPolicy> GetPolicyAsync(string policyName)
    {
        if (policyName.StartsWith(POLICY_PREFIX, StringComparison.OrdinalIgnoreCase) &&
            int.TryParse(policyName.Substring(POLICY_PREFIX.Length), out var age))
        {
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="67a8f-142">Несколько поставщиков политики авторизации</span><span class="sxs-lookup"><span data-stu-id="67a8f-142">Multiple authorization policy providers</span></span>

<span data-ttu-id="67a8f-143">При использовании пользовательских `IAuthorizationPolicyProvider` реализации, имейте в виду, что ASP.NET Core использует только один экземпляр `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="67a8f-143">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="67a8f-144">Если пользовательский поставщик не может предоставить политик авторизации для имена всех политик, он должен выполнить откат до резервного копирования поставщика.</span><span class="sxs-lookup"><span data-stu-id="67a8f-144">If a custom provider isn't able to provide authorization policies for all policy names, it should fall back to a backup provider.</span></span> <span data-ttu-id="67a8f-145">Имена политик могут включать, поступающими от политику по умолчанию для `[Authorize]` атрибуты без имени.</span><span class="sxs-lookup"><span data-stu-id="67a8f-145">Policy names might include those that come from a default policy for `[Authorize]` attributes without a name.</span></span>

<span data-ttu-id="67a8f-146">Например рассмотрим, что приложение требуется пользовательский срок действия политики и более традиционным получения политики на основе ролей.</span><span class="sxs-lookup"><span data-stu-id="67a8f-146">For example, consider an application needed both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="67a8f-147">Такое приложение может использовать поставщик политики настраиваемой авторизации:</span><span class="sxs-lookup"><span data-stu-id="67a8f-147">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="67a8f-148">Пытается проанализировать имена политик.</span><span class="sxs-lookup"><span data-stu-id="67a8f-148">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="67a8f-149">Вызовы в другой политике поставщика (например `DefaultAuthorizationPolicyProvider`) Если политика не может содержать age.</span><span class="sxs-lookup"><span data-stu-id="67a8f-149">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

## <a name="default-policy"></a><span data-ttu-id="67a8f-150">Политика по умолчанию</span><span class="sxs-lookup"><span data-stu-id="67a8f-150">Default policy</span></span>

<span data-ttu-id="67a8f-151">Помимо политики авторизации для именованный, настраиваемый `IAuthorizationPolicyProvider` должен реализовывать `GetDefaultPolicyAsync` для предоставления политику авторизации для `[Authorize]` атрибуты не указано имя политики.</span><span class="sxs-lookup"><span data-stu-id="67a8f-151">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="67a8f-152">Во многих случаях этот атрибут авторизации требуется только прошедшим проверку пользователем, и внести необходимые политики с помощью вызова `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="67a8f-152">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

<span data-ttu-id="67a8f-153">Как и во всех аспектов пользовательского `IAuthorizationPolicyProvider`, этот параметр можно изменить, при необходимости.</span><span class="sxs-lookup"><span data-stu-id="67a8f-153">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="67a8f-154">В некоторых случаях:</span><span class="sxs-lookup"><span data-stu-id="67a8f-154">In some cases:</span></span>

* <span data-ttu-id="67a8f-155">Политики авторизации по умолчанию не могут использоваться.</span><span class="sxs-lookup"><span data-stu-id="67a8f-155">Default authorization policies might not be used.</span></span>
* <span data-ttu-id="67a8f-156">Получение политики по умолчанию можно делегировать переход на резервный ресурс `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="67a8f-156">Retrieving the default policy can be delegated to a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="using-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="67a8f-157">С помощью пользовательского IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="67a8f-157">Using a Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="67a8f-158">Использование настраиваемых политик из `IAuthorizationPolicyProvider`, необходимо:</span><span class="sxs-lookup"><span data-stu-id="67a8f-158">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="67a8f-159">Зарегистрируйте соответствующий `AuthorizationHandler` типов с помощью внедрения зависимости (описано в [авторизации на основе политики](xref:security/authorization/policies#authorization-handlers)), как и для всех сценариев на основе политики авторизации.</span><span class="sxs-lookup"><span data-stu-id="67a8f-159">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="67a8f-160">Регистрация пользовательского `IAuthorizationPolicyProvider` тип в коллекцию служб для внедрения зависимостей приложения (в `Startup.ConfigureServices`) для замены поставщика политики по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="67a8f-160">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="67a8f-161">Полный пользовательский `IAuthorizationPolicyProvider` пример доступен в [репозитории GitHub aspnet или AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="67a8f-161">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).</span></span>
