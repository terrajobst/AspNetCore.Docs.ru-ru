---
title: Пользовательская авторизация политики поставщиков в ASP.NET Core
author: mjrousos
description: Сведения об использовании пользовательских IAuthorizationPolicyProvider в приложении ASP.NET Core для динамического создания политик авторизации.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: 524928a5b291e02556d11a762d86430a6dc94660
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277261"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Настраиваемые поставщики политики авторизации, используя IAuthorizationPolicyProvider в ASP.NET Core 

По [Майк Rousos](https://github.com/mjrousos)

Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies), политики регистрируются путем вызова `AuthorizationOptions.AddPolicy` как часть конфигурации авторизации службы. В некоторых сценариях, могут остаться возможно (или желательно) для регистрации всех политик авторизации, таким образом. В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` для управления, как передаются политик авторизации.

Примеры сценариев, где пользовательское [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) может оказаться полезным включить:

* С помощью внешней службы для обеспечения оценки политики.
* С помощью широкий диапазон политики (для номера разных мест или возрасте, например), поэтому не имеет смысла добавление каждой политики авторизации отдельных с `AuthorizationOptions.AddPolicy` вызова.
* Создание политик во время выполнения на основе информации из внешнего источника данных (например, база данных), или динамически определить требования к проверке подлинности посредством другого механизма.

## <a name="customizing-policy-retrieval"></a>Настройка политики извлечения

Приложения ASP.NET Core используется реализация `IAuthorizationPolicyProvider` интерфейс для получения политики авторизации. По умолчанию [DefaultAuthorizationPolicyProvider](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется. `DefaultAuthorizationPolicyProvider` Возвращает политики из `AuthorizationOptions` в `IServiceCollection.AddAuthorization` вызова.

Это поведение можно настроить путем регистрации другой `IAuthorizationPolicyProvider` реализации в приложении [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера. 

`IAuthorizationPolicyProvider` Интерфейс содержит два API:

* [GetPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync?view=aspnetcore-2.0#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) возвращает политику авторизации для данного имени.
* [GetDefaultPolicyAsync](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync?view=aspnetcore-2.0) возвращает политику авторизации по умолчанию (политику, используемую для `[Authorize]` атрибутов без политика указана). 

Реализуя эти два API-интерфейсы, можно настроить, каким образом предоставляются политик авторизации.

## <a name="parameterized-authorize-attribute-example"></a>Параметризованные авторизовать пример атрибута

Один из сценариев где `IAuthorizationPolicyProvider` полезно Включение пользовательских `[Authorize]` атрибуты которого требования зависят от параметра. Например, в [авторизации на основе политики](xref:security/authorization/policies) документации, на основе возраста («AtLeast21») политики использовался в качестве образца. Если другой контроллер действия в приложении должны быть доступны пользователям *другой* возраста, бывает полезно иметь много различных политик на основе возраста. Вместо регистрации всех различные возрастную политики необходимые приложения в `AuthorizationOptions`, можно создать политики динамически с пользовательским `IAuthorizationPolicyProvider`. Чтобы с помощью политики, проще, можно снабдить действия с атрибутом настраиваемой авторизации как `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Атрибуты настраиваемой авторизации

Политики авторизации идентифицируются по их именам. Пользовательский `MinimumAgeAuthorizeAttribute` описанные ранее, необходимо сопоставить аргументов в строку, которая может использоваться для получения соответствующей политики авторизации. Это можно сделать путем наследования от `AuthorizeAttribute` , делая `Age` свойство wrap `AuthorizeAttribute.Policy` свойство.

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

Этот тип атрибута имеет `Policy` строки на основе жестко префикса (`"MinimumAge"`) и является целым числом, переданный через конструктор.

Его можно применить к действиям так же, как другие `Authorize` атрибуты, за исключением того, что он принимает целое число как параметр.

```CSharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Пользовательские IAuthorizationPolicyProvider

Пользовательский `MinimumAgeAuthorizeAttribute` упрощает политики авторизации запроса для любой минимальный срок действия требуемого. Далее задачи для решения убедиться, что политики авторизации доступны для всех этих различных возрастов. Это место, куда `IAuthorizationPolicyProvider` полезно.

При использовании `MinimumAgeAuthorizationAttribute`, имена политик авторизации будет использовать этот подход `"MinimumAge" + Age`, поэтому пользовательский `IAuthorizationPolicyProvider` должен создать политики авторизации с:

* При синтаксическом анализе возраст с имя политики.
* С помощью `AuthorizationPolicyBuiler` для создания нового `AuthorizationPolicy`
* Добавление требований в политику на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`. Можно использовать в других сценариях `RequireClaim`, `RequireRole`, или `RequireUserName` вместо него.

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

## <a name="multiple-authorization-policy-providers"></a>Несколько поставщиков политики авторизации

При использовании пользовательских `IAuthorizationPolicyProvider` реализации, имейте в виду, что ASP.NET Core использует только один экземпляр `IAuthorizationPolicyProvider`. Если пользовательский поставщик не может предоставить политик авторизации для имена всех политик, он должен выполнить откат до резервного копирования поставщика. Имена политик могут включать, поступающими от политику по умолчанию для `[Authorize]` атрибуты без имени.

Например рассмотрим, что приложение требуется пользовательский срок действия политики и более традиционным получения политики на основе ролей. Такое приложение может использовать поставщик политики настраиваемой авторизации:

* Пытается проанализировать имена политик. 
* Вызовы в другой политике поставщика (например `DefaultAuthorizationPolicyProvider`) Если политика не может содержать age.

## <a name="default-policy"></a>Политика по умолчанию

Помимо политики авторизации для именованный, настраиваемый `IAuthorizationPolicyProvider` должен реализовывать `GetDefaultPolicyAsync` для предоставления политику авторизации для `[Authorize]` атрибуты не указано имя политики.

Во многих случаях этот атрибут авторизации требуется только прошедшим проверку пользователем, и внести необходимые политики с помощью вызова `RequireAuthenticatedUser`:

```CSharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Как и во всех аспектов пользовательского `IAuthorizationPolicyProvider`, этот параметр можно изменить, при необходимости. В некоторых случаях:

* Политики авторизации по умолчанию не могут использоваться.
* Получение политики по умолчанию можно делегировать переход на резервный ресурс `IAuthorizationPolicyProvider`.

## <a name="using-a-custom-iauthorizationpolicyprovider"></a>С помощью пользовательского IAuthorizationPolicyProvider

Использование настраиваемых политик из `IAuthorizationPolicyProvider`, необходимо:

* Зарегистрируйте соответствующий `AuthorizationHandler` типов с помощью внедрения зависимости (описано в [авторизации на основе политики](xref:security/authorization/policies#authorization-handlers)), как и для всех сценариев на основе политики авторизации.
* Регистрация пользовательского `IAuthorizationPolicyProvider` тип в коллекцию служб для внедрения зависимостей приложения (в `Startup.ConfigureServices`) для замены поставщика политики по умолчанию.

```CSharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Полный пользовательский `IAuthorizationPolicyProvider` пример доступен в [репозитории GitHub aspnet или AuthSamples](https://github.com/aspnet/AuthSamples/tree/dev/samples/CustomPolicyProvider).
