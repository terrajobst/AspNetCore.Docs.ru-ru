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
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Пользовательские поставщики политики авторизации с использованием Иаусоризатионполиципровидер в ASP.NET Core 

По [Майк Роусос](https://github.com/mjrousos)

Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies)политики регистрируются путем вызова `AuthorizationOptions.AddPolicy` в рамках настройки службы авторизации. В некоторых сценариях такой способ регистрации всех политик авторизации может быть невозможен (или желательно). В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` , чтобы контролировать, как предоставляются политики авторизации.

Примеры сценариев, в которых может быть полезна пользовательская [иаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) :

* Использование внешней службы для обеспечения оценки политики.
* Использование большого диапазона политик (например, для разных номеров мест или возраста), поэтому не имеет смысла добавлять каждую отдельную политику авторизации с помощью `AuthorizationOptions.AddPolicy` вызова.
* Создание политик во время выполнения на основе информации во внешнем источнике данных (например, базы данных) или определение требований авторизации динамически с помощью другого механизма.

[Просмотрите или Скачайте пример кода](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider) из [репозитория AspNetCore GitHub](https://github.com/aspnet/AspNetCore). Скачайте ZIP-файл репозитория ASPNET/AspNetCore. Распакуйте файл. Перейдите в папку проекта *src/Security/Samples/кустомполиципровидер* .

## <a name="customize-policy-retrieval"></a>Настройка получения политики

ASP.NET Core приложения используют реализацию `IAuthorizationPolicyProvider` интерфейса для получения политик авторизации. По умолчанию [дефаултаусоризатионполиципровидер](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется. `DefaultAuthorizationPolicyProvider`Возвращает политики из `AuthorizationOptions` предоставленного `IServiceCollection.AddAuthorization` в вызове.

Это поведение можно настроить, зарегистрировав другую `IAuthorizationPolicyProvider` реализацию в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection) приложения. 

`IAuthorizationPolicyProvider` Интерфейс содержит два интерфейса API:

* [Жетполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) Возвращает политику авторизации для заданного имени.
* [Жетдефаултполициасинк](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) Возвращает политику авторизации по умолчанию (политику, используемую для `[Authorize]` атрибутов без указанной политики). 

Реализуя эти два API, можно настроить политику авторизации.

## <a name="parameterized-authorize-attribute-example"></a>Пример параметризованного атрибута авторизации

Один из сценариев `IAuthorizationPolicyProvider` , в котором полезно включить `[Authorize]` настраиваемые атрибуты, требования к которым зависят от параметра. Например, в документации по [авторизации на основе политик](xref:security/authorization/policies) в качестве примера использовалась политика на основе возраста ("AtLeast21"). Если доступ к различным действиям контроллера в приложении должен предоставляться пользователям, имеющим *разный* возраст, может оказаться полезным использовать много политик на основе возраста. Вместо регистрации всех разных политик на основе возраста, которые понадобятся `AuthorizationOptions`приложению, можно динамически создавать политики с настраиваемым. `IAuthorizationPolicyProvider` Чтобы упростить использование политик, можно добавить заметки к действиям с помощью настраиваемого атрибута `[MinimumAgeAuthorize(20)]`авторизации, например.

## <a name="custom-authorization-attributes"></a>Пользовательские атрибуты авторизации

Политики авторизации идентифицируются по именам. Пользователь `MinimumAgeAuthorizeAttribute` , описанный выше, должен сопоставлять аргументы в строку, которая может использоваться для получения соответствующей политики авторизации. Это можно сделать, производя от `AuthorizeAttribute` и `Age` сделав свойство оболочкой `AuthorizeAttribute.Policy` для свойства.

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

Этот тип атрибута имеет `Policy` строку на основе жестко запрограммированного префикса (`"MinimumAge"`) и целого числа, переданного через конструктор.

Его можно применить к действиям так же, как и к `Authorize` другим атрибутам, за исключением того, что в качестве параметра принимается целое число.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Пользовательские Иаусоризатионполиципровидер

Пользовательский `MinimumAgeAuthorizeAttribute` параметр упрощает запрос политик авторизации для любого минимального возраста. Следующая проблема, которую необходимо решить, заключается в том, чтобы обеспечить доступность политик авторизации для всех этих сбоев. В `IAuthorizationPolicyProvider` этом случае полезно использовать.

При использовании `MinimumAgeAuthorizationAttribute`имена политик авторизации будут соответствовать шаблону `"MinimumAge" + Age`, поэтому пользователь `IAuthorizationPolicyProvider` должен создать политики авторизации следующим образом.

* Анализ возраста на основе имени политики.
* Использование `AuthorizationPolicyBuilder` для создания нового`AuthorizationPolicy`
* В этом и следующих примерах предполагается, что пользователь пройдет проверку подлинности с помощью файла cookie. Значение `AuthorizationPolicyBuilder` должно быть создано по крайней мере с одним именем схемы авторизации или всегда выполняться. В противном случае нет информации о том, как предоставить пользователю запрос, и будет выдано исключение.
* Добавление требований к политике на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`. В других сценариях вы можете использовать `RequireClaim`, `RequireRole`или `RequireUserName` .

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

