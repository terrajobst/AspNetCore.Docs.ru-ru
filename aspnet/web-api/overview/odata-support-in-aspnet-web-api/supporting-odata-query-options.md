---
uid: web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
title: "Поддержка параметров запроса OData в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/04/2013
ms.topic: article
ms.assetid: 50e6e62b-e72e-4a29-8293-4b67377bd21f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/supporting-odata-query-options
msc.type: authoredcontent
ms.openlocfilehash: 004c029db6f01627f7cadff26aaf5554ce2b93a5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="supporting-odata-query-options-in-aspnet-web-api-2"></a><span data-ttu-id="3698a-102">Поддержка параметров запроса OData в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="3698a-102">Supporting OData Query Options in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3698a-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3698a-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3698a-104">OData определяет параметры, которые можно использовать для изменения запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3698a-104">OData defines parameters that can be used to modify an OData query.</span></span> <span data-ttu-id="3698a-105">Клиент отправляет эти параметры в строке запроса URI запроса.</span><span class="sxs-lookup"><span data-stu-id="3698a-105">The client sends these parameters in the query string of the request URI.</span></span> <span data-ttu-id="3698a-106">Например чтобы отсортировать результаты, клиент использует параметр $orderby:</span><span class="sxs-lookup"><span data-stu-id="3698a-106">For example, to sort the results, a client uses the $orderby parameter:</span></span>

`http://localhost/Products?$orderby=Name`

<span data-ttu-id="3698a-107">Спецификация протокола OData вызывает эти параметры *параметры запроса*.</span><span class="sxs-lookup"><span data-stu-id="3698a-107">The OData specification calls these parameters *query options*.</span></span> <span data-ttu-id="3698a-108">Можно включить параметры запроса OData для любого контроллера веб-API в проекте &#8212; контроллер не требуется быть конечная точка OData.</span><span class="sxs-lookup"><span data-stu-id="3698a-108">You can enable OData query options for any Web API controller in your project &#8212; the controller does not need to be an OData endpoint.</span></span> <span data-ttu-id="3698a-109">Это обеспечивает удобный способ добавления компонентов, таких как фильтрация и сортировка, любое приложение веб-API.</span><span class="sxs-lookup"><span data-stu-id="3698a-109">This gives you a convenient way to add features such as filtering and sorting to any Web API application.</span></span>

