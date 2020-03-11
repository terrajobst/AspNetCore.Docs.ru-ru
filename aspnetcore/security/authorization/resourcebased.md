---
title: Авторизация на основе ресурсов в ASP.NET Core
author: scottaddie
description: Узнайте, как реализовать авторизацию на основе ресурсов в приложении ASP.NET Core, если атрибут авторизации не будет достаточным.
ms.author: scaddie
ms.custom: mvc
ms.date: 11/15/2018
uid: security/authorization/resourcebased
ms.openlocfilehash: 2be611c754583d996db7107f341b1be03cef73cf
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654064"
---
# <a name="resource-based-authorization-in-aspnet-core"></a><span data-ttu-id="fdd30-103">Авторизация на основе ресурсов в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="fdd30-103">Resource-based authorization in ASP.NET Core</span></span>

<span data-ttu-id="fdd30-104">Стратегия авторизации зависит от ресурса, к которому осуществляется доступ.</span><span class="sxs-lookup"><span data-stu-id="fdd30-104">Authorization strategy depends upon the resource being accessed.</span></span> <span data-ttu-id="fdd30-105">Рассмотрим документ со свойством Author.</span><span class="sxs-lookup"><span data-stu-id="fdd30-105">Consider a document that has an author property.</span></span> <span data-ttu-id="fdd30-106">Обновление документа разрешено только автору.</span><span class="sxs-lookup"><span data-stu-id="fdd30-106">Only the author is allowed to update the document.</span></span> <span data-ttu-id="fdd30-107">Следовательно, документ необходимо извлечь из хранилища данных, прежде чем может выполняться оценка авторизации.</span><span class="sxs-lookup"><span data-stu-id="fdd30-107">Consequently, the document must be retrieved from the data store before authorization evaluation can occur.</span></span>

<span data-ttu-id="fdd30-108">Оценка атрибутов выполняется перед привязкой данных и перед выполнением обработчика страницы или действия, которое загружает документ.</span><span class="sxs-lookup"><span data-stu-id="fdd30-108">Attribute evaluation occurs before data binding and before execution of the page handler or action that loads the document.</span></span> <span data-ttu-id="fdd30-109">По этим причинам декларативная авторизация с помощью атрибута `[Authorize]` недостаточна.</span><span class="sxs-lookup"><span data-stu-id="fdd30-109">For these reasons, declarative authorization with an `[Authorize]` attribute doesn't suffice.</span></span> <span data-ttu-id="fdd30-110">Вместо этого можно вызвать пользовательский метод авторизации&mdash;стиль, известный как *императивная авторизация*.</span><span class="sxs-lookup"><span data-stu-id="fdd30-110">Instead, you can invoke a custom authorization method&mdash;a style known as *imperative authorization*.</span></span>

::: moniker range=">= aspnetcore-3.0"
<span data-ttu-id="fdd30-111">[Просмотреть или скачать пример кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fdd30-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/3_0) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
<span data-ttu-id="fdd30-112">[Просмотреть или скачать пример кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fdd30-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/2_2) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
<span data-ttu-id="fdd30-113">[Просмотреть или скачать пример кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="fdd30-113">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/authorization/resourcebased/samples/1_1) ([how to download](xref:index#how-to-download-a-sample)).</span></span>
::: moniker-end

<span data-ttu-id="fdd30-114">[Создание приложения ASP.NET Core с данными пользователя, защищенными с помощью авторизации,](xref:security/authorization/secure-data) содержит пример приложения, использующего авторизацию на основе ресурсов.</span><span class="sxs-lookup"><span data-stu-id="fdd30-114">[Create an ASP.NET Core app with user data protected by authorization](xref:security/authorization/secure-data) contains a sample app that uses resource-based authorization.</span></span>

## <a name="use-imperative-authorization"></a><span data-ttu-id="fdd30-115">Использовать императивную авторизацию</span><span class="sxs-lookup"><span data-stu-id="fdd30-115">Use imperative authorization</span></span>

<span data-ttu-id="fdd30-116">Авторизация реализуется как служба [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) и регистрируется в коллекции служб в классе `Startup`.</span><span class="sxs-lookup"><span data-stu-id="fdd30-116">Authorization is implemented as an [IAuthorizationService](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizationservice) service and is registered in the service collection within the `Startup` class.</span></span> <span data-ttu-id="fdd30-117">Служба становится доступной посредством [внедрения зависимостей](xref:fundamentals/dependency-injection) в обработчики страниц или действия.</span><span class="sxs-lookup"><span data-stu-id="fdd30-117">The service is made available via [dependency injection](xref:fundamentals/dependency-injection) to page handlers or actions.</span></span>

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Controllers/DocumentController.cs?name=snippet_IAuthServiceDI&highlight=6)]

