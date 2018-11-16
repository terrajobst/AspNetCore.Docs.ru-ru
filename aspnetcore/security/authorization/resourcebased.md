---
title: Авторизация на основе ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как реализовать авторизацию на основе ресурсов в приложениях ASP.NET Core при атрибут Authorize будет недостаточно.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 6a163caa26b277fbee6b9d61f8f1d16a60c75b03
ms.sourcegitcommit: d3392f688cfebc1f25616da7489664d69c6ee330
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/16/2018
ms.locfileid: "51818373"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="8572a-103">Авторизация на основе ресурсов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8572a-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="8572a-104">Стратегии авторизации зависит от ресурса к которому выполняется доступ.</span><span class="sxs-lookup"><span data-stu-id="8572a-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="8572a-105">Рассмотрим, имеющему свойство автора документа.</span><span class="sxs-lookup"><span data-stu-id="8572a-105">Consider a document that has an author property.</span></span> <span data-ttu-id="8572a-106">Только автор может обновлять документа.</span><span class="sxs-lookup"><span data-stu-id="8572a-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="8572a-107">Следовательно документа должны извлекаться из хранилища данных для оценки авторизации.</span><span class="sxs-lookup"><span data-stu-id="8572a-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="8572a-108">Атрибут оценивается перед привязкой данных и перед выполнением обработчика страницы или действие, которое загружает документ.</span><span class="sxs-lookup"><span data-stu-id="8572a-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="8572a-109">По этим причинам декларативной авторизации с помощью `[Authorize]` атрибут недостаточно.</span><span class="sxs-lookup"><span data-stu-id="8572a-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="8572a-110">Вместо этого можно вызвать метод настраиваемой авторизации&mdash;стиль, известный как *принудительной авторизации*.</span><span class="sxs-lookup"><span data-stu-id="8572a-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

<span data-ttu-id="8572a-111">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="8572a-111">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="8572a-112">[Создание приложения ASP.NET Core с помощью данных пользователя с помощью авторизации](xref:security/authorization/secure-data) содержит пример приложения, использующего авторизации на основе ресурсов.</span><span class="sxs-lookup"><span data-stu-id="8572a-112">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="8572a-113">Использование принудительной авторизации</span><span class="sxs-lookup"><span data-stu-id="8572a-113">Use imperative authorization</span></span>

