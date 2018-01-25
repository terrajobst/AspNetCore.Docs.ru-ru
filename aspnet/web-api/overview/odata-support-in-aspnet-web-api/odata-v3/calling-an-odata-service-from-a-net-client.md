---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: "Вызов службы OData из клиента .NET (C#) | Документы Microsoft"
author: MikeWasson
description: "Этот учебник показывает, как для вызова службы OData из клиентского приложения C#. Версии программного обеспечения, используемые в учебник Visual Studio 2013 (работает с Visual S..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: 497102cfa98680f2156a56ff9e36d84b7c820020
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="c59c1-104">Вызов службы OData из клиента .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="c59c1-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="c59c1-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="c59c1-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="c59c1-106">Загрузка завершенного проекта</span><span class="sxs-lookup"><span data-stu-id="c59c1-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="c59c1-107">Этот учебник показывает, как для вызова службы OData из клиентского приложения C#.</span><span class="sxs-lookup"><span data-stu-id="c59c1-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="c59c1-108">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="c59c1-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="c59c1-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (работает с Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="c59c1-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="c59c1-110">Библиотека клиентов служб данных WCF</span><span class="sxs-lookup"><span data-stu-id="c59c1-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="c59c1-111">Веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="c59c1-111">Web API 2.</span></span> <span data-ttu-id="c59c1-112">(Пример службы OData построена на основе веб-API 2, но клиентское приложение не зависит от веб-API).</span><span class="sxs-lookup"><span data-stu-id="c59c1-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="c59c1-113">В этом учебнике мы рассмотрим создание клиентского приложения, которая вызывает службу OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="c59c1-114">Служба OData предоставляет следующие сущности:</span><span class="sxs-lookup"><span data-stu-id="c59c1-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="c59c1-115">Следующие статьи описывается, как реализовать службу OData в веб-API.</span><span class="sxs-lookup"><span data-stu-id="c59c1-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="c59c1-116">(Не требуется читать их для понимания этого учебника, однако.)</span><span class="sxs-lookup"><span data-stu-id="c59c1-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="c59c1-117">Создание конечной точки OData в веб-API 2</span><span class="sxs-lookup"><span data-stu-id="c59c1-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="c59c1-118">Отношениями сущностей OData в веб-API 2</span><span class="sxs-lookup"><span data-stu-id="c59c1-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="c59c1-119">Действия OData в веб-API 2</span><span class="sxs-lookup"><span data-stu-id="c59c1-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="c59c1-120">Создать прокси-службы</span><span class="sxs-lookup"><span data-stu-id="c59c1-120">Generate the Service Proxy</span></span>

