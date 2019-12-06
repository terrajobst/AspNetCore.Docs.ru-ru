---
title: Пользовательские поставщики политики авторизации в ASP.NET Core
author: mjrousos
description: Узнайте, как использовать пользовательский Иаусоризатионполиципровидер в приложении ASP.NET Core для динамического создания политик авторизации.
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: fe07a113a29ed3e14679e3f3f2249b0810c17593
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/06/2019
ms.locfileid: "74880696"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a><span data-ttu-id="9e5aa-103">Пользовательские поставщики политики авторизации с использованием Иаусоризатионполиципровидер в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="9e5aa-103">Custom Authorization Policy Providers using IAuthorizationPolicyProvider in ASP.NET Core</span></span> 

<span data-ttu-id="9e5aa-104">По [Майк Роусос](https://github.com/mjrousos)</span><span class="sxs-lookup"><span data-stu-id="9e5aa-104">By [Mike Rousos](https://github.com/mjrousos)</span></span>

<span data-ttu-id="9e5aa-105">Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies)политики регистрируются путем вызова `AuthorizationOptions.AddPolicy` как части конфигурации службы авторизации.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-105">Typically when using [policy-based authorization](xref:security/authorization/policies), policies are registered by calling `AuthorizationOptions.AddPolicy` as part of authorization service configuration.</span></span> <span data-ttu-id="9e5aa-106">В некоторых сценариях такой способ регистрации всех политик авторизации может быть невозможен (или желательно).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-106">In some scenarios, it may not be possible (or desirable) to register all authorization policies in this way.</span></span> <span data-ttu-id="9e5aa-107">В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` для управления использованием политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-107">In those cases, you can use a custom `IAuthorizationPolicyProvider` to control how authorization policies are supplied.</span></span>

<span data-ttu-id="9e5aa-108">Примеры сценариев, в которых может быть полезна пользовательская [иаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :</span><span class="sxs-lookup"><span data-stu-id="9e5aa-108">Examples of scenarios where a custom [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) may be useful include:</span></span>

* <span data-ttu-id="9e5aa-109">Использование внешней службы для обеспечения оценки политики.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-109">Using an external service to provide policy evaluation.</span></span>
* <span data-ttu-id="9e5aa-110">Использование большого диапазона политик (например, для разных номеров мест или возраста), поэтому не имеет смысла добавлять каждую отдельную политику авторизации с помощью вызова `AuthorizationOptions.AddPolicy`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-110">Using a large range of policies (for different room numbers or ages, for example), so it doesn't make sense to add each individual authorization policy with an `AuthorizationOptions.AddPolicy` call.</span></span>
* <span data-ttu-id="9e5aa-111">Создание политик во время выполнения на основе информации во внешнем источнике данных (например, базы данных) или определение требований авторизации динамически с помощью другого механизма.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-111">Creating policies at runtime based on information in an external data source (like a database) or determining authorization requirements dynamically through another mechanism.</span></span>

<span data-ttu-id="9e5aa-112">[Просмотрите или Скачайте пример кода](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) из [репозитория AspNetCore GitHub](https://github.com/aspnet/AspNetCore).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-112">[View or download sample code](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) from the [AspNetCore GitHub repository](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="9e5aa-113">Скачайте ZIP-файл репозитория ASPNET/AspNetCore.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-113">Download the aspnet/AspNetCore repository ZIP file.</span></span> <span data-ttu-id="9e5aa-114">Распакуйте файл.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-114">Unzip the file.</span></span> <span data-ttu-id="9e5aa-115">Перейдите в папку проекта *src/Security/Samples/кустомполиципровидер* .</span><span class="sxs-lookup"><span data-stu-id="9e5aa-115">Navigate to the *src/Security/samples/CustomPolicyProvider* project folder.</span></span>

## <a name="customize-policy-retrieval"></a><span data-ttu-id="9e5aa-116">Настройка получения политики</span><span class="sxs-lookup"><span data-stu-id="9e5aa-116">Customize policy retrieval</span></span>

<span data-ttu-id="9e5aa-117">ASP.NET Core приложения используют реализацию интерфейса `IAuthorizationPolicyProvider` для получения политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-117">ASP.NET Core apps use an implementation of the `IAuthorizationPolicyProvider` interface to retrieve authorization policies.</span></span> <span data-ttu-id="9e5aa-118">По умолчанию [дефаултаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-118">By default, [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) is registered and used.</span></span> <span data-ttu-id="9e5aa-119">`DefaultAuthorizationPolicyProvider` Возвращает политики из `AuthorizationOptions`, предоставленного в вызове `IServiceCollection.AddAuthorization`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-119">`DefaultAuthorizationPolicyProvider` returns policies from the `AuthorizationOptions` provided in an `IServiceCollection.AddAuthorization` call.</span></span>

<span data-ttu-id="9e5aa-120">Настройте это поведение, зарегистрировав другую реализацию `IAuthorizationPolicyProvider` в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-120">Customize this behavior by registering a different `IAuthorizationPolicyProvider` implementation in the app's [dependency injection](xref:fundamentals/dependency-injection) container.</span></span> 

<span data-ttu-id="9e5aa-121">Интерфейс `IAuthorizationPolicyProvider` содержит три интерфейса API:</span><span class="sxs-lookup"><span data-stu-id="9e5aa-121">The `IAuthorizationPolicyProvider` interface contains three APIs:</span></span>

* <span data-ttu-id="9e5aa-122">[Жетполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) Возвращает политику авторизации для заданного имени.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-122">[GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) returns an authorization policy for a given name.</span></span>
* <span data-ttu-id="9e5aa-123">[Жетдефаултполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) Возвращает политику авторизации по умолчанию (политику, используемую для `[Authorize]` атрибутов без указанной политики).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-123">[GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) returns the default authorization policy (the policy used for `[Authorize]` attributes without a policy specified).</span></span> 
* <span data-ttu-id="9e5aa-124">[Жетфаллбаккполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) возвращает резервную политику авторизации (политику, используемую по промежуточного слоя авторизации, если политика не указана).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-124">[GetFallbackPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) returns the fallback authorization policy (the policy used by the Authorization Middleware when no policy is specified).</span></span> 

<span data-ttu-id="9e5aa-125">Реализуя эти API, можно настроить политику авторизации.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-125">By implementing these APIs, you can customize how authorization policies are provided.</span></span>

## <a name="parameterized-authorize-attribute-example"></a><span data-ttu-id="9e5aa-126">Пример параметризованного атрибута авторизации</span><span class="sxs-lookup"><span data-stu-id="9e5aa-126">Parameterized authorize attribute example</span></span>

<span data-ttu-id="9e5aa-127">Один из сценариев, в котором `IAuthorizationPolicyProvider` полезно, включает пользовательские атрибуты `[Authorize]`, требования которых зависят от параметра.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-127">One scenario where `IAuthorizationPolicyProvider` is useful is enabling custom `[Authorize]` attributes whose requirements depend on a parameter.</span></span> <span data-ttu-id="9e5aa-128">Например, в документации по [авторизации на основе политик](xref:security/authorization/policies) в качестве примера использовалась политика на основе возраста ("AtLeast21").</span><span class="sxs-lookup"><span data-stu-id="9e5aa-128">For example, in [policy-based authorization](xref:security/authorization/policies) documentation, an age-based (“AtLeast21”) policy was used as a sample.</span></span> <span data-ttu-id="9e5aa-129">Если доступ к различным действиям контроллера в приложении должен предоставляться пользователям, имеющим *разный* возраст, может оказаться полезным использовать много политик на основе возраста.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-129">If different controller actions in an app should be made available to users of *different* ages, it might be useful to have many different age-based policies.</span></span> <span data-ttu-id="9e5aa-130">Вместо регистрации всех разных политик на основе возраста, которые потребуется приложению в `AuthorizationOptions`, можно динамически создавать политики с помощью настраиваемых `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-130">Instead of registering all the different age-based policies that the application will need in `AuthorizationOptions`, you can generate the policies dynamically with a custom `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="9e5aa-131">Чтобы упростить использование политик, можно добавить аннотации к действиям с помощью настраиваемого атрибута авторизации, например `[MinimumAgeAuthorize(20)]`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-131">To make using the policies easier, you can annotate actions with custom authorization attribute like `[MinimumAgeAuthorize(20)]`.</span></span>

