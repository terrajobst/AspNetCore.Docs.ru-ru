---
title: "Пользовательская авторизация на основе политик"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: e422a1b2-dc4a-4bcc-b8d9-7ee62009b6a3
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 2e3bbcc9ffd90d7cba974466860738f1f462d3b3
ms.sourcegitcommit: c29954cdfed0257eef92243175802ad6929e32bc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/13/2017
---
# <a name="custom-policy-based-authorization"></a>Пользовательская авторизация на основе политик

<a name=security-authorization-policies-based></a>

В системе [роли авторизации](roles.md) и [утверждений авторизации](claims.md) сделать использование требования, обработчик требование и предварительно настроенных политик. Эти блоки позволяют представить оценок авторизации кода, что обеспечивает более широкие, для повторного использования, и структура легко тестируемых авторизации.

Политика авторизации состоит из одного или нескольких требований и регистрируется при запуске приложения в процессе настройки службы авторизации, в `ConfigureServices` в *файла Startup.cs* файл.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });
}
```

Здесь вы увидите, что политики «Over21» создается один требование, что минимальный срок действия которого передается как параметр с требованием.

Политики применяются с помощью `Authorize` атрибут, указав имя политики, например;

```csharp
[Authorize(Policy="Over21")]
public class AlcoholPurchaseRequirementsController : Controller
{
    public ActionResult Login()
    {
    }

    public ActionResult Logout()
    {
    }
}
```

## <a name="requirements"></a>Требования

Требования к авторизации — это коллекция данных параметров, которые можно использовать политику для оценки текущего участника-пользователя. В политике минимальный возраст еще требуется один параметр, минимальный возраст. Необходимо реализовать требование `IAuthorizationRequirement`. Это пустой интерфейс-маркер. Параметризованные минимально допустимый возраст может быть реализовано следующим образом:

```csharp
public class MinimumAgeRequirement : IAuthorizationRequirement
{
    public int MinimumAge { get; private set; }
    
