---
title: "Авторизация на основе ресурсов в ASP.NET Core"
author: scottaddie
description: "Дополнительные сведения о реализации авторизации на основе ресурсов в приложении ASP.NET Core время авторизовать атрибута не будет достаточно."
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 11/07/2017
ms.devlang: csharp
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/authorization/resourcebased
ms.openlocfilehash: 723e371e0d0b4877f96898c68cd59b433fa97dc1
ms.sourcegitcommit: 7a87d66cf1d01febe6635c7306f2f679434901d1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/03/2018
---
# <a name="resource-based-authorization"></a><span data-ttu-id="b6620-103">Авторизация на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="b6620-103">Resource-based authorization</span></span>

<span data-ttu-id="b6620-104">Зависит от стратегии авторизации доступ к ресурсу.</span><span class="sxs-lookup"><span data-stu-id="b6620-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="b6620-105">Рассмотрим документ, который имеет свойство автора.</span><span class="sxs-lookup"><span data-stu-id="b6620-105">Consider a document which has an author property.</span></span> <span data-ttu-id="b6620-106">Только автор может обновлять документа.</span><span class="sxs-lookup"><span data-stu-id="b6620-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="b6620-107">Следовательно документа должны извлекаться из хранилища данных для оценки авторизации.</span><span class="sxs-lookup"><span data-stu-id="b6620-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="b6620-108">Атрибут оценки происходит перед привязкой данных и до выполнения действия, который загружает документ или обработчику страницы.</span><span class="sxs-lookup"><span data-stu-id="b6620-108">Attribute evaluation occurs before data binding and before execution of the page handler or action which loads the document.</span></span> <span data-ttu-id="b6620-109">По этим причинам декларативный авторизации с помощью `[Authorize]` атрибута не будет достаточно.</span><span class="sxs-lookup"><span data-stu-id="b6620-109">For these reasons, declarative authorization with an `[Authorize]` attribute won't suffice.</span></span> <span data-ttu-id="b6620-110">Вместо этого можно вызвать метод настраиваемой авторизации&mdash;стиля называется принудительной авторизации.</span><span class="sxs-lookup"><span data-stu-id="b6620-110">Instead, you can invoke a custom authorization method&mdash;a style known as imperative authorization.</span></span>

