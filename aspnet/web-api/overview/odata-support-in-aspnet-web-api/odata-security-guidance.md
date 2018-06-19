---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
title: Рекомендации по безопасности для ASP.NET Web API 2 OData | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/06/2013
ms.topic: article
ms.assetid: b91e6424-1544-4747-bd0b-d1f8418c9653
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-security-guidance
msc.type: authoredcontent
ms.openlocfilehash: 41b05f2a2f8247853d8358e6cc1246c8b438a6db
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868712"
---
<a name="security-guidance-for-aspnet-web-api-2-odata"></a><span data-ttu-id="90036-102">Рекомендации по безопасности для ASP.NET Web API 2 OData</span><span class="sxs-lookup"><span data-stu-id="90036-102">Security Guidance for ASP.NET Web API 2 OData</span></span>
====================
<span data-ttu-id="90036-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="90036-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="90036-104">В этом разделе описываются некоторые проблемы безопасности, которые следует учитывать при предоставлении набора данных через OData.</span><span class="sxs-lookup"><span data-stu-id="90036-104">This topic describes some of the security issues that you should consider when exposing a dataset through OData.</span></span>

## <a name="edm-security"></a><span data-ttu-id="90036-105">Модель EDM безопасности</span><span class="sxs-lookup"><span data-stu-id="90036-105">EDM Security</span></span>

<span data-ttu-id="90036-106">Семантики запросов основаны на модели EDM (модель EDM), не базовых типов модели.</span><span class="sxs-lookup"><span data-stu-id="90036-106">The query semantics are based on the entity data model (EDM), not the underlying model types.</span></span> <span data-ttu-id="90036-107">Можно исключить свойство из модели EDM, и он не будет видимым в запрос.</span><span class="sxs-lookup"><span data-stu-id="90036-107">You can exclude a property from the EDM and it will not be visible to the query.</span></span> <span data-ttu-id="90036-108">Например предположим, что модель включает в себя тип со свойством заработной платы сотрудника.</span><span class="sxs-lookup"><span data-stu-id="90036-108">For example, suppose your model includes an Employee type with a Salary property.</span></span> <span data-ttu-id="90036-109">Может потребоваться исключить это свойство из модели EDM, чтобы скрыть его от клиентов.</span><span class="sxs-lookup"><span data-stu-id="90036-109">You might want to exclude this property from the EDM to hide it from clients.</span></span>

<span data-ttu-id="90036-110">Существует два способа исключает свойство из модели EDM.</span><span class="sxs-lookup"><span data-stu-id="90036-110">There are two ways to exlude a property from the EDM.</span></span> <span data-ttu-id="90036-111">Можно задать **[IgnoreDataMember]** атрибут свойства в класс модели:</span><span class="sxs-lookup"><span data-stu-id="90036-111">You can set the **[IgnoreDataMember]** attribute on the property in the model class:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample1.cs)]

<span data-ttu-id="90036-112">Можно также удалить свойство из модели EDM программными средствами.</span><span class="sxs-lookup"><span data-stu-id="90036-112">You can also remove the property from the EDM programmatically:</span></span>

[!code-csharp[Main](odata-security-guidance/samples/sample2.cs)]

## <a name="query-security"></a><span data-ttu-id="90036-113">Запрос безопасности</span><span class="sxs-lookup"><span data-stu-id="90036-113">Query Security</span></span>

<span data-ttu-id="90036-114">Вредоносного или упрощенного клиента можно создать запрос, который занимает очень много времени для выполнения.</span><span class="sxs-lookup"><span data-stu-id="90036-114">A malicious or naive client may be able to construct a query that takes a very long time to execute.</span></span> <span data-ttu-id="90036-115">В худшем случае это может нарушить доступ к службе.</span><span class="sxs-lookup"><span data-stu-id="90036-115">In the worst case this can disrupt access to your service.</span></span>

