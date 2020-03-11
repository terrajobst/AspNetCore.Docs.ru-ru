---
title: Пользовательские поставщики политики авторизации в ASP.NET Core
author: mjrousos
description: Узнайте, как использовать пользовательский Иаусоризатионполиципровидер в приложении ASP.NET Core для динамического создания политик авторизации.
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2019
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 9f0a0cd5337f7f8d2fc8a4b6902a63b98f6bd702
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651754"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Пользовательские поставщики политики авторизации с использованием Иаусоризатионполиципровидер в ASP.NET Core 

Автор: [Майк Роусос (Mike Rousos)](https://github.com/mjrousos)

Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies)политики регистрируются путем вызова `AuthorizationOptions.AddPolicy` как части конфигурации службы авторизации. В некоторых сценариях такой способ регистрации всех политик авторизации может быть невозможен (или желательно). В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` для управления использованием политик авторизации.

Примеры сценариев, в которых может быть полезна пользовательская [иаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :

* Использование внешней службы для обеспечения оценки политики.
* Использование большого диапазона политик (например, для разных номеров мест или возраста), поэтому не имеет смысла добавлять каждую отдельную политику авторизации с помощью вызова `AuthorizationOptions.AddPolicy`.
* Создание политик во время выполнения на основе информации во внешнем источнике данных (например, базы данных) или определение требований авторизации динамически с помощью другого механизма.

[Просмотрите или Скачайте пример кода](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) из [репозитория AspNetCore GitHub](https://github.com/dotnet/AspNetCore). Скачайте ZIP-файл репозитория DotNet/AspNetCore. Распакуйте файл. Перейдите в папку проекта *src/Security/Samples/кустомполиципровидер* .

## <a name="customize-policy-retrieval"></a>Настройка получения политики

ASP.NET Core приложения используют реализацию интерфейса `IAuthorizationPolicyProvider` для получения политик авторизации. По умолчанию [дефаултаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется. `DefaultAuthorizationPolicyProvider` Возвращает политики из `AuthorizationOptions`, предоставленного в вызове `IServiceCollection.AddAuthorization`.

Настройте это поведение, зарегистрировав другую реализацию `IAuthorizationPolicyProvider` в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения. 

Интерфейс `IAuthorizationPolicyProvider` содержит три интерфейса API:

* [Жетполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) Возвращает политику авторизации для заданного имени.
* [Жетдефаултполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) Возвращает политику авторизации по умолчанию (политику, используемую для `[Authorize]` атрибутов без указанной политики). 
* [Жетфаллбаккполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getfallbackpolicyasync) возвращает резервную политику авторизации (политику, используемую по промежуточного слоя авторизации, если политика не указана). 

Реализуя эти API, можно настроить политику авторизации.

## <a name="parameterized-authorize-attribute-example"></a>Пример параметризованного атрибута авторизации

Один из сценариев, в котором `IAuthorizationPolicyProvider` полезно, включает пользовательские атрибуты `[Authorize]`, требования которых зависят от параметра. Например, в документации по [авторизации на основе политик](xref:security/authorization/policies) в качестве примера использовалась политика на основе возраста ("AtLeast21"). Если доступ к различным действиям контроллера в приложении должен предоставляться пользователям, имеющим *разный* возраст, может оказаться полезным использовать много политик на основе возраста. Вместо регистрации всех разных политик на основе возраста, которые потребуется приложению в `AuthorizationOptions`, можно динамически создавать политики с помощью настраиваемых `IAuthorizationPolicyProvider`. Чтобы упростить использование политик, можно добавить аннотации к действиям с помощью настраиваемого атрибута авторизации, например `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Пользовательские атрибуты авторизации

Политики авторизации идентифицируются по именам. Пользовательская `MinimumAgeAuthorizeAttribute`, описанная выше, должна сопоставлять аргументы в строку, которую можно использовать для получения соответствующей политики авторизации. Это можно сделать, производные от `AuthorizeAttribute` и сделав свойство `Age` оболочкой для свойства `AuthorizeAttribute.Policy`.

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

Этот тип атрибута имеет `Policy`ную строку на основе жестко запрограммированного префикса (`"MinimumAge"`) и целого числа, переданного через конструктор.

Его можно применить к действиям так же, как к другим атрибутам `Authorize`, за исключением того, что в качестве параметра принимается целое число.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Пользовательские Иаусоризатионполиципровидер

