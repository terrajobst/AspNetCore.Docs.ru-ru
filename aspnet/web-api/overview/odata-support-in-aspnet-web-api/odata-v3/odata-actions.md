---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: Корректирующие действия OData в веб-API 2 ASP.NET | Документация Майкрософт
author: MikeWasson
description: 'В OData действия предназначены для добавления поведений на стороне сервера, которые легко не определены как операций CRUD в объектах. Некоторые варианты применения действий включают: реализуйте...'
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837776"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a><span data-ttu-id="e3b39-104">Корректирующие действия OData в веб-API 2 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="e3b39-104">Supporting OData Actions in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="e3b39-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e3b39-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="e3b39-106">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="e3b39-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="e3b39-107">В OData *действия* позволяют добавлять поведения на стороне сервера, которые легко не определены как операций CRUD в объектах.</span><span class="sxs-lookup"><span data-stu-id="e3b39-107">In OData, *actions* are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="e3b39-108">Некоторые способы для действий:</span><span class="sxs-lookup"><span data-stu-id="e3b39-108">Some uses for actions include:</span></span>
> 
> - <span data-ttu-id="e3b39-109">Реализация сложных транзакциях.</span><span class="sxs-lookup"><span data-stu-id="e3b39-109">Implementing complex transactions.</span></span>
> - <span data-ttu-id="e3b39-110">Управление несколько сущностей за один раз.</span><span class="sxs-lookup"><span data-stu-id="e3b39-110">Manipulating several entities at once.</span></span>
> - <span data-ttu-id="e3b39-111">Разрешение обновлений только на определенные свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-111">Allowing updates only to certain properties of an entity.</span></span>
> - <span data-ttu-id="e3b39-112">Отправка данных на сервер, который не определен в сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-112">Sending information to the server that is not defined in an entity.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e3b39-113">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="e3b39-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e3b39-114">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="e3b39-114">Web API 2</span></span>
> - <span data-ttu-id="e3b39-115">OData версии 3</span><span class="sxs-lookup"><span data-stu-id="e3b39-115">OData Version 3</span></span>
> - <span data-ttu-id="e3b39-116">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e3b39-116">Entity Framework 6</span></span>


## <a name="example-rating-a-product"></a><span data-ttu-id="e3b39-117">Пример: Оценка продукта</span><span class="sxs-lookup"><span data-stu-id="e3b39-117">Example: Rating a Product</span></span>

<span data-ttu-id="e3b39-118">В этом примере мы хотим позволяют оценить продукты, а затем предоставить средние оценки для каждого продукта.</span><span class="sxs-lookup"><span data-stu-id="e3b39-118">In this example, we want to let users rate products, and then expose the average ratings for each product.</span></span> <span data-ttu-id="e3b39-119">В базе данных Мы сохраним список оценок, соответствующие продукты.</span><span class="sxs-lookup"><span data-stu-id="e3b39-119">On the database, we will store a list of ratings, keyed to products.</span></span>

<span data-ttu-id="e3b39-120">Ниже приведен модель, которую мы можем использовать для представления оценки в Entity Framework.</span><span class="sxs-lookup"><span data-stu-id="e3b39-120">Here is the model we might use to represent the ratings in Entity Framework:</span></span>

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

<span data-ttu-id="e3b39-121">Но мы не станем клиентам POST `ProductRating` объекта из коллекции «Оценки».</span><span class="sxs-lookup"><span data-stu-id="e3b39-121">But we don't want clients to POST a `ProductRating` object to a "Ratings" collection.</span></span> <span data-ttu-id="e3b39-122">Интуитивно оценка связан с коллекцией продуктов и клиент должен только необходимо отправить значение оценки.</span><span class="sxs-lookup"><span data-stu-id="e3b39-122">Intuitively, the rating is associated with the Products collection, and the client should only need to post the rating value.</span></span>

<span data-ttu-id="e3b39-123">Таким образом вместо того чтобы использовать обычные операции CRUD, мы определяем действие, которое клиент может вызывать по продукту.</span><span class="sxs-lookup"><span data-stu-id="e3b39-123">Therefore, instead of using the normal CRUD operations, we define an action that a client can invoke on a Product.</span></span> <span data-ttu-id="e3b39-124">В терминологии OData действии *привязан* для сущности продукта.</span><span class="sxs-lookup"><span data-stu-id="e3b39-124">In OData terminology, the action is *bound* to Product entities.</span></span>