<span data-ttu-id="90036-116">**[Queryable]** атрибут является фильтр действий, который выполняет синтаксический анализ, проверяет и применяет запрос.</span><span class="sxs-lookup"><span data-stu-id="90036-116">The **[Queryable]** attribute is an action filter that parses, validates, and applies the query.</span></span> <span data-ttu-id="90036-117">Фильтр преобразует параметры запроса в выражении LINQ.</span><span class="sxs-lookup"><span data-stu-id="90036-117">The filter converts the query options into a LINQ expression.</span></span> <span data-ttu-id="90036-118">Если контроллер OData возвращает **IQueryable** типа, **IQueryable** LINQ поставщик преобразует выражение LINQ в запрос.</span><span class="sxs-lookup"><span data-stu-id="90036-118">When the OData controller returns an **IQueryable** type, the **IQueryable** LINQ provider converts the LINQ expression into a query.</span></span> <span data-ttu-id="90036-119">Таким образом производительность зависит от поставщика LINQ, который можно использовать и определенными характеристиками схемы набора данных или базы данных.</span><span class="sxs-lookup"><span data-stu-id="90036-119">Therefore, performance depends on the LINQ provider that is used, and also on the particular characteristics of your dataset or database schema.</span></span>

<span data-ttu-id="90036-120">Дополнительные сведения об использовании параметров запроса OData в веб-API ASP.NET см. в разделе [поддержки параметров запроса OData](supporting-odata-query-options.md).</span><span class="sxs-lookup"><span data-stu-id="90036-120">For more information about using OData query options in ASP.NET Web API, see [Supporting OData Query Options](supporting-odata-query-options.md).</span></span>

<span data-ttu-id="90036-121">Если известно, что все клиенты доверия (например, в корпоративной среде) или небольшой набор данных, производительность запросов может оказаться проблемой.</span><span class="sxs-lookup"><span data-stu-id="90036-121">If you know that all clients are trusted (for example, in an enterprise environment), or if your dataset is small, query performance might not be an issue.</span></span> <span data-ttu-id="90036-122">В противном случае рассмотрите следующие рекомендации.</span><span class="sxs-lookup"><span data-stu-id="90036-122">Otherwise, you should consider the following recommendations.</span></span>