Настраиваемый `MinimumAgeAuthorizeAttribute` позволяет легко запрашивать политики авторизации для любого минимального необходимого срока. Следующая проблема, которую необходимо решить, заключается в том, чтобы обеспечить доступность политик авторизации для всех этих сбоев. Именно здесь удобно использовать `IAuthorizationPolicyProvider`.

При использовании `MinimumAgeAuthorizationAttribute`имена политик авторизации будут соответствовать шаблону `"MinimumAge" + Age`, поэтому пользовательские `IAuthorizationPolicyProvider` должны создавать политики авторизации следующим образом.

* Анализ возраста на основе имени политики.
* Использование `AuthorizationPolicyBuilder` для создания нового `AuthorizationPolicy`
* В этом и следующих примерах предполагается, что пользователь пройдет проверку подлинности с помощью файла cookie. `AuthorizationPolicyBuilder` должны быть созданы по крайней мере с одним именем схемы авторизации или всегда выполняться. В противном случае нет информации о том, как предоставить пользователю запрос, и будет выдано исключение.
* Добавление требований к политике на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`. В других сценариях вместо этого можно использовать `RequireClaim`, `RequireRole`или `RequireUserName`.

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

## <a name="multiple-authorization-policy-providers"></a>Несколько поставщиков политик авторизации

При использовании пользовательских реализаций `IAuthorizationPolicyProvider` Помните, что ASP.NET Core использует только один экземпляр `IAuthorizationPolicyProvider`. Если настраиваемый поставщик не может предоставить политики авторизации для всех имен политик, которые будут использоваться, он должен откладываться на поставщика резервного копирования. 

Например, рассмотрим приложение, для которого требуются пользовательские политики возраста и более традиционную политику получения политик на основе ролей. Такое приложение может использовать настраиваемый поставщик политики авторизации, который:

* Пытается проанализировать имена политик. 
* Обращается к другому поставщику политики (например, `DefaultAuthorizationPolicyProvider`), если имя политики не содержит возраст.

Приведенный выше пример `IAuthorizationPolicyProvider` реализации может быть обновлен для использования `DefaultAuthorizationPolicyProvider` путем создания поставщика политики резервного копирования в конструкторе (для использования в случае, если имя политики не соответствует ожидаемому шаблону "минимальный номер" + Age).

```csharp
private DefaultAuthorizationPolicyProvider BackupPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    BackupPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Затем можно обновить метод `GetPolicyAsync`, чтобы использовать `BackupPolicyProvider` вместо возврата значения NULL:

```csharp
...
return BackupPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Политика по умолчанию

В дополнение к именованным политикам авторизации пользовательское `IAuthorizationPolicyProvider` необходимо реализовать `GetDefaultPolicyAsync`, чтобы предоставить политику авторизации для `[Authorize]` атрибутов без указания имени политики.

Во многих случаях для этого атрибута авторизации требуется только пользователь, прошедший проверку подлинности, поэтому можно сделать необходимую политику с помощью вызова `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Как и в случае со всеми аспектами пользовательского `IAuthorizationPolicyProvider`, это можно настроить по мере необходимости. В некоторых случаях может быть желательно получить политику по умолчанию из резервного `IAuthorizationPolicyProvider`.

## <a name="fallback-policy"></a>Политика резервного применения

Пользовательское `IAuthorizationPolicyProvider` может при необходимости реализовать `GetFallbackPolicyAsync` для предоставления политики, используемой при [объединении политик](/dotnet/api/microsoft.aspnetcore.authorization.authorizationpolicy.combine) , и если политики не заданы. Если `GetFallbackPolicyAsync` Возвращает политику, отличную от NULL, то возвращаемая политика используется по промежуточного слоя авторизации, если для запроса не заданы политики.

Если резервная политика не требуется, поставщик может вернуть `null` или отложить на резервный поставщик:

```csharp
public Task<AuthorizationPolicy> GetFallbackPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Использование настраиваемого Иаусоризатионполиципровидер

Чтобы использовать пользовательские политики из `IAuthorizationPolicyProvider`, необходимо выполнить следующие действия.

* Зарегистрируйте соответствующие типы `AuthorizationHandler` с помощью внедрения зависимостей (см. в разделе [авторизация на основе политик](xref:security/authorization/policies#authorization-handlers)), как и в случае с любыми сценариями авторизации на основе политик.
* Зарегистрируйте пользовательский тип `IAuthorizationPolicyProvider` в коллекции службы внедрения зависимостей приложения (в `Startup.ConfigureServices`), чтобы заменить поставщика политик по умолчанию.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Полный пример настраиваемого `IAuthorizationPolicyProvider` доступен в [репозитории GitHub ASPNET/ауссамплес](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
