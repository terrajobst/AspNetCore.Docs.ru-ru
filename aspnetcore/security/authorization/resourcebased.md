---
title: "Авторизация на основе ресурсов"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 0902ba17-5304-4a12-a2d4-e0904569e988
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authorization/resourcebased
ms.openlocfilehash: 2f799588ba4aca4664e1679e4c34657e7ca121fb
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/11/2017
---
# <a name="resource-based-authorization"></a><span data-ttu-id="0d316-103">Авторизация на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="0d316-103">Resource Based Authorization</span></span>

<a name=security-authorization-resource-based></a>

<span data-ttu-id="0d316-104">Часто авторизации зависит от ресурса к которому выполняется доступ.</span><span class="sxs-lookup"><span data-stu-id="0d316-104">Often authorization depends upon the resource being accessed.</span></span> <span data-ttu-id="0d316-105">Пример документа может иметь свойство автора.</span><span class="sxs-lookup"><span data-stu-id="0d316-105">For example a document may have an author property.</span></span> <span data-ttu-id="0d316-106">Для обновления, поэтому необходимо загрузить ресурс из репозитория документа перед предоставлением авторизации оценку допускается только автор документа.</span><span class="sxs-lookup"><span data-stu-id="0d316-106">Only the document author would be allowed to update it, so the resource must be loaded from the document repository before an authorization evaluation can be made.</span></span> <span data-ttu-id="0d316-107">Это невозможно выполнить с атрибутом авторизовать как атрибут вычисление выполняется перед привязкой данных и перед запуском кода загрузки ресурса в действие.</span><span class="sxs-lookup"><span data-stu-id="0d316-107">This cannot be done with an Authorize attribute, as attribute evaluation takes place before data binding and before your own code to load a resource runs inside an action.</span></span> <span data-ttu-id="0d316-108">Вместо декларативного авторизации, атрибутов метода, необходимо использовать принудительной авторизации, где разработчик вызывает функцию авторизации в свой собственный код.</span><span class="sxs-lookup"><span data-stu-id="0d316-108">Instead of declarative authorization, the attribute method, we must use imperative authorization, where a developer calls an authorize function within their own code.</span></span>

## <a name="authorizing-within-your-code"></a><span data-ttu-id="0d316-109">Авторизация в коде</span><span class="sxs-lookup"><span data-stu-id="0d316-109">Authorizing within your code</span></span>