- <span data-ttu-id="90036-123">Тестирование службы с различных запросов и профиля базы данных.</span><span class="sxs-lookup"><span data-stu-id="90036-123">Test your service with various queries and profile the DB.</span></span>
- <span data-ttu-id="90036-124">Включите сервер разбиение на страницы предотвратить возвращение большого количества данных в одном запросе.</span><span class="sxs-lookup"><span data-stu-id="90036-124">Enable server-driven paging, to avoid returning a large data set in one query.</span></span> <span data-ttu-id="90036-125">Дополнительные сведения см. в разделе [Server-Driven подкачки](supporting-odata-query-options.md#server-paging).</span><span class="sxs-lookup"><span data-stu-id="90036-125">For more information, see [Server-Driven Paging](supporting-odata-query-options.md#server-paging).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample3.cs)]
- <span data-ttu-id="90036-126">Требуется $filter и $orderby?</span><span class="sxs-lookup"><span data-stu-id="90036-126">Do you need $filter and $orderby?</span></span> <span data-ttu-id="90036-127">Для некоторых приложений может разрешить клиент разбиения по страницам, используя $top и $skip, но отключите другие параметры запроса.</span><span class="sxs-lookup"><span data-stu-id="90036-127">Some applications might allow client paging, using $top and $skip, but disable the other query options.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample4.cs)]
- <span data-ttu-id="90036-128">Рекомендуется ограничить $orderby к свойствам в кластеризованном индексе.</span><span class="sxs-lookup"><span data-stu-id="90036-128">Consider restricting $orderby to properties in a clustered index.</span></span> <span data-ttu-id="90036-129">Сортировка больших объемов данных в кластеризованном индексе происходит слишком медленно.</span><span class="sxs-lookup"><span data-stu-id="90036-129">Sorting large data without a clustered index is slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample5.cs)]
- <span data-ttu-id="90036-130">Количество узлов максимальное: **MaxNodeCount** свойство **[Queryable]** задает максимальное число узлов, которые разрешены в синтаксического дерева $filter.</span><span class="sxs-lookup"><span data-stu-id="90036-130">Maximum node count: The **MaxNodeCount** property on **[Queryable]** sets the maximum number nodes allowed in the $filter syntax tree.</span></span> <span data-ttu-id="90036-131">Значение по умолчанию — 100, но вы можете задать более низкое значение, так как большое количество узлов может быть медленным для компиляции.</span><span class="sxs-lookup"><span data-stu-id="90036-131">The default value is 100, but you may want to set a lower value, because a large number of nodes can be slow to compile.</span></span> <span data-ttu-id="90036-132">Это особенно верно, если вы используете LINQ to Objects (т. е. запросов LINQ к коллекции в памяти, без использования промежуточного поставщика LINQ).</span><span class="sxs-lookup"><span data-stu-id="90036-132">This is particularly true if you are using LINQ to Objects (i.e., LINQ queries on a collection in memory, without the use of an intermediate LINQ provider).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample6.cs)]
- <span data-ttu-id="90036-133">Рекомендуется отключить функции any() и all(), как это может быть медленным.</span><span class="sxs-lookup"><span data-stu-id="90036-133">Consider disabling the any() and all() functions, as these can be slow.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample7.cs)]
- <span data-ttu-id="90036-134">Если любые свойства строки содержат больших строк & #8212for пример, описание продукта или запись в блоге & #8212consider отключение строковые функции.</span><span class="sxs-lookup"><span data-stu-id="90036-134">If any string properties contain large strings&#8212for example, a product description or a blog entry&#8212consider disabling the string functions.</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample8.cs)]
- <span data-ttu-id="90036-135">Рассмотрите возможность запрета, фильтрация по свойствам навигации.</span><span class="sxs-lookup"><span data-stu-id="90036-135">Consider disallowing filtering on navigation properties.</span></span> <span data-ttu-id="90036-136">Фильтрация по свойствам навигации может привести к соединению, которое может быть медленным, в зависимости от схемы базы данных.</span><span class="sxs-lookup"><span data-stu-id="90036-136">Filtering on navigation properties can result in a join, which might be slow, depending on your database schema.</span></span> <span data-ttu-id="90036-137">В следующем коде показано запроса проверяющий элемент управления, не позволяющая фильтрация по свойствам навигации.</span><span class="sxs-lookup"><span data-stu-id="90036-137">The following code shows a query validator that prevents filtering on navigation properties.</span></span> <span data-ttu-id="90036-138">Дополнительные сведения о запросе проверяющие элементы управления см. в разделе [проверки запроса](supporting-odata-query-options.md#query-validation).</span><span class="sxs-lookup"><span data-stu-id="90036-138">For more information about query validators, see [Query Validation](supporting-odata-query-options.md#query-validation).</span></span> 

    [!code-csharp[Main](odata-security-guidance/samples/sample9.cs)]
- <span data-ttu-id="90036-139">Рекомендуется ограничить запросов $filter записывая проверяющий элемент управления, настроенную для базы данных.</span><span class="sxs-lookup"><span data-stu-id="90036-139">Consider restricting $filter queries by writing a validator that is customized for your database.</span></span> <span data-ttu-id="90036-140">Например рассмотрим следующие два запроса:</span><span class="sxs-lookup"><span data-stu-id="90036-140">For example, consider these two queries:</span></span> 

  - <span data-ttu-id="90036-141">Все фильмы с субъектами, чья фамилия начинается с «А».</span><span class="sxs-lookup"><span data-stu-id="90036-141">All movies with actors whose last name starts with ‘A'.</span></span>
  - <span data-ttu-id="90036-142">Все фильмы, выпущенные в 1994 г.</span><span class="sxs-lookup"><span data-stu-id="90036-142">All movies released in 1994.</span></span>

    <span data-ttu-id="90036-143">Если субъекты индексированных фильмы, первый запрос может потребоваться ядра СУБД, чтобы просмотреть весь список фильмов.</span><span class="sxs-lookup"><span data-stu-id="90036-143">Unless movies are indexed by actors, the first query might require the DB engine to scan the entire list of movies.</span></span> <span data-ttu-id="90036-144">В то время как второй запрос может быть приемлемым, если предполагается, что фильмы индексируются по году выпуска.</span><span class="sxs-lookup"><span data-stu-id="90036-144">Whereas the second query might be acceptable, assuming movies are indexed by release year.</span></span>

    <span data-ttu-id="90036-145">В следующем коде показано проверяющий элемент управления позволяет фильтровать свойства «ReleaseYear» и «Title», но не других свойств.</span><span class="sxs-lookup"><span data-stu-id="90036-145">The following code shows a validator that allows filtering on the "ReleaseYear" and "Title" properties but no other properties.</span></span>

    [!code-csharp[Main](odata-security-guidance/samples/sample10.cs)]
- <span data-ttu-id="90036-146">В общем случае рекомендуется какие функции $filter.</span><span class="sxs-lookup"><span data-stu-id="90036-146">In general, consider which $filter functions you need.</span></span> <span data-ttu-id="90036-147">Если клиентам не требуется полный выразительность $filter, можно ограничить допустимых функций.</span><span class="sxs-lookup"><span data-stu-id="90036-147">If your clients do not need the full expressiveness of $filter, you can limit the allowed functions.</span></span>