<span data-ttu-id="fdd30-118">в `IAuthorizationService` есть две перегрузки метода `AuthorizeAsync`: один принимает ресурс и имя политики, а другой принимает ресурс и список требований для вычисления.</span><span class="sxs-lookup"><span data-stu-id="fdd30-118">`IAuthorizationService` has two `AuthorizeAsync` method overloads: one accepting the resource and the policy name and the other accepting the resource and a list of requirements to evaluate.</span></span>

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

<span data-ttu-id="fdd30-119">В следующем примере ресурс, который требуется защитить, загружается в пользовательский объект `Document`.</span><span class="sxs-lookup"><span data-stu-id="fdd30-119">In the following example, the resource to be secured is loaded into a custom `Document` object.</span></span> <span data-ttu-id="fdd30-120">Вызывается перегрузка `AuthorizeAsync`, чтобы определить, разрешено ли текущему пользователю изменять предоставленный документ.</span><span class="sxs-lookup"><span data-stu-id="fdd30-120">An `AuthorizeAsync` overload is invoked to determine whether the current user is allowed to edit the provided document.</span></span> <span data-ttu-id="fdd30-121">Пользовательская политика авторизации "Едитполици" разбивается на решение.</span><span class="sxs-lookup"><span data-stu-id="fdd30-121">A custom "EditPolicy" authorization policy is factored into the decision.</span></span> <span data-ttu-id="fdd30-122">Дополнительные сведения о создании политик авторизации см. в разделе [Настраиваемая авторизация на основе политик](xref:security/authorization/policies) .</span><span class="sxs-lookup"><span data-stu-id="fdd30-122">See [Custom policy-based authorization](xref:security/authorization/policies) for more on creating authorization policies.</span></span>

> [!NOTE]
> <span data-ttu-id="fdd30-123">В следующих примерах кода предполагается, что проверка подлинности выполнена и задано свойство `User`.</span><span class="sxs-lookup"><span data-stu-id="fdd30-123">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/Edit.cshtml.cs?name=snippet_DocumentEditHandler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentEditAction)]

::: moniker-end

## <a name="write-a-resource-based-handler"></a><span data-ttu-id="fdd30-124">Написание обработчика на основе ресурсов</span><span class="sxs-lookup"><span data-stu-id="fdd30-124">Write a resource-based handler</span></span>

<span data-ttu-id="fdd30-125">Написание обработчика для авторизации на основе ресурсов не сильно отличается от [написания обработчика простых требований](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span><span class="sxs-lookup"><span data-stu-id="fdd30-125">Writing a handler for resource-based authorization isn't much different than [writing a plain requirements handler](xref:security/authorization/policies#security-authorization-policies-based-authorization-handler).</span></span> <span data-ttu-id="fdd30-126">Создайте настраиваемый класс требований и Реализуйте класс обработчика требований.</span><span class="sxs-lookup"><span data-stu-id="fdd30-126">Create a custom requirement class, and implement a requirement handler class.</span></span> <span data-ttu-id="fdd30-127">Дополнительные сведения о создании класса требований см. в разделе [требования](xref:security/authorization/policies#requirements).</span><span class="sxs-lookup"><span data-stu-id="fdd30-127">For more information on creating a requirement class, see [Requirements](xref:security/authorization/policies#requirements).</span></span>

<span data-ttu-id="fdd30-128">Класс обработчика задает как требование, так и тип ресурса.</span><span class="sxs-lookup"><span data-stu-id="fdd30-128">The handler class specifies both the requirement and resource type.</span></span> <span data-ttu-id="fdd30-129">Например, обработчик, использующий `SameAuthorRequirement` и `Document` ресурс, выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="fdd30-129">For example, a handler utilizing a `SameAuthorRequirement` and a `Document` resource follows:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationHandler.cs?name=snippet_HandlerAndRequirement)]

