---
title: Поставщики пользовательской политики авторизации в ASP.NET Core
author: mjrousos
description: Сведения об использовании пользовательских IAuthorizationPolicyProvider в приложении ASP.NET Core для динамического создания политик авторизации.
ms.author: riande
ms.custom: mvc
ms.date: 05/02/2018
uid: security/authorization/iauthorizationpolicyprovider
ms.openlocfilehash: e3a534d3c3da5af4cfd3f72d105fac83e15135f0
ms.sourcegitcommit: d53e0cc71542b92de867bcce51575b054886f529
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/16/2018
ms.locfileid: "41829970"
---
# <a name="custom-authorization-policy-providers-using-iauthorizationpolicyprovider-in-aspnet-core"></a>Настраиваемые поставщики политики авторизации, используя IAuthorizationPolicyProvider в ASP.NET Core 

По [Майк Роусос](https://github.com/mjrousos)

Обычно при использовании [авторизации на основе политики](xref:security/authorization/policies), регистрации политики путем вызова `AuthorizationOptions.AddPolicy` как часть конфигурации службы авторизации. В некоторых случаях может оказаться возможно (или желательно) для регистрации всех политик авторизации таким образом. В таких случаях можно использовать пользовательский `IAuthorizationPolicyProvider` для управления, как передаются политик авторизации.

Примеры сценариев, где пользовательское [IAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider) может быть полезно включить:

* Использовать внешнюю службу для предоставления оценки политики.
* С помощью большой диапазон политики (для номера разных мест или возрасте, например), поэтому нет смысла для добавления каждой политики авторизации для отдельных с `AuthorizationOptions.AddPolicy` вызова.
* Создание политики в среде выполнения, на основе сведений из внешнего источника данных (например, базу данных) или динамически определить требования к проверке подлинности посредством другого механизма.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider) из [репозиторий GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples). Скачайте репозиторий aspnet/AuthSamples ZIP-файл.
Распакуйте *AuthSamples-master.zip* файл. Перейдите к *примеры/CustomPolicyProvider* папки проекта.

## <a name="customize-policy-retrieval"></a>Настройки политики извлечения

Приложения ASP.NET Core используют реализацию `IAuthorizationPolicyProvider` интерфейс для получения политики авторизации. По умолчанию [DefaultAuthorizationPolicyProvider](/dotnet/api/microsoft.aspnetcore.authorization.defaultauthorizationpolicyprovider) регистрируется и используется. `DefaultAuthorizationPolicyProvider` Возвращает политики из `AuthorizationOptions` в `IServiceCollection.AddAuthorization` вызова.

Это поведение можно настроить путем регистрации другой `IAuthorizationPolicyProvider` реализации в приложении [внедрения зависимостей](xref:fundamentals/dependency-injection) контейнера. 

`IAuthorizationPolicyProvider` Интерфейс содержит два API:

