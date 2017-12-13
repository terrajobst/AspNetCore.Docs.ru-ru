---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: "Поддержка действия OData в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: "В OData действий, способ добавления серверных поведения, которые легко не определены как операций CRUD в объектах. Некоторые варианты применения действия включают: реализуйте..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/25/2014
ms.topic: article
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: acb369ca8f1bab8d7cad14c15f46cfd44beb9fdd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="bb359-104">Поддержка действия OData в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="bb359-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="bb359-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="bb359-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="bb359-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="bb359-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="bb359-107">В OData *действия* позволяют добавить серверные поведения, которые легко не определены как операций CRUD в объектах.</span><span class="sxs-lookup"><span data-stu-id="bb359-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="bb359-108">Некоторые варианты применения действия включают:</span><span class="sxs-lookup"><span data-stu-id="bb359-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="bb359-109">Реализация сложных транзакциях.</span><span class="sxs-lookup"><span data-stu-id="bb359-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="bb359-110">Управление сразу несколько сущностей.</span><span class="sxs-lookup"><span data-stu-id="bb359-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="bb359-111">Разрешение обновлений только на определенные свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="bb359-112">Отправка сведений на сервер, который не определен в сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="bb359-113">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="bb359-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="bb359-114">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="bb359-114">Web API 2</span></span>
> - <span data-ttu-id="bb359-115">OData версии 3</span><span class="sxs-lookup"><span data-stu-id="bb359-115">OData Version 3</span></span>
> - <span data-ttu-id="bb359-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="bb359-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="bb359-117">Пример: Оценка продукта</span><span class="sxs-lookup"><span data-stu-id="bb359-117">Example: Rating a Product</span></span>

<span data-ttu-id="bb359-118">В этом примере мы хотим позволяют оценить продукты, а затем предоставить средние оценки для каждого продукта.</span><span class="sxs-lookup"><span data-stu-id="bb359-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="bb359-119">В базе данных сохраняется список оценок, соответствующие продукты.</span><span class="sxs-lookup"><span data-stu-id="bb359-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="bb359-120">Вот модели, который может использоваться для представления рейтинга в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="bb359-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="bb359-121">Но мы не хотим клиентам POST `ProductRating` объекта из коллекции «Оценки».</span><span class="sxs-lookup"><span data-stu-id="bb359-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="bb359-122">Интуитивно оценка связан с коллекцией продуктов и клиента необходимо только учет значение оценки.</span><span class="sxs-lookup"><span data-stu-id="bb359-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="bb359-123">Таким образом вместо обычных операций CRUD, действие, которое может вызывать клиент определяется по продукту.</span><span class="sxs-lookup"><span data-stu-id="bb359-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="bb359-124">В терминологии OData, такое действие было *привязан* для сущности продукта.</span><span class="sxs-lookup"><span data-stu-id="bb359-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="bb359-125">Действия имеют побочные эффекты на сервере.</span><span class="sxs-lookup"><span data-stu-id="bb359-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="bb359-126">По этой причине они вызываются с помощью HTTP-запросы POST.</span><span class="sxs-lookup"><span data-stu-id="bb359-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="bb359-127">Действия могут иметь параметры и возвращаемые типы, которые описаны в метаданных службы.</span><span class="sxs-lookup"><span data-stu-id="bb359-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="bb359-128">Клиент отправляет параметров в тексте запроса, а сервер отправляет возвращаемого значения в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="bb359-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="bb359-129">Для вызова действия «Частота продукт», клиент отправляет POST URI следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bb359-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="bb359-130">Данные в запросе POST — просто оценку продукта:</span><span class="sxs-lookup"><span data-stu-id="bb359-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="bb359-131">Объявить действие в модели EDM</span><span class="sxs-lookup"><span data-stu-id="bb359-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="bb359-132">Добавьте действие в настройку веб-API модели EDM (модель EDM):</span><span class="sxs-lookup"><span data-stu-id="bb359-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="bb359-133">Этот код определяет «RateProduct» как действие, которое может выполняться на сущностях продукта.</span><span class="sxs-lookup"><span data-stu-id="bb359-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="bb359-134">Он объявляет, что действие принимает **int** параметр с именем «Оценка» и возвращает **int** значение.</span><span class="sxs-lookup"><span data-stu-id="bb359-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="bb359-135">Добавить действие к контроллеру</span><span class="sxs-lookup"><span data-stu-id="bb359-135">Add the Action to the Controller</span></span>