><span data-ttu-id="e3b39-125">Действия имеют побочные эффекты на сервере.</span><span class="sxs-lookup"><span data-stu-id="e3b39-125">Actions have side-effects on the server.</span></span> <span data-ttu-id="e3b39-126">По этой причине они вызываются с помощью HTTP-запросы POST.</span><span class="sxs-lookup"><span data-stu-id="e3b39-126">For this reason, they are invoked using HTTP POST requests.</span></span> <span data-ttu-id="e3b39-127">Действия могут иметь параметры и возвращаемые типы, которые описаны в метаданных службы.</span><span class="sxs-lookup"><span data-stu-id="e3b39-127">Actions can have parameters and return types, which are described in the service metadata.</span></span> <span data-ttu-id="e3b39-128">Клиент отправляет параметры в тексте запроса, а сервер отправляет возвращаемое значение в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="e3b39-128">The client sends the parameters in the request body, and the server sends the return value in the response body.</span></span> <span data-ttu-id="e3b39-129">Для вызова действия «Скорость Product», клиент отправляет запрос POST на URI следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e3b39-129">To invoke the "Rate Product" action, the client sends a POST to a URI like the following:</span></span>

[!code-console[Main](odata-actions/samples/sample2.cmd)]

<span data-ttu-id="e3b39-130">Данные в запросе POST являются просто оценка продукта:</span><span class="sxs-lookup"><span data-stu-id="e3b39-130">The data in the POST request is simply the product rating:</span></span>

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a><span data-ttu-id="e3b39-131">Объявить действие в модели EDM</span><span class="sxs-lookup"><span data-stu-id="e3b39-131">Declare the Action in the Entity Data Model</span></span>

<span data-ttu-id="e3b39-132">В конфигурации веб-API добавьте действие в модель данных сущности (модель EDM):</span><span class="sxs-lookup"><span data-stu-id="e3b39-132">In your Web API configuration, add the action to the entity data model (EDM):</span></span>

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

<span data-ttu-id="e3b39-133">Этот код определяет «RateProduct» как действие, которое может быть выполнено в сущности продукта.</span><span class="sxs-lookup"><span data-stu-id="e3b39-133">This code defines "RateProduct" as an action that can be performed on Product entities.</span></span> <span data-ttu-id="e3b39-134">Он также объявляет, что действие принимает **int** параметр с именем «Оценка» и возвращает **int** значение.</span><span class="sxs-lookup"><span data-stu-id="e3b39-134">It also declares that the action takes an **int** parameter named "Rating", and returns an **int** value.</span></span>

## <a name="add-the-action-to-the-controller"></a><span data-ttu-id="e3b39-135">Добавление действия к контроллеру</span><span class="sxs-lookup"><span data-stu-id="e3b39-135">Add the Action to the Controller</span></span>

<span data-ttu-id="e3b39-136">Действие «RateProduct» привязан к сущности продукта.</span><span class="sxs-lookup"><span data-stu-id="e3b39-136">The "RateProduct" action is bound to Product entities.</span></span> <span data-ttu-id="e3b39-137">Чтобы реализовать это действие, добавьте метод с именем `RateProduct` к контроллеру продуктов:</span><span class="sxs-lookup"><span data-stu-id="e3b39-137">To implement the action, add a method named `RateProduct` to the Products controller:</span></span>

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

<span data-ttu-id="e3b39-138">Обратите внимание на то, что имя метода соответствует имени действия в модели EDM.</span><span class="sxs-lookup"><span data-stu-id="e3b39-138">Notice that the method name matches the name of the action in the EDM.</span></span> <span data-ttu-id="e3b39-139">Метод имеет два параметра:</span><span class="sxs-lookup"><span data-stu-id="e3b39-139">The method has two parameters:</span></span>

- <span data-ttu-id="e3b39-140">*ключ*: ключ продукта для частоты.</span><span class="sxs-lookup"><span data-stu-id="e3b39-140">*key*: The key for the product to rate.</span></span>
- <span data-ttu-id="e3b39-141">*Параметры*: словарь значений параметров действия.</span><span class="sxs-lookup"><span data-stu-id="e3b39-141">*parameters*: A dictionary of action parameter values.</span></span>

<span data-ttu-id="e3b39-142">Если вы используете соглашение о маршрутизации по умолчанию, параметра ключа необходимо присвоить имя «key».</span><span class="sxs-lookup"><span data-stu-id="e3b39-142">If you are using the default routing conventions, the key parameter must be named "key".</span></span> <span data-ttu-id="e3b39-143">Также важно включить **[FromOdataUri]** атрибута.</span><span class="sxs-lookup"><span data-stu-id="e3b39-143">It is also important to include the **[FromOdataUri]** attribute, as shown.</span></span> <span data-ttu-id="e3b39-144">Этот атрибут сообщает веб-API для использования правилами синтаксиса OData при синтаксическом анализе ключ из URI запроса.</span><span class="sxs-lookup"><span data-stu-id="e3b39-144">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