## <a name="custom-authorization-attributes"></a><span data-ttu-id="9e5aa-132">Пользовательские атрибуты авторизации</span><span class="sxs-lookup"><span data-stu-id="9e5aa-132">Custom Authorization attributes</span></span>

<span data-ttu-id="9e5aa-133">Политики авторизации идентифицируются по именам.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-133">Authorization policies are identified by their names.</span></span> <span data-ttu-id="9e5aa-134">Пользовательская `MinimumAgeAuthorizeAttribute`, описанная выше, должна сопоставлять аргументы в строку, которую можно использовать для получения соответствующей политики авторизации.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-134">The custom `MinimumAgeAuthorizeAttribute` described previously needs to map arguments into a string that can be used to retrieve the corresponding authorization policy.</span></span> <span data-ttu-id="9e5aa-135">Это можно сделать, производные от `AuthorizeAttribute` и сделав свойство `Age` оболочкой для свойства `AuthorizeAttribute.Policy`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-135">You can do this by deriving from `AuthorizeAttribute` and making the `Age` property wrap the `AuthorizeAttribute.Policy` property.</span></span>

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

<span data-ttu-id="9e5aa-136">Этот тип атрибута имеет `Policy`ную строку на основе жестко запрограммированного префикса (`"MinimumAge"`) и целого числа, переданного через конструктор.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-136">This attribute type has a `Policy` string based on the hard-coded prefix (`"MinimumAge"`) and an integer passed in via the constructor.</span></span>