<span data-ttu-id="bb359-136">Действие «RateProduct» привязан к сущности продукта.</span><span class="sxs-lookup"><span data-stu-id="bb359-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="bb359-137">Для выполнения действий, добавьте метод с именем `RateProduct` для контроллера Products:</span><span class="sxs-lookup"><span data-stu-id="bb359-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="bb359-138">Обратите внимание, что имя метода совпадает с именем действия в модели EDM.</span><span class="sxs-lookup"><span data-stu-id="bb359-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="bb359-139">Метод имеет два параметра:</span><span class="sxs-lookup"><span data-stu-id="bb359-139">The method has two parameters:</span></span>

- <span data-ttu-id="bb359-140">*ключ*: ключ продукта для частоты.</span><span class="sxs-lookup"><span data-stu-id="bb359-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="bb359-141">*Параметры*: словарь значений параметра действия.</span><span class="sxs-lookup"><span data-stu-id="bb359-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="bb359-142">При использовании соглашений о маршрутизации по умолчанию ключевой параметр должен иметь имя «key».</span><span class="sxs-lookup"><span data-stu-id="bb359-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="bb359-143">Также важно для включения **[FromOdataUri]** атрибута, как показано.</span><span class="sxs-lookup"><span data-stu-id="bb359-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="bb359-144">Этот атрибут сообщает веб-API для использования правилами синтаксиса OData при синтаксическом анализе ключ от URI запроса.</span><span class="sxs-lookup"><span data-stu-id="bb359-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="bb359-145">Используйте *параметры* словарь для получения параметров действия:</span><span class="sxs-lookup"><span data-stu-id="bb359-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="bb359-146">Если клиент отправляет параметров действия в правильный формат, значение **ModelState.IsValid** имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="bb359-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="bb359-147">В этом случае можно использовать **ODataActionParameters** словарь для получения значений параметров.</span><span class="sxs-lookup"><span data-stu-id="bb359-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="bb359-148">В этом примере `RateProduct` действие принимает один параметр с именем «Оценка».</span><span class="sxs-lookup"><span data-stu-id="bb359-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="bb359-149">Метаданные действия</span><span class="sxs-lookup"><span data-stu-id="bb359-149">Action Metadata</span></span>

<span data-ttu-id="bb359-150">Чтобы просмотреть метаданные службы, отправка запроса GET к метаданным /odata/$.</span><span class="sxs-lookup"><span data-stu-id="bb359-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="bb359-151">Вот часть метаданных, который объявляет `RateProduct` действия:</span><span class="sxs-lookup"><span data-stu-id="bb359-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="bb359-152">**FunctionImport** элемент объявляет действие.</span><span class="sxs-lookup"><span data-stu-id="bb359-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="bb359-153">Большинство полей говорят сами за себя, но два — Обратите внимание:</span><span class="sxs-lookup"><span data-stu-id="bb359-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="bb359-154">**Имеет значение IsBindable** означает действие может вызываться в целевой сущности по крайней мере некоторое время.</span><span class="sxs-lookup"><span data-stu-id="bb359-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="bb359-155">**IsAlwaysBindable** означает всегда можно вызвать действие в целевой сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="bb359-156">Разница заключается в некоторых действий, всегда будут доступны клиентам, что другие действия могут опираться на состояние сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="bb359-157">Например предположим, что определяется действие «Покупки».</span><span class="sxs-lookup"><span data-stu-id="bb359-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="bb359-158">Только вы можете приобрести элемента, который находится на складе.</span><span class="sxs-lookup"><span data-stu-id="bb359-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="bb359-159">Если элемент отсутствует на складе, клиент не может вызвать это действие.</span><span class="sxs-lookup"><span data-stu-id="bb359-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="bb359-160">При определении модели EDM, **действия** метод создает всегда привязываемых действие:</span><span class="sxs-lookup"><span data-stu-id="bb359-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="bb359-161">Мы поговорим о не всегда привязки действия (также называется *временные* действия) Далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="bb359-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="bb359-162">Вызвать действие</span><span class="sxs-lookup"><span data-stu-id="bb359-162">Invoking the Action</span></span>

