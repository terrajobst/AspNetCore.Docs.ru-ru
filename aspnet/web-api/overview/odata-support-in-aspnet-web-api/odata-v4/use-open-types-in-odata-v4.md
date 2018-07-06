---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
title: Открытые типы в OData v4 с веб-API ASP.NET | Документация Майкрософт
author: microsoft
description: В OData v4 открытого типа является типом stuctured, который содержит динамические свойства, а также любые свойства, объявленные в определении типа. Открыть...
ms.author: aspnetcontent
ms.date: 09/15/2014
ms.assetid: f25f5ac5-4800-4950-abe5-c97750a27fc6
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/use-open-types-in-odata-v4
msc.type: authoredcontent
ms.openlocfilehash: 560d47e0dc451847311eb9e2327190eed2209546
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832276"
---
<a name="open-types-in-odata-v4-with-aspnet-web-api"></a><span data-ttu-id="cd4c1-104">Открытые типы в OData v4 с веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="cd4c1-104">Open Types in OData v4 with ASP.NET Web API</span></span>
====================
<span data-ttu-id="cd4c1-105">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="cd4c1-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="cd4c1-106">В OData v4 *открытый тип* является типом stuctured, который содержит динамические свойства, а также любые свойства, объявленные в определении типа.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-106">In OData v4, an *open type* is a stuctured type that contains dynamic properties, in addition to any properties that are declared in the type definition.</span></span> <span data-ttu-id="cd4c1-107">Открытые типы позволяют добавлять гибкость к моделям данных.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-107">Open types let you add flexibility to your data models.</span></span> <span data-ttu-id="cd4c1-108">Этом руководстве показано, как использовать открытые типы в OData веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-108">This tutorial shows how to use open types in ASP.NET Web API OData.</span></span>
> 
> <span data-ttu-id="cd4c1-109">Предполагается, что вы уже знаете, как создать конечную точку OData в веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-109">This tutorial assumes that you already know how to create an OData endpoint in ASP.NET Web API.</span></span> <span data-ttu-id="cd4c1-110">Если это не так, обратитесь к статье [Создайте конечную точку OData v4](create-an-odata-v4-endpoint.md) первого.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-110">If not, start by reading [Create an OData v4 Endpoint](create-an-odata-v4-endpoint.md) first.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="cd4c1-111">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="cd4c1-111">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="cd4c1-112">Веб-API OData 5.3</span><span class="sxs-lookup"><span data-stu-id="cd4c1-112">Web API OData 5.3</span></span>
> - <span data-ttu-id="cd4c1-113">OData v4</span><span class="sxs-lookup"><span data-stu-id="cd4c1-113">OData v4</span></span>


<span data-ttu-id="cd4c1-114">Во-первых, термины OData:</span><span class="sxs-lookup"><span data-stu-id="cd4c1-114">First, some OData terminology:</span></span>

- <span data-ttu-id="cd4c1-115">Тип сущности: структурный тип с ключом.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-115">Entity type: A structured type with a key.</span></span>
- <span data-ttu-id="cd4c1-116">Сложный тип: структурированного типа без ключа.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-116">Complex type: A structured type without a key.</span></span>
- <span data-ttu-id="cd4c1-117">Открытый тип: тип с динамические свойства.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-117">Open type: A type with dynamic properties.</span></span> <span data-ttu-id="cd4c1-118">Типы сущностей и сложных типов может быть открыт.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-118">Both entity types and complex types can be open.</span></span>

<span data-ttu-id="cd4c1-119">Значение динамического свойства может быть тип-примитив, сложный тип или тип перечисления; или коллекцию любого из этих типов.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-119">The value of a dynamic property can be a primitive type, complex type, or enumeration type; or a collection of any of those types.</span></span> <span data-ttu-id="cd4c1-120">Дополнительные сведения об открытых типах см. в разделе [спецификации OData v4](http://www.odata.org/documentation/odata-version-4-0/).</span><span class="sxs-lookup"><span data-stu-id="cd4c1-120">For more information about open types, see the [OData v4 specification](http://www.odata.org/documentation/odata-version-4-0/).</span></span>

## <a name="install-the-web-odata-libraries"></a><span data-ttu-id="cd4c1-121">Установка библиотеки OData Web</span><span class="sxs-lookup"><span data-stu-id="cd4c1-121">Install the Web OData Libraries</span></span>

<span data-ttu-id="cd4c1-122">С помощью диспетчера пакетов NuGet для установки последних версий библиотек OData веб-API.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-122">Use NuGet Package Manager to install the latest Web API OData libraries.</span></span> <span data-ttu-id="cd4c1-123">В окне консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="cd4c1-123">From the Package Manager Console window:</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample1.cmd)]

## <a name="define-the-clr-types"></a><span data-ttu-id="cd4c1-124">Определение типов среды CLR</span><span class="sxs-lookup"><span data-stu-id="cd4c1-124">Define the CLR Types</span></span>

<span data-ttu-id="cd4c1-125">Начинается с определения модели EDM как типы среды CLR.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-125">Start by defining the EDM models as CLR types.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample2.cs)]

