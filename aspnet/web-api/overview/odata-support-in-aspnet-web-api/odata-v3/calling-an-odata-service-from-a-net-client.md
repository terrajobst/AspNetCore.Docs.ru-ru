---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
title: Вызов службы OData из клиента .NET (C#) | Документация Майкрософт
author: MikeWasson
description: Этом руководстве показано, как вызов службы OData из клиентского приложения C#. Версии программного обеспечения, используемые в руководства для Visual Studio 2013 (работает с Visual S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2014
ms.topic: article
ms.assetid: 6f448917-ad23-4dcc-9789-897fad74051b
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/calling-an-odata-service-from-a-net-client
msc.type: authoredcontent
ms.openlocfilehash: d987e7fbe737055b3e2b690ef3e8de5ca7e2b937
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369787"
---
<a name="calling-an-odata-service-from-a-net-client-c"></a><span data-ttu-id="3cdf5-104">Вызов службы OData из клиента .NET (C#)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-104">Calling an OData Service From a .NET Client (C#)</span></span>
====================
<span data-ttu-id="3cdf5-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

[<span data-ttu-id="3cdf5-106">Скачать завершенный проект</span><span class="sxs-lookup"><span data-stu-id="3cdf5-106">Download Completed Project</span></span>](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> <span data-ttu-id="3cdf5-107">Этом руководстве показано, как вызов службы OData из клиентского приложения C#.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-107">This tutorial shows how to call an OData service from a C# client application.</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3cdf5-108">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="3cdf5-108">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3cdf5-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (работает с Visual Studio 2012)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-109">[Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (works with Visual Studio 2012)</span></span>
> - [<span data-ttu-id="3cdf5-110">Библиотека клиентов служб данных WCF</span><span class="sxs-lookup"><span data-stu-id="3cdf5-110">WCF Data Services Client Library</span></span>](https://msdn.microsoft.com/library/cc668772.aspx)
> - <span data-ttu-id="3cdf5-111">Веб-API 2.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-111">Web API 2.</span></span> <span data-ttu-id="3cdf5-112">(Пример службы OData создается с помощью веб-API 2, но клиентское приложение не зависит от веб-API).</span><span class="sxs-lookup"><span data-stu-id="3cdf5-112">(The example OData service is built using Web API 2, but the client application does not depend on Web API.)</span></span>


<span data-ttu-id="3cdf5-113">В этом руководстве мы рассмотрим создание клиентского приложения, которая вызывает службу OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-113">In this tutorial, I'll walk through creating a client application that calls an OData service.</span></span> <span data-ttu-id="3cdf5-114">Служба OData предоставляет следующие сущности:</span><span class="sxs-lookup"><span data-stu-id="3cdf5-114">The OData service exposes the following entities:</span></span>

- `Product`
- `Supplier`
- `ProductRating`

![](calling-an-odata-service-from-a-net-client/_static/image1.png)

<span data-ttu-id="3cdf5-115">В следующих статьях описываются способы реализации службы OData в веб-API.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-115">The following articles describe how to implement the OData service in Web API.</span></span> <span data-ttu-id="3cdf5-116">(Не требуется читать их, чтобы понять этот учебник, однако.)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-116">(You don't need to read them to understand this tutorial, however.)</span></span>

- [<span data-ttu-id="3cdf5-117">Создание конечной точки OData в веб-API 2</span><span class="sxs-lookup"><span data-stu-id="3cdf5-117">Creating an OData Endpoint in Web API 2</span></span>](creating-an-odata-endpoint.md)
- [<span data-ttu-id="3cdf5-118">Отношения сущностей OData в веб-API 2</span><span class="sxs-lookup"><span data-stu-id="3cdf5-118">OData Entity Relations in Web API 2</span></span>](working-with-entity-relations.md)
- [<span data-ttu-id="3cdf5-119">Действия OData в веб-API 2</span><span class="sxs-lookup"><span data-stu-id="3cdf5-119">OData Actions in Web API 2</span></span>](odata-actions.md)

## <a name="generate-the-service-proxy"></a><span data-ttu-id="3cdf5-120">Создание прокси-службы</span><span class="sxs-lookup"><span data-stu-id="3cdf5-120">Generate the Service Proxy</span></span>

<span data-ttu-id="3cdf5-121">Первым шагом является создание прокси-службы.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-121">The first step is to generate a service proxy.</span></span> <span data-ttu-id="3cdf5-122">Прокси-службы представляет собой класс .NET, который определяет методы для доступа к службе OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-122">The service proxy is a .NET class that defines methods for accessing the OData service.</span></span> <span data-ttu-id="3cdf5-123">Прокси-сервер преобразует вызовы метода в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-123">The proxy translates method calls into HTTP requests.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image2.png)

<span data-ttu-id="3cdf5-124">Сначала откройте проект службы OData в Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-124">Start by opening the OData service project in Visual Studio.</span></span> <span data-ttu-id="3cdf5-125">Нажмите клавиши CTRL + F5, чтобы запустить службу локально в IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-125">Press CTRL+F5 to run the service locally in IIS Express.</span></span> <span data-ttu-id="3cdf5-126">Обратите внимание, локальный адрес, включая номер порта, который назначает Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-126">Note the local address, including the port number that Visual Studio assigns.</span></span> <span data-ttu-id="3cdf5-127">Этот адрес понадобится при создании прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-127">You will need this address when you create the proxy.</span></span>

<span data-ttu-id="3cdf5-128">Далее откройте другой экземпляр Visual Studio и создайте проект консольного приложения.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-128">Next, open another instance of Visual Studio and create a console application project.</span></span> <span data-ttu-id="3cdf5-129">Консольное приложение будет OData в клиентском приложении.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-129">The console application will be our OData client application.</span></span> <span data-ttu-id="3cdf5-130">(Можно также добавить проект в решение как службу.)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-130">(You can also add the project to the same solution as the service.)</span></span>

> [!NOTE]
> <span data-ttu-id="3cdf5-131">Остальные шаги см. в проект.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-131">The remaining steps refer the console project.</span></span>


<span data-ttu-id="3cdf5-132">В обозревателе решений щелкните правой кнопкой мыши **ссылки** и выберите **Add Service Reference**.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-132">In Solution Explorer, right-click **References** and select **Add Service Reference**.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image3.png)

<span data-ttu-id="3cdf5-133">В **Add Service Reference** диалоговом окне введите адрес службы OData:</span><span class="sxs-lookup"><span data-stu-id="3cdf5-133">In the **Add Service Reference** dialog, type the address of the OData service:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample1.cmd)]

<span data-ttu-id="3cdf5-134">где *порт* — номер порта.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-134">where *port* is the port number.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image5.png)](calling-an-odata-service-from-a-net-client/_static/image4.png)

<span data-ttu-id="3cdf5-135">Для **пространства имен**, введите «ProductService».</span><span class="sxs-lookup"><span data-stu-id="3cdf5-135">For **Namespace**, type "ProductService".</span></span> <span data-ttu-id="3cdf5-136">Этот параметр определяет пространство имен класса-посредника.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-136">This option defines the namespace of the proxy class.</span></span>

<span data-ttu-id="3cdf5-137">Нажмите **Перейти**.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-137">Click **Go**.</span></span> <span data-ttu-id="3cdf5-138">Visual Studio считывает документ метаданных OData для обнаружения сущностей в службе.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-138">Visual Studio reads the OData metadata document to discover the entities in the service.</span></span>

[![](calling-an-odata-service-from-a-net-client/_static/image7.png)](calling-an-odata-service-from-a-net-client/_static/image6.png)

<span data-ttu-id="3cdf5-139">Нажмите кнопку **ОК** для добавления прокси-класса в проект.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-139">Click **OK** to add the proxy class to your project.</span></span>

![](calling-an-odata-service-from-a-net-client/_static/image8.png)

## <a name="create-an-instance-of-the-service-proxy-class"></a><span data-ttu-id="3cdf5-140">Создайте экземпляр прокси-класса службы</span><span class="sxs-lookup"><span data-stu-id="3cdf5-140">Create an Instance of the Service Proxy Class</span></span>

<span data-ttu-id="3cdf5-141">Внутри вашей `Main` метод, создайте новый экземпляр класса-посредника, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3cdf5-141">Inside your `Main` method, create a new instance of the proxy class, as follows:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample2.cs)]

<span data-ttu-id="3cdf5-142">Опять же используйте действительный номер порта, где служба запущена.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-142">Again, use the actual port number where your service is running.</span></span> <span data-ttu-id="3cdf5-143">При развертывании службы используется URI службы в реальном времени.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-143">When you deploy your service, you will use the URI of the live service.</span></span> <span data-ttu-id="3cdf5-144">Не нужно обновить прокси-сервер.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-144">You don't need to update the proxy.</span></span>

<span data-ttu-id="3cdf5-145">Следующий код добавляет обработчик событий, который выводит запросы URI в окно консоли.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-145">The following code adds an event handler that prints the request URIs to the console window.</span></span> <span data-ttu-id="3cdf5-146">Этот шаг не является обязательным, но это интересно посмотреть, URI для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-146">This step isn't required, but it's interesting to see the URIs for each query.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample3.cs)]

## <a name="query-the-service"></a><span data-ttu-id="3cdf5-147">Запросы к службе</span><span class="sxs-lookup"><span data-stu-id="3cdf5-147">Query the Service</span></span>

<span data-ttu-id="3cdf5-148">Следующий код возвращает список продуктов из службы OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-148">The following code gets the list of products from the OData service.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample4.cs)]

<span data-ttu-id="3cdf5-149">Обратите внимание на то, что не нужно писать код для отправки HTTP-запроса или проанализировать ответ.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-149">Notice that you don't need to write any code to send the HTTP request or parse the response.</span></span> <span data-ttu-id="3cdf5-150">Класс прокси проводит при этом автоматически при перечислении `Container.Products` коллекции в **foreach** цикла.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-150">The proxy class does this automatically when you enumerate the `Container.Products` collection in the **foreach** loop.</span></span>

<span data-ttu-id="3cdf5-151">При запуске приложения результат должен выглядеть следующим образом:</span><span class="sxs-lookup"><span data-stu-id="3cdf5-151">When you run the application, the output should look like the following:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample5.cmd)]

