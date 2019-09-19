---
title: Пользовательские поставщики политики авторизации в ASP.NET Core
author: mjrousos
description: Узнайте, как использовать пользовательский Иаусоризатионполиципровидер в приложении ASP.NET Core для динамического создания политик авторизации.
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: b11f7f94e1042e65d5f2ff8ab0fe9ea7838bebeb
ms.sourcegitcommit: b1e480e1736b0fe0e4d8dce4a4cf5c8e47fc2101
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2019
ms.locfileid: "71108063"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="99cac-103">Пользовательские поставщики политики авторизации с использованием Иаусоризатионполиципровидер в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="99cac-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="99cac-104">По [Майк Роусос](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="99cac-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="99cac-105">Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies)политики регистрируются путем вызова `AuthorizationOptions.AddPolicy` в рамках настройки службы авторизации.</span><span class="sxs-lookup"><span data-stu-id="99cac-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="99cac-106">В некоторых сценариях такой способ регистрации всех политик авторизации может быть невозможен (или желательно).</span><span class="sxs-lookup"><span data-stu-id="99cac-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="99cac-107">В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` , чтобы контролировать, как предоставляются политики авторизации.</span><span class="sxs-lookup"><span data-stu-id="99cac-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="99cac-108">Примеры сценариев, в которых может быть полезна пользовательская [иаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :</span><span class="sxs-lookup"><span data-stu-id="99cac-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="99cac-109">Использование внешней службы для обеспечения оценки политики.</span><span class="sxs-lookup"><span data-stu-id="99cac-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="99cac-110">Использование большого диапазона политик (например, для разных номеров мест или возраста), поэтому не имеет смысла добавлять каждую отдельную политику авторизации с помощью `AuthorizationOptions.AddPolicy` вызова.</span><span class="sxs-lookup"><span data-stu-id="99cac-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn’t make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="99cac-111">Создание политик во время выполнения на основе информации во внешнем источнике данных (например, базы данных) или определение требований авторизации динамически с помощью другого механизма.</span><span class="sxs-lookup"><span data-stu-id="99cac-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="99cac-112">[Просмотрите или Скачайте пример кода](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) из [репозитория AspNetCore GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="99cac-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="99cac-113">Скачайте ZIP-файл репозитория ASPNET/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="99cac-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="99cac-114">Распакуйте файл.</span><span class="sxs-lookup"><span data-stu-id="99cac-114">Unzip the file.</span></span> <span data-ttu-id="99cac-115">Перейдите в папку проекта *src/Security/Samples/кустомполиципровидер* .</span><span class="sxs-lookup"><span data-stu-id="99cac-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="99cac-116">Настройка получения политики</span><span class="sxs-lookup"><span data-stu-id="99cac-116">Customize policy retrieval</span></span>

<span data-ttu-id="99cac-117">ASP.NET Core приложения используют реализацию `IAuthorizationPolicyProvider` интерфейса для получения политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="99cac-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="99cac-118">По умолчанию [дефаултаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется.</span><span class="sxs-lookup"><span data-stu-id="99cac-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="99cac-119">`DefaultAuthorizationPolicyProvider`Возвращает политики из `AuthorizationOptions` предоставленного `IServiceCollection.AddAuthorization` в вызове.</span><span class="sxs-lookup"><span data-stu-id="99cac-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="99cac-120">Это поведение можно настроить, зарегистрировав другую `IAuthorizationPolicyProvider` реализацию в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="99cac-120">You can customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app’s [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="99cac-121">`IAuthorizationPolicyProvider` Интерфейс содержит два интерфейса API:</span><span class="sxs-lookup"><span data-stu-id="99cac-121">The `IAuthorizationPolicyProvider` interface contains two APIs:</span></span>

* <span data-ttu-id="99cac-122">[Жетполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) Возвращает политику авторизации для заданного имени.</span><span class="sxs-lookup"><span data-stu-id="99cac-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="99cac-123">[Жетдефаултполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) Возвращает политику авторизации по умолчанию (политику, используемую для `[Authorize]` атрибутов без указанной политики).</span><span class="sxs-lookup"><span data-stu-id="99cac-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 

<span data-ttu-id="99cac-124">Реализуя эти два API, можно настроить политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="99cac-124">By implementing these two APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="99cac-125">Пример параметризованного атрибута авторизации</span><span class="sxs-lookup"><span data-stu-id="99cac-125">Parameterized authorize attribute example</span></span>

<span data-ttu-id="99cac-126">Один из сценариев `IAuthorizationPolicyProvider` , в котором полезно включить `[Authorize]` настраиваемые атрибуты, требования к которым зависят от параметра.</span><span class="sxs-lookup"><span data-stu-id="99cac-126">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="99cac-127">Например, в документации по [авторизации на основе политик](xref:security/authorization/policies) в качестве примера использовалась политика на основе возраста ("AtLeast21").</span><span class="sxs-lookup"><span data-stu-id="99cac-127">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="99cac-128">Если доступ к различным действиям контроллера в приложении должен предоставляться пользователям, имеющим *разный* возраст, может оказаться полезным использовать много политик на основе возраста.</span><span class="sxs-lookup"><span data-stu-id="99cac-128">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="99cac-129">Вместо регистрации всех разных политик на основе возраста, которые понадобятся `AuthorizationOptions`приложению, можно динамически создавать политики с настраиваемым. `IAuthorizationPolicyProvider`</span><span class="sxs-lookup"><span data-stu-id="99cac-129">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="99cac-130">Чтобы упростить использование политик, можно добавить заметки к действиям с помощью настраиваемого атрибута `[MinimumAgeAuthorize(20)]`авторизации, например.</span><span class="sxs-lookup"><span data-stu-id="99cac-130">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="99cac-131">Пользовательские атрибуты авторизации</span><span class="sxs-lookup"><span data-stu-id="99cac-131">Custom Authorization attributes</span></span>

<span data-ttu-id="99cac-132">Политики авторизации идентифицируются по именам.</span><span class="sxs-lookup"><span data-stu-id="99cac-132">Authorization policies are identified by their names.</span></span> <span data-ttu-id="99cac-133">Пользователь `MinimumAgeAuthorizeAttribute` , описанный выше, должен сопоставлять аргументы в строку, которая может использоваться для получения соответствующей политики авторизации.</span><span class="sxs-lookup"><span data-stu-id="99cac-133">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="99cac-134">Это можно сделать, производя от `AuthorizeAttribute` и `Age` сделав свойство оболочкой `AuthorizeAttribute.Policy` для свойства.</span><span class="sxs-lookup"><span data-stu-id="99cac-134">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

```csharp
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

