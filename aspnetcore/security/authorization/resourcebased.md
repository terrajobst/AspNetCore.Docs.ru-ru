---
title: Авторизация на основе ресурсов в ASP.NET Core
author: scottaddie
description: Дополнительные сведения о реализации авторизации на основе ресурсов в приложении ASP.NET Core время авторизовать атрибута не будет достаточно.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
uid: security/authorization/resourcebased
ms.openlocfilehash: 577af3ba45361aec715a49fa59b9ec9869ced851
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273351"
---
# <a name="resource-based-authorization-in-aspnet-core"></a>Авторизация на основе ресурсов в ASP.NET Core

Зависит от стратегии авторизации доступ к ресурсу. Рассмотрим документ, который имеет свойство автора. Только автор может обновлять документа. Следовательно документа должны извлекаться из хранилища данных для оценки авторизации.

Атрибут оценки происходит перед привязкой данных и до выполнения действия, который загружает документ или обработчику страницы. По этим причинам декларативный авторизации с помощью `[Authorize]` атрибута не будет достаточно. Вместо этого можно вызвать метод настраиваемой авторизации&mdash;стиля называется принудительной авторизации.

Используйте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) для изучения функций, описанных в этом разделе.

[Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации](xref:security/authorization/secure-data) содержит пример приложения, использующего авторизации на основе ресурсов.

## <a name="use-imperative-authorization"></a>Использование принудительной авторизации

Авторизация реализуется в виде [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) службы и регистрируется в службе коллекции в течение `Startup` класса. Службы доступны через [внедрения зависимостей](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) обработчики страницы или действий.

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

`IAuthorizationService` имеет два `AuthorizeAsync` перегруженных версий метода: один прием, ресурс и имя политики, а другой принимает ресурс и список требований для оценки.

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

В следующем примере загружается ресурса с защитой в пользовательской `Document` объекта. `AuthorizeAsync` Перегрузка вызывается для определения, разрешен ли текущий пользователь для изменения указанного документа. Пользовательскую политику авторизации «EditPolicy» добавляется в решение. В разделе [авторизации на основе политики настраиваемый](xref:security/authorization/policies) для получения дополнительных сведений о создании политик авторизации.

> [!NOTE]
> Следующий код, образцы предполагается выполнения проверки подлинности и набор `User` свойство.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a>Написание обработчика, основанное на ресурсах

Написание обработчика для авторизации на основе ресурсов не сильно отличается от [написание обработчика plain требования](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler). Создание класса пользовательского требования и реализовать класс обработчика требование. Класс обработчика указывает требований и тип ресурса. Например, обработчик использование `SameAuthorRequirement` требование и `Document` ресурсов выглядит следующим образом:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

Регистрация требование и обработчик в `Startup.ConfigureServices` метод:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a>Операционные требования

При создании решения на основе результатов операций CRUD (Создание, чтение, обновление, удаление), используйте [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) вспомогательного класса. Этот класс позволяет создавать один обработчик вместо отдельный класс для каждого типа операции. Чтобы использовать его, предоставляют некоторые имена операции:

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

Обработчик реализуется следующим образом, с помощью `OperationAuthorizationRequirement` требование и `Document` ресурсов:

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

Предыдущий обработчик проверяет операцию с использованием ресурсов, удостоверение пользователя и требование `Name` свойство.

Для вызова обработчика оперативной ресурсов, указать операцию при вызове `AuthorizeAsync` в обработчику страницы или действия. В следующем примере определяется, разрешено ли авторизованного пользователя для просмотра указанного документа.

> [!NOTE]
> Следующий код, образцы предполагается выполнения проверки подлинности и набор `User` свойство.

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

Если авторизации завершается успешно, возвращается страницы для просмотра документа. Если происходит сбой авторизации, но пользователь прошел проверку подлинности, возвращая `ForbidResult` сообщает все по промежуточного слоя проверки подлинности, которое не удалось выполнить авторизацию. Значение `ChallengeResult` возвращается, если необходимо выполнить проверку подлинности. Для интерактивного обозревателя клиента он может подойти перенаправлять пользователя на страницу входа.

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

Если авторизации завершается успешно, возвращается представление для документа. В случае непрохождения авторизации возвращение `ChallengeResult` информирует любое по промежуточного слоя проверки подлинности, не удалось выполнить авторизацию, что по промежуточного слоя может принимать соответствующий ответ. Соответствующий ответ может вернул код состояния 401 или 403. Для интерактивного обозревателя клиента это может означать перенаправления пользователя на страницу входа.

---