<span data-ttu-id="3cdf5-152">Чтобы получить сущность по Идентификатору, используйте `where` предложение.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-152">To get an entity by ID, use a `where` clause.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample6.cs)]

<span data-ttu-id="3cdf5-153">Для остальной части этого раздела, я не буду приводить всего `Main` функционировать только код, необходимый для вызова службы.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-153">For the rest of this topic, I won't show the entire `Main` function, just the code needed to call the service.</span></span>

## <a name="apply-query-options"></a><span data-ttu-id="3cdf5-154">Применить параметры запроса</span><span class="sxs-lookup"><span data-stu-id="3cdf5-154">Apply Query Options</span></span>

<span data-ttu-id="3cdf5-155">OData определяет [параметры запроса](../supporting-odata-query-options.md) , можно использовать для фильтрации, сортировки, страницы данных и т. д.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-155">OData defines [query options](../supporting-odata-query-options.md) that can be used to filter, sort, page data, and so forth.</span></span> <span data-ttu-id="3cdf5-156">В случае прокси сервиса эти параметры можно применить с помощью различных выражений LINQ.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-156">In the service proxy, you can apply these options by using various LINQ expressions.</span></span>

<span data-ttu-id="3cdf5-157">В этом разделе я покажу краткие примеры.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-157">In this section, I'll show brief examples.</span></span> <span data-ttu-id="3cdf5-158">Дополнительные сведения см. в разделе [рекомендации по LINQ (службы данных WCF)](https://msdn.microsoft.com/library/ee622463.aspx) на сайте MSDN.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-158">For more details, see the topic [LINQ Considerations (WCF Data Services)](https://msdn.microsoft.com/library/ee622463.aspx) on MSDN.</span></span>

### <a name="filtering-filter"></a><span data-ttu-id="3cdf5-159">Фильтрации ($filter)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-159">Filtering ($filter)</span></span>

<span data-ttu-id="3cdf5-160">Для фильтрации используйте `where` предложение.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-160">To filter, use a `where` clause.</span></span> <span data-ttu-id="3cdf5-161">В следующем примере отфильтровываются по категориям продуктов.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-161">The following example filters by product category.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample7.cs)]

