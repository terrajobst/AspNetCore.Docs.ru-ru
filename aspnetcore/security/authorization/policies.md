---
title: Авторизация на основе политик в ASP.NET Core
author: rick-anderson
description: Узнайте, как создать и использовать обработчики политики авторизации для принудительного применения требований к авторизации в приложении ASP.NET Core.
ms.author: riande
ms.custom: mvc
ms.date: 04/05/2019
uid: security/authorization/policies
ms.openlocfilehash: ea9d687d3810c104d5b3fa39033849c21569709b
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891831"
---
# <a name="policy-based-authorization-in-aspnet-core"></a>Авторизация на основе политик в ASP.NET Core

Принципы работы [авторизации на основе ролей](xref:security/authorization/roles) и [авторизации на основе утверждений](xref:security/authorization/claims) требование, обработчик требование и предварительно настроенная политика. Эти блоки поддерживают выражения вычислений авторизации в коде. Результат представляет собой структуру авторизации более широкие, многократно используемых, пригодного для тестирования.

Политика авторизации состоит из одной или нескольким требованиям. Он зарегистрирован как часть конфигурации службы авторизации, в `Startup.ConfigureServices` метод:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,66)]

В приведенном выше примере создается с помощью политики «AtLeast21». Он имеет одну потребность&mdash;с минимальным возрастом, который передается в качестве параметра с требованием.

## <a name="applying-policies-to-mvc-controllers"></a>Применение политик для контроллеров MVC

Если вы используете Razor Pages, см. в разделе [применения политик к Razor Pages](#applying-policies-to-razor-pages) в этом документе.

Политики применяются к контроллерам с помощью `[Authorize]` атрибут с именем политики. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Controllers/AlcoholPurchaseController.cs?name=snippet_AlcoholPurchaseControllerClass&highlight=4)]

## <a name="applying-policies-to-razor-pages"></a>Применение политик для Razor Pages

Политики применяются к Razor Pages с помощью `[Authorize]` атрибут с именем политики. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp2/Pages/AlcoholPurchase.cshtml.cs?name=snippet_AlcoholPurchaseModelClass&highlight=4)]

Политики могут также применяться для Razor Pages с помощью [соглашение об авторизации](xref:security/authorization/razor-pages-authorization).

## <a name="requirements"></a>Требования

Потребность в авторизации является коллекцией данных параметров, которые можно использовать политику для оценки текущего участника-пользователя. В политике «AtLeast21», это единственный параметр&mdash;минимальный возраст. Реализует требования [IAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationrequirement), который является интерфейсом пустой маркер. Параметризованные минимальный возраст требование может быть реализован следующим образом:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/MinimumAgeRequirement.cs?name=snippet_MinimumAgeRequirementClass)]

Если политика авторизации содержит несколько требований к авторизации, всем требованиям необходимо передать в порядке, для успешного вычисления политики. Другими словами, несколько требований к авторизации, добавляемый в единую политику авторизации, обрабатываются на **AND** основы.

> [!NOTE]
> Требования не должны иметь данных или свойства.

<a name="security-authorization-policies-based-authorization-handler"></a>

## <a name="authorization-handlers"></a>Обработчики авторизации

Обработчик авторизации отвечает за вычисление свойств требование. Обработки авторизации оценивает требования к предоставленному [AuthorizationHandlerContext](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext) для определения того, разрешен ли доступ.

Требования могут иметь [несколько обработчиков](#security-authorization-policies-based-multiple-handlers). Обработчик может наследовать [AuthorizationHandler\<TRequirement >](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandler-1), где `TRequirement` является требованием для обработки. Кроме того, может реализовать обработчик [IAuthorizationHandler](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationhandler) для обработки более чем один тип требования.

### <a name="use-a-handler-for-one-requirement"></a>Используйте обработчик для одно требование

<a name="security-authorization-handler-example"></a>

Ниже приведен пример взаимно-однозначной связи, в котором не менее обработчик использует единый требование:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/MinimumAgeHandler.cs?name=snippet_MinimumAgeHandlerClass)]

Приведенный выше код определяет, обладает ли текущий пользователь участника дату рождения утверждения, который выдан известного и надежного издателя. Авторизация не может произойти при утверждении отсутствует; в этом случае возвращается завершенной задачи. Если заявка присутствует, вычисляется его возраст. Если пользователь соответствует минимальный возраст определяется требование, авторизация была успешно выполнена. При успешном выполнении авторизации `context.Succeed` вызывается удовлетворяет всем требованиям как единственного параметра.

### <a name="use-a-handler-for-multiple-requirements"></a>Используйте обработчик для несколько требований

Ниже приведен пример отношения один ко многим, в котором разрешение обработчик может обрабатывать три различных типа требования:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/PermissionHandler.cs?name=snippet_PermissionHandlerClass)]

Приведенный выше код проходит через [PendingRequirements](/dotnet/api/microsoft.aspnetcore.authorization.authorizationhandlercontext.pendingrequirements#Microsoft_AspNetCore_Authorization_AuthorizationHandlerContext_PendingRequirements)&mdash;свойство, содержащее требованиям не помечен как об успешном. Для `ReadPermission` требование, пользователь должен быть владельцем или спонсора для доступа к запрошенному ресурсу. В случае использования `EditPermission` или `DeletePermission` требование, он должен быть владельцем, чтобы получить доступ к запрошенному ресурсу.

<a name="security-authorization-policies-based-handler-registration"></a>

### <a name="handler-registration"></a>Регистрация обработчика

Обработчики, регистрируются в коллекции служб во время настройки. Пример:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=32-33,48-53,61,62-63,66)]