<span data-ttu-id="e3b39-145">Используйте *параметры* словаря для получения параметров действия:</span><span class="sxs-lookup"><span data-stu-id="e3b39-145">Use the *parameters* dictionary to get the action parameters:</span></span>

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

<span data-ttu-id="e3b39-146">Если клиент отправляет параметры действий в правильный формат, значение **ModelState.IsValid** имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="e3b39-146">If the client sends the action parameters in the correct format, the value of **ModelState.IsValid** is true.</span></span> <span data-ttu-id="e3b39-147">В этом случае можно использовать **ODataActionParameters** словаря, чтобы получить значения параметров.</span><span class="sxs-lookup"><span data-stu-id="e3b39-147">In that case, you can use the **ODataActionParameters** dictionary to get the parameter values.</span></span> <span data-ttu-id="e3b39-148">В этом примере `RateProduct` действие принимает один параметр с именем «Rating».</span><span class="sxs-lookup"><span data-stu-id="e3b39-148">In this example, the `RateProduct` action takes a single parameter named "Rating".</span></span>

## <a name="action-metadata"></a><span data-ttu-id="e3b39-149">Метаданные действия</span><span class="sxs-lookup"><span data-stu-id="e3b39-149">Action Metadata</span></span>

<span data-ttu-id="e3b39-150">Чтобы просмотреть метаданные службы, отправьте запрос GET к метаданным /odata/$.</span><span class="sxs-lookup"><span data-stu-id="e3b39-150">To view the service metadata, send a GET request to /odata/$metadata.</span></span> <span data-ttu-id="e3b39-151">Ниже приведен фрагмент метаданных, который объявляет `RateProduct` действия:</span><span class="sxs-lookup"><span data-stu-id="e3b39-151">Here is the portion of the metadata that declares the `RateProduct` action:</span></span>

[!code-xml[Main](odata-actions/samples/sample7.xml)]

<span data-ttu-id="e3b39-152">**FunctionImport** элемент объявляет действие.</span><span class="sxs-lookup"><span data-stu-id="e3b39-152">The **FunctionImport** element declares the action.</span></span> <span data-ttu-id="e3b39-153">Большинство полей говорят сами за себя, но два стоит обратить внимание:</span><span class="sxs-lookup"><span data-stu-id="e3b39-153">Most of the fields are self-explanatory, but two are worth noting:</span></span>

- <span data-ttu-id="e3b39-154">**Имеет значение IsBindable** означает, что действие может вызываться в целевой сущности, по крайней мере некоторое время.</span><span class="sxs-lookup"><span data-stu-id="e3b39-154">**IsBindable** means the action can be invoked on the target entity, at least some of the time.</span></span>
- <span data-ttu-id="e3b39-155">**IsAlwaysBindable** означает, что действие, всегда будут вызываться в целевой сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-155">**IsAlwaysBindable** means the action can always be invoked on the target entity.</span></span>

<span data-ttu-id="e3b39-156">Разница в том случае, что некоторые действия всегда будут доступны клиентам, но другие действия могут опираться на состояние сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-156">The difference is that some actions are always available to clients, but other actions might depend on the state of the entity.</span></span> <span data-ttu-id="e3b39-157">Например предположим, что вы определяете действие «Приобрести».</span><span class="sxs-lookup"><span data-stu-id="e3b39-157">For example, suppose you define a "Purchase" action.</span></span> <span data-ttu-id="e3b39-158">Можно приобретать только элемент, который находится на складе.</span><span class="sxs-lookup"><span data-stu-id="e3b39-158">You can only purchase an item that is in stock.</span></span> <span data-ttu-id="e3b39-159">Если элемент отсутствует на складе, клиент не может вызвать это действие.</span><span class="sxs-lookup"><span data-stu-id="e3b39-159">If the item is out of stock, a client cannot invoke that action.</span></span>

<span data-ttu-id="e3b39-160">При определении модели EDM **действие** метод создает всегда привязываемых действие:</span><span class="sxs-lookup"><span data-stu-id="e3b39-160">When you define the EDM, the **Action** method creates an always-bindable action:</span></span>

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

<span data-ttu-id="e3b39-161">Я буду говорить не всегда-возможностью привязки действия (также называется *временных* действия) Далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="e3b39-161">I'll talk about not-always-bindable actions (also called *transient* actions) later in this topic.</span></span>