<span data-ttu-id="3cdf5-162">Этот код соответствует следующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-162">This code corresponds to the following OData query.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample8.cmd)]

<span data-ttu-id="3cdf5-163">Обратите внимание, что прокси-сервер преобразует `where` предложение в OData `$filter` выражение.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-163">Notice that the proxy converts the `where` clause into an OData `$filter` expression.</span></span>

### <a name="sorting-orderby"></a><span data-ttu-id="3cdf5-164">Сортировка ($orderby)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-164">Sorting ($orderby)</span></span>

<span data-ttu-id="3cdf5-165">Чтобы отсортировать данные, используйте `orderby` предложение.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-165">To sort, use an `orderby` clause.</span></span> <span data-ttu-id="3cdf5-166">Следующий пример сортирует по цене от самого высокого до самого низкого.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-166">The following example sorts by price, from highest to lowest.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample9.cs)]

<span data-ttu-id="3cdf5-167">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-167">Here is the corresponding OData request.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample10.cmd)]

### <a name="client-side-paging-skip-and-top"></a><span data-ttu-id="3cdf5-168">Разбиение по страницам на стороне клиента ($skip и $top)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-168">Client-Side Paging ($skip and $top)</span></span>

<span data-ttu-id="3cdf5-169">Для больших наборов сущностей клиент может потребоваться ограничить количество результатов.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-169">For large entity sets, the client might want to limit the number of results.</span></span> <span data-ttu-id="3cdf5-170">Например клиент может показывать 10 записей за раз.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-170">For example, a client might show 10 entries at a time.</span></span> <span data-ttu-id="3cdf5-171">Это называется *разбиение по страницам на стороне клиента*.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-171">This is called *client-side paging*.</span></span> <span data-ttu-id="3cdf5-172">(Также [серверное разбиение по страницам](../supporting-odata-query-options.md#server-paging), где сервер ограничивает число результатов.) Чтобы выполнить разбиение по страницам на стороне клиента, использовать LINQ **Skip** и **занять** методы.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-172">(There is also [server-side paging](../supporting-odata-query-options.md#server-paging), where the server limits the number of results.) To perform client-side paging, use the LINQ **Skip** and **Take** methods.</span></span> <span data-ttu-id="3cdf5-173">В следующем примере пропускает первых 40 результата и принимает следующие 10.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-173">The following example skips the first 40 results and takes the next 10.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample11.cs)]

<span data-ttu-id="3cdf5-174">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-174">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample12.cmd)]

### <a name="select-select-and-expand-expand"></a><span data-ttu-id="3cdf5-175">SELECT ($select) и развернуть ($expand)</span><span class="sxs-lookup"><span data-stu-id="3cdf5-175">Select ($select) and Expand ($expand)</span></span>

<span data-ttu-id="3cdf5-176">Чтобы включить связанные сущности, используйте `DataServiceQuery<t>.Expand` метод.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-176">To include related entities, use the `DataServiceQuery<t>.Expand` method.</span></span> <span data-ttu-id="3cdf5-177">Например, чтобы включить `Supplier` для каждого `Product`:</span><span class="sxs-lookup"><span data-stu-id="3cdf5-177">For example, to include the `Supplier` for each `Product`:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample13.cs)]

<span data-ttu-id="3cdf5-178">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-178">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample14.cmd)]

<span data-ttu-id="3cdf5-179">Чтобы изменить форму ответа, использовать LINQ **выберите** предложение.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-179">To change the shape of the response, use the LINQ **select** clause.</span></span> <span data-ttu-id="3cdf5-180">Следующий пример возвращает только имя каждого продукта, с помощью никакие другие свойства.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-180">The following example gets just the name of each product, with no other properties.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample15.cs)]