<span data-ttu-id="c59c1-121">Первым шагом является создание прокси-службы.</span><span class="sxs-lookup"><span data-stu-id="c59c1-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="c59c1-122">Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="c59c1-123">Прокси-сервер преобразует вызовы метода в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="c59c1-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="c59c1-124">Сначала откройте проект службы OData в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c59c1-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="c59c1-125">Нажмите клавиши CTRL + F5, чтобы запустить службу локально в IIS Express.</span><span class="sxs-lookup"><span data-stu-id="c59c1-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="c59c1-126">Обратите внимание, локальный адрес, включая номер порта, присвоенный Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="c59c1-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="c59c1-127">Этот адрес потребуется при создании учетной записи-посредника.</span><span class="sxs-lookup"><span data-stu-id="c59c1-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="c59c1-128">Далее откройте другой экземпляр Visual Studio и создайте проект консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="c59c1-129">Консольное приложение будет OData клиентского приложения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-129">The console application will be our OData client application.</span></span> <span data-ttu-id="c59c1-130">(Можно также добавить проект в решение службы.)</span><span class="sxs-lookup"><span data-stu-id="c59c1-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="c59c1-131">Оставшиеся шаги см. в проект.</span><span class="sxs-lookup"><span data-stu-id="c59c1-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="c59c1-132">В обозревателе решений щелкните правой кнопкой мыши **ссылки** и выберите **добавить ссылку на службу**.</span><span class="sxs-lookup"><span data-stu-id="c59c1-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="c59c1-133">В **добавить ссылку на службу** диалоговое окно, введите адрес службы OData:</span><span class="sxs-lookup"><span data-stu-id="c59c1-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="c59c1-134">где *порт* — номер порта.</span><span class="sxs-lookup"><span data-stu-id="c59c1-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="c59c1-135">Для **имен**, введите «ProductService».</span><span class="sxs-lookup"><span data-stu-id="c59c1-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="c59c1-136">Этот параметр определяет пространство имен класса-посредника.</span><span class="sxs-lookup"><span data-stu-id="c59c1-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="c59c1-137">Нажмите **Перейти**.</span><span class="sxs-lookup"><span data-stu-id="c59c1-137">Click **Go**.</span></span> <span data-ttu-id="c59c1-138">Visual Studio считывает документ метаданных OData для обнаружения сущностей в службе.</span><span class="sxs-lookup"><span data-stu-id="c59c1-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="c59c1-139">Нажмите кнопку **ОК** для добавления класса-посредника в проект.</span><span class="sxs-lookup"><span data-stu-id="c59c1-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="c59c1-140">Создайте экземпляр класса прокси-сервера службы</span><span class="sxs-lookup"><span data-stu-id="c59c1-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="c59c1-141">Внутри вашей `Main` метод создания нового экземпляра класса-посредника, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c59c1-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="c59c1-142">Еще раз используйте действительный номер порта, где служба запущена.</span><span class="sxs-lookup"><span data-stu-id="c59c1-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="c59c1-143">При развертывании службы используется URI интерактивную службу.</span><span class="sxs-lookup"><span data-stu-id="c59c1-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="c59c1-144">Нет необходимости обновления учетной записи-посредника.</span><span class="sxs-lookup"><span data-stu-id="c59c1-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="c59c1-145">Следующий код добавляет обработчик событий, который выводит URI-адресов в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="c59c1-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="c59c1-146">Этот шаг не требуется, но интересно. в разделе URL-адреса для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="c59c1-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="c59c1-147">Запрос службы</span><span class="sxs-lookup"><span data-stu-id="c59c1-147">Query the Service</span></span>

<span data-ttu-id="c59c1-148">Следующий код возвращает список продуктов из службы OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="c59c1-149">Обратите внимание, что не нужно писать код для отправки HTTP-запроса или проанализировать ответ.</span><span class="sxs-lookup"><span data-stu-id="c59c1-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="c59c1-150">Класс прокси проводит при этом автоматически при перечислении `Container.Products` коллекции в **foreach** цикла.</span><span class="sxs-lookup"><span data-stu-id="c59c1-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="c59c1-151">При запуске приложение вывод должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="c59c1-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="c59c1-152">Чтобы получить сущность по Идентификатору, используйте `where` предложения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="c59c1-153">Для остальной части этого раздела, не будут отображаться всего `Main` функционировать только код, необходимый для вызова службы.</span><span class="sxs-lookup"><span data-stu-id="c59c1-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="c59c1-154">Применить параметры запроса</span><span class="sxs-lookup"><span data-stu-id="c59c1-154">Apply Query Options</span></span>

<span data-ttu-id="c59c1-155">OData определяет [параметры запроса](../supporting-odata-query-options.md) может использоваться для фильтрации, сортировки, страницы данных и т. д.</span><span class="sxs-lookup"><span data-stu-id="c59c1-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="c59c1-156">В прокси-службы можно применить эти параметры с помощью разных выражений LINQ.</span><span class="sxs-lookup"><span data-stu-id="c59c1-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="c59c1-157">В этом разделе мы рассмотрим краткие примеры.</span><span class="sxs-lookup"><span data-stu-id="c59c1-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="c59c1-158">Дополнительные сведения см. в разделе [рекомендации по LINQ (службы данных WCF)](https://msdn.microsoft.com/library/ee622463.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="c59c1-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="c59c1-159">Фильтрации ($filter)</span><span class="sxs-lookup"><span data-stu-id="c59c1-159">Filtering ($filter)</span></span>

<span data-ttu-id="c59c1-160">Чтобы отфильтровать, используйте `where` предложения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="c59c1-161">В следующем примере фильтрация по категории продукта.</span><span class="sxs-lookup"><span data-stu-id="c59c1-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="c59c1-162">Этот код соответствует следующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="c59c1-163">Обратите внимание, что прокси-сервер преобразует `where` предложение в OData `$filter` выражение.</span><span class="sxs-lookup"><span data-stu-id="c59c1-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="c59c1-164">Сортировка ($orderby)</span><span class="sxs-lookup"><span data-stu-id="c59c1-164">Sorting ($orderby)</span></span>

