---
title: Авторизация на основе политик в ASP.NET Core
author: rick-anderson
description: Узнайте, как создавать и использовать обработчики политики авторизации для реализации требования к проверке подлинности в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 11/21/2017
uid: security/authorization/policies
ms.openlocfilehash: 81ca6ad9ddba3de094762f5608bb6a5719bca7a1
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36277986"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Авторизация на основе политик в ASP.NET Core

В системе [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) использовать требования, требования обработчик и предварительно настроенных политик. Выражение вычисления авторизации поддерживают эти блоки в коде. Результат представляет собой структуру богатый многократно используемых и тестируемых авторизации.

Политика авторизации состоит из одного или нескольких требования. Он зарегистрирован в процессе настройки службы авторизации, `Startup.ConfigureServices` метод:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63,72)]

В предыдущем примере создается политика «AtLeast21». Он имеет одну потребность&mdash;, минимальный срок действия, который указан как параметр с требованием.

Политики применяются с помощью `[Authorize]` атрибут с именем политики. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="requirements"></a>Требования

Требования к авторизации — это коллекция данных параметров, которые можно использовать политику для оценки текущего участника-пользователя. В политике «AtLeast21» требуется один параметр&mdash;минимальный возраст. Реализует требование [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), которая является интерфейсом пустой маркер. Параметризованные минимально допустимый возраст может быть реализован следующим образом:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

> [!NOTE]
> Это требование не должны иметь данные или свойства.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Обработчики авторизации

Обработчик авторизации отвечает за вычисление свойств является обязательным. Обработчик авторизации оценивает требования, к указанному [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) для определения, разрешен доступ.

Это требование может иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers). Обработчик может наследовать [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` требуется обработать. Кроме того, могут реализовать обработчик [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) обрабатывать более одного типа требование.

### <a name="use-a-handler-for-one-requirement"></a>Использование обработчика для одного требования

<a name="security-authorization-handler-example"></a>

Ниже приведен пример взаимно-однозначной связи, в котором минимальный возраст обработчик использует один требование:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Предыдущий код определяет, обладает ли текущий пользователь основной Дата рождения утверждений, который выдал известного и надежного издателя. Авторизация может произойти при утверждении отсутствует, в этом случае возвращается Завершенная задача. Если утверждение присутствует, вычисляется возраста пользователя. Если пользователь не отвечает минимальный возраст определяется требование, авторизация была успешно выполнена. При успешном выполнении авторизации `context.Succeed` вызывается с удовлетворяет потребность в качестве единственного параметра.

### <a name="use-a-handler-for-multiple-requirements"></a>Использовать обработчик для нескольких требований

Ниже приведен пример один ко многим отношения, в котором разрешение обработчик использует три требования:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Предыдущий код проходит через [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойство, содержащее требования не помечен как успешно. Если пользователь имеет разрешение на чтение, он или она необходимо владельца или спонсоров для доступа к запрошенному ресурсу. Если пользователь имеет изменить или удалить разрешение, он или она должны быть владельцем для доступа к запрошенному ресурсу. При успешном выполнении авторизации `context.Succeed` вызывается с удовлетворяет потребность в качестве единственного параметра.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Регистрации обработчика

Обработчики, зарегистрированные в коллекции служб во время настройки. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=40-41,50-55,63-65,72)]

Каждый обработчик добавляется в коллекцию службы путем вызова `services.AddSingleton<IAuthorizationHandler, YourHandlerClass>();`.

## <a name="what-should-a-handler-return"></a>Что следует возвратить обработчик?

Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений. Как это состояние, успех или сбой?

* Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требования, которые успешно проверен.

* Обработчик не требуется, как правило, обработка ошибок, как может быть успешным, другие обработчики для того же требования.

* Чтобы гарантировать сбоя, даже если другие обработчики требование успешно, вызовите `context.Fail`.

Если задано значение `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) выполнения обработчиков игнорирует свойство (в ASP.NET Core 1.1 и более поздние версии) при `context.Fail` вызывается. `InvokeHandlersAfterFailure` по умолчанию используется значение `true`, в этом случае все обработчики вызываются. Это позволяет требования для создания побочные эффекты, такие как ведение журнала, которые всегда выполняются даже в том случае, если `context.Fail` был вызван в другой обработчик.

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