<span data-ttu-id="8572a-114">Авторизации реализуется как [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) службы и регистрируется в службе коллекции в течение `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="8572a-114">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="8572a-115">Сделан доступным через службу [внедрения зависимостей](xref:fundamentals/dependency-injection) обработчики страницы или действий.</span><span class="sxs-lookup"><span data-stu-id="8572a-115">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="8572a-116">`IAuthorizationService` имеет два `AuthorizeAsync` перегрузок метода: один принимает ресурс и имя политики, а другой принимает ресурс и список требований для оценки.</span><span class="sxs-lookup"><span data-stu-id="8572a-116">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="8572a-117">В следующем примере ресурса должна быть обеспечена безопасность загружается в пользовательский `Document` объекта.</span><span class="sxs-lookup"><span data-stu-id="8572a-117">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="8572a-118">`AuthorizeAsync` Перегруженный метод вызывается, чтобы определить, разрешено ли текущий пользователь изменять указанный документ.</span><span class="sxs-lookup"><span data-stu-id="8572a-118">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="8572a-119">Пользовательские политики авторизации «EditPolicy» добавляется в решение.</span><span class="sxs-lookup"><span data-stu-id="8572a-119">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="8572a-120">См. в разделе [авторизации на основе политик — Custom](xref:security/authorization/policies) Дополнительные сведения о создании политик авторизации.</span><span class="sxs-lookup"><span data-stu-id="8572a-120">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="8572a-121">Следующий код в приведенных примерах предполагается, выполнена проверка подлинности и набор `User` свойство.</span><span class="sxs-lookup"><span data-stu-id="8572a-121">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="8572a-122">Написание обработчика, на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="8572a-122">Write a resource-based handler</span></span>

<span data-ttu-id="8572a-123">Написание обработчика для авторизации на основе ресурсов не сильно отличается от [написание обработчика plain требования](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="8572a-123">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="8572a-124">Создание класса пользовательского требования и реализовать класс обработчика требование.</span><span class="sxs-lookup"><span data-stu-id="8572a-124">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="8572a-125">Дополнительные сведения о создании класс требования, см. в разделе [требования](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="8572a-125">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="8572a-126">Класс обработчика задает требование и тип ресурса.</span><span class="sxs-lookup"><span data-stu-id="8572a-126">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="8572a-127">Например, обработчик использование `SameAuthorRequirement` и `Document` ресурсов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="8572a-127">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="8572a-128">Представим, что в приведенном выше примере `SameAuthorRequirement` является особым случаем более универсальной `SpecificAuthorRequirement` класса.</span><span class="sxs-lookup"><span data-stu-id="8572a-128">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="8572a-129">`SpecificAuthorRequirement` Класс (не показано) содержит `Name` свойство, представляющее имя автора.</span><span class="sxs-lookup"><span data-stu-id="8572a-129">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="8572a-130">`Name` Удалось задать свойство для текущего пользователя.</span><span class="sxs-lookup"><span data-stu-id="8572a-130">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="8572a-131">Зарегистрируйте требование и обработчик в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="8572a-131">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]

### <a name="operational-requirements"></a><span data-ttu-id="8572a-132">Эксплуатационные требования</span><span class="sxs-lookup"><span data-stu-id="8572a-132">Operational requirements</span></span>

<span data-ttu-id="8572a-133">При создании решения, основываясь на результаты операции CRUD (Create, Read, Update, Delete), используйте [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) вспомогательный класс.</span><span class="sxs-lookup"><span data-stu-id="8572a-133">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="8572a-134">Этот класс позволяет написать один обработчик вместо отдельный класс для каждого типа операции.</span><span class="sxs-lookup"><span data-stu-id="8572a-134">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="8572a-135">Чтобы использовать его, укажите имена некоторых операций:</span><span class="sxs-lookup"><span data-stu-id="8572a-135">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="8572a-136">Обработчик реализуется следующим образом, с помощью `OperationAuthorizationRequirement` требование и `Document` ресурсов:</span><span class="sxs-lookup"><span data-stu-id="8572a-136">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="8572a-137">Предыдущий обработчик проверяет операцию, используя ресурс, удостоверение пользователя и требование `Name` свойство.</span><span class="sxs-lookup"><span data-stu-id="8572a-137">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

<span data-ttu-id="8572a-138">Для вызова обработчика оперативной ресурсов, указать операцию, при вызове `AuthorizeAsync` в обработчик страницы или действия.</span><span class="sxs-lookup"><span data-stu-id="8572a-138">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="8572a-139">В следующем примере определяется, разрешено ли прошедшего проверку подлинности пользователя для просмотра предоставленного документа.</span><span class="sxs-lookup"><span data-stu-id="8572a-139">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="8572a-140">Следующий код в приведенных примерах предполагается, выполнена проверка подлинности и набор `User` свойство.</span><span class="sxs-lookup"><span data-stu-id="8572a-140">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="8572a-141">Если авторизация выполняется успешно, возвращается на страницу для просмотра документа.</span><span class="sxs-lookup"><span data-stu-id="8572a-141">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="8572a-142">Если происходит сбой авторизации, но пользователь проходит проверку подлинности, возвращая `ForbidResult` информирует любое по промежуточного слоя проверки подлинности, который не удалось выполнить авторизацию.</span><span class="sxs-lookup"><span data-stu-id="8572a-142">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="8572a-143">Объект `ChallengeResult` возвращается, если необходимо выполнить проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="8572a-143">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="8572a-144">Для интерактивного обозревателя клиента может потребоваться перенаправить пользователя на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="8572a-144">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="8572a-145">Если авторизация выполняется успешно, возвращается представление для документа.</span><span class="sxs-lookup"><span data-stu-id="8572a-145">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="8572a-146">Если авторизация заканчивается неудачей, возвращая `ChallengeResult` информирует любое по промежуточного слоя проверки подлинности, что не удалось выполнить авторизацию, и по промежуточного слоя может принимать ответные действия.</span><span class="sxs-lookup"><span data-stu-id="8572a-146">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="8572a-147">Код состояния 401 или 403 может возвращаться соответствующий ответ.</span><span class="sxs-lookup"><span data-stu-id="8572a-147">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="8572a-148">Для интерактивного обозревателя клиента это может означать перенаправления пользователя на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="8572a-148">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
