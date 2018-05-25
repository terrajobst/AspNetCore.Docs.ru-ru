---
uid: web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
title: Развернуть с помощью $select, $ и $value в ASP.NET Web API 2 OData | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/11/2013
ms.topic: article
ms.assetid: 43279a80-a96c-4564-b6ea-ad992a2d6828
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/using-select-expand-and-value
msc.type: authoredcontent
ms.openlocfilehash: f229cdbd8850a787dd3585e0640e8e66f6109331
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="using-select-expand-and-value-in-aspnet-web-api-2-odata"></a><span data-ttu-id="8826d-102">Развернуть с помощью $select, $ и $value в ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="8826d-102">Using $select, $expand, and $value in ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="8826d-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="8826d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="8826d-104">Веб-API 2 добавляет поддержку для $разверните $select и возможности $value OData.</span><span class="sxs-lookup"><span data-stu-id="8826d-104">Web API 2 adds support for the $expand, $select, and $value options in OData.</span></span> <span data-ttu-id="8826d-105">Эти параметры позволяют контролировать представление, он получает от сервера клиенту.</span><span class="sxs-lookup"><span data-stu-id="8826d-105">These options allow a client to control the representation that it gets back from the server.</span></span>

- <span data-ttu-id="8826d-106">**$expand** вызывает связанные сущности быть указаны в ответе.</span><span class="sxs-lookup"><span data-stu-id="8826d-106">**$expand** causes related entities to be included inline in the response.</span></span>
- <span data-ttu-id="8826d-107">**$select** выбирает подмножество свойств для включения в ответ.</span><span class="sxs-lookup"><span data-stu-id="8826d-107">**$select** selects a subset of properties to include in the response.</span></span>
- <span data-ttu-id="8826d-108">**$value** получает необработанное значение свойства.</span><span class="sxs-lookup"><span data-stu-id="8826d-108">**$value** gets the raw value of a property.</span></span>

## <a name="example-schema"></a><span data-ttu-id="8826d-109">Пример схемы</span><span class="sxs-lookup"><span data-stu-id="8826d-109">Example Schema</span></span>

<span data-ttu-id="8826d-110">В этой статье я буду использовать службы OData, определяющий три сущности: продукта, поставщика и категории.</span><span class="sxs-lookup"><span data-stu-id="8826d-110">For this article, I'll use an OData service that defines three entities: Product, Supplier, and Category.</span></span> <span data-ttu-id="8826d-111">Каждый продукт состоит из одной категории и одного поставщика.</span><span class="sxs-lookup"><span data-stu-id="8826d-111">Each product has one category and one supplier.</span></span>

![](using-select-expand-and-value/_static/image1.png)

<span data-ttu-id="8826d-112">Ниже приведены классов C#, которые определяют модели сущностей.</span><span class="sxs-lookup"><span data-stu-id="8826d-112">Here are the C# classes that define the entity models:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample1.cs)]

<span data-ttu-id="8826d-113">Обратите внимание, что `Product` класс определяет свойства навигации для `Supplier` и `Category`.</span><span class="sxs-lookup"><span data-stu-id="8826d-113">Notice that the `Product` class defines navigation properties for the `Supplier` and `Category`.</span></span> <span data-ttu-id="8826d-114">`Category` Класс определяет свойство навигации для продуктов в каждой категории.</span><span class="sxs-lookup"><span data-stu-id="8826d-114">The `Category` class defines a navigation property for the products in each category.</span></span>

<span data-ttu-id="8826d-115">Создание конечной точки OData для этой схемы, использовать формирование шаблонов Visual Studio 2013, как описано в [Создание конечной точки OData в веб-API ASP.NET](odata-v3/creating-an-odata-endpoint.md).</span><span class="sxs-lookup"><span data-stu-id="8826d-115">To create an OData endpoint for this schema, use the Visual Studio 2013 scaffolding, as described in [Creating an OData Endpoint in ASP.NET Web API](odata-v3/creating-an-odata-endpoint.md).</span></span> <span data-ttu-id="8826d-116">Добавьте отдельные контроллеры для продукта, категорию и поставщика.</span><span class="sxs-lookup"><span data-stu-id="8826d-116">Add separate controllers for Product, Category, and Supplier.</span></span>