<span data-ttu-id="9e5aa-137">Его можно применить к действиям так же, как к другим атрибутам `Authorize`, за исключением того, что в качестве параметра принимается целое число.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-137">You can apply it to actions in the same way as other `Authorize` attributes except that it takes an integer as a parameter.</span></span>

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a><span data-ttu-id="9e5aa-138">Пользовательские Иаусоризатионполиципровидер</span><span class="sxs-lookup"><span data-stu-id="9e5aa-138">Custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="9e5aa-139">Настраиваемый `MinimumAgeAuthorizeAttribute` позволяет легко запрашивать политики авторизации для любого минимального необходимого срока.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-139">The custom `MinimumAgeAuthorizeAttribute` makes it easy to request authorization policies for any minimum age desired.</span></span> <span data-ttu-id="9e5aa-140">Следующая проблема, которую необходимо решить, заключается в том, чтобы обеспечить доступность политик авторизации для всех этих сбоев.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-140">The next problem to solve is making sure that authorization policies are available for all of those different ages.</span></span> <span data-ttu-id="9e5aa-141">Именно здесь удобно использовать `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-141">This is where an `IAuthorizationPolicyProvider` is useful.</span></span>

<span data-ttu-id="9e5aa-142">При использовании `MinimumAgeAuthorizationAttribute`имена политик авторизации будут соответствовать шаблону `"MinimumAge" + Age`, поэтому пользовательские `IAuthorizationPolicyProvider` должны создавать политики авторизации следующим образом.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-142">When using `MinimumAgeAuthorizationAttribute`, the authorization policy names will follow the pattern `"MinimumAge" + Age`, so the custom `IAuthorizationPolicyProvider` should generate authorization policies by:</span></span>

* <span data-ttu-id="9e5aa-143">Анализ возраста на основе имени политики.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-143">Parsing the age from the policy name.</span></span>
* <span data-ttu-id="9e5aa-144">Использование `AuthorizationPolicyBuilder` для создания нового `AuthorizationPolicy`</span><span class="sxs-lookup"><span data-stu-id="9e5aa-144">Using `AuthorizationPolicyBuilder` to create a new `AuthorizationPolicy`</span></span>
* <span data-ttu-id="9e5aa-145">В этом и следующих примерах предполагается, что пользователь пройдет проверку подлинности с помощью файла cookie.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-145">In this and following examples it will be assumed that the user is authenticated via a cookie.</span></span> <span data-ttu-id="9e5aa-146">`AuthorizationPolicyBuilder` должны быть созданы по крайней мере с одним именем схемы авторизации или всегда выполняться.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-146">The `AuthorizationPolicyBuilder` should either be constructed with at least one authorization scheme name or always succeed.</span></span> <span data-ttu-id="9e5aa-147">В противном случае нет информации о том, как предоставить пользователю запрос, и будет выдано исключение.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-147">Otherwise there is no information on how to provide a challenge to the user and an exception will be thrown.</span></span>
* <span data-ttu-id="9e5aa-148">Добавление требований к политике на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-148">Adding requirements to the policy based on the age with `AuthorizationPolicyBuilder.AddRequirements`.</span></span> <span data-ttu-id="9e5aa-149">В других сценариях вместо этого можно использовать `RequireClaim`, `RequireRole`или `RequireUserName`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-149">In other scenarios, you might use `RequireClaim`, `RequireRole`, or `RequireUserName` instead.</span></span>

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

## <a name="multiple-authorization-policy-providers"></a><span data-ttu-id="9e5aa-150">Несколько поставщиков политик авторизации</span><span class="sxs-lookup"><span data-stu-id="9e5aa-150">Multiple authorization policy providers</span></span>

<span data-ttu-id="9e5aa-151">При использовании пользовательских реализаций `IAuthorizationPolicyProvider` Помните, что ASP.NET Core использует только один экземпляр `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-151">When using custom `IAuthorizationPolicyProvider` implementations, keep in mind that ASP.NET Core only uses one instance of `IAuthorizationPolicyProvider`.</span></span> <span data-ttu-id="9e5aa-152">Если настраиваемый поставщик не может предоставить политики авторизации для всех имен политик, которые будут использоваться, он должен откладываться на поставщика резервного копирования.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-152">If a custom provider isn't able to provide authorization policies for all policy names that will be used, it should defer to a backup provider.</span></span> 