## <a name="invoking-the-action"></a><span data-ttu-id="e3b39-162">Вызвать действие</span><span class="sxs-lookup"><span data-stu-id="e3b39-162">Invoking the Action</span></span>

<span data-ttu-id="e3b39-163">Теперь давайте посмотрим, каким образом клиент может вызывать это действие.</span><span class="sxs-lookup"><span data-stu-id="e3b39-163">Now let's see how a client would invoke this action.</span></span> <span data-ttu-id="e3b39-164">Предположим, что клиент хочет дать оценку 2 продукта с Идентификатором = 4.</span><span class="sxs-lookup"><span data-stu-id="e3b39-164">Suppose the client wants to give a rating of 2 to the product with ID = 4.</span></span> <span data-ttu-id="e3b39-165">Ниже приведен пример сообщения запроса, используя формат JSON в тексте запроса.</span><span class="sxs-lookup"><span data-stu-id="e3b39-165">Here is an example request message, using JSON format for the request body:</span></span>

[!code-console[Main](odata-actions/samples/sample9.cmd)]

<span data-ttu-id="e3b39-166">Ниже приведен ответного сообщения.</span><span class="sxs-lookup"><span data-stu-id="e3b39-166">Here is the response message:</span></span>

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a><span data-ttu-id="e3b39-167">Привязки действия к набору сущностей</span><span class="sxs-lookup"><span data-stu-id="e3b39-167">Binding an Action to an Entity Set</span></span>

<span data-ttu-id="e3b39-168">В предыдущем примере, действие привязано к одной сущности: клиент оценивает один продукт.</span><span class="sxs-lookup"><span data-stu-id="e3b39-168">In the previous example, the action is bound to a single entity: The client rates a single product.</span></span> <span data-ttu-id="e3b39-169">Можно также привязать действие к коллекции сущностей.</span><span class="sxs-lookup"><span data-stu-id="e3b39-169">You can also bind an action to a collection of entities.</span></span> <span data-ttu-id="e3b39-170">Просто внесите следующие изменения:</span><span class="sxs-lookup"><span data-stu-id="e3b39-170">Just make the following changes:</span></span>

<span data-ttu-id="e3b39-171">В модели EDM, добавьте действие в сущность **коллекции** свойство.</span><span class="sxs-lookup"><span data-stu-id="e3b39-171">In the EDM, add the action to the entity's **Collection** property.</span></span>

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

<span data-ttu-id="e3b39-172">В методе контроллера опустить *ключ* параметра.</span><span class="sxs-lookup"><span data-stu-id="e3b39-172">In the controller method, omit the *key* parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

<span data-ttu-id="e3b39-173">Теперь клиент вызывает действие для набора сущностей продуктов:</span><span class="sxs-lookup"><span data-stu-id="e3b39-173">Now the client invokes the action on the Products entity set:</span></span>

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a><span data-ttu-id="e3b39-174">Действия с параметрами коллекции</span><span class="sxs-lookup"><span data-stu-id="e3b39-174">Actions with Collection Parameters</span></span>

<span data-ttu-id="e3b39-175">У действий может быть параметров, которые принимают коллекцию значений.</span><span class="sxs-lookup"><span data-stu-id="e3b39-175">Actions can have parameters that take a collection of values.</span></span> <span data-ttu-id="e3b39-176">В модели EDM, использовать **CollectionParameter&lt;T&gt;**  для объявления параметра.</span><span class="sxs-lookup"><span data-stu-id="e3b39-176">In the EDM, use **CollectionParameter&lt;T&gt;** to declare the parameter.</span></span>

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

<span data-ttu-id="e3b39-177">Этот код объявляет параметр с именем «Оценки», который принимает коллекцию **int** значения.</span><span class="sxs-lookup"><span data-stu-id="e3b39-177">This declares a parameter named "Ratings" that takes a collection of **int** values.</span></span> <span data-ttu-id="e3b39-178">В методе контроллера по-прежнему получить значение параметра из **ODataActionParameters** объекта, но теперь значение **ICollection&lt;int&gt;**  значение:</span><span class="sxs-lookup"><span data-stu-id="e3b39-178">In the controller method, you still get the parameter value from the **ODataActionParameters** object, but now the value is an **ICollection&lt;int&gt;** value:</span></span>

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a><span data-ttu-id="e3b39-179">Временные действия</span><span class="sxs-lookup"><span data-stu-id="e3b39-179">Transient Actions</span></span>

