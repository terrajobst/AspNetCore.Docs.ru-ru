---
title: "Авторизация на основе ресурсов"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 7f7df52bf51a81558818836450997281a21b5839
ms.sourcegitcommit: f303a457644ed034a49aa89edecb4e79d9028cb1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2017
---
# <a name="resource-based-authorization"></a>Авторизация на основе ресурсов

<a name=security-authorization-resource-based></a>

Часто авторизации зависит от ресурса к которому выполняется доступ. Например документ может иметь свойство автора. Для обновления, поэтому необходимо загрузить ресурс из репозитория документа перед предоставлением авторизации оценку допускается только автор документа. Это невозможно выполнить с атрибутом авторизовать как атрибут вычисление выполняется перед привязкой данных и перед запуском кода загрузки ресурса в действие. Вместо декларативного авторизации, атрибутов метода, необходимо использовать принудительной авторизации, где разработчик вызывает функцию авторизации в свой собственный код.

## <a name="authorizing-within-your-code"></a>Авторизация в коде

Авторизации реализован в виде службы, `IAuthorizationService`, зарегистрированных в службе коллекции и доступны через [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) для контроллеров для доступа к.

```csharp
public class DocumentController : Controller
{
    IAuthorizationService _authorizationService;

    public DocumentController(IAuthorizationService authorizationService)
    {
        _authorizationService = authorizationService;
    }
}
```

`IAuthorizationService`поддерживает два метода, один где передается ресурса и имя политики и другие где передается ресурса и список требований для оценки.

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

Для вызова службы, загрузить ресурс в пределах действия затем вызвать `AuthorizeAsync` требуется перегрузки. Пример:

```csharp
public async Task<IActionResult> Edit(Guid documentId)
{
    Document document = documentRepository.Find(documentId);

    if (document == null)
    {
        return new HttpNotFoundResult();
    }

    if (await _authorizationService.AuthorizeAsync(User, document, "EditPolicy"))
    {
        return View(document);
    }
    else
    {
        return new ChallengeResult();
    }
}
```

## <a name="writing-a-resource-based-handler"></a>Написание обработчика на основе ресурсов

Написание обработчика для авторизации на основе ресурсов, это не слишком отличается для [написание обработчика plain требования](policies.md#security-authorization-policies-based-authorization-handler). Создать требование и затем Реализуйте обработчик требование, указав требование как до, а также от типа ресурса. Например обработчик, который может принимать ресурса документа будет выглядеть следующим образом:

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    protected override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

Не забывайте также должен Зарегистрируйте обработчик в `ConfigureServices` метод:

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a>Операционные требования

При принятии решений на основе операций, таких как чтение, запись, обновление и удаление, можно использовать `OperationAuthorizationRequirement` класса в `Microsoft.AspNetCore.Authorization.Infrastructure` пространства имен. Этот класс требования готовых позволяет написать один обработчик с именем параметризованные операции, а не создавать отдельные классы для каждой операции. Чтобы использовать его, предоставляют некоторые имена операции:

```csharp
public static class Operations
{
    public static OperationAuthorizationRequirement Create =
        new OperationAuthorizationRequirement { Name = "Create" };
    public static OperationAuthorizationRequirement Read =
        new OperationAuthorizationRequirement   { Name = "Read" };
    public static OperationAuthorizationRequirement Update =
        new OperationAuthorizationRequirement { Name = "Update" };
    public static OperationAuthorizationRequirement Delete =
        new OperationAuthorizationRequirement { Name = "Delete" };
}
```

Ваш обработчик может затем быть реализован следующим образом с использованием гипотетической `Document` класса в качестве ресурса:

```csharp
public class DocumentAuthorizationHandler :
    AuthorizationHandler<OperationAuthorizationRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                OperationAuthorizationRequirement requirement,
                                                Document resource)
    {
        // Validate the operation using the resource, the identity and
        // the Name property value from the requirement.

        return Task.CompletedTask;
    }
}
```

Вы увидите работает обработчик `OperationAuthorizationRequirement`. Код в обработчике необходимо выполнить свойства имени указанного требования в учетную запись, при создании его оценки.

Для вызова обработчика оперативной ресурсов, необходимо указать операцию, при вызове `AuthorizeAsync` в действии. Пример:

```csharp
if (await _authorizationService.AuthorizeAsync(User, document, Operations.Read))
{
    return View(document);
}
else
{
    return new ChallengeResult();
}
```

В этом примере проверяется, является ли пользователь может выполнять операции чтения для текущего `document` экземпляра. После успешного авторизации будет возвращаться представления документа. Если завершается неудачно авторизации, возвращая `ChallengeResult` сообщит проверку подлинности произошел сбой авторизации по промежуточного слоя и по промежуточного слоя может принимать соответствующий ответ, например возвращая код состояния 401 или 403 или перенаправления пользователя на страницу входа для Интерактивный обозревателя клиента.