<span data-ttu-id="9e5aa-153">Например, рассмотрим приложение, для которого требуются пользовательские политики возраста и более традиционную политику получения политик на основе ролей.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-153">For example, consider an application that needs both custom age policies and more traditional role-based policy retrieval.</span></span> <span data-ttu-id="9e5aa-154">Такое приложение может использовать настраиваемый поставщик политики авторизации, который:</span><span class="sxs-lookup"><span data-stu-id="9e5aa-154">Such an app could use a custom authorization policy provider that:</span></span>

* <span data-ttu-id="9e5aa-155">Пытается проанализировать имена политик.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-155">Attempts to parse policy names.</span></span> 
* <span data-ttu-id="9e5aa-156">Обращается к другому поставщику политики (например, `DefaultAuthorizationPolicyProvider`), если имя политики не содержит возраст.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-156">Calls into a different policy provider (like `DefaultAuthorizationPolicyProvider`) if the policy name doesn't contain an age.</span></span>

<span data-ttu-id="9e5aa-157">Приведенный выше пример `IAuthorizationPolicyProvider` реализации может быть обновлен для использования `DefaultAuthorizationPolicyProvider` путем создания поставщика политики резервного копирования в конструкторе (для использования в случае, если имя политики не соответствует ожидаемому шаблону "минимальный номер" + Age).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-157">The example `IAuthorizationPolicyProvider` implementation shown above can be updated to use the `DefaultAuthorizationPolicyProvider` by creating a backup policy provider in its constructor (to be used in case the policy name doesn't match its expected pattern of 'MinimumAge' + age).</span></span>

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

<span data-ttu-id="9e5aa-158">Затем можно обновить метод `GetPolicyAsync`, чтобы использовать `BackupPolicyProvider` вместо возврата значения NULL:</span><span class="sxs-lookup"><span data-stu-id="9e5aa-158">Then, the `GetPolicyAsync` method can be updated to use the `BackupPolicyProvider` instead of returning null:</span></span>

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a><span data-ttu-id="9e5aa-159">Политика по умолчанию</span><span class="sxs-lookup"><span data-stu-id="9e5aa-159">Default policy</span></span>

<span data-ttu-id="9e5aa-160">В дополнение к именованным политикам авторизации пользовательское `IAuthorizationPolicyProvider` необходимо реализовать `GetDefaultPolicyAsync`, чтобы предоставить политику авторизации для `[Authorize]` атрибутов без указания имени политики.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-160">In addition to providing named authorization policies, a custom `IAuthorizationPolicyProvider` needs to implement `GetDefaultPolicyAsync` to provide an authorization policy for `[Authorize]` attributes without a policy name specified.</span></span>