<span data-ttu-id="3cdf5-181">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-181">Here is the corresponding OData request:</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample16.cmd)]

<span data-ttu-id="3cdf5-182">Предложение select может включать связанные сущности.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-182">A select clause can include related entities.</span></span> <span data-ttu-id="3cdf5-183">В этом случае не следует вызывать **Expand**; прокси-сервер автоматически включает в себя развертывание в этом случае.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-183">In that case, do not call **Expand**; the proxy automatically includes the expansion in this case.</span></span> <span data-ttu-id="3cdf5-184">В следующем примере возвращается имя и поставщика каждого продукта.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-184">The following example gets the name and supplier of each product.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample17.cs)]

<span data-ttu-id="3cdf5-185">Ниже приведен соответствующий запрос OData.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-185">Here is the corresponding OData request.</span></span> <span data-ttu-id="3cdf5-186">Обратите внимание, что он включает **$expand** параметр.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-186">Notice that it includes the **$expand** option.</span></span>

[!code-console[Main](calling-an-odata-service-from-a-net-client/samples/sample18.cmd)]

<span data-ttu-id="3cdf5-187">Дополнительные сведения о $select и $expand развернуть, см. в разделе [использование $select, $expand и $value в веб-API 2](../using-select-expand-and-value.md).</span><span class="sxs-lookup"><span data-stu-id="3cdf5-187">For more information about $select and $expand, see [Using $select, $expand, and $value in Web API 2](../using-select-expand-and-value.md).</span></span>