Приведенный выше код регистрирует `MinimumAgeHandler` как Singleton-класс путем вызова `services.AddSingleton<IAuthorizationHandler, MinimumAgeHandler>();`. Обработчики могут быть зарегистрированы с помощью любого из встроенных [службы времени существования](xref:fundamentals/dependency-injection#service-lifetimes).

## <a name="what-should-a-handler-return"></a>Что должен возвращать обработчик?

Обратите внимание, что `Handle` метод в [пример обработчика](#security-authorization-handler-example) не возвращает значений. Как такое состояние об успешном или обнаруженное?

* Обработчик указывает на успешное завершение, вызвав `context.Succeed(IAuthorizationRequirement requirement)`, передав требование, проверка прошла успешно.

* Обработчику не нужно обрабатывать сбои в общем случае, как другие обработчики для требования может завершиться успешно.

* Чтобы гарантировать сбоя, даже если другие обработчики требований, вызовите `context.Fail`.

Если обработчик вызывает `context.Succeed` или `context.Fail`, по-прежнему вызываются все обработчики. Благодаря этому потребности для получения побочные эффекты, такие как ведение журнала, что производится, даже если другой обработчик имеет успешно проверен или не удалось выполнить требование. Если задано значение `false`, [InvokeHandlersAfterFailure](/dotnet/api/microsoft.aspnetcore.authorization.authorizationoptions.invokehandlersafterfailure#Microsoft_AspNetCore_Authorization_AuthorizationOptions_InvokeHandlersAfterFailure) запуск обработчиков игнорирует свойство (в ASP.NET Core 1.1 и более поздние версии) при `context.Fail` вызывается. `InvokeHandlersAfterFailure` по умолчанию используется `true`, в этом случае вызываются все обработчики.

> [!NOTE]
> Авторизация обработчики вызываются в том случае, даже в случае сбоя проверки подлинности.

<a name="security-authorization-policies-based-multiple-handlers"></a>

## <a name="why-would-i-want-multiple-handlers-for-a-requirement"></a>Зачем несколько обработчиков для требования?

В тех случаях, когда вычисление на **или** основы, реализовать несколько обработчиков для одного требования. Например Корпорация Майкрософт двери, которые только открытых ключей карты. Если ключ-карта забыть дома, секретарь выводится временный наклейку и открывает возможность для вас. В этом случае будет иметь одну потребность, *BuildingEntry*, но несколько обработчиков, каждый из них действует только одно требование проверки.

*BuildingEntryRequirement.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Requirements/BuildingEntryRequirement.cs?name=snippet_BuildingEntryRequirementClass)]

*BadgeEntryHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/BadgeEntryHandler.cs?name=snippet_BadgeEntryHandlerClass)]

*TemporaryStickerHandler.cs*

[!code-csharp[](policies/samples/PoliciesAuthApp1/Services/Handlers/TemporaryStickerHandler.cs?name=snippet_TemporaryStickerHandlerClass)]

Убедитесь, что оба обработчика [зарегистрирован](xref:security/authorization/policies#security-authorization-policies-based-handler-registration). Если обработчик либо завершается успешно, если политика оценивает `BuildingEntryRequirement`, оценка политики завершается успешно.

## <a name="using-a-func-to-fulfill-a-policy"></a>С помощью func для выполнения политики

Могут возникнуть ситуации, в какие выполнении политики прост для выражения в коде. Это можно указать `Func<AuthorizationHandlerContext, bool>` при настройке политики с `RequireAssertion` построитель политики.

Например, предыдущий `BadgeEntryHandler` можно переписать следующим образом:

[!code-csharp[](policies/samples/PoliciesAuthApp1/Startup.cs?range=50-51,55-61)]

## <a name="accessing-mvc-request-context-in-handlers"></a>Доступ к контексту запроса MVC в обработчиках

`HandleRequirementAsync` Метод реализовать в обработчике авторизации имеет два параметра: `AuthorizationHandlerContext` и `TRequirement` обрабатываются. Платформы, такие как MVC или Jabbr предоставляются бесплатно добавить любой объект `Resource` свойство `AuthorizationHandlerContext` для передачи дополнительных сведений.

Например, MVC передает экземпляр [AuthorizationFilterContext](/dotnet/api/?term=AuthorizationFilterContext) в `Resource` свойство. Это свойство предоставляет доступ к `HttpContext`, `RouteData`и все, что предоставленный другим, MVC и Razor Pages.

Использование `Resource` свойство является определенной платформы. Используя информацию в `Resource` свойство ограничивает политик авторизации для конкретной платформы. Следует привести `Resource` свойства с помощью `is` ключевое слово, а затем подтвердите приведение выполнено успешно, чтобы обеспечить отсутствие сбоев кода с `InvalidCastException` при запуске на другие платформы:

```csharp
// Requires the following import:
//     using Microsoft.AspNetCore.Mvc.Filters;
if (context.Resource is AuthorizationFilterContext mvcContext)
{
    // Examine MVC-specific things like routing data.
}
```