<span data-ttu-id="3698a-110">Прежде чем включать параметры запроса, ознакомьтесь с разделом [рекомендации по безопасности OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="3698a-110">Before enabling query options, please read the topic [OData Security Guidance](odata-security-guidance.md).</span></span>

- [<span data-ttu-id="3698a-111">Включение параметров запроса OData</span><span class="sxs-lookup"><span data-stu-id="3698a-111">Enabling OData Query Options</span></span>](#enable)
- [<span data-ttu-id="3698a-112">Примеры запросов</span><span class="sxs-lookup"><span data-stu-id="3698a-112">Example Queries</span></span>](#examples)
- [<span data-ttu-id="3698a-113">Разбиение на страницы сервера</span><span class="sxs-lookup"><span data-stu-id="3698a-113">Server-Driven Paging</span></span>](#server-paging)
- [<span data-ttu-id="3698a-114">Ограничения параметров запроса</span><span class="sxs-lookup"><span data-stu-id="3698a-114">Limiting the Query Options</span></span>](#limiting_query_options)
- [<span data-ttu-id="3698a-115">Вызов непосредственно параметры запроса</span><span class="sxs-lookup"><span data-stu-id="3698a-115">Invoking Query Options Directly</span></span>](#ODataQueryOptions)
- [<span data-ttu-id="3698a-116">Проверка запроса</span><span class="sxs-lookup"><span data-stu-id="3698a-116">Query Validation</span></span>](#query-validation)

<a id="enable"></a>
## <a name="enabling-odata-query-options"></a><span data-ttu-id="3698a-117">Включение параметров запроса OData</span><span class="sxs-lookup"><span data-stu-id="3698a-117">Enabling OData Query Options</span></span>

<span data-ttu-id="3698a-118">Веб-API поддерживает следующие параметры запроса OData:</span><span class="sxs-lookup"><span data-stu-id="3698a-118">Web API supports the following OData query options:</span></span>

| <span data-ttu-id="3698a-119">Параметр</span><span class="sxs-lookup"><span data-stu-id="3698a-119">Option</span></span> | <span data-ttu-id="3698a-120">Описание</span><span class="sxs-lookup"><span data-stu-id="3698a-120">Description</span></span> |
| --- | --- |
| <span data-ttu-id="3698a-121">$expand</span><span class="sxs-lookup"><span data-stu-id="3698a-121">$expand</span></span> | <span data-ttu-id="3698a-122">При развертывании встроенного связанных сущностей.</span><span class="sxs-lookup"><span data-stu-id="3698a-122">Expands related entities inline.</span></span> |
| <span data-ttu-id="3698a-123">$filter</span><span class="sxs-lookup"><span data-stu-id="3698a-123">$filter</span></span> | <span data-ttu-id="3698a-124">Фильтрует результаты на основе логического условия.</span><span class="sxs-lookup"><span data-stu-id="3698a-124">Filters the results, based on a Boolean condition.</span></span> |
| <span data-ttu-id="3698a-125">$inlinecount</span><span class="sxs-lookup"><span data-stu-id="3698a-125">$inlinecount</span></span> | <span data-ttu-id="3698a-126">Указывает, что сервер для включения общее число соответствующих сущностей в ответе.</span><span class="sxs-lookup"><span data-stu-id="3698a-126">Tells the server to include the total count of matching entities in the response.</span></span> <span data-ttu-id="3698a-127">(Полезно для постраничный просмотр на стороне сервера).</span><span class="sxs-lookup"><span data-stu-id="3698a-127">(Useful for server-side paging.)</span></span> |
| <span data-ttu-id="3698a-128">$orderby</span><span class="sxs-lookup"><span data-stu-id="3698a-128">$orderby</span></span> | <span data-ttu-id="3698a-129">Сортирует результаты.</span><span class="sxs-lookup"><span data-stu-id="3698a-129">Sorts the results.</span></span> |
| <span data-ttu-id="3698a-130">$select</span><span class="sxs-lookup"><span data-stu-id="3698a-130">$select</span></span> | <span data-ttu-id="3698a-131">Выбирает, какие свойства будут включены в ответе.</span><span class="sxs-lookup"><span data-stu-id="3698a-131">Selects which properties to include in the response.</span></span> |
| <span data-ttu-id="3698a-132">$skip</span><span class="sxs-lookup"><span data-stu-id="3698a-132">$skip</span></span> | <span data-ttu-id="3698a-133">Пропускает первые n результатов.</span><span class="sxs-lookup"><span data-stu-id="3698a-133">Skips the first n results.</span></span> |
| <span data-ttu-id="3698a-134">$top</span><span class="sxs-lookup"><span data-stu-id="3698a-134">$top</span></span> | <span data-ttu-id="3698a-135">Возвращает только первые n результатов.</span><span class="sxs-lookup"><span data-stu-id="3698a-135">Returns only the first n the results.</span></span> |

<span data-ttu-id="3698a-136">Чтобы использовать параметры запроса OData, необходимо включить их явно.</span><span class="sxs-lookup"><span data-stu-id="3698a-136">To use OData query options, you must enable them explicitly.</span></span> <span data-ttu-id="3698a-137">Можно включить глобально для всего приложения или включить их для конкретных контроллеров или определенных действий.</span><span class="sxs-lookup"><span data-stu-id="3698a-137">You can enable them globally for the entire application, or enable them for specific controllers or specific actions.</span></span>

<span data-ttu-id="3698a-138">Чтобы включить параметры запроса OData глобально, вызовите **EnableQuerySupport** на **HttpConfiguration** класс во время запуска:</span><span class="sxs-lookup"><span data-stu-id="3698a-138">To enable OData query options globally, call **EnableQuerySupport** on the **HttpConfiguration** class at startup:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample1.cs)]

<span data-ttu-id="3698a-139">**EnableQuerySupport** метод включает параметры запроса глобально для любого действия контроллера, который возвращает **IQueryable** типа.</span><span class="sxs-lookup"><span data-stu-id="3698a-139">The **EnableQuerySupport** method enables query options globally for any controller action that returns an **IQueryable** type.</span></span> <span data-ttu-id="3698a-140">Если вы не хотите параметры запроса, которые включены для всего приложения, можно включить их для действий конкретного контроллера, добавив **[Queryable]** атрибут к методу действия.</span><span class="sxs-lookup"><span data-stu-id="3698a-140">If you don't want query options enabled for the entire application, you can enable them for specific controller actions by adding the **[Queryable]** attribute to the action method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample2.cs)]

<a id="examples"></a>
## <a name="example-queries"></a><span data-ttu-id="3698a-141">Примеры запросов</span><span class="sxs-lookup"><span data-stu-id="3698a-141">Example Queries</span></span>

<span data-ttu-id="3698a-142">В этом разделе показаны типы запросов, которые, возможно с помощью параметров запроса OData.</span><span class="sxs-lookup"><span data-stu-id="3698a-142">This section shows the types of queries that are possible using the OData query options.</span></span> <span data-ttu-id="3698a-143">Подробности о параметрах запроса см. по адресу OData [www.odata.org](http://www.odata.org/).</span><span class="sxs-lookup"><span data-stu-id="3698a-143">For specific details about the query options, refer to the OData documentation at [www.odata.org](http://www.odata.org/).</span></span>

<span data-ttu-id="3698a-144">Сведения о $разверните и $select, в разделе [развернуть с помощью $select, $ и $value в ASP.NET Web API OData](using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="3698a-144">For information about $expand and $select, see [Using $select, $expand, and $value in ASP.NET Web API OData](using-select-expand-and-value.md).</span></span>

<span data-ttu-id="3698a-145">**Разбиение на страницы клиента**</span><span class="sxs-lookup"><span data-stu-id="3698a-145">**Client-Driven Paging**</span></span>

<span data-ttu-id="3698a-146">Для больших наборов сущностей клиент может потребоваться ограничить количество результатов.</span><span class="sxs-lookup"><span data-stu-id="3698a-146">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="3698a-147">Например клиент может показывать 10 записей одновременно со ссылками «Далее» для получения следующей страницы результатов.</span><span class="sxs-lookup"><span data-stu-id="3698a-147">For example, a client might show 10 entries at a time, with "next" links to get the next page of results.</span></span> <span data-ttu-id="3698a-148">Чтобы сделать это, клиент использует параметры $top и $skip.</span><span class="sxs-lookup"><span data-stu-id="3698a-148">To do this, the client uses the $top and $skip options.</span></span>

`http://localhost/Products?$top=10&$skip=20`

<span data-ttu-id="3698a-149">Параметр $top дает максимальное количество возвращаемых записей, а параметр $skip дает число записей для пропуска.</span><span class="sxs-lookup"><span data-stu-id="3698a-149">The $top option gives the maximum number of entries to return, and the $skip option gives the number of entries to skip.</span></span> <span data-ttu-id="3698a-150">Предыдущий пример извлекает записи 21-30.</span><span class="sxs-lookup"><span data-stu-id="3698a-150">The previous example fetches entries 21 through 30.</span></span>

<span data-ttu-id="3698a-151">**Фильтрация**</span><span class="sxs-lookup"><span data-stu-id="3698a-151">**Filtering**</span></span>

<span data-ttu-id="3698a-152">Параметр $filter позволяет клиенту фильтрации результатов путем применения логического выражения.</span><span class="sxs-lookup"><span data-stu-id="3698a-152">The $filter option lets a client filter the results by applying a Boolean expression.</span></span> <span data-ttu-id="3698a-153">Критерии фильтра весьма значительны; они включают логические и арифметические операторы, строковые функции и функции даты.</span><span class="sxs-lookup"><span data-stu-id="3698a-153">The filter expressions are quite powerful; they include logical and arithmetic operators, string functions, and date functions.</span></span>

| <span data-ttu-id="3698a-154">Возвратить все продукты с категорией равно «Игрушки».</span><span class="sxs-lookup"><span data-stu-id="3698a-154">Return all products with category equal to "Toys".</span></span> | <span data-ttu-id="3698a-155">`http://localhost/Products?$filter=Category`EQ 'Toys»</span><span class="sxs-lookup"><span data-stu-id="3698a-155">`http://localhost/Products?$filter=Category` eq 'Toys'</span></span> |
| --- | --- |
| <span data-ttu-id="3698a-156">Возвратить все продукты с ценой меньше 10.</span><span class="sxs-lookup"><span data-stu-id="3698a-156">Return all products with price less than 10.</span></span> | <span data-ttu-id="3698a-157">`http://localhost/Products?$filter=Price`lt 10</span><span class="sxs-lookup"><span data-stu-id="3698a-157">`http://localhost/Products?$filter=Price` lt 10</span></span> |
| <span data-ttu-id="3698a-158">Логические операторы: возврата всех продуктов, цена которых > = 5 и цена < = 15.</span><span class="sxs-lookup"><span data-stu-id="3698a-158">Logical operators: Return all products where price >= 5 and price <= 15.</span></span> | <span data-ttu-id="3698a-159">`http://localhost/Products?$filter=Price`GE 5 и цена le 15</span><span class="sxs-lookup"><span data-stu-id="3698a-159">`http://localhost/Products?$filter=Price` ge 5 and Price le 15</span></span> |
| <span data-ttu-id="3698a-160">Строковые функции: возвратить все продукты с «zz» в имени.</span><span class="sxs-lookup"><span data-stu-id="3698a-160">String functions: Return all products with "zz" in the name.</span></span> | `http://localhost/Products?$filter=substringof('zz',Name)` |
| <span data-ttu-id="3698a-161">Функции даты: возвратить все продукты с ReleaseDate за 2005.</span><span class="sxs-lookup"><span data-stu-id="3698a-161">Date functions: Return all products with ReleaseDate after 2005.</span></span> | <span data-ttu-id="3698a-162">`http://localhost/Products?$filter=year(ReleaseDate)`gt 2005</span><span class="sxs-lookup"><span data-stu-id="3698a-162">`http://localhost/Products?$filter=year(ReleaseDate)` gt 2005</span></span> |

<span data-ttu-id="3698a-163">**Сортировка**</span><span class="sxs-lookup"><span data-stu-id="3698a-163">**Sorting**</span></span>

<span data-ttu-id="3698a-164">Чтобы отсортировать результаты, используйте фильтр $orderby.</span><span class="sxs-lookup"><span data-stu-id="3698a-164">To sort the results, use the $orderby filter.</span></span>

| <span data-ttu-id="3698a-165">Сортировка по цене.</span><span class="sxs-lookup"><span data-stu-id="3698a-165">Sort by price.</span></span> | `http://localhost/Products?$orderby=Price` |
| --- | --- |
| <span data-ttu-id="3698a-166">Сортировка по цене в убывающем порядке (от большего к меньшему).</span><span class="sxs-lookup"><span data-stu-id="3698a-166">Sort by price in descending order (highest to lowest).</span></span> | `http://localhost/Products?$orderby=Price desc` |
| <span data-ttu-id="3698a-167">Сортировка по категориям, а затем отсортировать по цене в нисходящем порядке по категориям.</span><span class="sxs-lookup"><span data-stu-id="3698a-167">Sort by category, then sort by price in descending order within categories.</span></span> | `http://localhost/odata/Products?$orderby=Category,Price desc` |

<a id="server-paging"></a>
## <a name="server-driven-paging"></a><span data-ttu-id="3698a-168">Разбиение на страницы сервера</span><span class="sxs-lookup"><span data-stu-id="3698a-168">Server-Driven Paging</span></span>

<span data-ttu-id="3698a-169">Если база данных содержит миллионы записей, вы не хотите отправлять их все в одной полезной нагрузке.</span><span class="sxs-lookup"><span data-stu-id="3698a-169">If your database contains millions of records, you don't want to send them all in one payload.</span></span> <span data-ttu-id="3698a-170">Чтобы избежать этого, сервер можно ограничить число записей, он отправляет в одном ответе.</span><span class="sxs-lookup"><span data-stu-id="3698a-170">To prevent this, the server can limit the number of entries that it sends in a single response.</span></span> <span data-ttu-id="3698a-171">Чтобы включить разбиение на страницы сервера, задайте **PageSize** свойство в **Queryable** атрибута.</span><span class="sxs-lookup"><span data-stu-id="3698a-171">To enable server paging, set the **PageSize** property in the **Queryable** attribute.</span></span> <span data-ttu-id="3698a-172">Значение — максимальное количество возвращаемых элементов.</span><span class="sxs-lookup"><span data-stu-id="3698a-172">The value is the maximum number of entries to return.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample3.cs)]

<span data-ttu-id="3698a-173">Если ваш контроллер возвращает формат OData, текст ответа будет содержать ссылку на следующую страницу данных:</span><span class="sxs-lookup"><span data-stu-id="3698a-173">If your controller returns OData format, the response body will contain a link to the next page of data:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample4.json?highlight=8)]

<span data-ttu-id="3698a-174">Клиент может использовать эту ссылку для получения следующей страницы.</span><span class="sxs-lookup"><span data-stu-id="3698a-174">The client can use this link to fetch the next page.</span></span> <span data-ttu-id="3698a-175">Чтобы получить общее число записей в результирующем наборе, клиент может задать значение параметра запроса $inlinecount «allpages».</span><span class="sxs-lookup"><span data-stu-id="3698a-175">To learn the total number of entries in the result set, the client can set the $inlinecount query option with the value "allpages".</span></span>

`http://localhost/Products?$inlinecount=allpages`

<span data-ttu-id="3698a-176">Значение «allpages» указывает, что сервер для включения общее число объектов в ответе:</span><span class="sxs-lookup"><span data-stu-id="3698a-176">The value "allpages" tells the server to include the total count in the response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample5.json?highlight=3)]

> [!NOTE]
> <span data-ttu-id="3698a-177">Ссылки следующей страницы и встроенное количество, требуют формате OData.</span><span class="sxs-lookup"><span data-stu-id="3698a-177">Next-page links and inline count both require OData format.</span></span> <span data-ttu-id="3698a-178">Причина в том, что OData определяет специальные поля в тексте ответа для хранения ссылки и count.</span><span class="sxs-lookup"><span data-stu-id="3698a-178">The reason is that OData defines special fields in the response body to hold the link and count.</span></span>


<span data-ttu-id="3698a-179">Для форматов не OData можно по-прежнему поддерживает счетчик ссылок и встроенные следующей страницы, заключив результаты запроса в **PageResult&lt;T&gt;**  объекта.</span><span class="sxs-lookup"><span data-stu-id="3698a-179">For non-OData formats, it is still possible to support next-page links and inline count, by wrapping the query results in a **PageResult&lt;T&gt;** object.</span></span> <span data-ttu-id="3698a-180">Однако он требует немного больше кода.</span><span class="sxs-lookup"><span data-stu-id="3698a-180">However, it requires a bit more code.</span></span> <span data-ttu-id="3698a-181">Пример:</span><span class="sxs-lookup"><span data-stu-id="3698a-181">Here is an example:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample6.cs)]

<span data-ttu-id="3698a-182">Ниже приведен пример ответа JSON:</span><span class="sxs-lookup"><span data-stu-id="3698a-182">Here is an example JSON response:</span></span>

[!code-json[Main](supporting-odata-query-options/samples/sample7.json)]

<a id="limiting_query_options"></a>
## <a name="limiting-the-query-options"></a><span data-ttu-id="3698a-183">Ограничения параметров запроса</span><span class="sxs-lookup"><span data-stu-id="3698a-183">Limiting the Query Options</span></span>

<span data-ttu-id="3698a-184">Параметры запроса предоставьте клиенту много контроль над запроса, который выполняется на сервере.</span><span class="sxs-lookup"><span data-stu-id="3698a-184">The query options give the client a lot of control over the query that is run on the server.</span></span> <span data-ttu-id="3698a-185">В некоторых случаях может потребоваться ограничить доступные параметры для повышения производительности и безопасности.</span><span class="sxs-lookup"><span data-stu-id="3698a-185">In some cases, you might want to limit the available options for security or performance reasons.</span></span> <span data-ttu-id="3698a-186">**[Queryable]** атрибута есть некоторые встроенные свойства для этого.</span><span class="sxs-lookup"><span data-stu-id="3698a-186">The **[Queryable]** attribute has some built in properties for this.</span></span> <span data-ttu-id="3698a-187">Ниже приводятся некоторые примеры.</span><span class="sxs-lookup"><span data-stu-id="3698a-187">Here are some examples.</span></span>

<span data-ttu-id="3698a-188">Разрешить только $skip и $top, для поддержки разбиения на страницы и ничего более:</span><span class="sxs-lookup"><span data-stu-id="3698a-188">Allow only $skip and $top, to support paging and nothing else:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample8.cs)]

