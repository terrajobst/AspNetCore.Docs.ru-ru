---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
title: Поддержка отношений сущностей в OData v3 с веб-API 2 | Документация Майкрософт
author: MikeWasson
description: 'В большинстве наборов данных определения отношений между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты, имеют поставщики. С помощью OData, клиенты могут переходить по...'
ms.author: riande
ms.date: 02/26/2014
ms.assetid: 1e4c2eb4-b6cf-42ff-8a65-4d71ddca0394
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/working-with-entity-relations
msc.type: authoredcontent
ms.openlocfilehash: f78b5cf36789032f90d3d073698f7a439507277f
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206865"
---
<a name="supporting-entity-relations-in-odata-v3-with-web-api-2"></a><span data-ttu-id="19f6a-104">Поддержка отношений сущностей в OData v3 с веб-API 2</span><span class="sxs-lookup"><span data-stu-id="19f6a-104">Supporting Entity Relations in OData v3 with Web API 2</span></span>
====================
<span data-ttu-id="19f6a-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="19f6a-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="19f6a-106">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="19f6a-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="19f6a-107">В большинстве наборов данных определения отношений между сущностями: клиенты имеют заказы; у книги может быть авторов; продукты, имеют поставщики.</span><span class="sxs-lookup"><span data-stu-id="19f6a-107">Most data sets define relations between entities: Customers have orders; books have authors; products have suppliers.</span></span> <span data-ttu-id="19f6a-108">С помощью OData, клиенты можно переходить через отношения сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-108">Using OData, clients can navigate over entity relations.</span></span> <span data-ttu-id="19f6a-109">Учитывая продукта, можно найти поставщика.</span><span class="sxs-lookup"><span data-stu-id="19f6a-109">Given a product, you can find the supplier.</span></span> <span data-ttu-id="19f6a-110">Также можно создать или удалить связи.</span><span class="sxs-lookup"><span data-stu-id="19f6a-110">You can also create or remove relationships.</span></span> <span data-ttu-id="19f6a-111">Например можно задать поставщик для продукта.</span><span class="sxs-lookup"><span data-stu-id="19f6a-111">For example, you can set the supplier for a product.</span></span>
> 
> <span data-ttu-id="19f6a-112">Этом руководстве показано, как для поддержки этих операций в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="19f6a-112">This tutorial shows how to support these operations in ASP.NET Web API.</span></span> <span data-ttu-id="19f6a-113">Учебном курсе руководство [Создание конечной точки OData v3 с веб-API 2](creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="19f6a-113">The tutorial builds on the tutorial [Creating an OData v3 Endpoint with Web API 2](creating-an-odata-endpoint.md).</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="19f6a-114">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="19f6a-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="19f6a-115">Веб-API 2</span><span class="sxs-lookup"><span data-stu-id="19f6a-115">Web API 2</span></span>
> - <span data-ttu-id="19f6a-116">OData версии 3</span><span class="sxs-lookup"><span data-stu-id="19f6a-116">OData Version 3</span></span>
> - <span data-ttu-id="19f6a-117">Entity Framework 6</span><span class="sxs-lookup"><span data-stu-id="19f6a-117">Entity Framework 6</span></span>


## <a name="add-a-supplier-entity"></a><span data-ttu-id="19f6a-118">Добавление сущности Supplier</span><span class="sxs-lookup"><span data-stu-id="19f6a-118">Add a Supplier Entity</span></span>

<span data-ttu-id="19f6a-119">Во-первых, нам нужно добавить новый тип сущности для наших веб-канала OData.</span><span class="sxs-lookup"><span data-stu-id="19f6a-119">First we need to add a new entity type to our OData feed.</span></span> <span data-ttu-id="19f6a-120">Мы добавим `Supplier` класса.</span><span class="sxs-lookup"><span data-stu-id="19f6a-120">We'll add a `Supplier` class.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample1.cs)]

<span data-ttu-id="19f6a-121">Этот класс использует строку для ключа сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-121">This class uses a string for the entity key.</span></span> <span data-ttu-id="19f6a-122">На практике, это может быть реже, чем с помощью целочисленного ключа.</span><span class="sxs-lookup"><span data-stu-id="19f6a-122">In practice, that might be less common than using an integer key.</span></span> <span data-ttu-id="19f6a-123">Но стоит видеть, как OData обрабатывает другие типы ключей, помимо целых чисел.</span><span class="sxs-lookup"><span data-stu-id="19f6a-123">But it's worth seeing how OData handles other key types besides integers.</span></span>

<span data-ttu-id="19f6a-124">Далее, мы создадим связь путем добавления `Supplier` свойства `Product` класса:</span><span class="sxs-lookup"><span data-stu-id="19f6a-124">Next, we'll create a relation by adding a `Supplier` property to the `Product` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample2.cs)]

<span data-ttu-id="19f6a-125">Добавьте новый **DbSet** для `ProductServiceContext` класса, таким образом, чтобы платформа Entity Framework будет включать `Supplier` таблицы в базе данных.</span><span class="sxs-lookup"><span data-stu-id="19f6a-125">Add a new **DbSet** to the `ProductServiceContext` class, so that Entity Framework will include the `Supplier` table in the database.</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample3.cs?highlight=9)]