## <a name="enabling-expand-and-select"></a><span data-ttu-id="8826d-117">Включение $разверните и $select</span><span class="sxs-lookup"><span data-stu-id="8826d-117">Enabling $expand and $select</span></span>

<span data-ttu-id="8826d-118">В Visual Studio 2013 формирование шаблонов OData веб-API будет создан контроллер, автоматически поддерживает $expand и $select.</span><span class="sxs-lookup"><span data-stu-id="8826d-118">In Visual Studio 2013, the Web API OData scaffolding creates a controller that automatically supports $expand and $select.</span></span> <span data-ttu-id="8826d-119">Справочник, имеются требования к поддержке $разверните и $select в контроллере.</span><span class="sxs-lookup"><span data-stu-id="8826d-119">For reference, here are the requirements to support $expand and $select in a controller.</span></span>

<span data-ttu-id="8826d-120">Для коллекций, контроллер `Get` метод должен возвращать **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="8826d-120">For collections, the controller's `Get` method must return an **IQueryable**.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample2.cs)]

<span data-ttu-id="8826d-121">Для одной сущности возвращают **SingleResult&lt;T&gt;**, где T — **IQueryable** , содержащая более одной сущности.</span><span class="sxs-lookup"><span data-stu-id="8826d-121">For single entities, return a **SingleResult&lt;T&gt;**, where T is an **IQueryable** that contains zero or one entities.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample3.cs)]

<span data-ttu-id="8826d-122">Кроме того, оформление вашей `Get` методы с **[Queryable]** атрибута, как показано в предыдущем фрагменты кода.</span><span class="sxs-lookup"><span data-stu-id="8826d-122">Also, decorate your `Get` methods with the **[Queryable]** attribute, as shown in the previous code snippets.</span></span> <span data-ttu-id="8826d-123">Кроме того, вызвать **EnableQuerySupport** на **HttpConfiguration** объекта во время запуска.</span><span class="sxs-lookup"><span data-stu-id="8826d-123">Alternatively, call **EnableQuerySupport** on the **HttpConfiguration** object at startup.</span></span> <span data-ttu-id="8826d-124">(Дополнительные сведения см. в разделе [включения параметров запроса OData](supporting-odata-query-options.md#enable).)</span><span class="sxs-lookup"><span data-stu-id="8826d-124">(For more information, see [Enabling OData Query Options](supporting-odata-query-options.md#enable).)</span></span>

## <a name="using-expand"></a><span data-ttu-id="8826d-125">Разверните узел с помощью $</span><span class="sxs-lookup"><span data-stu-id="8826d-125">Using $expand</span></span>

<span data-ttu-id="8826d-126">При запросе OData сущность или коллекцию ответа по умолчанию не содержит связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="8826d-126">When you query an OData entity or collection, the default response does not include related entities.</span></span> <span data-ttu-id="8826d-127">Например здесь является ответом по умолчанию для набора сущностей категории:</span><span class="sxs-lookup"><span data-stu-id="8826d-127">For example, here is the default response for the Categories entity set:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample4.cmd)]

<span data-ttu-id="8826d-128">Как видите, ответ не содержит какие-либо продукты хотя сущности «Категория» имеется ссылка навигации продуктов.</span><span class="sxs-lookup"><span data-stu-id="8826d-128">As you can see, the response does not include any products, even though the Category entity has a Products navigation link.</span></span> <span data-ttu-id="8826d-129">Тем не менее, клиент может использовать $разверните, чтобы получить список продуктов для каждой категории.</span><span class="sxs-lookup"><span data-stu-id="8826d-129">However, the client can use $expand to get the list of products for each category.</span></span> <span data-ttu-id="8826d-130">$Разверните параметр переходит в строке запроса:</span><span class="sxs-lookup"><span data-stu-id="8826d-130">The $expand option goes in the query string of the request:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample5.cmd)]

<span data-ttu-id="8826d-131">Теперь сервер будет включать продукты для каждой категории встроенных категорий.</span><span class="sxs-lookup"><span data-stu-id="8826d-131">Now the server will include the products for each category, inline with the categories.</span></span> <span data-ttu-id="8826d-132">Вот полезные данные ответа.</span><span class="sxs-lookup"><span data-stu-id="8826d-132">Here is the response payload:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample6.cmd)]

