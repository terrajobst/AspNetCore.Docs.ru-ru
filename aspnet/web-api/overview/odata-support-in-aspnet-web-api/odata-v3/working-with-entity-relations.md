---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: "Поддержка отношениями сущностей в OData v3 с веб-API 2 | Документы Microsoft"
author: MikeWasson
description: "Большинство наборов данных определить отношения между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты имеют поставщиков. С помощью OData, клиенты могут переходить по..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: dec7e10e59cc2441c967afe062df227b105106a1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="a5630-104">Поддержка отношениями сущностей в OData v3 с веб-API 2</span><span class="sxs-lookup"><span data-stu-id="a5630-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="a5630-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="a5630-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="a5630-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="a5630-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="a5630-107">Большинство наборов данных определить отношения между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты имеют поставщиков.</span><span class="sxs-lookup"><span data-stu-id="a5630-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="a5630-108">С помощью OData, клиенты могут переходить через отношения сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="a5630-109">Имея продукта, можно найти сведения о поставщике.</span><span class="sxs-lookup"><span data-stu-id="a5630-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="a5630-110">Также можно создать или удалить связи.</span><span class="sxs-lookup"><span data-stu-id="a5630-110">You can also create or remove relationships.</span></span> <span data-ttu-id="a5630-111">Например можно задать сведения о поставщике для продукта.</span><span class="sxs-lookup"><span data-stu-id="a5630-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="a5630-112">Этот учебник показывает, как для поддержки этих операций в ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="a5630-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="a5630-113">Учебник построен на учебника [Создание конечной точки OData v3 с веб-API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="a5630-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a5630-114">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="a5630-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a5630-115">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="a5630-115">Web API 2</span></span>
> - <span data-ttu-id="a5630-116">OData версии 3</span><span class="sxs-lookup"><span data-stu-id="a5630-116">OData Version 3</span></span>
> - <span data-ttu-id="a5630-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="a5630-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="a5630-118">Добавление сущности поставщика</span><span class="sxs-lookup"><span data-stu-id="a5630-118">Add a Supplier Entity</span></span>

<span data-ttu-id="a5630-119">Сначала необходимо добавить новый тип сущности для наших веб-канала OData.</span><span class="sxs-lookup"><span data-stu-id="a5630-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="a5630-120">Мы добавим `Supplier` класса.</span><span class="sxs-lookup"><span data-stu-id="a5630-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="a5630-121">Этот класс использует строку для ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="a5630-122">На практике, который может быть реже, чем с помощью ключа целое число со знаком.</span><span class="sxs-lookup"><span data-stu-id="a5630-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="a5630-123">Но стоит отображается как OData обрабатывает другие типы ключей, помимо целых чисел.</span><span class="sxs-lookup"><span data-stu-id="a5630-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="a5630-124">Далее предстоит создать отношение путем добавления `Supplier` свойства `Product` класса:</span><span class="sxs-lookup"><span data-stu-id="a5630-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="a5630-125">Добавьте новый **DbSet** для `ProductServiceContext` класса, чтобы платформа Entity Framework будет включать `Supplier` таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a5630-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="a5630-126">В WebApiConfig.cs добавьте сущности «Поставщики» с моделью EDM.</span><span class="sxs-lookup"><span data-stu-id="a5630-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="a5630-127">Свойства навигации</span><span class="sxs-lookup"><span data-stu-id="a5630-127">Navigation Properties</span></span>

<span data-ttu-id="a5630-128">Чтобы получить сведения о поставщике для продукта, клиент отправляет запрос GET:</span><span class="sxs-lookup"><span data-stu-id="a5630-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="a5630-129">Здесь «Поставщик» является свойством навигации на `Product` типа.</span><span class="sxs-lookup"><span data-stu-id="a5630-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="a5630-130">В этом случае `Supplier` ссылается один элемент, но навигацию свойство также может вернуть коллекцию (связь один ко многим "или" многие ко многим).</span><span class="sxs-lookup"><span data-stu-id="a5630-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="a5630-131">Для поддержки этого запроса, добавьте следующий метод `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="a5630-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="a5630-132">*Ключ* параметр — это ключ продукта.</span><span class="sxs-lookup"><span data-stu-id="a5630-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="a5630-133">Метод возвращает связанный объект &#8212;в этом случае `Supplier` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="a5630-133">The method returns the related entity&#8212in this case, a `Supplier` instance.</span></span> <span data-ttu-id="a5630-134">Имя метода и имени параметра являются как важные.</span><span class="sxs-lookup"><span data-stu-id="a5630-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="a5630-135">Как правило если свойство навигации с именем «X», необходимо добавить метод с именем «Методы GetX».</span><span class="sxs-lookup"><span data-stu-id="a5630-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="a5630-136">Метод не должен принимать параметр с именем «*ключ*», соответствующий тип данных ключа родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="a5630-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="a5630-137">Также важно для включения **[FromOdataUri]** атрибута в *ключ* параметра.</span><span class="sxs-lookup"><span data-stu-id="a5630-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="a5630-138">Этот атрибут сообщает веб-API для использования правилами синтаксиса OData при синтаксическом анализе ключ от URI запроса.</span><span class="sxs-lookup"><span data-stu-id="a5630-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="a5630-139">Создание и удаление ссылки</span><span class="sxs-lookup"><span data-stu-id="a5630-139">Creating and Deleting Links</span></span>

<span data-ttu-id="a5630-140">OData поддерживает создание или удаление отношений между двумя сущностями.</span><span class="sxs-lookup"><span data-stu-id="a5630-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="a5630-141">В терминологии OData связь является «связи».</span><span class="sxs-lookup"><span data-stu-id="a5630-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="a5630-142">Каждая ссылка имеет URI с формой *сущности*/$links /*сущности*.</span><span class="sxs-lookup"><span data-stu-id="a5630-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="a5630-143">Например ссылка из продукта в поставщик выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a5630-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="a5630-144">Чтобы создать новую ссылку, клиент отправляет запрос POST к URI ссылки.</span><span class="sxs-lookup"><span data-stu-id="a5630-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="a5630-145">Текст запроса является URI целевой сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="a5630-146">Например предположим, что нет других поставщиков с помощью ключа «CTSO».</span><span class="sxs-lookup"><span data-stu-id="a5630-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="a5630-147">Чтобы создать ссылку из «Product(1)» на «Supplier('CTSO')», клиент отправляет запрос следующим образом:</span><span class="sxs-lookup"><span data-stu-id="a5630-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="a5630-148">Чтобы удалить ссылку, клиент отправляет запрос DELETE URI ссылки.</span><span class="sxs-lookup"><span data-stu-id="a5630-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="a5630-149">**Создание ссылок**</span><span class="sxs-lookup"><span data-stu-id="a5630-149">**Creating Links**</span></span>

<span data-ttu-id="a5630-150">Чтобы клиент для создания ссылки продукт поставщика, добавьте следующий код в `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="a5630-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="a5630-151">Этот метод принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="a5630-151">This method takes three parameters:</span></span>

- <span data-ttu-id="a5630-152">*ключ*: ключа на родительский объект («продукт»)</span><span class="sxs-lookup"><span data-stu-id="a5630-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="a5630-153">*Свойство navigationProperty*: имя свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="a5630-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="a5630-154">В этом примере свойства навигации единственным допустимым является «Поставщик».</span><span class="sxs-lookup"><span data-stu-id="a5630-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="a5630-155">*ссылка*: URI OData связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="a5630-156">Это значение берется из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="a5630-156">This value is taken from the request body.</span></span> <span data-ttu-id="a5630-157">Например, ссылка URI может быть "`http://localhost/odata/Suppliers('CTSO')`, это означает поставщика с Идентификатором = «CTSO».</span><span class="sxs-lookup"><span data-stu-id="a5630-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="a5630-158">Метод использует ссылки для поиска поставщика.</span><span class="sxs-lookup"><span data-stu-id="a5630-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="a5630-159">При обнаружении соответствующего поставщика, метод задает `Product.Supplier` свойства и сохраняет результат в базе данных.</span><span class="sxs-lookup"><span data-stu-id="a5630-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="a5630-160">Самым сложным является синтаксический анализ URI ссылки.</span><span class="sxs-lookup"><span data-stu-id="a5630-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="a5630-161">По сути необходимо моделировать результат при отправке запроса GET для этого идентификатора URI.</span><span class="sxs-lookup"><span data-stu-id="a5630-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="a5630-162">Следующий вспомогательный метод показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="a5630-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="a5630-163">Этот метод вызывает процесс маршрутизации веб-API и получает обратно **ODataPath** экземпляр, представляющий синтаксического анализа пути OData.</span><span class="sxs-lookup"><span data-stu-id="a5630-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="a5630-164">Для URI ссылки один из сегментов должно быть ключ сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="a5630-165">(В противном случае клиент отправил недопустимый URI.)</span><span class="sxs-lookup"><span data-stu-id="a5630-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="a5630-166">**Удаление связей**</span><span class="sxs-lookup"><span data-stu-id="a5630-166">**Deleting Links**</span></span>

<span data-ttu-id="a5630-167">Чтобы удалить ссылку, добавьте следующий код в `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="a5630-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="a5630-168">В этом примере свойства навигации представляет собой одну `Supplier` сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="a5630-169">Если свойство навигации является коллекцией, URI для удаления связи должен содержать ключ для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="a5630-170">Пример:</span><span class="sxs-lookup"><span data-stu-id="a5630-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="a5630-171">Этот запрос удаляет заказа 1 из клиента 1.</span><span class="sxs-lookup"><span data-stu-id="a5630-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="a5630-172">В этом случае метод DeleteLink будут иметь следующую сигнатуру:</span><span class="sxs-lookup"><span data-stu-id="a5630-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="a5630-173">*RelatedKey* параметр предоставляет ключ для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="a5630-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="a5630-174">Так в вашей `DeleteLink` метод для поиска основной сущности по *ключ* параметра, найти связанные сущности, *relatedKey* параметра, а затем удалите связь.</span><span class="sxs-lookup"><span data-stu-id="a5630-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="a5630-175">В зависимости от модели данных, может потребоваться реализовать обе версии `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="a5630-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="a5630-176">Веб-API вызовет правильную версию на основе запроса URI.</span><span class="sxs-lookup"><span data-stu-id="a5630-176">Web API will call the correct version based on the request URI.</span></span>