<span data-ttu-id="cd4c1-126">При создании Entity Data Model (EDM)</span><span class="sxs-lookup"><span data-stu-id="cd4c1-126">When the Entity Data Model (EDM) is created,</span></span>

- <span data-ttu-id="cd4c1-127">`Category` является типом перечисления.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-127">`Category` is an enumeration type.</span></span>
- <span data-ttu-id="cd4c1-128">`Address` — Это сложный тип.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-128">`Address` is a complex type.</span></span> <span data-ttu-id="cd4c1-129">(Он не имеет ключа, поэтому он не является типом сущности.)</span><span class="sxs-lookup"><span data-stu-id="cd4c1-129">(It does not have a key, so it is not an entity type.)</span></span>
- <span data-ttu-id="cd4c1-130">`Customer` является типом сущности.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-130">`Customer` is an entity type.</span></span> <span data-ttu-id="cd4c1-131">(Он имеет ключ).</span><span class="sxs-lookup"><span data-stu-id="cd4c1-131">(It has a key.)</span></span>
- <span data-ttu-id="cd4c1-132">`Press` имеет открытый сложный тип.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-132">`Press` is an open complex type.</span></span>
- <span data-ttu-id="cd4c1-133">`Book` Это тип сущности открытым.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-133">`Book` is an open entity type.</span></span>

<span data-ttu-id="cd4c1-134">Чтобы создать открытый тип, тип среды CLR должен иметь свойство типа `IDictionary<string, object>`, который содержит динамические свойства.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-134">To create an open type, the CLR type must have a property of type `IDictionary<string, object>`, which holds the dynamic properties.</span></span>

## <a name="build-the-edm-model"></a><span data-ttu-id="cd4c1-135">Создание модели EDM</span><span class="sxs-lookup"><span data-stu-id="cd4c1-135">Build the EDM Model</span></span>

<span data-ttu-id="cd4c1-136">Если вы используете **ODataConventionModelBuilder** для создания модели EDM `Press` и `Book` автоматически добавляется в качестве открытых типов, в зависимости от наличия `IDictionary<string, object>` свойство.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-136">If you use **ODataConventionModelBuilder** to create the EDM, `Press` and `Book` are automatically added as open types, based on the presence of a `IDictionary<string, object>` property.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample3.cs)]

<span data-ttu-id="cd4c1-137">Вы также можете создавать модели EDM явно, с помощью **ODataModelBuilder**.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-137">You can also build the EDM explicitly, using **ODataModelBuilder**.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample4.cs)]

## <a name="add-an-odata-controller"></a><span data-ttu-id="cd4c1-138">Добавить контроллер OData</span><span class="sxs-lookup"><span data-stu-id="cd4c1-138">Add an OData Controller</span></span>

<span data-ttu-id="cd4c1-139">Добавьте контроллер OData.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-139">Next, add an OData controller.</span></span> <span data-ttu-id="cd4c1-140">В этом учебнике мы будем использовать упрощенный контроллер поддерживает только GET и POST запрашивает что использует список в памяти для хранения сущностей.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-140">For this tutorial, we'll use a simplified controller that just supports GET and POST requests, and uses an in-memory list to store entities.</span></span>

[!code-csharp[Main](use-open-types-in-odata-v4/samples/sample5.cs)]

<span data-ttu-id="cd4c1-141">Обратите внимание, что первый `Book` экземпляр не имеет динамических свойств.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-141">Notice that the first `Book` instance has no dynamic properties.</span></span> <span data-ttu-id="cd4c1-142">Второй `Book` экземпляр имеет следующие динамические свойства:</span><span class="sxs-lookup"><span data-stu-id="cd4c1-142">The second `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="cd4c1-143">«Опубликовано»: тип-примитив</span><span class="sxs-lookup"><span data-stu-id="cd4c1-143">"Published": Primitive type</span></span>
- <span data-ttu-id="cd4c1-144">«Авторы»: коллекции типов-примитивов</span><span class="sxs-lookup"><span data-stu-id="cd4c1-144">"Authors": Collection of primitive types</span></span>
- <span data-ttu-id="cd4c1-145">«OtherCategories»: коллекцию типов перечисления.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-145">"OtherCategories": Collection of enumeration types.</span></span>

<span data-ttu-id="cd4c1-146">Кроме того `Press` , свойство `Book` экземпляра имеет следующие динамические свойства:</span><span class="sxs-lookup"><span data-stu-id="cd4c1-146">Also, the `Press` property of that `Book` instance has the following dynamic properties:</span></span>

- <span data-ttu-id="cd4c1-147">«Блог»: тип-примитив</span><span class="sxs-lookup"><span data-stu-id="cd4c1-147">"Blog": Primitive type</span></span>
- <span data-ttu-id="cd4c1-148">«Address»: сложный тип</span><span class="sxs-lookup"><span data-stu-id="cd4c1-148">"Address": Complex type</span></span>

## <a name="query-the-metadata"></a><span data-ttu-id="cd4c1-149">Запрос метаданных</span><span class="sxs-lookup"><span data-stu-id="cd4c1-149">Query the Metadata</span></span>