    public MinimumAgeRequirement(int minimumAge)
    {
        MinimumAge = minimumAge;
    }
}
```

Это требование не должны иметь данные или свойства.

<a name=security-authorization-policies-based-authorization-handler></a>

## <a name="authorization-handlers"></a>Обработчики авторизации

Обработчик авторизации отвечает за вычисление любой свойств является обязательным. Обработчик авторизации необходимо их оценки относительно указанного `AuthorizationHandlerContext` решаете, разрешена ли авторизация. Это требование может иметь [несколько обработчиков](policies.md#security-authorization-policies-based-multiple-handlers). Обработчики должны наследовать `AuthorizationHandler<T>` там, где T — требование, обрабатывает его.

<a name=security-authorization-handler-example></a>

Минимальный возраст обработчик может выглядеть следующим образом:

```csharp
public class MinimumAgeHandler : AuthorizationHandler<MinimumAgeRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, MinimumAgeRequirement requirement)
    {
        if (!context.User.HasClaim(c => c.Type == ClaimTypes.DateOfBirth &&
                                   c.Issuer == "http://contoso.com"))
        {
            // .NET 4.x -> return Task.FromResult(0);
            return Task.CompletedTask;
        }

        var dateOfBirth = Convert.ToDateTime(context.User.FindFirst(
            c => c.Type == ClaimTypes.DateOfBirth && c.Issuer == "http://contoso.com").Value);

        int calculatedAge = DateTime.Today.Year - dateOfBirth.Year;
        if (dateOfBirth > DateTime.Today.AddYears(-calculatedAge))
        {
            calculatedAge--;
        }

        if (calculatedAge >= requirement.MinimumAge)
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

В коде выше мы сначала найдите ли основной текущий пользователь не имеет даты рождения утверждения, которой выполнялась, мы знаем, что издатель и доверия. Если утверждение отсутствует мы не удается авторизовать так мы возвращаем. Если у нас есть утверждение, мы выяснить, насколько стара пользователь является и, если они удовлетворяют минимальный возраст, передаваемые с требование затем авторизация прошла успешно. После успешной авторизации мы называем `context.Succeed()` передав требование, которое было выполнено успешно, как параметр.

<a name=security-authorization-policies-based-handler-registration></a>

Обработчики должны быть зарегистрированы в коллекции служб во время настройки, например;

```csharp

public void ConfigureServices(IServiceCollection services)
{
    services.AddMvc();

    services.AddAuthorization(options =>
    {
        options.AddPolicy("Over21",
                          policy => policy.Requirements.Add(new MinimumAgeRequirement(21)));
    });

    services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();
}
```

Каждый обработчик добавляется в коллекцию служб с помощью `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();` передав класс-обработчик.

## <a name="what-should-a-handler-return"></a>Что следует возвратить обработчик?

Вы увидите на нашем [пример обработчика](policies.md#security-authorization-handler-example) , `Handle()` метод не имеет возвращаемого значения, так как мы указывают на успешное или неуспешное?

* Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требования, которые успешно проверен.

* Как правило, обработка ошибок, как другие обработчики для того же требование может быть успешным обработчик необязательно.

* Чтобы гарантировать сбоя, даже если другие обработчики для требования завершиться успешно, вызовите `context.Fail`.

Независимо от того, вызывается в обработчике все обработчики для требования будет вызываться, если политика требует требование. Это позволяет требования с побочными эффектами, например ведение журнала, который всегда будет иметь место даже в том случае, если `context.Fail()` был вызван в другой обработчик.

<a name=security-authorization-policies-based-multiple-handlers></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Зачем нужен несколько обработчиков для требования?

В случаях, когда нужно оценки на **или** основы реализовать несколько обработчиков для одного требования. Например Корпорация Майкрософт имеет двери, которые только открытых ключей карты. Если оставить ключ-карта домашнего секретарь выводит временные наклейке и создает предпосылки для вас. В этом случае будет иметь одну потребность *EnterBuilding*, но несколько обработчиков, каждый из них один требование проверки.

```csharp
public class EnterBuildingRequirement : IAuthorizationRequirement
{
}

public class BadgeEntryHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.BadgeId &&
                                       c.Issuer == "http://microsoftsecurity"))
        {
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}

public class HasTemporaryStickerHandler : AuthorizationHandler<EnterBuildingRequirement>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context, EnterBuildingRequirement requirement)
    {
        if (context.User.HasClaim(c => c.Type == ClaimTypes.TemporaryBadgeId &&
                                       c.Issuer == "https://microsoftsecurity"))
        {
            // We'd also check the expiration date on the sticker.
            context.Succeed(requirement);
        }
        return Task.CompletedTask;
    }
}
```

Теперь, при условии, что оба обработчика [зарегистрированные](xref:security/authorization/policies#security-authorization-policies-based-handler-registration) когда оценивается политика `EnterBuildingRequirement` после успешного либо обработчик оценки политики будет выполнена успешно.

## <a name="using-a-func-to-fulfill-a-policy"></a>Используя функцию для выполнения политики

Возможны случаи где поступивший политику простой для выражения в коде. Можно просто предоставить `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.

Например, предыдущие `BadgeEntryHandler` можно переписать следующим образом:

```csharp
services.AddAuthorization(options =>
    {
        options.AddPolicy("BadgeEntry",
                          policy => policy.RequireAssertion(context =>
                                  context.User.HasClaim(c =>
                                     (c.Type == ClaimTypes.BadgeId ||
                                      c.Type == ClaimTypes.TemporaryBadgeId)
                                      && c.Issuer == "https://microsoftsecurity"));
                          }));
    }
 }
```

## <a name="accessing-mvc-request-context-in-handlers"></a>Доступ к контексту запроса MVC в обработчиках

`Handle` Метод необходимо реализовать в обработчике авторизации имеет два параметра `AuthorizationContext` и `Requirement` обрабатывается. Платформы, например MVC или Jabbr добавьте любой объект `Resource` свойство `AuthorizationContext` пройти дополнительную информацию.

Например, MVC передает экземпляр `Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext` в свойство ресурса, который используется для доступа к HttpContext RouteData и все остальные MVC предоставляет.

Использование `Resource` свойство имеет определенную платформу. Используя информацию в `Resource` свойство ограничивает политик авторизации для определенной платформы. Необходимо привести `Resource` свойства с помощью `as` ключевое слово, а затем проверяет, имеет приведение успешно, чтобы ваш код не приводит к сбою с `InvalidCastExceptions` при выполнении на другие платформы;

```csharp
if (context.Resource is Microsoft.AspNetCore.Mvc.Filters.AuthorizationFilterContext mvcContext)
{
    // Examine MVC specific things like routing data.
}
```