При использовании пользовательских `IAuthorizationPolicyProvider` реализаций Помните, что ASP.NET Core использует только один `IAuthorizationPolicyProvider`экземпляр. Если настраиваемый поставщик не может предоставить политики авторизации для всех имен политик, которые будут использоваться, она должна вернуться к поставщику резервных копий. 

Например, рассмотрим приложение, для которого требуются пользовательские политики возраста и более традиционную политику получения политик на основе ролей. Такое приложение может использовать настраиваемый поставщик политики авторизации, который:

* Пытается проанализировать имена политик. 
* Обращается к другому поставщику политики ( `DefaultAuthorizationPolicyProvider`например,), если имя политики не содержит возраст.

Приведенный `IAuthorizationPolicyProvider` выше пример реализации можно обновить для `DefaultAuthorizationPolicyProvider` использования, создав резервный поставщик политики в своем конструкторе (для использования в случае, если имя политики не соответствует ожидаемому шаблону "минимальная подлинность" + Age).

```csharp
private DefaultAuthorizationPolicyProvider FallbackPolicyProvider { get; }

public MinimumAgePolicyProvider(IOptions<AuthorizationOptions> options)
{
    // ASP.NET Core only uses one authorization policy provider, so if the custom implementation
    // doesn't handle all policies it should fall back to an alternate provider.
    FallbackPolicyProvider = new DefaultAuthorizationPolicyProvider(options);
}
```

Затем метод можно обновить для `FallbackPolicyProvider` использования вместо возврата значения NULL: `GetPolicyAsync`

```csharp
...
return FallbackPolicyProvider.GetPolicyAsync(policyName);
```

## <a name="default-policy"></a>Политика по умолчанию

В дополнение к именованным политикам авторизации пользователь `IAuthorizationPolicyProvider` должен реализовать `GetDefaultPolicyAsync` для `[Authorize]` предоставления политики авторизации атрибутов без указания имени политики.

Во многих случаях для этого атрибута авторизации требуется только пользователь, прошедший проверку подлинности, поэтому можно сделать необходимую политику с помощью `RequireAuthenticatedUser`вызова:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder(CookieAuthenticationDefaults.AuthenticationScheme).RequireAuthenticatedUser().Build());
```

Как и в случае со всеми аспектами пользовательского `IAuthorizationPolicyProvider`, это можно настроить при необходимости. В некоторых случаях может быть желательно извлечь политику по умолчанию из резервной стратегии `IAuthorizationPolicyProvider`.

## <a name="required-policy"></a>Обязательная политика

Пользователь `IAuthorizationPolicyProvider` должен реализовать `GetRequiredPolicyAsync` , при необходимости, указать политику, которая всегда является обязательной. Если `GetRequiredPolicyAsync` параметр возвращает политику, отличную от NULL, эта политика будет сочетаться с любыми другими запрошенными политиками (именованные или по умолчанию).

Если требуемая политика не требуется, поставщик может просто вернуть значение null или отложить его на резервный поставщик:

```csharp
public Task<AuthorizationPolicy> GetRequiredPolicyAsync() => 
    Task.FromResult<AuthorizationPolicy>(null);
```

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Использование настраиваемого Иаусоризатионполиципровидер

Чтобы использовать пользовательские политики из `IAuthorizationPolicyProvider`, необходимо:

* Зарегистрируйте соответствующие `AuthorizationHandler` типы с помощью внедрения зависимостей (см. в разделе [авторизация на основе политик](xref:security/authorization/policies#authorization-handlers)), как и в случае с любыми сценариями авторизации на основе политик.
* Зарегистрируйте пользовательский `IAuthorizationPolicyProvider` тип в коллекции службы внедрения зависимостей приложения (в `Startup.ConfigureServices`), чтобы заменить поставщика политик по умолчанию.

```csharp
services.AddSingleton<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Полный пользовательский `IAuthorizationPolicyProvider` пример доступен в [репозитории GitHub/ауссамплес](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider).