<span data-ttu-id="8826d-133">Обратите внимание, что каждая запись в массиве «значение» содержит список продуктов.</span><span class="sxs-lookup"><span data-stu-id="8826d-133">Notice that each entry in the "value" array contains a Products list.</span></span>

<span data-ttu-id="8826d-134">$Разверните параметр принимает разделенный запятыми список свойств навигации для развертывания.</span><span class="sxs-lookup"><span data-stu-id="8826d-134">The $expand option takes a comma-separated list of navigation properties to expand.</span></span> <span data-ttu-id="8826d-135">Следующий запрос расширяет категории и поставщика для продукта.</span><span class="sxs-lookup"><span data-stu-id="8826d-135">The following request expands both the category and the supplier for a product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample7.cmd)]

<span data-ttu-id="8826d-136">Ниже приведен текст ответа:</span><span class="sxs-lookup"><span data-stu-id="8826d-136">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample8.cmd)]

<span data-ttu-id="8826d-137">Вы можете развернуть более одного уровня свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="8826d-137">You can expand more than one level of navigation property.</span></span> <span data-ttu-id="8826d-138">Следующий пример содержит все продукты для категории, а также поставщик для каждого продукта.</span><span class="sxs-lookup"><span data-stu-id="8826d-138">The following example includes all the products for a category and also the supplier for each product.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample9.cmd)]

<span data-ttu-id="8826d-139">Ниже приведен текст ответа:</span><span class="sxs-lookup"><span data-stu-id="8826d-139">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample10.cmd)]

<span data-ttu-id="8826d-140">По умолчанию веб-API ограничивает максимальное глубину 2.</span><span class="sxs-lookup"><span data-stu-id="8826d-140">By default, Web API limits the maximum expansion depth to 2.</span></span> <span data-ttu-id="8826d-141">Который предотвращает отправку сложных запросов, например клиент `$expand=Orders/OrderDetails/Product/Supplier/Region`, который может быть неэффективным для запроса и создания большие ответы.</span><span class="sxs-lookup"><span data-stu-id="8826d-141">That prevents the client from sending complex requests like `$expand=Orders/OrderDetails/Product/Supplier/Region`, which might be inefficient to query and create large responses.</span></span> <span data-ttu-id="8826d-142">Чтобы переопределить значение по умолчанию, задайте **MaxExpansionDepth** свойство **[Queryable]** атрибута.</span><span class="sxs-lookup"><span data-stu-id="8826d-142">To override the default, set the **MaxExpansionDepth** property on the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample11.cs)]

<span data-ttu-id="8826d-143">Дополнительные сведения о $разверните параметр, см. в разделе [параметр системного запроса ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) в официальной документации OData.</span><span class="sxs-lookup"><span data-stu-id="8826d-143">For more information about the $expand option, see [Expand System Query Option ($expand)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#46_Expand_System_Query_Option_expand) in the official OData documentation.</span></span>

## <a name="using-select"></a><span data-ttu-id="8826d-144">С помощью $select</span><span class="sxs-lookup"><span data-stu-id="8826d-144">Using $select</span></span>

<span data-ttu-id="8826d-145">Параметр $select определяет подмножество свойств для включения в тексте ответа.</span><span class="sxs-lookup"><span data-stu-id="8826d-145">The $select option specifies a subset of properties to include in the response body.</span></span> <span data-ttu-id="8826d-146">Например чтобы получить только имя и цены каждого продукта, используйте следующий запрос:</span><span class="sxs-lookup"><span data-stu-id="8826d-146">For example, to get only the name and price of each product, use the following query:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample12.cmd)]

<span data-ttu-id="8826d-147">Ниже приведен текст ответа:</span><span class="sxs-lookup"><span data-stu-id="8826d-147">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample13.cmd)]

<span data-ttu-id="8826d-148">Можно объединять $select и $expand в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="8826d-148">You can combine $select and $expand in the same query.</span></span> <span data-ttu-id="8826d-149">Убедитесь, что для включения расширенного свойства в параметр $select.</span><span class="sxs-lookup"><span data-stu-id="8826d-149">Make sure to include the expanded property in the $select option.</span></span> <span data-ttu-id="8826d-150">Например следующий запрос возвращает имя продукта и поставщика.</span><span class="sxs-lookup"><span data-stu-id="8826d-150">For example, the following request gets the product name and supplier.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample14.cmd)]