<span data-ttu-id="cd4c1-150">Чтобы получить документ метаданных OData, отправьте запрос GET к `~/$metadata`.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-150">To get the OData metadata document, send a GET request to `~/$metadata`.</span></span> <span data-ttu-id="cd4c1-151">Текст ответа должен выглядеть примерно следующим образом:</span><span class="sxs-lookup"><span data-stu-id="cd4c1-151">The response body should look similar to this:</span></span>

[!code-xml[Main](use-open-types-in-odata-v4/samples/sample6.xml?highlight=5,21)]

<span data-ttu-id="cd4c1-152">Из документа метаданных можно увидеть, что:</span><span class="sxs-lookup"><span data-stu-id="cd4c1-152">From the metadata document, you can see that:</span></span>

- <span data-ttu-id="cd4c1-153">Для `Book` и `Press` типов — значение `OpenType` атрибут имеет значение true.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-153">For the `Book` and `Press` types, the value of the `OpenType` attribute is true.</span></span> <span data-ttu-id="cd4c1-154">`Customer` И `Address` типы не имеют этого атрибута.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-154">The `Customer` and `Address` types don't have this attribute.</span></span>
- <span data-ttu-id="cd4c1-155">`Book` Тип сущности имеет три объявленных свойств: ISBN, заголовок и нажмите клавишу.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-155">The `Book` entity type has three declared properties: ISBN, Title, and Press.</span></span> <span data-ttu-id="cd4c1-156">OData метаданные не содержат `Book.Properties` свойство из класса CLR.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-156">The OData metadata does not include the `Book.Properties` property from the CLR class.</span></span>
- <span data-ttu-id="cd4c1-157">Аналогичным образом `Press` сложный тип имеет только два объявленных свойств: имени и категории.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-157">Similarly, the `Press` complex type has only two declared properties: Name and Category.</span></span> <span data-ttu-id="cd4c1-158">Метаданные не содержат `Press.DynamicProperties` свойство из класса CLR.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-158">The metadata does not include the `Press.DynamicProperties` property from the CLR class.</span></span>

## <a name="query-an-entity"></a><span data-ttu-id="cd4c1-159">Запрос сущности</span><span class="sxs-lookup"><span data-stu-id="cd4c1-159">Query an Entity</span></span>

<span data-ttu-id="cd4c1-160">Чтобы получить в книге с ISBN, равным «978-0-7356-7942-9», отправьте запрос GET для `~/Books('978-0-7356-7942-9')`.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-160">To get the book with ISBN equal to "978-0-7356-7942-9", send a GET request to `~/Books('978-0-7356-7942-9')`.</span></span> <span data-ttu-id="cd4c1-161">Текст ответа должен выглядеть следующим образом.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-161">The response body should look similar to the following.</span></span> <span data-ttu-id="cd4c1-162">(Отступ сделать код более удобочитаемым.)</span><span class="sxs-lookup"><span data-stu-id="cd4c1-162">(Indented to make it more readable.)</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample7.cmd?highlight=8-13,15-23)]

<span data-ttu-id="cd4c1-163">Обратите внимание, что динамические свойства встраивается внутрь объявленные свойства.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-163">Notice that the dynamic properties are included inline with the declared properties.</span></span>

## <a name="post-an-entity"></a><span data-ttu-id="cd4c1-164">POST сущности</span><span class="sxs-lookup"><span data-stu-id="cd4c1-164">POST an Entity</span></span>

<span data-ttu-id="cd4c1-165">Чтобы добавить сущность книги, отправьте запрос POST к `~/Books`.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-165">To add a Book entity, send a POST request to `~/Books`.</span></span> <span data-ttu-id="cd4c1-166">Клиент может задать динамические свойства в полезных данных запроса.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-166">The client can set dynamic properties in the request payload.</span></span>

<span data-ttu-id="cd4c1-167">Ниже приведен пример запроса.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-167">Here is an example request.</span></span> <span data-ttu-id="cd4c1-168">Обратите внимание, в свойствах «Price» и «Опубликовано».</span><span class="sxs-lookup"><span data-stu-id="cd4c1-168">Note the "Price" and "Published" properties.</span></span>

[!code-console[Main](use-open-types-in-odata-v4/samples/sample8.cmd?highlight=10)]

<span data-ttu-id="cd4c1-169">Если установить точку останова в методе контроллера, вы увидите, что веб-API добавлены эти свойства, чтобы `Properties` словаря.</span><span class="sxs-lookup"><span data-stu-id="cd4c1-169">If you set a breakpoint in the controller method, you can see that Web API added these properties to the `Properties` dictionary.</span></span>

![](use-open-types-in-odata-v4/_static/image1.png)

## <a name="additional-resources"></a><span data-ttu-id="cd4c1-170">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="cd4c1-170">Additional Resources</span></span>

[<span data-ttu-id="cd4c1-171">Образец типа открытым OData</span><span class="sxs-lookup"><span data-stu-id="cd4c1-171">OData Open Type Sample</span></span>](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v4/ODataOpenTypeSample/ReadMe.txt)