<span data-ttu-id="19f6a-126">В файле WebApiConfig.cs добавьте модель EDM сущности «Поставщики»:</span><span class="sxs-lookup"><span data-stu-id="19f6a-126">In WebApiConfig.cs, add a "Suppliers" entity to the EDM model:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample4.cs?highlight=4)]

## <a name="navigation-properties"></a><span data-ttu-id="19f6a-127">Свойства навигации</span><span class="sxs-lookup"><span data-stu-id="19f6a-127">Navigation Properties</span></span>

<span data-ttu-id="19f6a-128">Чтобы получить поставщик для продукта, клиент отправляет запрос GET:</span><span class="sxs-lookup"><span data-stu-id="19f6a-128">To get the supplier for a product, the client sends a GET request:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample5.cmd)]

<span data-ttu-id="19f6a-129">Здесь «Поставщик» является свойством навигации на `Product` типа.</span><span class="sxs-lookup"><span data-stu-id="19f6a-129">Here "Supplier" is a navigation property on the `Product` type.</span></span> <span data-ttu-id="19f6a-130">В этом случае `Supplier` ссылается на один элемент, но переход свойство также может возвращать коллекции (отношения один ко многим "или" многие ко многим).</span><span class="sxs-lookup"><span data-stu-id="19f6a-130">In this case, `Supplier` refers to a single item, but a navigation property can also return a collection (one-to-many or many-to-many relation).</span></span>