<span data-ttu-id="b6620-111">Используйте [образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([загрузке](xref:tutorials/index#how-to-download-a-sample)) для изучения функций, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="b6620-111">Use the [sample apps](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:tutorials/index#how-to-download-a-sample)) to explore the features described in this topic.</span></span>

<span data-ttu-id="b6620-112">[Создание приложения ASP.NET Core пользовательскими данными, защищенных авторизации](xref:security/authorization/secure-data) содержит пример приложения, использующего авторизации на основе ресурсов.</span><span class="sxs-lookup"><span data-stu-id="b6620-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="b6620-113">Использование принудительной авторизации</span><span class="sxs-lookup"><span data-stu-id="b6620-113">Use imperative authorization</span></span>

<span data-ttu-id="b6620-114">Авторизация реализуется в виде [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) службы и регистрируется в службе коллекции в течение `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="b6620-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="b6620-115">Службы доступны через [внедрения зависимостей](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) обработчики страницы или действий.</span><span class="sxs-lookup"><span data-stu-id="b6620-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection#fundamentals-dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="b6620-116">`IAuthorizationService`имеет два `AuthorizeAsync` перегруженных версий метода: один прием, ресурс и имя политики, а другой принимает ресурс и список требований для оценки.</span><span class="sxs-lookup"><span data-stu-id="b6620-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6620-117">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6620-117">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

```csharp
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          IEnumerable<IAuthorizationRequirement> requirements);
Task<AuthorizationResult> AuthorizeAsync(ClaimsPrincipal user,
                          object resource,
                          string policyName);
```

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6620-118">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6620-118">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

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

<span data-ttu-id="b6620-119">В следующем примере загружается ресурса с защитой в пользовательской `Document` объекта.</span><span class="sxs-lookup"><span data-stu-id="b6620-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="b6620-120">`AuthorizeAsync` Перегрузка вызывается для определения, разрешен ли текущий пользователь для изменения указанного документа.</span><span class="sxs-lookup"><span data-stu-id="b6620-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="b6620-121">Пользовательскую политику авторизации «EditPolicy» добавляется в решение.</span><span class="sxs-lookup"><span data-stu-id="b6620-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="b6620-122">В разделе [авторизации на основе политики настраиваемый](xref:security/authorization/policies) для получения дополнительных сведений о создании политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="b6620-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="b6620-123">Следующий код, образцы предполагается выполнения проверки подлинности и набор `User` свойство.</span><span class="sxs-lookup"><span data-stu-id="b6620-123">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6620-124">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6620-124">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6620-125">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6620-125">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

---

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="b6620-126">Написание обработчика, основанное на ресурсах</span><span class="sxs-lookup"><span data-stu-id="b6620-126">Write a resource-based handler</span></span>

<span data-ttu-id="b6620-127">Написание обработчика для авторизации на основе ресурсов не сильно отличается от [написание обработчика plain требования](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="b6620-127">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="b6620-128">Создание класса пользовательского требования и реализовать класс обработчика требование.</span><span class="sxs-lookup"><span data-stu-id="b6620-128">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="b6620-129">Класс обработчика указывает требований и тип ресурса.</span><span class="sxs-lookup"><span data-stu-id="b6620-129">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="b6620-130">Например, обработчик использование `SameAuthorRequirement` требование и `Document` ресурсов выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="b6620-130">For example, a handler utilizing a `SameAuthorRequirement` requirement and a `Document` resource looks as follows:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6620-131">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6620-131">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6620-132">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6620-132">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

---

<span data-ttu-id="b6620-133">Регистрация требование и обработчик в `Startup.ConfigureServices` метод:</span><span class="sxs-lookup"><span data-stu-id="b6620-133">Register the requirement and handler in the `Startup.ConfigureServices` method:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="b6620-134">Операционные требования</span><span class="sxs-lookup"><span data-stu-id="b6620-134">Operational requirements</span></span>

<span data-ttu-id="b6620-135">Если вы вносите решений, основанных на результаты CRUD (**C**оздать, **R**честь, **U**бновить, **D**далить) операции, используют [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) вспомогательного класса.</span><span class="sxs-lookup"><span data-stu-id="b6620-135">If you're making decisions based on the outcomes of CRUD (**C**reate, **R**ead, **U**pdate, **D**elete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="b6620-136">Этот класс позволяет создавать один обработчик вместо отдельный класс для каждого типа операции.</span><span class="sxs-lookup"><span data-stu-id="b6620-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="b6620-137">Чтобы использовать его, предоставляют некоторые имена операции:</span><span class="sxs-lookup"><span data-stu-id="b6620-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="b6620-138">Обработчик реализуется следующим образом, с помощью `OperationAuthorizationRequirement` требование и `Document` ресурсов:</span><span class="sxs-lookup"><span data-stu-id="b6620-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6620-139">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6620-139">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6620-140">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6620-140">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

---

<span data-ttu-id="b6620-141">Предыдущий обработчик проверяет операцию с использованием ресурсов, удостоверение пользователя и требование `Name` свойство.</span><span class="sxs-lookup"><span data-stu-id="b6620-141">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="b6620-142">Для вызова обработчика оперативной ресурсов, указать операцию при вызове `AuthorizeAsync` в обработчику страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="b6620-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="b6620-143">В следующем примере определяется, разрешено ли авторизованного пользователя для просмотра указанного документа.</span><span class="sxs-lookup"><span data-stu-id="b6620-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="b6620-144">Следующий код, образцы предполагается выполнения проверки подлинности и набор `User` свойство.</span><span class="sxs-lookup"><span data-stu-id="b6620-144">The following code samples assume authentication has run and set the `User` property.</span></span>

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="b6620-145">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="b6620-145">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="b6620-146">Если авторизации завершается успешно, возвращается страницы для просмотра документа.</span><span class="sxs-lookup"><span data-stu-id="b6620-146">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="b6620-147">Если происходит сбой авторизации, но пользователь прошел проверку подлинности, возвращая `ForbidResult` сообщает все по промежуточного слоя проверки подлинности, которое не удалось выполнить авторизацию.</span><span class="sxs-lookup"><span data-stu-id="b6620-147">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="b6620-148">Значение `ChallengeResult` возвращается, если необходимо выполнить проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="b6620-148">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="b6620-149">Для интерактивного обозревателя клиента он может подойти перенаправлять пользователя на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="b6620-149">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="b6620-150">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="b6620-150">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="b6620-151">Если авторизации завершается успешно, возвращается представление для документа.</span><span class="sxs-lookup"><span data-stu-id="b6620-151">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="b6620-152">В случае непрохождения авторизации возвращение `ChallengeResult` информирует любое по промежуточного слоя проверки подлинности, не удалось выполнить авторизацию, что по промежуточного слоя может принимать соответствующий ответ.</span><span class="sxs-lookup"><span data-stu-id="b6620-152">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="b6620-153">Соответствующий ответ может вернул код состояния 401 или 403.</span><span class="sxs-lookup"><span data-stu-id="b6620-153">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="b6620-154">Для интерактивного обозревателя клиента это может означать перенаправления пользователя на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="b6620-154">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

---