<span data-ttu-id="e3b39-180">В примере «RateProduct» пользователи всегда можно оценить продукт, поэтому всегда доступно действие.</span><span class="sxs-lookup"><span data-stu-id="e3b39-180">In the "RateProduct" example, users can always rate a product, so the action is always available.</span></span> <span data-ttu-id="e3b39-181">Но некоторые действия зависят от состояния сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-181">But some actions depend on the state of the entity.</span></span> <span data-ttu-id="e3b39-182">Например в службе аренды видео, действие «Оформление заказа» не всегда доступен.</span><span class="sxs-lookup"><span data-stu-id="e3b39-182">For example, in a video rental service, the "CheckOut" action is not always available.</span></span> <span data-ttu-id="e3b39-183">(Оно зависит от доступен ли копия этого видео.) Этот тип действия вызывается *временных* действие.</span><span class="sxs-lookup"><span data-stu-id="e3b39-183">(It depends whether a copy of that video is available.) This type of action is called a *transient* action.</span></span>

<span data-ttu-id="e3b39-184">В метаданных службы, имеет временных действие **IsAlwaysBindable** равным false.</span><span class="sxs-lookup"><span data-stu-id="e3b39-184">In the service metadata, a transient action has **IsAlwaysBindable** equal to false.</span></span> <span data-ttu-id="e3b39-185">Это фактически значение по умолчанию, поэтому метаданные будут выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="e3b39-185">That's actually the default value, so the metadata will look like this:</span></span>

[!code-xml[Main](odata-actions/samples/sample16.xml)]

<span data-ttu-id="e3b39-186">Вот почему это важно: Если действие является временной, этот сервер должен сообщить клиенту, когда действие доступно.</span><span class="sxs-lookup"><span data-stu-id="e3b39-186">Here's why this matters: If an action is transient, the server needs to tell the client when the action is available.</span></span> <span data-ttu-id="e3b39-187">Это достигается посредством добавления ссылки к действию в сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-187">It does this by including a link to the action in the entity.</span></span> <span data-ttu-id="e3b39-188">Вот пример для сущности фильма:</span><span class="sxs-lookup"><span data-stu-id="e3b39-188">Here is an example for a Movie entity:</span></span>

[!code-console[Main](odata-actions/samples/sample17.cmd)]

<span data-ttu-id="e3b39-189">Свойство «#CheckOut» содержит ссылку на действие извлечения.</span><span class="sxs-lookup"><span data-stu-id="e3b39-189">The "#CheckOut" property contains a link to the CheckOut action.</span></span> <span data-ttu-id="e3b39-190">Если действие недоступно, сервер пропускает ссылку.</span><span class="sxs-lookup"><span data-stu-id="e3b39-190">If the action is not available, the server omits the link.</span></span>

<span data-ttu-id="e3b39-191">Для объявления временных действий в модели EDM, вызовите **TransientAction** метод:</span><span class="sxs-lookup"><span data-stu-id="e3b39-191">To declare a transient action in the EDM, call the **TransientAction** method:</span></span>

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

<span data-ttu-id="e3b39-192">Кроме того необходимо указать функцию, которая возвращает ссылку на действие для данной сущности.</span><span class="sxs-lookup"><span data-stu-id="e3b39-192">Also, you must provide a function that returns an action link for a given entity.</span></span> <span data-ttu-id="e3b39-193">Задать эту функцию, вызвав **HasActionLink**.</span><span class="sxs-lookup"><span data-stu-id="e3b39-193">Set this function by calling **HasActionLink**.</span></span> <span data-ttu-id="e3b39-194">Можно написать функцию, как лямбда-выражения:</span><span class="sxs-lookup"><span data-stu-id="e3b39-194">You can write the function as a lambda expression:</span></span>

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

<span data-ttu-id="e3b39-195">Если действие доступно, лямбда-выражение возвращает ссылку к действию.</span><span class="sxs-lookup"><span data-stu-id="e3b39-195">If the action is available, the lambda expression returns a link to the action.</span></span> <span data-ttu-id="e3b39-196">При сериализации сущности, сериализатор OData включает в себя эту ссылку.</span><span class="sxs-lookup"><span data-stu-id="e3b39-196">The OData serializer includes this link when it serializes the entity.</span></span> <span data-ttu-id="e3b39-197">Если действие недоступно, функция возвращает значение `null`.</span><span class="sxs-lookup"><span data-stu-id="e3b39-197">When the action is not available, the function returns `null`.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e3b39-198">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e3b39-198">Additional Resources</span></span>

[<span data-ttu-id="e3b39-199">Пример действий OData</span><span class="sxs-lookup"><span data-stu-id="e3b39-199">OData Actions Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