* [GetPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getpolicyasync#Microsoft_AspNetCore_Authorization_IAuthorizationPolicyProvider_GetPolicyAsync_System_String_) возвращает политику авторизации для заданного имени.
* [GetDefaultPolicyAsync](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationpolicyprovider.getdefaultpolicyasync) возвращает политику авторизации по умолчанию (политика, используемая для `[Authorize]` атрибутов без политику, указанную). 

Реализовав эти два API-интерфейсы, вы можете настроить, каким образом предоставляются политики авторизации.

## <a name="parameterized-authorize-attribute-example"></a>Параметризованные авторизовать пример атрибута

Один из сценариев где `IAuthorizationPolicyProvider` полезно используется, чтобы предоставить настраиваемый `[Authorize]` атрибуты, чьи требования зависят от параметра. Например, в [авторизации на основе политики](xref:security/authorization/policies) документации, на основе возраста («AtLeast21») политика была использована в качестве образца. Если другой контроллер действия в приложении должны быть доступными для пользователей *различных* возраста, может оказаться полезным для многих различных политик на основе возраста. Вместо регистрации все различные возрастное политики, которые приложение нужно включить в `AuthorizationOptions`, вы можете создать политики динамически на основе собственной `IAuthorizationPolicyProvider`. Чтобы сделать с помощью политики, проще, можно добавить заметку действия с пользовательский атрибут авторизации как `[MinimumAgeAuthorize(20)]`.

## <a name="custom-authorization-attributes"></a>Атрибуты настраиваемой авторизации

Политики авторизации идентифицируются по их именам. Пользовательский `MinimumAgeAuthorizeAttribute` описано ранее, необходимо сопоставить аргументов в строку, которая может использоваться для получения соответствующей политики авторизации. Это можно сделать путем наследования от `AuthorizeAttribute` и `Age` свойство wrap `AuthorizeAttribute.Policy` свойство.

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

Этот тип атрибута имеет `Policy` строки на основе жестко префикса (`"MinimumAge"`) и является целым числом, переданный через конструктор.

Его можно применить к действиям в так же, как другие `Authorize` атрибуты, за исключением того, что он принимает целое число как параметр.

```csharp
[MinimumAgeAuthorize(10)]
public IActionResult RequiresMinimumAge10()
```

## <a name="custom-iauthorizationpolicyprovider"></a>Пользовательские IAuthorizationPolicyProvider

Пользовательский `MinimumAgeAuthorizeAttribute` облегчает политики запроса авторизации для любой минимальный возраст требуемого. Далее проблемы убедиться, что политики авторизации доступны для всех этих разных возрастов. Именно здесь `IAuthorizationPolicyProvider` полезно.

При использовании `MinimumAgeAuthorizationAttribute`, имена политик авторизации будет соответствовать шаблону `"MinimumAge" + Age`, поэтому пользовательский `IAuthorizationPolicyProvider` должен создать политики авторизации с:

* Синтаксический анализ возраст с имя политики.
* С помощью `AuthorizationPolicyBuilder` для создания нового `AuthorizationPolicy`
* Добавление требований к политике на основе возраста с `AuthorizationPolicyBuilder.AddRequirements`. Можно использовать в других сценариях `RequireClaim`, `RequireRole`, или `RequireUserName` вместо этого.

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
            var policy = new AuthorizationPolicyBuilder();
            policy.AddRequirements(new MinimumAgeRequirement(age));
            return Task.FromResult(policy.Build());
        }

        return Task.FromResult<AuthorizationPolicy>(null);
    }
}
```

## <a name="multiple-authorization-policy-providers"></a>Несколько поставщиков политики авторизации

При использовании пользовательских `IAuthorizationPolicyProvider` реализаций, имейте в виду, что ASP.NET Core использует только один экземпляр `IAuthorizationPolicyProvider`. Если пользовательский поставщик не может предоставить политики авторизации для имена всех политик, он должен выполнить откат до резервного копирования поставщика. Имена политик могут включать, поступающими от политики по умолчанию для `[Authorize]` атрибуты без имени.

Например рассмотрим, что приложение требуется политики возраста пользовательский и более традиционных получения политик на основе ролей. Такое приложение может использовать поставщик политики настраиваемой авторизации:

* Пытается проанализировать имена политик. 
* Вызывает другую политику поставщика (например `DefaultAuthorizationPolicyProvider`) Если политика не может содержать возраст.

## <a name="default-policy"></a>Политика по умолчанию

Помимо предоставления политики авторизации для именованный, настраиваемый `IAuthorizationPolicyProvider` должен реализовывать `GetDefaultPolicyAsync` для предоставления политику авторизации для `[Authorize]` атрибутов без указанное имя политики.

Во многих случаях этот атрибут авторизации требуется только прошедший проверку пользователь, чтобы принимать необходимые политики с помощью вызова `RequireAuthenticatedUser`:

```csharp
public Task<AuthorizationPolicy> GetDefaultPolicyAsync() => 
    Task.FromResult(new AuthorizationPolicyBuilder().RequireAuthenticatedUser().Build());
```

Как и в случае со всеми аспектами пользовательского `IAuthorizationPolicyProvider`, этот параметр можно изменить, при необходимости. В некоторых случаях:

* Политики авторизации по умолчанию не может быть использована.
* Получение политики по умолчанию можно делегировать переход на резервный ресурс `IAuthorizationPolicyProvider`.

## <a name="use-a-custom-iauthorizationpolicyprovider"></a>Используйте пользовательские IAuthorizationPolicyProvider

Для использования настраиваемых политик из `IAuthorizationPolicyProvider`, необходимо:

* Зарегистрируйте соответствующий `AuthorizationHandler` типов с помощью внедрения зависимостей (описано в разделе [авторизации на основе политики](xref:security/authorization/policies#authorization-handlers)), как и для всех сценариев на основе политик авторизации.
* Регистрация пользовательского `IAuthorizationPolicyProvider` типа в коллекцию служб для внедрения зависимостей приложения (в `Startup.ConfigureServices`) для замены поставщика политики по умолчанию.

```csharp
services.AddTransient<IAuthorizationPolicyProvider, MinimumAgePolicyProvider>();
```

Полный пользовательский `IAuthorizationPolicyProvider` пример можно найти в [репозиторий GitHub aspnet/AuthSamples](https://github.com/aspnet/AuthSamples/tree/master/samples/CustomPolicyProvider).