<span data-ttu-id="19f6a-131">Чтобы этот запрос в службу поддержки, добавьте следующий метод в `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="19f6a-131">To support this request, add the following method to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample6.cs)]

<span data-ttu-id="19f6a-132">*Ключ* параметр — ключ продукта.</span><span class="sxs-lookup"><span data-stu-id="19f6a-132">The *key* parameter is the key of the product.</span></span> <span data-ttu-id="19f6a-133">Этот метод возвращает связанные сущности&#8212;в этом случае `Supplier` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="19f6a-133">The method returns the related entity&#8212;in this case, a `Supplier` instance.</span></span> <span data-ttu-id="19f6a-134">Имя метода и имени параметра являются важным.</span><span class="sxs-lookup"><span data-stu-id="19f6a-134">The method name and parameter name are both important.</span></span> <span data-ttu-id="19f6a-135">Как правило если свойство навигации называется «X», необходимо добавить метод с именем «GetX».</span><span class="sxs-lookup"><span data-stu-id="19f6a-135">In general, if the navigation property is named "X", you need to add a method named "GetX".</span></span> <span data-ttu-id="19f6a-136">Метод должен принимать параметр с именем "*ключ*", соответствующий тип данных ключа родительского элемента.</span><span class="sxs-lookup"><span data-stu-id="19f6a-136">The method must take a parameter named "*key*" that matches the data type of the parent's key.</span></span>

<span data-ttu-id="19f6a-137">Также важно включить **[FromOdataUri]** атрибут в *ключ* параметра.</span><span class="sxs-lookup"><span data-stu-id="19f6a-137">It is also important to include the **[FromOdataUri]** attribute in the *key* parameter.</span></span> <span data-ttu-id="19f6a-138">Этот атрибут сообщает веб-API для использования правилами синтаксиса OData при синтаксическом анализе ключ из URI запроса.</span><span class="sxs-lookup"><span data-stu-id="19f6a-138">This attribute tells Web API to use OData syntax rules when it parses the key from the request URI.</span></span>

## <a name="creating-and-deleting-links"></a><span data-ttu-id="19f6a-139">Создание и удаление ссылки</span><span class="sxs-lookup"><span data-stu-id="19f6a-139">Creating and Deleting Links</span></span>

<span data-ttu-id="19f6a-140">OData поддерживает создание или удаление связи между двумя сущностями.</span><span class="sxs-lookup"><span data-stu-id="19f6a-140">OData supports creating or removing relationships between two entities.</span></span> <span data-ttu-id="19f6a-141">В терминологии OData связь является «ссылку».</span><span class="sxs-lookup"><span data-stu-id="19f6a-141">In OData terminology, the relationship is a "link."</span></span> <span data-ttu-id="19f6a-142">Каждая ссылка имеет URI с формой *сущности*/$links /*сущности*.</span><span class="sxs-lookup"><span data-stu-id="19f6a-142">Each link has a URI with the form *entity*/$links/*entity*.</span></span> <span data-ttu-id="19f6a-143">Например ссылка продукта поставщика выглядит следующим образом:</span><span class="sxs-lookup"><span data-stu-id="19f6a-143">For example, the link from product to supplier looks like this:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample7.cmd)]

<span data-ttu-id="19f6a-144">Чтобы создать новую ссылку, клиент отправляет запрос POST к URI ссылки.</span><span class="sxs-lookup"><span data-stu-id="19f6a-144">To create a new link, the client sends a POST request to the link URI.</span></span> <span data-ttu-id="19f6a-145">Текст запроса является URI целевой сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-145">The body of the request is the URI of the target entity.</span></span> <span data-ttu-id="19f6a-146">Например предположим, что имеется поставщик с ключом «CTSO».</span><span class="sxs-lookup"><span data-stu-id="19f6a-146">For example, suppose there is a supplier with the key "CTSO".</span></span> <span data-ttu-id="19f6a-147">Чтобы создать ссылку из «Product(1)» на «Supplier('CTSO')», клиент отправляет запрос следующего вида:</span><span class="sxs-lookup"><span data-stu-id="19f6a-147">To create a link from "Product(1)" to "Supplier('CTSO')", the client sends a request like the following:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample8.cmd)]

<span data-ttu-id="19f6a-148">Чтобы удалить ссылку, клиент отправляет запрос DELETE к URI ссылки.</span><span class="sxs-lookup"><span data-stu-id="19f6a-148">To delete a link, the client sends a DELETE request to the link URI.</span></span>

<span data-ttu-id="19f6a-149">**Создание связей между элементами**</span><span class="sxs-lookup"><span data-stu-id="19f6a-149">**Creating Links**</span></span>

<span data-ttu-id="19f6a-150">Чтобы включить клиента для создания ссылок на поставщика продукта, добавьте следующий код, чтобы `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="19f6a-150">To enable a client to create product-supplier links, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample9.cs)]

<span data-ttu-id="19f6a-151">Этот метод принимает три параметра:</span><span class="sxs-lookup"><span data-stu-id="19f6a-151">This method takes three parameters:</span></span>

- <span data-ttu-id="19f6a-152">*ключ*: ключ на родительскую сущность (продукт)</span><span class="sxs-lookup"><span data-stu-id="19f6a-152">*key*: The key to the parent entity (the product)</span></span>
- <span data-ttu-id="19f6a-153">*navigationProperty*: имя свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="19f6a-153">*navigationProperty*: The name of the navigation property.</span></span> <span data-ttu-id="19f6a-154">В этом примере свойство навигации, единственным допустимым является «Поставщик».</span><span class="sxs-lookup"><span data-stu-id="19f6a-154">In this example, the only valid navigation property is "Supplier".</span></span>
- <span data-ttu-id="19f6a-155">*ссылка*: URI OData связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-155">*link*: The OData URI of the related entity.</span></span> <span data-ttu-id="19f6a-156">Это значение берется из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="19f6a-156">This value is taken from the request body.</span></span> <span data-ttu-id="19f6a-157">Например, ссылка URI может быть "`http://localhost/odata/Suppliers('CTSO')`, то есть поставщика с Идентификатором = «CTSO».</span><span class="sxs-lookup"><span data-stu-id="19f6a-157">For example, the link URI might be "`http://localhost/odata/Suppliers('CTSO')`, meaning the supplier with ID = ‘CTSO'.</span></span>

<span data-ttu-id="19f6a-158">Данный метод использует ссылку для поиска поставщика.</span><span class="sxs-lookup"><span data-stu-id="19f6a-158">The method uses the link to look up the supplier.</span></span> <span data-ttu-id="19f6a-159">Если найден соответствующий поставщик, метод устанавливает `Product.Supplier` свойство и сохраняет результат в базе данных.</span><span class="sxs-lookup"><span data-stu-id="19f6a-159">If the matching supplier is found, the method sets the `Product.Supplier` property and saves the result to the database.</span></span>

