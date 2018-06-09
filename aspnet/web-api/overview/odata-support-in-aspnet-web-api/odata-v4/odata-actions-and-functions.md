---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Действия и функции в OData v4, с помощью ASP.NET Web API 2.2 | Документы Microsoft
author: MikeWasson
description: В OData действия и функции — это способ добавить серверные поведения, которые легко не определены как операций CRUD в объектах. В этом учебнике показано как...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/27/2014
ms.topic: article
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 532362f0c0faaaf0cb0c04726856f0497e5261b5
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/08/2018
ms.locfileid: "26508233"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="81580-104">Действия и функции в OData v4, с помощью ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="81580-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="81580-105">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="81580-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="81580-106">В OData действия и функции — это способ добавить серверные поведения, которые легко не определены как операций CRUD в объектах.</span><span class="sxs-lookup"><span data-stu-id="81580-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="81580-107">Этот учебник демонстрирует добавление действия и функции для конечной точки OData v4, с помощью Web API 2.2.</span><span class="sxs-lookup"><span data-stu-id="81580-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="81580-108">Учебник построен на учебника [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="81580-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="81580-109">Версии программного обеспечения, используемая в этом учебнике</span><span class="sxs-lookup"><span data-stu-id="81580-109">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="81580-110">Веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="81580-110">Web API 2.2</span></span>
> - <span data-ttu-id="81580-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="81580-111">OData v4</span></span>
> - [<span data-ttu-id="81580-112">Visual Studio 2013 с обновлением 2</span><span class="sxs-lookup"><span data-stu-id="81580-112">Visual Studio 2013 Update 2</span></span>](https://www.visualstudio.com/downloads/download-visual-studio-vs)
> - <span data-ttu-id="81580-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="81580-113">.NET 4.5</span></span>
> 
> 
> ## <a name="tutorial-versions"></a><span data-ttu-id="81580-114">Версии учебника</span><span class="sxs-lookup"><span data-stu-id="81580-114">Tutorial versions</span></span>
> 
> <span data-ttu-id="81580-115">OData версии 3, в разделе [действия OData в ASP.NET Web API 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="81580-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>


<span data-ttu-id="81580-116">Разница между *действия* и *функции* что действий может иметь побочные эффекты, и функции — нет.</span><span class="sxs-lookup"><span data-stu-id="81580-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="81580-117">Действия и функции могут возвращать данные.</span><span class="sxs-lookup"><span data-stu-id="81580-117">Both actions and functions can return data.</span></span> <span data-ttu-id="81580-118">Некоторые варианты применения действия включают:</span><span class="sxs-lookup"><span data-stu-id="81580-118">Some uses for actions include:</span></span>

- <span data-ttu-id="81580-119">Сложных транзакциях.</span><span class="sxs-lookup"><span data-stu-id="81580-119">Complex transactions.</span></span>
- <span data-ttu-id="81580-120">Управление сразу несколько сущностей.</span><span class="sxs-lookup"><span data-stu-id="81580-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="81580-121">Разрешение обновлений только на определенные свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="81580-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="81580-122">Отправка данных, который не является сущностью.</span><span class="sxs-lookup"><span data-stu-id="81580-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="81580-123">Функции полезны для извлечения информации, не относящиеся непосредственно к сущности или коллекции.</span><span class="sxs-lookup"><span data-stu-id="81580-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="81580-124">Действие (или функции) можно выбрать целевую единственную сущность или коллекцию.</span><span class="sxs-lookup"><span data-stu-id="81580-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="81580-125">В терминологии OData это *привязки*.</span><span class="sxs-lookup"><span data-stu-id="81580-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="81580-126">Вы также можете &quot;несвязанного&quot; действий и функций, которые вызываются как статические операции службы.</span><span class="sxs-lookup"><span data-stu-id="81580-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="81580-127">Пример: Добавление действий</span><span class="sxs-lookup"><span data-stu-id="81580-127">Example: Adding an Action</span></span>

<span data-ttu-id="81580-128">Давайте определить действие, чтобы оценить продукт.</span><span class="sxs-lookup"><span data-stu-id="81580-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="81580-129">Этот учебник построен на учебника [создания OData v4 конечной точки с помощью веб-API ASP.NET 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="81580-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="81580-130">Сначала добавьте `ProductRating` модели для представления рейтинга.</span><span class="sxs-lookup"><span data-stu-id="81580-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="81580-131">Кроме того, добавить **DbSet** для `ProductsContext` класса, чтобы EF создаст таблицу оценок в базе данных.</span><span class="sxs-lookup"><span data-stu-id="81580-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="81580-132">Добавить действие к модели EDM</span><span class="sxs-lookup"><span data-stu-id="81580-132">Add the Action to the EDM</span></span>

<span data-ttu-id="81580-133">В WebApiConfig.cs добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="81580-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="81580-134">**EntityTypeConfiguration.Action** метод добавляет действие в модель данных сущности (EDM).</span><span class="sxs-lookup"><span data-stu-id="81580-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="81580-135">**Параметр** метод задает типизированный параметр для действия.</span><span class="sxs-lookup"><span data-stu-id="81580-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="81580-136">Этот код также задает пространство имен для модели EDM.</span><span class="sxs-lookup"><span data-stu-id="81580-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="81580-137">Пространство имен имеет значение, так как полное имя включает в себя URI для действия:</span><span class="sxs-lookup"><span data-stu-id="81580-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="81580-138">В типичной конфигурации IIS точка в этот URL-адрес заставит IIS возвращает ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="81580-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="81580-139">Решить эту проблему, добавив следующий раздел в файл Web.Config:</span><span class="sxs-lookup"><span data-stu-id="81580-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="81580-140">Добавьте метод контроллера для действия</span><span class="sxs-lookup"><span data-stu-id="81580-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="81580-141">Чтобы включить &quot;скорость&quot; действия, добавьте следующий метод `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="81580-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="81580-142">Обратите внимание, что имя метода совпадает с именем действия.</span><span class="sxs-lookup"><span data-stu-id="81580-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="81580-143">**[HttpPost]** атрибут указывает, что метод вызывается методом HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="81580-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="81580-144">Для вызова действия, клиент отправляет запрос HTTP POST, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="81580-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="81580-145">&quot;Скорость&quot; привязано действие экземпляров продукта, поэтому URI для действия имя действия полное, добавляемый в конец URI сущности.</span><span class="sxs-lookup"><span data-stu-id="81580-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="81580-146">(Помните, что мы задании пространства имен EDM &quot;ProductService&quot;, поэтому полное действие названо &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="81580-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="81580-147">Текст запроса содержит параметры действия как полезные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="81580-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="81580-148">Веб-API автоматически преобразует полезные данные JSON для **ODataActionParameters** объект, используемый словарь значений параметров.</span><span class="sxs-lookup"><span data-stu-id="81580-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="81580-149">Этот словарь можно используйте для доступа к параметрам метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="81580-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="81580-150">Если клиент отправляет параметров действия в неверную форматирования, значения **ModelState.IsValid** имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="81580-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="81580-151">Установить этот флаг в методе контроллера и возвращает ошибку, если **IsValid** имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="81580-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="81580-152">Пример: Добавление функции</span><span class="sxs-lookup"><span data-stu-id="81580-152">Example: Adding a Function</span></span>

<span data-ttu-id="81580-153">Теперь добавим функцию OData, возвращает самый дорогостоящий продукта.</span><span class="sxs-lookup"><span data-stu-id="81580-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="81580-154">Как и прежде, первым шагом является добавление функция модели EDM.</span><span class="sxs-lookup"><span data-stu-id="81580-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="81580-155">В WebApiConfig.cs добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="81580-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="81580-156">В этом случае функция привязан к коллекции продуктов, а не отдельных экземпляров продукта.</span><span class="sxs-lookup"><span data-stu-id="81580-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="81580-157">Функция вызывается клиентов, отправив запрос GET:</span><span class="sxs-lookup"><span data-stu-id="81580-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="81580-158">Далее приводится метод контроллера для этой функции.</span><span class="sxs-lookup"><span data-stu-id="81580-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="81580-159">Обратите внимание, что имя метода совпадает с именем функции.</span><span class="sxs-lookup"><span data-stu-id="81580-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="81580-160">**[HttpGet]** атрибут указывает, что метод вызывается метод HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="81580-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="81580-161">Ниже приведен в HTTP-ответе.</span><span class="sxs-lookup"><span data-stu-id="81580-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="81580-162">Пример: Добавление несвязанную функцию</span><span class="sxs-lookup"><span data-stu-id="81580-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="81580-163">Предыдущий пример был функции связаны с коллекцией.</span><span class="sxs-lookup"><span data-stu-id="81580-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="81580-164">В этом примере мы создадим *несвязанного* функции.</span><span class="sxs-lookup"><span data-stu-id="81580-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="81580-165">Свободные функции вызываются как статические операции службы.</span><span class="sxs-lookup"><span data-stu-id="81580-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="81580-166">В этом примере функция возвращает налог для данного почтового индекса.</span><span class="sxs-lookup"><span data-stu-id="81580-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="81580-167">В файле WebApiConfig добавьте функцию в модель EDM:</span><span class="sxs-lookup"><span data-stu-id="81580-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="81580-168">Обратите внимание, что поступает вызов **функция** непосредственно на **ODataModelBuilder**, вместо типа сущности или коллекции.</span><span class="sxs-lookup"><span data-stu-id="81580-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="81580-169">Это значение определяет построитель модели, что функция является свободной.</span><span class="sxs-lookup"><span data-stu-id="81580-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="81580-170">Ниже приведен метод контроллера, который реализует функцию:</span><span class="sxs-lookup"><span data-stu-id="81580-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="81580-171">Неважно, какой контроллер веб-API, поместите в этот метод.</span><span class="sxs-lookup"><span data-stu-id="81580-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="81580-172">Вы можете поместить в `ProductsController`, или определить отдельные контроллера.</span><span class="sxs-lookup"><span data-stu-id="81580-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="81580-173">**[ODataRoute]** атрибут определяет шаблон URI для функции.</span><span class="sxs-lookup"><span data-stu-id="81580-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="81580-174">Ниже приведен пример запроса клиента.</span><span class="sxs-lookup"><span data-stu-id="81580-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="81580-175">HTTP-ответа:</span><span class="sxs-lookup"><span data-stu-id="81580-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