<span data-ttu-id="bb359-163">Теперь давайте посмотрим, как клиент может вызвать это действие.</span><span class="sxs-lookup"><span data-stu-id="bb359-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="bb359-164">Предположим, что клиенту необходимо предоставить оценку 2 для продукта с Идентификатором = 4.</span><span class="sxs-lookup"><span data-stu-id="bb359-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="bb359-165">Ниже приведен пример сообщения запроса, с использованием формата JSON для текста запроса.</span><span class="sxs-lookup"><span data-stu-id="bb359-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="bb359-166">Вот ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="bb359-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="bb359-167">Привязка действие в наборе сущностей</span><span class="sxs-lookup"><span data-stu-id="bb359-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="bb359-168">В предыдущем примере действие привязано к одной сущности: клиент оценивает за единицу продукта.</span><span class="sxs-lookup"><span data-stu-id="bb359-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="bb359-169">Можно также привязать действие к коллекцию сущностей.</span><span class="sxs-lookup"><span data-stu-id="bb359-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="bb359-170">Сделать следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="bb359-170">Just make the following changes:</span></span>

<span data-ttu-id="bb359-171">Добавьте действие сущности в модели EDM, **коллекции** свойство.</span><span class="sxs-lookup"><span data-stu-id="bb359-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="bb359-172">В методе контроллера опустить *ключ* параметра.</span><span class="sxs-lookup"><span data-stu-id="bb359-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="bb359-173">Теперь клиент вызывает действие в наборе сущностей продуктов:</span><span class="sxs-lookup"><span data-stu-id="bb359-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="bb359-174">Действия с параметрами коллекции</span><span class="sxs-lookup"><span data-stu-id="bb359-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="bb359-175">Действия могут иметь параметры, которые принимают коллекцию значений.</span><span class="sxs-lookup"><span data-stu-id="bb359-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="bb359-176">В модели EDM, используйте **CollectionParameter&lt;T&gt;**  для объявления параметра.</span><span class="sxs-lookup"><span data-stu-id="bb359-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="bb359-177">Этот код объявляет параметр с именем «Оценки», который принимает коллекцию **int** значения.</span><span class="sxs-lookup"><span data-stu-id="bb359-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="bb359-178">В этом методе контроллера, по-прежнему получить значение параметра из **ODataActionParameters** объекта, но теперь значение **ICollection&lt;int&gt;**  значение:</span><span class="sxs-lookup"><span data-stu-id="bb359-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="bb359-179">Временные действия</span><span class="sxs-lookup"><span data-stu-id="bb359-179">Transient Actions</span></span>

<span data-ttu-id="bb359-180">В примере «RateProduct» пользователи всегда можно оценить продукт, действие всегда доступно.</span><span class="sxs-lookup"><span data-stu-id="bb359-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="bb359-181">Однако некоторые действия зависят от состояния сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="bb359-182">Например в службе видео аренды, действие «Оформление заказа» не всегда доступен.</span><span class="sxs-lookup"><span data-stu-id="bb359-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="bb359-183">(Оно зависит, доступен ли копия этого видео). Такие действия называются *временные* действие.</span><span class="sxs-lookup"><span data-stu-id="bb359-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="bb359-184">В метаданных службы, имеет временный действие **IsAlwaysBindable** равно false.</span><span class="sxs-lookup"><span data-stu-id="bb359-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="bb359-185">Это фактически значение по умолчанию, так что метаданных будет выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="bb359-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="bb359-186">Вот почему это важно: Если действие является временным, этот сервер должен сообщить клиенту, когда действие доступно.</span><span class="sxs-lookup"><span data-stu-id="bb359-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="bb359-187">Это делается посредством добавления ссылки к действию в сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="bb359-188">Ниже приведен пример для сущности фильма:</span><span class="sxs-lookup"><span data-stu-id="bb359-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="bb359-189">Свойство «#CheckOut» содержит ссылку на действие извлечения.</span><span class="sxs-lookup"><span data-stu-id="bb359-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="bb359-190">Если действие недоступно, сервер пропускает по ссылке.</span><span class="sxs-lookup"><span data-stu-id="bb359-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="bb359-191">Чтобы объявить временных действий в модели EDM, вызовите **TransientAction** метод:</span><span class="sxs-lookup"><span data-stu-id="bb359-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="bb359-192">Кроме того необходимо предоставить функцию, которая возвращает ссылку действия для данной сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="bb359-193">Задать эту функцию, вызвав **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="bb359-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="bb359-194">Можно написать функцию как лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="bb359-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="bb359-195">Если действие доступно, лямбда-выражение возвращает ссылку действия.</span><span class="sxs-lookup"><span data-stu-id="bb359-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="bb359-196">Сериализатор OData включает эта ссылка при сериализации сущности.</span><span class="sxs-lookup"><span data-stu-id="bb359-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="bb359-197">Если действие недоступно, функция возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="bb359-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bb359-198">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bb359-198">Additional Resources</span></span>

[<span data-ttu-id="bb359-199">Пример действий OData</span><span class="sxs-lookup"><span data-stu-id="bb359-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