::: moniker-end

<span data-ttu-id="fdd30-130">В предыдущем примере Представьте, что `SameAuthorRequirement` является особым случаем для более универсального `SpecificAuthorRequirement` класса.</span><span class="sxs-lookup"><span data-stu-id="fdd30-130">In the preceding example, imagine that `SameAuthorRequirement` is a special case of a more generic `SpecificAuthorRequirement` class.</span></span> <span data-ttu-id="fdd30-131">Класс `SpecificAuthorRequirement` (не показан) содержит свойство `Name`, представляющее имя автора.</span><span class="sxs-lookup"><span data-stu-id="fdd30-131">The `SpecificAuthorRequirement` class (not shown) contains a `Name` property representing the name of the author.</span></span> <span data-ttu-id="fdd30-132">Свойству `Name` может быть присвоено значение "текущий пользователь".</span><span class="sxs-lookup"><span data-stu-id="fdd30-132">The `Name` property could be set to the current user.</span></span>

<span data-ttu-id="fdd30-133">Зарегистрируйте требование и обработчик в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="fdd30-133">Register the requirement and handler in `Startup.ConfigureServices`:</span></span>

::: moniker range=">= aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=4-8,10)]
::: moniker-end

 ::: moniker range=">= aspnetcore-2.0 < aspnetcore-3.0"
[!code-csharp[](resourcebased/samples/2_2/ResourceBasedAuthApp2/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

::: moniker range="<= aspnetcore-1.1"
[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Startup.cs?name=snippet_ConfigureServicesSample&highlight=3-7,9)]
::: moniker-end

### <a name="operational-requirements"></a><span data-ttu-id="fdd30-134">Требования к эксплуатации</span><span class="sxs-lookup"><span data-stu-id="fdd30-134">Operational requirements</span></span>

<span data-ttu-id="fdd30-135">Если вы принимаете решения на основе результатов операций CRUD (создание, чтение, обновление, удаление), используйте вспомогательный класс [оператионаусоризатионрекуиремент](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) .</span><span class="sxs-lookup"><span data-stu-id="fdd30-135">If you're making decisions based on the outcomes of CRUD (Create, Read, Update, Delete) operations, use the [OperationAuthorizationRequirement](/dotnet/api/microsoft.aspnetcore.authorization.infrastructure.operationauthorizationrequirement) helper class.</span></span> <span data-ttu-id="fdd30-136">Этот класс позволяет написать отдельный обработчик, а не отдельный класс для каждого типа операции.</span><span class="sxs-lookup"><span data-stu-id="fdd30-136">This class enables you to write a single handler instead of an individual class for each operation type.</span></span> <span data-ttu-id="fdd30-137">Чтобы использовать его, укажите некоторые имена операций:</span><span class="sxs-lookup"><span data-stu-id="fdd30-137">To use it, provide some operation names:</span></span>

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_OperationsClass)]

<span data-ttu-id="fdd30-138">Обработчик реализуется следующим образом: с помощью `OperationAuthorizationRequirement` требования и `Document` ресурса.</span><span class="sxs-lookup"><span data-stu-id="fdd30-138">The handler is implemented as follows, using an `OperationAuthorizationRequirement` requirement and a `Document` resource:</span></span>

 ::: moniker range=">= aspnetcore-2.0"
[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Services/DocumentAuthorizationCrudHandler.cs?name=snippet_Handler)]

::: moniker-end

<span data-ttu-id="fdd30-139">Предыдущий обработчик проверяет операцию с помощью ресурса, удостоверения пользователя и свойства `Name` требования.</span><span class="sxs-lookup"><span data-stu-id="fdd30-139">The preceding handler validates the operation using the resource, the user's identity, and the requirement's `Name` property.</span></span>

## <a name="challenge-and-forbid-with-an-operational-resource-handler"></a><span data-ttu-id="fdd30-140">Запрос и запрет с помощью обработчика рабочих ресурсов</span><span class="sxs-lookup"><span data-stu-id="fdd30-140">Challenge and forbid with an operational resource handler</span></span>