<span data-ttu-id="3698a-189">Разрешается только в определенные свойства запретить сортировку по свойствам, которые не проиндексированы в базе данных:</span><span class="sxs-lookup"><span data-stu-id="3698a-189">Allow ordering only by certain properties, to prevent sorting on properties that are not indexed in the database:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample9.cs)]

<span data-ttu-id="3698a-190">Разрешить использование логической функции «eq», но не логические функции:</span><span class="sxs-lookup"><span data-stu-id="3698a-190">Allow the "eq" logical function but no other logical functions:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample10.cs)]

<span data-ttu-id="3698a-191">Не разрешать все арифметические операторы:</span><span class="sxs-lookup"><span data-stu-id="3698a-191">Do not allow any arithmetic operators:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample11.cs)]

<span data-ttu-id="3698a-192">Вы можете ограничить параметры глобально, создав **QueryableAttribute** экземпляра и передается командлету **EnableQuerySupport** функции:</span><span class="sxs-lookup"><span data-stu-id="3698a-192">You can restrict options globally by constructing a **QueryableAttribute** instance and passing it to the **EnableQuerySupport** function:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample12.cs)]

<a id="ODataQueryOptions"></a>
## <a name="invoking-query-options-directly"></a><span data-ttu-id="3698a-193">Вызов непосредственно параметры запроса</span><span class="sxs-lookup"><span data-stu-id="3698a-193">Invoking Query Options Directly</span></span>