<span data-ttu-id="8826d-151">Ниже приведен текст ответа:</span><span class="sxs-lookup"><span data-stu-id="8826d-151">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample15.cmd)]

<span data-ttu-id="8826d-152">Можно также выбрать свойства в расширенное свойство.</span><span class="sxs-lookup"><span data-stu-id="8826d-152">You can also select the properties within an expanded property.</span></span> <span data-ttu-id="8826d-153">Следующий запрос расширяет продуктов и выбирает имя категории, а также название продукта.</span><span class="sxs-lookup"><span data-stu-id="8826d-153">The following request expands Products and selects category name plus product name.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample16.cmd)]

<span data-ttu-id="8826d-154">Ниже приведен текст ответа:</span><span class="sxs-lookup"><span data-stu-id="8826d-154">Here is the response body:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample17.cmd)]

<span data-ttu-id="8826d-155">Дополнительные сведения о параметре $select см. в разделе [параметр системного запроса ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) в официальной документации OData.</span><span class="sxs-lookup"><span data-stu-id="8826d-155">For more information about the $select option, see [Select System Query Option ($select)](http://www.odata.org/documentation/odata-v2-documentation/uri-conventions/#48_Select_System_Query_Option_select) in the official OData documentation.</span></span>

## <a name="getting-individual-properties-of-an-entity-value"></a><span data-ttu-id="8826d-156">Получение отдельных свойств сущности ($value)</span><span class="sxs-lookup"><span data-stu-id="8826d-156">Getting Individual Properties of an Entity ($value)</span></span>

<span data-ttu-id="8826d-157">Существует два способа для клиента для получения отдельного свойства из сущности OData.</span><span class="sxs-lookup"><span data-stu-id="8826d-157">There are two ways for an OData client to get an individual property from an entity.</span></span> <span data-ttu-id="8826d-158">Клиента можно получить значение в формате OData или получить необработанное значение свойства.</span><span class="sxs-lookup"><span data-stu-id="8826d-158">The client can either get the value in OData format, or get the raw value of the property.</span></span>

<span data-ttu-id="8826d-159">Следующий запрос возвращает свойство в формате OData.</span><span class="sxs-lookup"><span data-stu-id="8826d-159">The following request gets a property in OData format.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample18.cmd)]

<span data-ttu-id="8826d-160">Ниже приведен пример ответа в формате JSON:</span><span class="sxs-lookup"><span data-stu-id="8826d-160">Here is an example response in JSON format:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample19.cmd)]

<span data-ttu-id="8826d-161">Чтобы получить необработанное значение свойства, добавление $value URI:</span><span class="sxs-lookup"><span data-stu-id="8826d-161">To get the raw value of the property, append $value to the URI:</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample20.cmd)]

<span data-ttu-id="8826d-162">Ниже представлен ответ.</span><span class="sxs-lookup"><span data-stu-id="8826d-162">Here is the response.</span></span> <span data-ttu-id="8826d-163">Обратите внимание, что тип содержимого «text/plain» не является JSON.</span><span class="sxs-lookup"><span data-stu-id="8826d-163">Notice that the content type is "text/plain", not JSON.</span></span>

[!code-console[Main](using-select-expand-and-value/samples/sample21.cmd)]

<span data-ttu-id="8826d-164">Для поддержки этих запросов в контроллер OData, добавьте метод с именем `GetProperty`, где `Property` имя свойства.</span><span class="sxs-lookup"><span data-stu-id="8826d-164">To support these queries in your OData controller, add a method named `GetProperty`, where `Property` is the name of the property.</span></span> <span data-ttu-id="8826d-165">Например, метод, чтобы получить значение свойства Name будет называться `GetName`.</span><span class="sxs-lookup"><span data-stu-id="8826d-165">For example, the method to get the Name property would be named `GetName`.</span></span> <span data-ttu-id="8826d-166">Метод должен возвращать значение этого свойства:</span><span class="sxs-lookup"><span data-stu-id="8826d-166">The method should return the value of that property:</span></span>

[!code-csharp[Main](using-select-expand-and-value/samples/sample22.cs)]