<span data-ttu-id="fdd30-141">В этом разделе показано, как обрабатываются результаты запроса и запрета действий, а также методы и запрета различий.</span><span class="sxs-lookup"><span data-stu-id="fdd30-141">This section shows how the challenge and forbid action results are processed and how challenge and forbid differ.</span></span>

<span data-ttu-id="fdd30-142">Чтобы вызвать обработчик рабочих ресурсов, укажите операцию при вызове `AuthorizeAsync` в обработчике страницы или в действии.</span><span class="sxs-lookup"><span data-stu-id="fdd30-142">To call an operational resource handler, specify the operation when invoking `AuthorizeAsync` in your page handler or action.</span></span> <span data-ttu-id="fdd30-143">В следующем примере определяется, разрешено ли пользователю, прошедшему проверку подлинности, просматривать предоставленный документ.</span><span class="sxs-lookup"><span data-stu-id="fdd30-143">The following example determines whether the authenticated user is permitted to view the provided document.</span></span>

> [!NOTE]
> <span data-ttu-id="fdd30-144">В следующих примерах кода предполагается, что проверка подлинности выполнена и задано свойство `User`.</span><span class="sxs-lookup"><span data-stu-id="fdd30-144">The following code samples assume authentication has run and set the `User` property.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](resourcebased/samples/3_0/ResourceBasedAuthApp2/Pages/Document/View.cshtml.cs?name=snippet_DocumentViewHandler&highlight=10-11)]

<span data-ttu-id="fdd30-145">Если авторизация выполнена, возвращается страница для просмотра документа.</span><span class="sxs-lookup"><span data-stu-id="fdd30-145">If authorization succeeds, the page for viewing the document is returned.</span></span> <span data-ttu-id="fdd30-146">Если авторизация не выполняется, но пользователь прошел проверку подлинности, возвращает `ForbidResult` уведомляет любое промежуточное по проверки подлинности об ошибке авторизации.</span><span class="sxs-lookup"><span data-stu-id="fdd30-146">If authorization fails but the user is authenticated, returning `ForbidResult` informs any authentication middleware that authorization failed.</span></span> <span data-ttu-id="fdd30-147">Если необходимо выполнить проверку подлинности, возвращается `ChallengeResult`.</span><span class="sxs-lookup"><span data-stu-id="fdd30-147">A `ChallengeResult` is returned when authentication must be performed.</span></span> <span data-ttu-id="fdd30-148">Для интерактивных клиентов браузера может быть целесообразно перенаправить пользователя на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="fdd30-148">For interactive browser clients, it may be appropriate to redirect the user to a login page.</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

[!code-csharp[](resourcebased/samples/1_1/ResourceBasedAuthApp1/Controllers/DocumentController.cs?name=snippet_DocumentViewAction&highlight=11-12)]

<span data-ttu-id="fdd30-149">Если авторизация выполнена, возвращается представление документа.</span><span class="sxs-lookup"><span data-stu-id="fdd30-149">If authorization succeeds, the view for the document is returned.</span></span> <span data-ttu-id="fdd30-150">Если авторизация завершается неудачно, возвращается `ChallengeResult` уведомляет по промежуточного слоя проверки подлинности, что ошибка авторизации, и по промежуточного слоя может принять соответствующий ответ.</span><span class="sxs-lookup"><span data-stu-id="fdd30-150">If authorization fails, returning `ChallengeResult` informs any authentication middleware that authorization failed, and the middleware can take the appropriate response.</span></span> <span data-ttu-id="fdd30-151">Соответствующий ответ может возвращать код состояния 401 или 403.</span><span class="sxs-lookup"><span data-stu-id="fdd30-151">An appropriate response could be returning a 401 or 403 status code.</span></span> <span data-ttu-id="fdd30-152">Для интерактивных клиентов браузера это может означать перенаправление пользователя на страницу входа.</span><span class="sxs-lookup"><span data-stu-id="fdd30-152">For interactive browser clients, it could mean redirecting the user to a login page.</span></span>

::: moniker-end