<span data-ttu-id="19f6a-160">Самая важная часть синтаксического анализа URI ссылки.</span><span class="sxs-lookup"><span data-stu-id="19f6a-160">The hardest part is parsing the link URI.</span></span> <span data-ttu-id="19f6a-161">По сути вам нужно имитировать результат при отправке запроса GET к этому URI.</span><span class="sxs-lookup"><span data-stu-id="19f6a-161">Basically, you need to simulate the result of sending a GET request to that URI.</span></span> <span data-ttu-id="19f6a-162">Следующий вспомогательный метод показано, как это сделать.</span><span class="sxs-lookup"><span data-stu-id="19f6a-162">The following helper method shows how to do this.</span></span> <span data-ttu-id="19f6a-163">Метод вызывает процесс маршрутизации веб-API, а затем возвращает **ODataPath** экземпляр, представляющий проанализированный пути OData.</span><span class="sxs-lookup"><span data-stu-id="19f6a-163">The method invokes the Web API routing process and gets back an **ODataPath** instance that represents the parsed OData path.</span></span> <span data-ttu-id="19f6a-164">Для URI ссылки один из сегментов должно быть ключом сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-164">For a link URI, one of the segments should be the entity key.</span></span> <span data-ttu-id="19f6a-165">(В противном случае клиент отправил недопустимый URI.)</span><span class="sxs-lookup"><span data-stu-id="19f6a-165">(If not, the client sent a bad URI.)</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample10.cs)]

<span data-ttu-id="19f6a-166">**Удаление ссылки**</span><span class="sxs-lookup"><span data-stu-id="19f6a-166">**Deleting Links**</span></span>

<span data-ttu-id="19f6a-167">Чтобы удалить ссылку, добавьте следующий код, чтобы `ProductsController` класса:</span><span class="sxs-lookup"><span data-stu-id="19f6a-167">To delete a link, add the following code to the `ProductsController` class:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample11.cs)]

<span data-ttu-id="19f6a-168">В этом примере свойство навигации представляет собой одну `Supplier` сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-168">In this example, the navigation property is a single `Supplier` entity.</span></span> <span data-ttu-id="19f6a-169">Если свойство навигации является коллекцией, URI, чтобы удалить ссылку необходимо включить ключ для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-169">If the navigation property is a collection, the URI to delete a link must include a key for the related entity.</span></span> <span data-ttu-id="19f6a-170">Пример:</span><span class="sxs-lookup"><span data-stu-id="19f6a-170">For example:</span></span>

[!code-console[Main](working-with-entity-relations/samples/sample12.cmd)]

<span data-ttu-id="19f6a-171">Этот запрос удаляет порядка 1 из клиента 1.</span><span class="sxs-lookup"><span data-stu-id="19f6a-171">This request removes order 1 from customer 1.</span></span> <span data-ttu-id="19f6a-172">В этом случае метод DeleteLink будут иметь следующую сигнатуру:</span><span class="sxs-lookup"><span data-stu-id="19f6a-172">In this case, the DeleteLink method will have the following signature:</span></span>

[!code-csharp[Main](working-with-entity-relations/samples/sample13.cs)]

<span data-ttu-id="19f6a-173">*RelatedKey* параметр предоставляет ключ для связанной сущности.</span><span class="sxs-lookup"><span data-stu-id="19f6a-173">The *relatedKey* parameter gives the key for the related entity.</span></span> <span data-ttu-id="19f6a-174">Таким образом, в вашей `DeleteLink` метод, для поиска основной сущностью, *ключ* параметра, найти связанные сущности, *relatedKey* параметра, а затем удалите связь.</span><span class="sxs-lookup"><span data-stu-id="19f6a-174">So in your `DeleteLink` method, look up the primary entity by the *key* parameter, find the related entity by the *relatedKey* parameter, and then remove the association.</span></span> <span data-ttu-id="19f6a-175">В зависимости от вашей модели данных, может потребоваться реализовать обе версии `DeleteLink`.</span><span class="sxs-lookup"><span data-stu-id="19f6a-175">Depending on your data model, you might need to implement both versions of `DeleteLink`.</span></span> <span data-ttu-id="19f6a-176">Веб-API вызовет правильную версию, в зависимости от URI запроса.</span><span class="sxs-lookup"><span data-stu-id="19f6a-176">Web API will call the correct version based on the request URI.</span></span>