<span data-ttu-id="99cac-135">Этот тип атрибута имеет `Policy` строку на основе жестко запрограммированного префикса (`"MinimumAge"`) и целого числа, переданного через конструктор.</span><span class="sxs-lookup"><span data-stu-id="99cac-135">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="99cac-136">Его можно применить к действиям так же, как и к `Authorize` другим атрибутам, за исключением того, что в качестве параметра принимается целое число.</span><span class="sxs-lookup"><span data-stu-id="99cac-136">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="99cac-137">Пользовательские Иаусоризатионполиципровидер</span><span class="sxs-lookup"><span data-stu-id="99cac-137">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="99cac-138">Пользовательский `MinimumAgeAuthorizeAttribute` параметр упрощает запрос политик авторизации для любого минимального возраста.</span><span class="sxs-lookup"><span data-stu-id="99cac-138">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="99cac-139">Следующая проблема, которую необходимо решить, заключается в том, чтобы обеспечить доступность политик авторизации для всех этих сбоев.</span><span class="sxs-lookup"><span data-stu-id="99cac-139">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="99cac-140">В `IAuthorizationPolicyProvider` этом случае полезно использовать.</span><span class="sxs-lookup"><span data-stu-id="99cac-140">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="99cac-141">При использовании `MinimumAgeAuthorizationAttribute`имена политик авторизации будут соответствовать шаблону `"MinimumAge" + Age`, поэтому пользователь `IAuthorizationPolicyProvider` должен создать политики авторизации следующим образом.</span><span class="sxs-lookup"><span data-stu-id="99cac-141">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="99cac-142">Анализ возраста на основе имени политики.</span><span class="sxs-lookup"><span data-stu-id="99cac-142">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="99cac-143">Использование `AuthorizationPolicyBuilder` для создания нового`AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="99cac-143">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="99cac-144">В этом и следующих примерах предполагается, что пользователь пройдет проверку подлинности с помощью файла cookie.</span><span class="sxs-lookup"><span data-stu-id="99cac-144">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="99cac-145">Значение `AuthorizationPolicyBuilder` должно быть создано по крайней мере с одним именем схемы авторизации или всегда выполняться.</span><span class="sxs-lookup"><span data-stu-id="99cac-145">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="99cac-146">В противном случае нет информации о том, как предоставить пользователю запрос, и будет выдано исключение.</span><span class="sxs-lookup"><span data-stu-id="99cac-146">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="99cac-147">Добавление требований к политике на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="99cac-147">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="99cac-148">В других сценариях вы можете использовать `RequireClaim`, `RequireRole`или `RequireUserName` .</span><span class="sxs-lookup"><span data-stu-id="99cac-148">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

```csharp
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
            var policy = new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme);
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="99cac-149">Несколько поставщиков политик авторизации</span><span class="sxs-lookup"><span data-stu-id="99cac-149">Multiple authorization policy providers</span></span>

<span data-ttu-id="99cac-150">При использовании пользовательских `IAuthorizationPolicyProvider` реализаций Помните, что ASP.NET Core использует только один `IAuthorizationPolicyProvider`экземпляр.</span><span class="sxs-lookup"><span data-stu-id="99cac-150">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="99cac-151">Если настраиваемый поставщик не может предоставить политики авторизации для всех имен политик, которые будут использоваться, она должна вернуться к поставщику резервных копий.</span><span class="sxs-lookup"><span data-stu-id="99cac-151">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should fall back to a backup provider.</span></span> 