<span data-ttu-id="0d316-110">Авторизации реализован в виде службы, `IAuthorizationService`, зарегистрированных в службе коллекции и доступны через [внедрения зависимостей](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) для контроллеров для доступа к.</span><span class="sxs-lookup"><span data-stu-id="0d316-110">Authorization is implemented as a service, `IAuthorizationService`, registered in the service collection and available via [dependency injection](../../fundamentals/dependency-injection.md#fundamentals-dependency-injection) for Controllers to access.</span></span>

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

<span data-ttu-id="0d316-111">`IAuthorizationService`поддерживает два метода, один где передается ресурса и имя политики и другие где передается ресурса и список требований для оценки.</span><span class="sxs-lookup"><span data-stu-id="0d316-111">`IAuthorizationService` has two methods, one where you pass the resource and the policy name and the other where you pass the resource and a list of requirements to evaluate.</span></span>

```csharp
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<bool> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

<a name=security-authorization-resource-based-imperative></a>

<span data-ttu-id="0d316-112">Для вызова службы нагрузки ресурс в действие затем вызвать `AuthorizeAsync` требуется перегрузки.</span><span class="sxs-lookup"><span data-stu-id="0d316-112">To call the service load your resource within your action then call the `AuthorizeAsync` overload you require.</span></span> <span data-ttu-id="0d316-113">Пример</span><span class="sxs-lookup"><span data-stu-id="0d316-113">For example</span></span>

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

## <a name="writing-a-resource-based-handler"></a><span data-ttu-id="0d316-114">Написание обработчика на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="0d316-114">Writing a resource based handler</span></span>

<span data-ttu-id="0d316-115">Написание обработчика для авторизации на основе ресурсов, это не слишком отличается для [написание обработчика plain требования](policies.md#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="0d316-115">Writing a handler for resource based authorization is not that much different to [writing a plain requirements handler](policies.md#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="0d316-116">Создать требование и затем Реализуйте обработчик требование, указав требование как до, а также от типа ресурса.</span><span class="sxs-lookup"><span data-stu-id="0d316-116">You create a requirement, and then implement a handler for the requirement, specifying the requirement as before and also the resource type.</span></span> <span data-ttu-id="0d316-117">Например обработчик, который может принимать ресурса документа будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0d316-117">For example, a handler which might accept a Document resource would look as follows;</span></span>

```csharp
public class DocumentAuthorizationHandler : AuthorizationHandler<MyRequirement, Document>
{
    public override Task HandleRequirementAsync(AuthorizationHandlerContext context,
                                                MyRequirement requirement,
                                                Document resource)
    {
        // Validate the requirement against the resource and identity.

        return Task.CompletedTask;
    }
}
```

<span data-ttu-id="0d316-118">Не забывайте также должен Зарегистрируйте обработчик в `ConfigureServices` метода.</span><span class="sxs-lookup"><span data-stu-id="0d316-118">Don't forget you also need to register your handler in the `ConfigureServices` method;</span></span>

```csharp
services.AddSingleton<IAuthorizationHandler, DocumentAuthorizationHandler>();
```

### <a name="operational-requirements"></a><span data-ttu-id="0d316-119">Операционные требования</span><span class="sxs-lookup"><span data-stu-id="0d316-119">Operational Requirements</span></span>

<span data-ttu-id="0d316-120">При принятии решений на основе операций, таких как чтение, запись, обновление и удаление, можно использовать `OperationAuthorizationRequirement` класса в `Microsoft.AspNetCore.Authorization.Infrastructure` пространства имен.</span><span class="sxs-lookup"><span data-stu-id="0d316-120">If you are making decisions based on operations such as read, write, update and delete, you can use the `OperationAuthorizationRequirement` class in the `Microsoft.AspNetCore.Authorization.Infrastructure` namespace.</span></span> <span data-ttu-id="0d316-121">Этот класс требования готовых позволяет написать один обработчик с именем параметризованные операции, а не создавать отдельные классы для каждой операции.</span><span class="sxs-lookup"><span data-stu-id="0d316-121">This prebuilt requirement class enables you to write a single handler which has a parameterized operation name, rather than create individual classes for each operation.</span></span> <span data-ttu-id="0d316-122">Чтобы использовать его указать имена некоторых операций:</span><span class="sxs-lookup"><span data-stu-id="0d316-122">To use it provide some operation names:</span></span>

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

<span data-ttu-id="0d316-123">Ваш обработчик может затем быть реализован следующим образом с использованием гипотетической `Document` класса в качестве ресурса;</span><span class="sxs-lookup"><span data-stu-id="0d316-123">Your handler could then be implemented as follows, using a hypothetical `Document` class as the resource;</span></span>

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

<span data-ttu-id="0d316-124">Вы увидите работает обработчик `OperationAuthorizationRequirement`.</span><span class="sxs-lookup"><span data-stu-id="0d316-124">You can see the handler works on `OperationAuthorizationRequirement`.</span></span> <span data-ttu-id="0d316-125">Код в обработчике необходимо выполнить свойства имени указанного требования в учетную запись, при создании его оценки.</span><span class="sxs-lookup"><span data-stu-id="0d316-125">The code inside the handler must take the Name property of the supplied requirement into account when making its evaluations.</span></span>

<span data-ttu-id="0d316-126">Для вызова обработчика оперативной ресурсов, необходимо указать операцию, при вызове `AuthorizeAsync` в действии.</span><span class="sxs-lookup"><span data-stu-id="0d316-126">To call an operational resource handler you need to specify the operation when calling `AuthorizeAsync` in your action.</span></span> <span data-ttu-id="0d316-127">Пример</span><span class="sxs-lookup"><span data-stu-id="0d316-127">For example</span></span>

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

<span data-ttu-id="0d316-128">В этом примере проверяется, является ли пользователь может выполнять операции чтения для текущего `document` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="0d316-128">This example checks if the User is able to perform the Read operation for the current `document` instance.</span></span> <span data-ttu-id="0d316-129">После успешного авторизации будет возвращаться представления документа.</span><span class="sxs-lookup"><span data-stu-id="0d316-129">If authorization succeeds the view for the document will be returned.</span></span> <span data-ttu-id="0d316-130">Если завершается неудачно авторизации, возвращая `ChallengeResult` сообщит проверку подлинности произошел сбой авторизации по промежуточного слоя и по промежуточного слоя может принимать соответствующий ответ, например возвращая код состояния 401 или 403 или перенаправления пользователя на страницу входа для Интерактивный обозревателя клиента.</span><span class="sxs-lookup"><span data-stu-id="0d316-130">If authorization fails returning `ChallengeResult` will inform any authentication middleware authorization has failed and the middleware can take the appropriate response, for example returning a 401 or 403 status code, or redirecting the user to a login page for interactive browser clients.</span></span>