## <a name="add-a-new-entity"></a><span data-ttu-id="3cdf5-188">Добавить новую сущность</span><span class="sxs-lookup"><span data-stu-id="3cdf5-188">Add a New Entity</span></span>

<span data-ttu-id="3cdf5-189">Чтобы добавить новую сущность в наборе сущностей, вызовите `AddToEntitySet`, где *EntitySet* имя набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-189">To add a new entity to an entity set, call `AddToEntitySet`, where *EntitySet* is the name of the entity set.</span></span> <span data-ttu-id="3cdf5-190">Например `AddToProducts` добавляет новый `Product` для `Products` набора сущностей.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-190">For example, `AddToProducts` adds a new `Product` to the `Products` entity set.</span></span> <span data-ttu-id="3cdf5-191">При создании прокси-сервера, службы данных WCF автоматически создает эти со строгой типизацией **AddTo** методы.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-191">When you generate the proxy, WCF Data Services automatically creates these strongly-typed **AddTo** methods.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample19.cs)]

<span data-ttu-id="3cdf5-192">Чтобы добавить связь между двумя сущностями, используйте **AddLink** и **SetLink** методы.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-192">To add a link between two entities, use the **AddLink** and **SetLink** methods.</span></span> <span data-ttu-id="3cdf5-193">Следующий код добавляет нового поставщика и новый продукт, а затем создает связи между ними.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-193">The following code adds a new supplier and a new product, and then creates links between them.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample20.cs)]

<span data-ttu-id="3cdf5-194">Используйте **AddLink** при свойство навигации является коллекцией.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-194">Use **AddLink** when the navigation property is a collection.</span></span> <span data-ttu-id="3cdf5-195">В этом примере мы добавляем продукт для `Products` коллекции поставщика.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-195">In this example, we are adding a product to the `Products` collection on the supplier.</span></span>

<span data-ttu-id="3cdf5-196">Используйте **SetLink** Если это свойство навигации имеет одну сущность.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-196">Use **SetLink** when the navigation property is a single entity.</span></span> <span data-ttu-id="3cdf5-197">В этом примере мы указываем `Supplier` свойство над продуктом.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-197">In this example, we are setting the `Supplier` property on the product.</span></span>

## <a name="update--patch"></a><span data-ttu-id="3cdf5-198">Обновления или исправления</span><span class="sxs-lookup"><span data-stu-id="3cdf5-198">Update / Patch</span></span>

<span data-ttu-id="3cdf5-199">Чтобы обновить сущность, вызовите **UpdateObject** метод.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-199">To update an entity, call the **UpdateObject** method.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample21.cs)]

<span data-ttu-id="3cdf5-200">Обновление выполняется при вызове **SaveChanges**.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-200">The update is performed when you call **SaveChanges**.</span></span> <span data-ttu-id="3cdf5-201">По умолчанию WCF отправляет запрос HTTP MERGE.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-201">By default, WCF sends an HTTP MERGE request.</span></span> <span data-ttu-id="3cdf5-202">**PatchOnUpdate** параметр предписывает WCF для отправки HTTP, PATCH, чтобы вместо этого.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-202">The **PatchOnUpdate** option tells WCF to send an HTTP PATCH instead.</span></span>

