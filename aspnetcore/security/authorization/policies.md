---
title: "Пользовательская авторизация на основе политик в ASP.NET Core"
author: rick-anderson
description: "Узнайте, как создавать и использовать обработчики настраиваемой авторизации политики для реализации требования к проверке подлинности в приложении ASP.NET Core."
ms.author: riande
ms.custom: mvc
manager: wpickett
ms.date: 11/21/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/policies
ms.openlocfilehash: 1067e97dd6e71021929aa3690b0c3f5bfc6c9724
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/19/2018
---
# <a name="custom-policy-based-authorization"></a>Пользовательская авторизация на основе политик

В системе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) использовать требования, требования обработчик и предварительно настроенных политик. Выражение вычисления авторизации поддерживают эти блоки в коде. Результат представляет собой структуру богатый многократно используемых и тестируемых авторизации.

Политика авторизации состоит из одного или нескольких требования. Он зарегистрирован в процессе настройки службы авторизации, `ConfigureServices` метод `Startup` класса:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

В предыдущем примере создается политика «AtLeast21». Он имеет один требование, что минимальный возраст, который показан как параметр с требованием.

Политики применяются с помощью `[Authorize]` атрибут с именем политики. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Требования

Требования к авторизации — это коллекция данных параметров, которые можно использовать политику для оценки текущего участника-пользователя. В политике «AtLeast21» требуется один параметр&mdash;минимальный возраст. Реализует требование `IAuthorizationRequirement`, которая является интерфейсом пустой маркер. Параметризованные минимально допустимый возраст может быть реализован следующим образом:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Это требование не должны иметь данные или свойства.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Обработчики авторизации

Обработчик авторизации отвечает за вычисление свойств является обязательным. Обработчик авторизации оценивает требования, к указанному `AuthorizationHandlerContext` для определения, разрешен доступ. Это требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers). Наследовать обработчики `AuthorizationHandler<T>`, где `T` требуется обработать.

<a name="security-authorization-handler-example"></a>

Минимальный возраст обработчик может выглядеть следующим образом:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Предыдущий код определяет, обладает ли текущий пользователь основной Дата рождения утверждений, который выдал известного и надежного издателя. Авторизация может произойти при утверждении отсутствует, в этом случае возвращается Завершенная задача. Если утверждение присутствует, вычисляется возраста пользователя. Если пользователь не отвечает минимальный возраст определяется требование, авторизация была успешно выполнена. При успешном выполнении авторизации `context.Succeed` вызывается удовлетворяет требованию как параметр.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Регистрации обработчика

Обработчики, зарегистрированные в коллекции служб во время настройки. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Каждый обработчик добавляется в коллекцию службы путем вызова `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Что следует возвратить обработчик?

Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений. Как это состояние, успех или сбой?

* Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требования, которые успешно проверен.

* Как правило, обработка ошибок, как другие обработчики для того же требование может быть успешным обработчик необязательно.

* Чтобы гарантировать сбоя, даже если другие обработчики требование успешно, вызовите `context.Fail`.

Независимо от того, вызывается, внутри обработчиком все обработчики для требования будет вызываться при политики требует требование. Это позволяет требования с побочными эффектами, например ведение журнала, который всегда будет иметь место даже в том случае, если `context.Fail()` был вызван в другой обработчик.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Зачем нужен несколько обработчиков для требования?

В случаях, когда нужно оценки на **или** основы, реализовать несколько обработчиков для одного требования. Например Корпорация Майкрософт имеет двери, которые только открытых ключей карты. Если оставить ключ-карта дома, секретарь выводит временные наклейке и создает предпосылки для вас. В этом случае будет иметь одну потребность *BuildingEntry*, но несколько обработчиков, каждый из них один требование проверки.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Убедитесь, что оба обработчика [зарегистрированные](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Если обработчик либо завершается успешно, если политика вычисляется `BuildingEntryRequirement`, прохождения проверки политики.

## <a name="using-a-func-to-fulfill-a-policy"></a>Используя функцию для выполнения политики

Могут возникнуть ситуации, в какие выполнении политику простой для выражения в коде. Можно указать `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.

Например, предыдущие `BadgeEntryHandler` можно переписать следующим образом:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=52-53,57-63)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Доступ к контексту запроса MVC в обработчиках

`HandleRequirementAsync` Метод реализовать в обработчике авторизации имеет два параметра: `AuthorizationHandlerContext` и `TRequirement` обрабатывается. Платформы, например MVC или Jabbr добавьте любой объект `Resource` свойство `AuthorizationHandlerContext` для передачи дополнительных сведений.

Например, MVC передает экземпляр [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство. Это свойство предоставляет доступ к `HttpContext`, `RouteData`и все еще предоставляемых MVC и страниц Razor.

Использование `Resource` свойство имеет определенную платформу. Используя информацию в `Resource` свойство ограничивает политик авторизации для определенной платформы. Необходимо привести `Resource` свойства с помощью `as` ключевое слово и убедитесь, что приведение имеет успешно чтобы ваш код не приводит к сбою с `InvalidCastException` при выполнении на других платформах:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