<span data-ttu-id="3698a-194">Вместо использования **[Queryable]** атрибут, можно вызвать параметры запроса непосредственно в контроллере.</span><span class="sxs-lookup"><span data-stu-id="3698a-194">Instead of using the **[Queryable]** attribute, you can invoke the query options directly in your controller.</span></span> <span data-ttu-id="3698a-195">Чтобы сделать это, добавьте **ODataQueryOptions** параметр метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="3698a-195">To do so, add an **ODataQueryOptions** parameter to the controller method.</span></span> <span data-ttu-id="3698a-196">В этом случае не требуется **[Queryable]** атрибута.</span><span class="sxs-lookup"><span data-stu-id="3698a-196">In this case, you don't need the **[Queryable]** attribute.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample13.cs)]

<span data-ttu-id="3698a-197">Заполняет веб-API **ODataQueryOptions** строка запроса из URI.</span><span class="sxs-lookup"><span data-stu-id="3698a-197">Web API populates the **ODataQueryOptions** from the URI query string.</span></span> <span data-ttu-id="3698a-198">Чтобы применить запрос, передайте **IQueryable** для **ApplyTo** метод.</span><span class="sxs-lookup"><span data-stu-id="3698a-198">To apply the query, pass an **IQueryable** to the **ApplyTo** method.</span></span> <span data-ttu-id="3698a-199">Метод возвращает другой **IQueryable**.</span><span class="sxs-lookup"><span data-stu-id="3698a-199">The method returns another **IQueryable**.</span></span>

