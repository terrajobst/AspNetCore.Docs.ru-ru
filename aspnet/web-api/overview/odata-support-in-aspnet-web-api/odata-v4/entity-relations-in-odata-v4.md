---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
title: Отношения сущностей в OData v4 с помощью ASP.NET Web API 2.2 | Документация Майкрософт
author: MikeWasson
description: 'В большинстве наборов данных определения отношений между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты, имеют поставщики. С помощью OData, клиенты могут переходить по...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2014
ms.topic: article
ms.assetid: 72657550-ec09-4779-9bfc-2fb15ecd51c7
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/entity-relations-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 56cadbabaa7ca64ba39232495e25178849d5e3c2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377650"
---
<a name="entity-relations-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="e6ed4-104">Отношения сущностей в OData v4 с помощью ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="e6ed4-104">Entity Relations in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="e6ed4-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="e6ed4-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="e6ed4-106">В большинстве наборов данных определения отношений между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты, имеют поставщики.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-106">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="e6ed4-107">С помощью OData, клиенты можно переходить через отношения сущности.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-107">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="e6ed4-108">Учитывая продукта, можно найти поставщика.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-108">Given a product, you can find the supplier.</span></span> <span data-ttu-id="e6ed4-109">Также можно создать или удалить связи.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-109">You can also create or remove relationships.</span></span> <span data-ttu-id="e6ed4-110">Например можно задать поставщик для продукта.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-110">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="e6ed4-111">Этом руководстве показано, как для поддержки этих операций в OData v4, с помощью веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-111">This tutorial shows how to support these operations in OData v4 using ASP.NET Web API.</span></span> <span data-ttu-id="e6ed4-112">Учебном курсе руководство [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e6ed4-112">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e6ed4-113">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="e6ed4-113">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e6ed4-114">Веб-API 2.1</span><span class="sxs-lookup"><span data-stu-id="e6ed4-114">Web API 2.1</span></span>
> - <span data-ttu-id="e6ed4-115">OData v4</span><span class="sxs-lookup"><span data-stu-id="e6ed4-115">OData v4</span></span>
> - [<span data-ttu-id="e6ed4-116">Visual Studio 2013 с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="e6ed4-116">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="e6ed4-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="e6ed4-117">Entity Framework 6</span></span>
> - <span data-ttu-id="e6ed4-118">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="e6ed4-118">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="e6ed4-119">Учебника по версии</span><span class="sxs-lookup"><span data-stu-id="e6ed4-119">Tutorial versions</span></span>
> 
> <span data-ttu-id="e6ed4-120">OData версии 3, см. в разделе [поддержка отношений сущностей в OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span><span class="sxs-lookup"><span data-stu-id="e6ed4-120">For the OData Version 3, see [Supporting Entity Relations in OData v3](https://asp.net/web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations).</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="e6ed4-121">Добавление сущности Supplier</span><span class="sxs-lookup"><span data-stu-id="e6ed4-121">Add a Supplier Entity</span></span>

> [!NOTE]
> <span data-ttu-id="e6ed4-122">Учебном курсе руководство [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="e6ed4-122">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md).</span></span>


<span data-ttu-id="e6ed4-123">Во-первых мы должны связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-123">First, we need a related entity.</span></span> <span data-ttu-id="e6ed4-124">Добавьте класс с именем `Supplier` в папку Models.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-124">Add a class named `Supplier` in the Models folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample1.cs)]

<span data-ttu-id="e6ed4-125">Добавьте свойство навигации, чтобы `Product` класса:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-125">Add a navigation property to the `Product` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample2.cs?highlight=13-15)]

<span data-ttu-id="e6ed4-126">Добавьте новый **DbSet** для `ProductsContext` класса, таким образом, чтобы платформа Entity Framework будет включать в таблице поставщиков в базе данных.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-126">Add a new **DbSet** to the `ProductsContext` class, so that Entity Framework will include the Supplier table in the database.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample3.cs?highlight=10)]

<span data-ttu-id="e6ed4-127">В файле WebApiConfig.cs добавьте &quot;поставщики&quot; набора сущностей в модели EDM:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-127">In WebApiConfig.cs, add a &quot;Suppliers&quot; entity set to the entity data model:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample4.cs?highlight=6)]

## <a name="add-a-suppliers-controller"></a><span data-ttu-id="e6ed4-128">Добавить контроллер поставщики</span><span class="sxs-lookup"><span data-stu-id="e6ed4-128">Add a Suppliers Controller</span></span>

<span data-ttu-id="e6ed4-129">Добавление `SuppliersController` класс в папку "контроллеры".</span><span class="sxs-lookup"><span data-stu-id="e6ed4-129">Add a `SuppliersController` class to the Controllers folder.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="e6ed4-130">Я не будет показано, как добавлять операции CRUD для этого контроллера.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-130">I won't show how to add CRUD operations for this controller.</span></span> <span data-ttu-id="e6ed4-131">Действия не отличаются от контроллера Products (см. в разделе [Создайте конечную точку OData v4](create-an-odata-v4-endpoint.md)).</span><span class="sxs-lookup"><span data-stu-id="e6ed4-131">The steps are the same as for the Products controller (see [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md)).</span></span>

## <a name="getting-related-entities"></a><span data-ttu-id="e6ed4-132">Получение связанных сущностей</span><span class="sxs-lookup"><span data-stu-id="e6ed4-132">Getting Related Entities</span></span>

<span data-ttu-id="e6ed4-133">Чтобы получить поставщик для продукта, клиент отправляет запрос GET:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-133">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample6.cmd)]

<span data-ttu-id="e6ed4-134">Чтобы этот запрос в службу поддержки, добавьте следующий метод в `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-134">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample7.cs)]

<span data-ttu-id="e6ed4-135">Этот метод использует соглашение об именовании по умолчанию</span><span class="sxs-lookup"><span data-stu-id="e6ed4-135">This method uses a default naming convention</span></span>

- <span data-ttu-id="e6ed4-136">Имя метода: GetX, где X — свойство навигации.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-136">Method name: GetX, where X is the navigation property.</span></span>
- <span data-ttu-id="e6ed4-137">Имя параметра: *ключ*</span><span class="sxs-lookup"><span data-stu-id="e6ed4-137">Parameter name: *key*</span></span>

<span data-ttu-id="e6ed4-138">Если следовать соглашению по именованию, веб-API автоматически сопоставляет HTTP-запроса для метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-138">If you follow this naming convention, Web API automatically maps the HTTP request to the controller method.</span></span>

<span data-ttu-id="e6ed4-139">Пример HTTP-запроса:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-139">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample8.cmd)]

<span data-ttu-id="e6ed4-140">Пример ответа HTTP:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-140">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample9.cmd)]

### <a name="getting-a-related-collection"></a><span data-ttu-id="e6ed4-141">Получение связанной коллекции</span><span class="sxs-lookup"><span data-stu-id="e6ed4-141">Getting a related collection</span></span>

<span data-ttu-id="e6ed4-142">В предыдущем примере продукт имеет один поставщик.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-142">In the previous example, a product has one supplier.</span></span> <span data-ttu-id="e6ed4-143">Свойство навигации может также возвращать коллекцию.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-143">A navigation property can also return a collection.</span></span> <span data-ttu-id="e6ed4-144">Следующий код получает продукты для поставщика:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-144">The following code gets the products for a supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample10.cs)]

<span data-ttu-id="e6ed4-145">В этом случае метод возвращает **IQueryable** вместо **— SingleResult&lt;T&gt;**</span><span class="sxs-lookup"><span data-stu-id="e6ed4-145">In this case, the method returns an **IQueryable** instead of a **SingleResult&lt;T&gt;**</span></span>

<span data-ttu-id="e6ed4-146">Пример HTTP-запроса:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-146">Example HTTP request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample11.cmd)]

<span data-ttu-id="e6ed4-147">Пример ответа HTTP:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-147">Example HTTP response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample12.cmd)]

## <a name="creating-a-relationship-between-entities"></a><span data-ttu-id="e6ed4-148">Создание связи между сущностями</span><span class="sxs-lookup"><span data-stu-id="e6ed4-148">Creating a Relationship Between Entities</span></span>

<span data-ttu-id="e6ed4-149">OData поддерживает создание или удаление связи между двумя существующими сущностями.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-149">OData supports creating or removing relationships between two existing entities.</span></span> <span data-ttu-id="e6ed4-150">В терминологии OData v4, связь является &quot;ссылку&quot;.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-150">In OData v4 terminology, the relationship is a &quot;reference&quot;.</span></span> <span data-ttu-id="e6ed4-151">(OData v3, связь была вызвана *ссылку*.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-151">(In OData v3, the relationship was called a *link*.</span></span> <span data-ttu-id="e6ed4-152">Различия протокола не имеет значения для этого руководства.)</span><span class="sxs-lookup"><span data-stu-id="e6ed4-152">The protocol differences don't matter for this tutorial.)</span></span>

<span data-ttu-id="e6ed4-153">Ссылка имеет свой собственный URI с формой `/Entity/NavigationProperty/$ref`.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-153">A reference has its own URI, with the form `/Entity/NavigationProperty/$ref`.</span></span> <span data-ttu-id="e6ed4-154">Например вот URI для адресации ссылки между продуктом и его поставщиком:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-154">For example, here is the URI to address the reference between a product and its supplier:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample13.cmd)]

<span data-ttu-id="e6ed4-155">Добавление отношения, клиент отправляет запрос POST или PUT по этому адресу.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-155">To add a relationship, the client sends a POST or PUT request to this address.</span></span>

- <span data-ttu-id="e6ed4-156">ПОМЕСТИТЬ, если свойство навигации является единственной сущностью, такой как `Product.Supplier`.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-156">PUT if the navigation property is a single entity, such as `Product.Supplier`.</span></span>
- <span data-ttu-id="e6ed4-157">Учет, если свойство навигации является коллекцией, такие как `Supplier.Products`.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-157">POST if the navigation property is a collection, such as `Supplier.Products`.</span></span>

<span data-ttu-id="e6ed4-158">Текст запроса содержит идентификатор URI сущности, в связи.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-158">The body of the request contains the URI of the other entity in the relation.</span></span> <span data-ttu-id="e6ed4-159">Ниже приведен пример запроса:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-159">Here is an example request:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample14.cmd)]

<span data-ttu-id="e6ed4-160">В этом примере клиент отправляет запрос PUT к `/Products(6)/Supplier/$ref`, который представляет собой $ref URI `Supplier` продукта с Идентификатором = 6.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-160">In this example, the client sends a PUT request to `/Products(6)/Supplier/$ref`, which is the $ref URI for the `Supplier` of the product with ID = 6.</span></span> <span data-ttu-id="e6ed4-161">Если запрос выполнен успешно, сервер отправляет ответа 204 (нет содержимого):</span><span class="sxs-lookup"><span data-stu-id="e6ed4-161">If the request succeeds, the server sends a 204 (No Content) response:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample15.cmd)]

<span data-ttu-id="e6ed4-162">Ниже приведен метод контроллера, чтобы добавить отношение к `Product`:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-162">Here is the controller method to add a relationship to a `Product`:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample16.cs)]

<span data-ttu-id="e6ed4-163">*NavigationProperty* параметр указывает, какие связи для задания.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-163">The *navigationProperty* parameter specifies which relationship to set.</span></span> <span data-ttu-id="e6ed4-164">(Если имеется более одного свойства навигации в сущности, можно добавить несколько `case` инструкций.)</span><span class="sxs-lookup"><span data-stu-id="e6ed4-164">(If there is more than one navigation property on the entity, you can add more `case` statements.)</span></span>

<span data-ttu-id="e6ed4-165">*Ссылку* параметр содержит URI поставщика.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-165">The *link* parameter contains the URI of the supplier.</span></span> <span data-ttu-id="e6ed4-166">Веб-API автоматически выполняет синтаксический анализ текста запроса для получения значения для этого параметра.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-166">Web API automatically parses the request body to get the value for this parameter.</span></span>

<span data-ttu-id="e6ed4-167">Чтобы найти поставщика, нам нужен идентификатор (или ключ), который является частью *ссылку* параметра.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-167">To look up the supplier, we need the ID (or key), which is part of the *link* parameter.</span></span> <span data-ttu-id="e6ed4-168">Чтобы сделать это, используйте следующий вспомогательный метод:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-168">To do this, use the following helper method:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample17.cs)]

<span data-ttu-id="e6ed4-169">По сути этот метод использует библиотеку OData разбить на сегменты пути URI, найти сегмент, который содержит ключ и преобразовать его в правильный тип.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-169">Basically, this method uses the OData library to split the URI path into segments, find the segment that contains the key, and convert the key into the correct type.</span></span>

## <a name="deleting-a-relationship-between-entities"></a><span data-ttu-id="e6ed4-170">Удаление связи между сущностями</span><span class="sxs-lookup"><span data-stu-id="e6ed4-170">Deleting a Relationship Between Entities</span></span>

<span data-ttu-id="e6ed4-171">Чтобы удалить связь, клиент отправляет запрос HTTP DELETE к $ref URI:</span><span class="sxs-lookup"><span data-stu-id="e6ed4-171">To delete a relationship, the client sends an HTTP DELETE request to the $ref URI:</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample18.cmd)]

<span data-ttu-id="e6ed4-172">Ниже приведен метод контроллера, удаление связи между продуктом и других поставщиков.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-172">Here is the controller method to delete the relationship between a Product and a Supplier:</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample19.cs)]

<span data-ttu-id="e6ed4-173">В этом случае `Product.Supplier` — &quot;1&quot; конец отношения 1-ко многим, чтобы вы могли удалить связи, просто задав `Product.Supplier` для `null`.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-173">In this case, `Product.Supplier` is the &quot;1&quot; end of a 1-to-many relation, so you can remove the relationship just by setting `Product.Supplier` to `null`.</span></span>

<span data-ttu-id="e6ed4-174">В &quot;многих&quot; конечную точку связи клиента необходимо указать, какие связанные сущности для удаления.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-174">In the &quot;many&quot; end of a relationship, the client must specify which related entity to remove.</span></span> <span data-ttu-id="e6ed4-175">Чтобы сделать это, клиент отправляет URI связанной сущности в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-175">To do so, the client sends the URI of the related entity in the query string of the request.</span></span> <span data-ttu-id="e6ed4-176">Например, чтобы удалить «Product 1» из «Поставщик 1":</span><span class="sxs-lookup"><span data-stu-id="e6ed4-176">For example, to remove "Product 1" from "Supplier 1":</span></span>

[!code-console[Main](entity-relations-in-odata-v4/samples/sample20.cmd?highlight=1)]

<span data-ttu-id="e6ed4-177">Для этого в веб-API, нам нужно включать дополнительный параметр в `DeleteRef` метод.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-177">To support this in Web API, we need to include an extra parameter in the `DeleteRef` method.</span></span> <span data-ttu-id="e6ed4-178">Ниже приведен метод контроллера, чтобы удалить продукт из `Supplier.Products` отношения.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-178">Here is the controller method to remove a product from the `Supplier.Products` relation.</span></span>

[!code-csharp[Main](entity-relations-in-odata-v4/samples/sample21.cs)]

<span data-ttu-id="e6ed4-179">*Ключ* является основой для поставщика и *relatedKey* параметр — это ключ продукта для удаления из `Products` связи.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-179">The *key* parameter is the key for the supplier, and the *relatedKey* parameter is the key for the product to remove from the `Products` relationship.</span></span> <span data-ttu-id="e6ed4-180">Обратите внимание на то, что веб-API автоматически получает ключ из строки запроса.</span><span class="sxs-lookup"><span data-stu-id="e6ed4-180">Note that Web API automatically gets the key from the query string.</span></span>
