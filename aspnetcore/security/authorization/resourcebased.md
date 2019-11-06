---
title: Авторизация на основе ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как реализовать авторизацию на основе ресурсов в приложении ASP.NET Core, если атрибут авторизации не будет достаточным.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: acc931da1be0940fac72b0aabe07ab17ca7e63bd
ms.sourcegitcommit: 6628cd23793b66e4ce88788db641a5bbf470c3c1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73660002"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Авторизация на основе ресурсов в ASP.NET Core

Стратегия авторизации зависит от ресурса, к которому осуществляется доступ. Рассмотрим документ со свойством Author. Обновление документа разрешено только автору. Следовательно, документ необходимо извлечь из хранилища данных, прежде чем может выполняться оценка авторизации.

Оценка атрибутов выполняется перед привязкой данных и перед выполнением обработчика страницы или действия, которое загружает документ. По этим причинам декларативная авторизация с помощью атрибута `[Authorize]` недостаточна. Вместо этого можно вызвать пользовательский метод авторизации &mdash;a стилем, называемым *императивной авторизацией*.

::: moniker range=">= aspnetcore-3.0"
[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([описание скачивания](xref:index#how-to-download-a-sample)).
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([описание скачивания](xref:index#how-to-download-a-sample)).
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([описание скачивания](xref:index#how-to-download-a-sample)).
::: moniker-end

[Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации,](xref:security/authorization/secure-data) содержит пример приложения, использующего авторизацию на основе ресурсов.

## <a name="use-imperative-authorization"></a>Использовать императивную авторизацию

Авторизация реализуется как служба [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) и регистрируется в коллекции служб в классе `Startup`. Служба становится доступной посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) в обработчики страниц или действия.

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

в `IAuthorizationService` есть две перегрузки метода `AuthorizeAsync`: один принимает ресурс и имя политики, а другой принимает ресурс и список требований для вычисления.

::: moniker range=">= aspnetcore-2.0"

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

::: moniker-end

<a name="security-authorization-resource-based-imperative"></a>

В следующем примере ресурс, который требуется защитить, загружается в пользовательский объект `Document`. Вызывается перегрузка `AuthorizeAsync`, чтобы определить, разрешено ли текущему пользователю изменять предоставленный документ. Пользовательская политика авторизации "Едитполици" разбивается на решение. Дополнительные сведения о создании политик авторизации см. в разделе [Настраиваемая авторизация на основе политик](xref:security/authorization/policies) .

> [!NOTE]
> В следующих примерах кода предполагается, что проверка подлинности выполнена и задано свойство `User`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a>Написание обработчика на основе ресурсов

Написание обработчика для авторизации на основе ресурсов не сильно отличается от [написания обработчика простых требований](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Создайте настраиваемый класс требований и Реализуйте класс обработчика требований. Дополнительные сведения о создании класса требований см. в разделе [требования](xref:security/authorization/policies#requirements).

Класс обработчика задает как требование, так и тип ресурса. Например, обработчик, использующий `SameAuthorRequirement` и `Document` ресурс, выглядит следующим образом:

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

В предыдущем примере Представьте, что `SameAuthorRequirement` является особым случаем для более универсального `SpecificAuthorRequirement` класса. Класс `SpecificAuthorRequirement` (не показан) содержит свойство `Name`, представляющее имя автора. Свойству `Name` может быть присвоено значение "текущий пользователь".

Зарегистрируйте требование и обработчик в `Startup.ConfigureServices`:

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=4-8,10)]
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/2_2/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

### <a name="operational-requirements"></a>Требования к эксплуатации

Если вы принимаете решения на основе результатов операций CRUD (создание, чтение, обновление, удаление), используйте вспомогательный класс [оператионаусоризатионрекуиремент](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) . Этот класс позволяет написать отдельный обработчик, а не отдельный класс для каждого типа операции. Чтобы использовать его, укажите некоторые имена операций:

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Обработчик реализуется следующим образом: с помощью `OperationAuthorizationRequirement` требования и `Document` ресурса.

 ::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

Предыдущий обработчик проверяет операцию с помощью ресурса, удостоверения пользователя и свойства `Name` требования.

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a>Запрос и запрет с помощью обработчика рабочих ресурсов

В этом разделе показано, как обрабатываются результаты запроса и запрета действий, а также методы и запрета различий.

Чтобы вызвать обработчик рабочих ресурсов, укажите операцию при вызове `AuthorizeAsync` в обработчике страницы или в действии. В следующем примере определяется, разрешено ли пользователю, прошедшему проверку подлинности, просматривать предоставленный документ.

> [!NOTE]
> В следующих примерах кода предполагается, что проверка подлинности выполнена и задано свойство `User`.

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Если авторизация выполнена, возвращается страница для просмотра документа. Если авторизация не выполняется, но пользователь прошел проверку подлинности, возвращает `ForbidResult` уведомляет любое промежуточное по проверки подлинности об ошибке авторизации. Если необходимо выполнить проверку подлинности, возвращается `ChallengeResult`. Для интерактивных клиентов браузера может быть целесообразно перенаправить пользователя на страницу входа.

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Если авторизация выполнена, возвращается представление документа. Если авторизация завершается неудачно, возвращается `ChallengeResult` уведомляет по промежуточного слоя проверки подлинности, что ошибка авторизации, и по промежуточного слоя может принять соответствующий ответ. Соответствующий ответ может возвращать код состояния 401 или 403. Для интерактивных клиентов браузера это может означать перенаправление пользователя на страницу входа.

::: moniker-end