<span data-ttu-id="3698a-200">Для более сложных сценариев, если у вас **IQueryable** поставщик запросов, можно изучить **ODataQueryOptions** и преобразовать параметры запроса формы в другую.</span><span class="sxs-lookup"><span data-stu-id="3698a-200">For advanced scenarios, if you do not have an **IQueryable** query provider, you can examine the **ODataQueryOptions** and translate the query options into another form.</span></span> <span data-ttu-id="3698a-201">(К примеру, см. RaghuRam Nadiminti блога [запросов преобразования OData в HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), также включает [пример](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span><span class="sxs-lookup"><span data-stu-id="3698a-201">(For example, see RaghuRam Nadiminti's blog post [Translating OData queries to HQL](https://blogs.msdn.com/b/webdev/archive/2013/02/25/translating-odata-queries-to-hql.aspx), which also includes a [sample](http://aspnet.codeplex.com/SourceControl/changeset/view/75a56ec99968#Samples/WebApi/NHibernateQueryableSample/Readme.txt).)</span></span>

<a id="query-validation"></a>
## <a name="query-validation"></a><span data-ttu-id="3698a-202">Проверка запроса</span><span class="sxs-lookup"><span data-stu-id="3698a-202">Query Validation</span></span>

<span data-ttu-id="3698a-203">**[Queryable]** атрибут проверяет запрос перед его выполнением.</span><span class="sxs-lookup"><span data-stu-id="3698a-203">The **[Queryable]** attribute validates the query before executing it.</span></span> <span data-ttu-id="3698a-204">Действие проверки выполняется в **QueryableAttribute.ValidateQuery** метод.</span><span class="sxs-lookup"><span data-stu-id="3698a-204">The validation step is performed in the **QueryableAttribute.ValidateQuery** method.</span></span> <span data-ttu-id="3698a-205">Можно также настроить процесс проверки.</span><span class="sxs-lookup"><span data-stu-id="3698a-205">You can also customize the validation process.</span></span>

<span data-ttu-id="3698a-206">См. также [рекомендации по безопасности OData](odata-security-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="3698a-206">Also see [OData Security Guidance](odata-security-guidance.md).</span></span>

<span data-ttu-id="3698a-207">Во-первых, переопределение одного модуля проверки классы, определенные в **Web.Http.OData.Query.Validators** пространства имен.</span><span class="sxs-lookup"><span data-stu-id="3698a-207">First, override one of the validator classes that is defined in the **Web.Http.OData.Query.Validators** namespace.</span></span> <span data-ttu-id="3698a-208">Например приведенный ниже класс проверяющего элемента управления отключает параметр «desc» для параметра $orderby.</span><span class="sxs-lookup"><span data-stu-id="3698a-208">For example, the following validator class disables the 'desc' option for the $orderby option.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample14.cs)]

<span data-ttu-id="3698a-209">Подкласс **[Queryable]** атрибута для переопределения **ValidateQuery** метод.</span><span class="sxs-lookup"><span data-stu-id="3698a-209">Subclass the **[Queryable]** attribute to override the **ValidateQuery** method.</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample15.cs)]

<span data-ttu-id="3698a-210">Задайте пользовательский атрибут либо глобально или на контроллер:</span><span class="sxs-lookup"><span data-stu-id="3698a-210">Then set your custom attribute either globally or per-controller:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample16.cs)]

<span data-ttu-id="3698a-211">Если вы используете **ODataQueryOptions** напрямую, установить проверяющий элемент управления с параметрами:</span><span class="sxs-lookup"><span data-stu-id="3698a-211">If you are using **ODataQueryOptions** directly, set the validator on the options:</span></span>

[!code-csharp[Main](supporting-odata-query-options/samples/sample17.cs)]