<span data-ttu-id="9e5aa-161">Во многих случаях для этого атрибута авторизации требуется только пользователь, прошедший проверку подлинности, поэтому можно сделать необходимую политику с помощью вызова `RequireAuthenticatedUser`:</span><span class="sxs-lookup"><span data-stu-id="9e5aa-161">In many cases, this authorization attribute only requires an authenticated user, so you can make the necessary policy with a call to `RequireAuthenticatedUser`:</span></span>

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

<span data-ttu-id="9e5aa-162">Как и в случае со всеми аспектами пользовательского `IAuthorizationPolicyProvider`, это можно настроить по мере необходимости.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-162">As with all aspects of a custom `IAuthorizationPolicyProvider`, you can customize this, as needed.</span></span> <span data-ttu-id="9e5aa-163">В некоторых случаях может быть желательно получить политику по умолчанию из резервного `IAuthorizationPolicyProvider`.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-163">In some cases, it may be desirable to retrieve the default policy from a fallback `IAuthorizationPolicyProvider`.</span></span>

## <a name="fallback-policy"></a><span data-ttu-id="9e5aa-164">Политика резервного применения</span><span class="sxs-lookup"><span data-stu-id="9e5aa-164">Fallback policy</span></span>

<span data-ttu-id="9e5aa-165">Пользовательское `IAuthorizationPolicyProvider` может при необходимости реализовать `GetFallbackPolicyAsync` для предоставления политики, используемой при [объединении политик](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) , и если политики не заданы.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-165">A custom `IAuthorizationPolicyProvider` can optionally implement `GetFallbackPolicyAsync` to provide a policy that's used when [combining policies](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) and when no policies are specified.</span></span> <span data-ttu-id="9e5aa-166">Если `GetFallbackPolicyAsync` Возвращает политику, отличную от NULL, то возвращаемая политика используется по промежуточного слоя авторизации, если для запроса не заданы политики.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-166">If `GetFallbackPolicyAsync` returns a non-null policy, the returned policy is used by the Authorization Middleware when no policies are specified for the request.</span></span>

<span data-ttu-id="9e5aa-167">Если резервная политика не требуется, поставщик может вернуть `null` или отложить на резервный поставщик:</span><span class="sxs-lookup"><span data-stu-id="9e5aa-167">If no fallback policy is required, the provider can return `null` or defer to the fallback provider:</span></span>

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a><span data-ttu-id="9e5aa-168">Использование настраиваемого Иаусоризатионполиципровидер</span><span class="sxs-lookup"><span data-stu-id="9e5aa-168">Use a custom IAuthorizationPolicyProvider</span></span>

<span data-ttu-id="9e5aa-169">Чтобы использовать пользовательские политики из `IAuthorizationPolicyProvider`, необходимо выполнить следующие действия.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-169">To use custom policies from an `IAuthorizationPolicyProvider`, you must:</span></span>

* <span data-ttu-id="9e5aa-170">Зарегистрируйте соответствующие типы `AuthorizationHandler` с помощью внедрения зависимостей (см. в разделе [авторизация на основе политик](xref:security/authorization/policies#authorization-handlers)), как и в случае с любыми сценариями авторизации на основе политик.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-170">Register the appropriate `AuthorizationHandler` types with dependency injection (described in [policy-based authorization](xref:security/authorization/policies#authorization-handlers)), as with all policy-based authorization scenarios.</span></span>
* <span data-ttu-id="9e5aa-171">Зарегистрируйте пользовательский тип `IAuthorizationPolicyProvider` в коллекции службы внедрения зависимостей приложения (в `Startup.ConfigureServices`), чтобы заменить поставщика политик по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="9e5aa-171">Register the custom `IAuthorizationPolicyProvider` type in the app's dependency injection service collection (in `Startup.ConfigureServices`) to replace the default policy provider.</span></span>

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

<span data-ttu-id="9e5aa-172">Полный пример настраиваемого `IAuthorizationPolicyProvider` доступен в [репозитории GitHub ASPNET/ауссамплес](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span><span class="sxs-lookup"><span data-stu-id="9e5aa-172">A complete custom `IAuthorizationPolicyProvider` sample is available in the [aspnet/AuthSamples GitHub repository](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).</span></span>