> [!NOTE]
> <span data-ttu-id="3cdf5-203">Почему PATCH и MERGE?</span><span class="sxs-lookup"><span data-stu-id="3cdf5-203">Why PATCH versus MERGE?</span></span> <span data-ttu-id="3cdf5-204">Исходная спецификация HTTP 1.1 ([RCF 2616](http://tools.ietf.org/html/rfc2616)) не был определен любого метода HTTP с семантикой «частичное обновление».</span><span class="sxs-lookup"><span data-stu-id="3cdf5-204">The original HTTP 1.1 specification ([RCF 2616](http://tools.ietf.org/html/rfc2616)) did not define any HTTP method with "partial update" semantics.</span></span> <span data-ttu-id="3cdf5-205">Чтобы обеспечить поддержку частичных обновлений, спецификации протокола OData определен метод MERGE.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-205">To support partial updates, the OData specification defined the MERGE method.</span></span> <span data-ttu-id="3cdf5-206">В 2010 [RFC 5789](http://tools.ietf.org/html/rfc5789) определенный метод PATCH для частичного обновления.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-206">In 2010, [RFC 5789](http://tools.ietf.org/html/rfc5789) defined the PATCH method for partial updates.</span></span> <span data-ttu-id="3cdf5-207">Можно ознакомиться с некоторыми журнала в этом [блога](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) блоге WCF Data Services.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-207">You can read some of the history in this [blog post](https://blogs.msdn.com/b/astoriateam/archive/2008/05/20/merge-vs-replace-semantics-for-update-operations.aspx) on the WCF Data Services Blog.</span></span> <span data-ttu-id="3cdf5-208">В настоящее время ИСПРАВЛЕНИЙ предпочтительнее СЛИЯНИЯ.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-208">Today, PATCH is preferred over MERGE.</span></span> <span data-ttu-id="3cdf5-209">Контроллер OData, созданные путем формирования шаблонов веб-API поддерживает оба метода.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-209">The OData controller created by the Web API scaffolding supports both methods.</span></span>


<span data-ttu-id="3cdf5-210">Если вы хотите заменить всю сущность (семантикой PUT), укажите **ReplaceOnUpdate** параметр.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-210">If you want to replace the entire entity (PUT semantics), specify the **ReplaceOnUpdate** option.</span></span> <span data-ttu-id="3cdf5-211">В результате WCF для отправки запроса HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-211">This causes WCF to send an HTTP PUT request.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample22.cs)]

## <a name="delete-an-entity"></a><span data-ttu-id="3cdf5-212">Удаление сущности</span><span class="sxs-lookup"><span data-stu-id="3cdf5-212">Delete an Entity</span></span>

<span data-ttu-id="3cdf5-213">Чтобы удалить сущность, вызовите **DeleteObject**.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-213">To delete an entity, call **DeleteObject**.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample23.cs)]

## <a name="invoke-an-odata-action"></a><span data-ttu-id="3cdf5-214">Вызвать действие OData</span><span class="sxs-lookup"><span data-stu-id="3cdf5-214">Invoke an OData Action</span></span>

<span data-ttu-id="3cdf5-215">В OData [действия](odata-actions.md) позволяют добавлять поведения на стороне сервера, которые легко не определены как операций CRUD в объектах.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-215">In OData, [actions](odata-actions.md) are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span>

<span data-ttu-id="3cdf5-216">Несмотря на то, что документ метаданных OData описывает действия, прокси-класса не создает никаких строго типизированных методов для них.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-216">Although the OData metadata document describes the actions, the proxy class does not create any strongly-typed methods for them.</span></span> <span data-ttu-id="3cdf5-217">По-прежнему можно вызвать действие OData с помощью универсального **Execute** метод.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-217">You can still invoke an OData action by using the generic **Execute** method.</span></span> <span data-ttu-id="3cdf5-218">Тем не менее необходимо знать типы данных параметров и возвращаемого значения.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-218">However, you will need to know the data types of the parameters and the return value.</span></span>

<span data-ttu-id="3cdf5-219">Например `RateProduct` действие имеет параметр с именем «Rating» типа `Int32` и возвращает `double`.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-219">For example, the `RateProduct` action takes parameter named "Rating" of type `Int32` and returns a `double`.</span></span> <span data-ttu-id="3cdf5-220">Ниже показано, как вызывать это действие.</span><span class="sxs-lookup"><span data-stu-id="3cdf5-220">The following code shows how to invoke this action.</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample24.cs)]

<span data-ttu-id="3cdf5-221">Дополнительные сведения см. в разделе[вызов операций служб и процессов](https://msdn.microsoft.com/library/hh230677.aspx).</span><span class="sxs-lookup"><span data-stu-id="3cdf5-221">For more information, see[Calling Service Operations and Actions](https://msdn.microsoft.com/library/hh230677.aspx).</span></span>

<span data-ttu-id="3cdf5-222">Один из вариантов является расширение **контейнера** класс, чтобы обеспечить строго типизированный метод, который вызывает действие:</span><span class="sxs-lookup"><span data-stu-id="3cdf5-222">One option is to extend the **Container** class to provide a strongly typed method that invokes the action:</span></span>

[!code-csharp[Main](calling-an-odata-service-from-a-net-client/samples/sample25.cs)]