<span data-ttu-id="c59c1-165">Для сортировки используйте `orderby` предложения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="c59c1-166">Следующий пример Сортировка по цене от наибольшего к наименьшему.</span><span class="sxs-lookup"><span data-stu-id="c59c1-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="c59c1-167">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="c59c1-168">Клиентские подкачки ($skip и $top)</span><span class="sxs-lookup"><span data-stu-id="c59c1-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="c59c1-169">Для больших наборов сущностей клиент может потребоваться ограничить количество результатов.</span><span class="sxs-lookup"><span data-stu-id="c59c1-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="c59c1-170">Например клиент может показывать 10 записей за раз.</span><span class="sxs-lookup"><span data-stu-id="c59c1-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="c59c1-171">Это называется *постраничный просмотр на стороне клиента*.</span><span class="sxs-lookup"><span data-stu-id="c59c1-171">This is called *client-side paging*.</span></span> <span data-ttu-id="c59c1-172">(Также [постраничный просмотр на стороне сервера](../supporting-odata-query-options.md#server-paging), где сервер ограничивает количество результатов.) Чтобы выполнить постраничный просмотр на стороне клиента, используйте LINQ **пропустить** и **принимать** методы.</span><span class="sxs-lookup"><span data-stu-id="c59c1-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="c59c1-173">В следующем примере пропускает первых 40 результаты и принимает следующие 10.</span><span class="sxs-lookup"><span data-stu-id="c59c1-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="c59c1-174">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="c59c1-175">SELECT ($select) и развернуть ($expand)</span><span class="sxs-lookup"><span data-stu-id="c59c1-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="c59c1-176">Чтобы включить связанные сущности, используйте `DataServiceQuery<t>.Expand` метод.</span><span class="sxs-lookup"><span data-stu-id="c59c1-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="c59c1-177">Например, чтобы включить `Supplier` для каждого `Product`:</span><span class="sxs-lookup"><span data-stu-id="c59c1-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="c59c1-178">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="c59c1-179">Чтобы изменить форму ответа, использовать LINQ **выберите** предложения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="c59c1-180">Следующий пример возвращает только имя каждого продукта, а не другие свойства.</span><span class="sxs-lookup"><span data-stu-id="c59c1-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="c59c1-181">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="c59c1-182">Предложение select может включать связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="c59c1-182">A select clause can include related entities.</span></span> <span data-ttu-id="c59c1-183">В этом случае не вызывать **развернуть**; прокси-сервера автоматически включает в себя расширение в этом случае.</span><span class="sxs-lookup"><span data-stu-id="c59c1-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="c59c1-184">В следующем примере возвращается имя и поставщика каждого продукта.</span><span class="sxs-lookup"><span data-stu-id="c59c1-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="c59c1-185">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="c59c1-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="c59c1-186">Обратите внимание, что он включает **$expand** параметр.</span><span class="sxs-lookup"><span data-stu-id="c59c1-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="c59c1-187">Дополнительные сведения о $select и $expand разверните см. в разделе [развернуть с помощью $select, $ и $value в веб-API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="c59c1-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="c59c1-188">Добавление новой сущности</span><span class="sxs-lookup"><span data-stu-id="c59c1-188">Add a New Entity</span></span>

<span data-ttu-id="c59c1-189">Чтобы добавить новую сущность в наборе сущностей, вызовите `AddToEntitySet`, где *EntitySet* имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c59c1-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="c59c1-190">Например `AddToProducts` добавляет новый `Product` для `Products` набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="c59c1-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="c59c1-191">При создании учетной записи-посредника служб данных WCF автоматически создает эти строго типизированных **AddTo** методы.</span><span class="sxs-lookup"><span data-stu-id="c59c1-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="c59c1-192">Чтобы добавить связь между двумя сущностями, используйте **AddLink** и **setlink будет работать** методы.</span><span class="sxs-lookup"><span data-stu-id="c59c1-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="c59c1-193">Следующий код добавляет нового поставщика и новый продукт, а затем создает связи между ними.</span><span class="sxs-lookup"><span data-stu-id="c59c1-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="c59c1-194">Используйте **AddLink** при свойство навигации коллекции.</span><span class="sxs-lookup"><span data-stu-id="c59c1-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="c59c1-195">В этом примере мы добавляем продукт `Products` коллекции на поставщика.</span><span class="sxs-lookup"><span data-stu-id="c59c1-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="c59c1-196">Используйте **setlink будет работать** при одной сущности с помощью свойства навигации.</span><span class="sxs-lookup"><span data-stu-id="c59c1-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="c59c1-197">В этом примере рекомендуется задать `Supplier` свойства продукта.</span><span class="sxs-lookup"><span data-stu-id="c59c1-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="c59c1-198">Обновления или исправления</span><span class="sxs-lookup"><span data-stu-id="c59c1-198">Update / Patch</span></span>

<span data-ttu-id="c59c1-199">Чтобы обновить сущность, вызовите **UpdateObject** метод.</span><span class="sxs-lookup"><span data-stu-id="c59c1-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="c59c1-200">Обновление выполняется при вызове **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="c59c1-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="c59c1-201">По умолчанию WCF отправляет запрос HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="c59c1-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="c59c1-202">**PatchOnUpdate** параметр предписывает WCF для отправки HTTP, PATCH, чтобы вместо этого.</span><span class="sxs-lookup"><span data-stu-id="c59c1-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="c59c1-203">Почему PATCH или MERGE?</span><span class="sxs-lookup"><span data-stu-id="c59c1-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="c59c1-204">Исходной спецификации HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) не был определен любой метод HTTP в соответствии с семантикой «частичное обновление».</span><span class="sxs-lookup"><span data-stu-id="c59c1-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="c59c1-205">Для поддержки частичных обновлений, спецификации OData определен метод MERGE.</span><span class="sxs-lookup"><span data-stu-id="c59c1-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="c59c1-206">В 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) определен метод PATCH для частичного обновления.</span><span class="sxs-lookup"><span data-stu-id="c59c1-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="c59c1-207">Некоторые из журнала в этом можно прочитать [блога](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) блоге WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="c59c1-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="c59c1-208">В настоящее время ИСПРАВЛЕНИЯ предпочтительнее СЛИЯНИЯ.</span><span class="sxs-lookup"><span data-stu-id="c59c1-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="c59c1-209">Контроллера OData, созданного посредством формирования шаблонов веб-API поддерживает оба метода.</span><span class="sxs-lookup"><span data-stu-id="c59c1-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="c59c1-210">Заменяет всю сущность (PUT семантика) укажите **ReplaceOnUpdate** параметр.</span><span class="sxs-lookup"><span data-stu-id="c59c1-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="c59c1-211">В этом случае WCF для отправки запроса HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="c59c1-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="c59c1-212">Удаление сущности</span><span class="sxs-lookup"><span data-stu-id="c59c1-212">Delete an Entity</span></span>

<span data-ttu-id="c59c1-213">Чтобы удалить сущность, вызовите **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="c59c1-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="c59c1-214">Вызвать действие OData</span><span class="sxs-lookup"><span data-stu-id="c59c1-214">Invoke an OData Action</span></span>

<span data-ttu-id="c59c1-215">В OData [действия](odata-actions.md) позволяют добавить серверные поведения, которые легко не определены как операций CRUD в объектах.</span><span class="sxs-lookup"><span data-stu-id="c59c1-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="c59c1-216">Несмотря на то, что документ метаданных OData описывает действия, прокси-класса не создает никаких строго типизированных методов для них.</span><span class="sxs-lookup"><span data-stu-id="c59c1-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="c59c1-217">По-прежнему можно вызвать действие OData с помощью универсального **Execute** метод.</span><span class="sxs-lookup"><span data-stu-id="c59c1-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="c59c1-218">Тем не менее необходимо знать типы данных параметров и возвращаемого значения.</span><span class="sxs-lookup"><span data-stu-id="c59c1-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="c59c1-219">Например `RateProduct` действие имеет параметр с именем «Оценка» типа `Int32` и возвращает `double`.</span><span class="sxs-lookup"><span data-stu-id="c59c1-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="c59c1-220">Ниже показано, как вызвать это действие.</span><span class="sxs-lookup"><span data-stu-id="c59c1-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="c59c1-221">Дополнительные сведения см. в разделе[действия и вызова операций службы](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="c59c1-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="c59c1-222">Один из вариантов является расширение **контейнера** класса для предоставления строго типизированный метод, который вызывает действие:</span><span class="sxs-lookup"><span data-stu-id="c59c1-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
