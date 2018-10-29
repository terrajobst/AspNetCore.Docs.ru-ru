---
title: Авторизация на основе ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как реализовать авторизацию на основе ресурсов в приложениях ASP.NET Core при атрибут Authorize будет недостаточно.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 2cb3844a38f7482c27fb471343109d51a516ea20
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206700"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Авторизация на основе ресурсов в ASP.NET Core

Стратегии авторизации зависит от ресурса к которому выполняется доступ. Рассмотрим документ, который имеет свойство автора. Только автор может обновлять документа. Следовательно документа должны извлекаться из хранилища данных для оценки авторизации.

Атрибут оценивается перед привязкой данных и перед выполнением обработчика страницы или действие, которое загружает документ. По этим причинам декларативной авторизации с помощью `[Authorize]` атрибут будет недостаточно. Вместо этого можно вызвать метод настраиваемой авторизации&mdash;стиль называется принудительной авторизации.

[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).

[Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации](xref:security/authorization/secure-data) содержит пример приложения, использующего авторизации на основе ресурсов.

## <a name="use-imperative-authorization"></a>Использование принудительной авторизации

Авторизации реализуется как [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) службы и регистрируется в службе коллекции в течение `Startup` класса. Сделан доступным через службу [внедрения зависимостей](xref:fundamentals/dependency-injection) обработчики страницы или действий.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` имеет два `AuthorizeAsync` перегрузок метода: один принимает ресурс и имя политики, а другой принимает ресурс и список требований для оценки.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

---

<a name="security-authorization-resource-based-imperative"></a>

В следующем примере ресурса должна быть обеспечена безопасность загружается в пользовательский `Document` объекта. `AuthorizeAsync` Перегруженный метод вызывается, чтобы определить, разрешено ли текущий пользователь изменять указанный документ. Пользовательские политики авторизации «EditPolicy» добавляется в решение. См. в разделе [авторизации на основе политик — Custom](xref:security/authorization/policies) Дополнительные сведения о создании политик авторизации.

> [!NOTE]
> Следующий код в приведенных примерах предполагается, выполнена проверка подлинности и набор `User` свойство.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Написание обработчика, на основе ресурсов

Написание обработчика для авторизации на основе ресурсов не сильно отличается от [написание обработчика plain требования](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Создание класса пользовательского требования и реализовать класс обработчика требование. Класс обработчика задает требование и тип ресурса. Например, обработчик использование `SameAuthorRequirement` требование и `Document` ресурсов выглядит следующим образом:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Зарегистрируйте требование и обработчик в `Startup.ConfigureServices` метод:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Эксплуатационные требования

При создании решения, основываясь на результаты операции CRUD (Create, Read, Update, Delete), используйте [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) вспомогательный класс. Этот класс позволяет написать один обработчик вместо отдельный класс для каждого типа операции. Чтобы использовать его, укажите имена некоторых операций:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Обработчик реализуется следующим образом, с помощью `OperationAuthorizationRequirement` требование и `Document` ресурсов:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Предыдущий обработчик проверяет операцию, используя ресурс, удостоверение пользователя и требование `Name` свойство.

Для вызова обработчика оперативной ресурсов, указать операцию, при вызове `AuthorizeAsync` в обработчик страницы или действия. В следующем примере определяется, разрешено ли прошедшего проверку подлинности пользователя для просмотра предоставленного документа.

> [!NOTE]
> Следующий код в приведенных примерах предполагается, выполнена проверка подлинности и набор `User` свойство.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Если авторизация выполняется успешно, возвращается на страницу для просмотра документа. Если происходит сбой авторизации, но пользователь проходит проверку подлинности, возвращая `ForbidResult` информирует любое по промежуточного слоя проверки подлинности, который не удалось выполнить авторизацию. Объект `ChallengeResult` возвращается, если необходимо выполнить проверку подлинности. Для интерактивного обозревателя клиента может потребоваться перенаправить пользователя на страницу входа.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Если авторизация выполняется успешно, возвращается представление для документа. Если авторизация заканчивается неудачей, возвращая `ChallengeResult` информирует любое по промежуточного слоя проверки подлинности, что не удалось выполнить авторизацию, и по промежуточного слоя может принимать ответные действия. Код состояния 401 или 403 может возвращаться соответствующий ответ. Для интерактивного обозревателя клиента это может означать перенаправления пользователя на страницу входа.

---