<span data-ttu-id="99cac-152">Например, рассмотрим приложение, для которого требуются пользовательские политики возраста и более традиционную политику получения политик на основе ролей.</span><span class="sxs-lookup"><span data-stu-id="99cac-152">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="99cac-153">Такое приложение может использовать настраиваемый поставщик политики авторизации, который:</span><span class="sxs-lookup"><span data-stu-id="99cac-153">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="99cac-154">Пытается проанализировать имена политик.</span><span class="sxs-lookup"><span data-stu-id="99cac-154">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="99cac-155">Обращается к другому поставщику политики ( `DefaultAuthorizationPolicyProvider`например,), если имя политики не содержит возраст.</span><span class="sxs-lookup"><span data-stu-id="99cac-155">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="99cac-156">Приведенный `IAuthorizationPolicyProvider` выше пример реализации можно обновить для `DefaultAuthorizationPolicyProvider` использования, создав резервный поставщик политики в своем конструкторе (для использования в случае, если имя политики не соответствует ожидаемому шаблону "минимальная подлинность" + Age).</span><span class="sxs-lookup"><span data-stu-id="99cac-156">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a fallback policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="99cac-157">Затем метод можно обновить для `FallbackPolicyProvider` использования вместо возврата значения NULL: `GetPolicyAsync`</span><span class="sxs-lookup"><span data-stu-id="99cac-157">Then, the `GetPolicyAsync` method can be updated to use the `FallbackPolicyProvider` instead of returning null:</span></span>

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="99cac-158">Политика по умолчанию</span><span class="sxs-lookup"><span data-stu-id="99cac-158">Default policy</span></span>

<span data-ttu-id="99cac-159">В дополнение к именованным политикам авторизации пользователь `IAuthorizationPolicyProvider` должен реализовать `GetDefaultPolicyAsync` для `[Authorize]` предоставления политики авторизации атрибутов без указания имени политики.</span><span class="sxs-lookup"><span data-stu-id="99cac-159">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="99cac-160">Во многих случаях для этого атрибута авторизации требуется только пользователь, прошедший проверку подлинности, поэтому можно сделать необходимую политику с помощью `RequireAuthenticatedUser`вызова:</span><span class="sxs-lookup"><span data-stu-id="99cac-160">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="99cac-161">Как и в случае со всеми аспектами пользовательского `IAuthorizationPolicyProvider`, это можно настроить при необходимости.</span><span class="sxs-lookup"><span data-stu-id="99cac-161">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="99cac-162">В некоторых случаях может быть желательно извлечь политику по умолчанию из резервной стратегии `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="99cac-162">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="required-policy"></a><span data-ttu-id="99cac-163">Обязательная политика</span><span class="sxs-lookup"><span data-stu-id="99cac-163">Required policy</span></span>

<span data-ttu-id="99cac-164">Пользователь `IAuthorizationPolicyProvider` должен реализовать `GetRequiredPolicyAsync` , при необходимости, указать политику, которая всегда является обязательной.</span><span class="sxs-lookup"><span data-stu-id="99cac-164">A custom `IAuthorizationPolicyProvider` needs to implement `GetRequiredPolicyAsync` to, optionally, provide a policy that is always required.</span></span> <span data-ttu-id="99cac-165">Если `GetRequiredPolicyAsync` параметр возвращает политику, отличную от NULL, эта политика будет сочетаться с любыми другими запрошенными политиками (именованные или по умолчанию).</span><span class="sxs-lookup"><span data-stu-id="99cac-165">If `GetRequiredPolicyAsync` returns a non-null policy, that policy will be combined with any other (named or default) policy that is requested.</span></span>

<span data-ttu-id="99cac-166">Если требуемая политика не требуется, поставщик может просто вернуть значение null или отложить его на резервный поставщик:</span><span class="sxs-lookup"><span data-stu-id="99cac-166">If no required policy is needed, the provider can just return null or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="99cac-167">Использование настраиваемого Иаусоризатионполиципровидер</span><span class="sxs-lookup"><span data-stu-id="99cac-167">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="99cac-168">Чтобы использовать пользовательские политики из `IAuthorizationPolicyProvider`, необходимо:</span><span class="sxs-lookup"><span data-stu-id="99cac-168">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="99cac-169">Зарегистрируйте соответствующие `AuthorizationHandler` типы с помощью внедрения зависимостей (см. в разделе [авторизация на основе политик](xref:security/authorization/policies#authorization-handlers)), как и в случае с любыми сценариями авторизации на основе политик.</span><span class="sxs-lookup"><span data-stu-id="99cac-169">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="99cac-170">Зарегистрируйте пользовательский `IAuthorizationPolicyProvider` тип в коллекции службы внедрения зависимостей приложения (в `Startup.ConfigureServices`), чтобы заменить поставщика политик по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="99cac-170">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="99cac-171">Полный пользовательский `IAuthorizationPolicyProvider` пример доступен в [репозитории GitHub/ауссамплес](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="99cac-171">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
