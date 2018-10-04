---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
title: Действия и функции в OData v4, с помощью ASP.NET Web API 2.2 | Документация Майкрософт
author: MikeWasson
description: В OData действия и функции — это способ для добавления поведений на стороне сервера, которые легко не определены как операций CRUD в объектах. В этом руководстве показано как...
ms.author: riande
ms.date: 06/27/2014
ms.assetid: 0e6fb03c-b16d-4bb0-ab0b-552bd2b6ece1
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v4/odata-actions-and-functions
msc.type: authoredcontent
ms.openlocfilehash: 45b84ec4ee76e83ece99bf6841c28e13c3ab7728
ms.sourcegitcommit: 7890dfb5a8f8c07d813f166d3ab0c263f893d0c6
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/04/2018
ms.locfileid: "48795271"
---
<a name="actions-and-functions-in-odata-v4-using-aspnet-web-api-22"></a><span data-ttu-id="5de26-104">Действия и функции в OData v4, с помощью ASP.NET Web API 2.2</span><span class="sxs-lookup"><span data-stu-id="5de26-104">Actions and Functions in OData v4 Using ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="5de26-105">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5de26-105">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

> <span data-ttu-id="5de26-106">В OData действия и функции — это способ для добавления поведений на стороне сервера, которые легко не определены как операций CRUD в объектах.</span><span class="sxs-lookup"><span data-stu-id="5de26-106">In OData, actions and functions are a way to add server-side behaviors that are not easily defined as CRUD operations on entities.</span></span> <span data-ttu-id="5de26-107">Этом руководстве показано, как добавить действия и функции для конечной точки OData v4, с помощью веб-API 2.2.</span><span class="sxs-lookup"><span data-stu-id="5de26-107">This tutorial shows how to add actions and functions to an OData v4 endpoint, using Web API 2.2.</span></span> <span data-ttu-id="5de26-108">Учебном курсе руководство [создания OData v4 конечной точки с помощью ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="5de26-108">The tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>
>
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="5de26-109">Версии программного обеспечения, используемые в этом руководстве</span><span class="sxs-lookup"><span data-stu-id="5de26-109">Software versions used in the tutorial</span></span>
>
> - <span data-ttu-id="5de26-110">Веб-API 2.2</span><span class="sxs-lookup"><span data-stu-id="5de26-110">Web API 2.2</span></span>
> - <span data-ttu-id="5de26-111">OData v4</span><span class="sxs-lookup"><span data-stu-id="5de26-111">OData v4</span></span>
> - <span data-ttu-id="5de26-112">Visual Studio 2013 (скачать Visual Studio 2017 [здесь](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span><span class="sxs-lookup"><span data-stu-id="5de26-112">Visual Studio 2013 (download Visual Studio 2017 [here](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017))</span></span>
> - <span data-ttu-id="5de26-113">.NET 4.5</span><span class="sxs-lookup"><span data-stu-id="5de26-113">.NET 4.5</span></span>
>
> ## <a name="tutorial-versions"></a><span data-ttu-id="5de26-114">Учебника по версии</span><span class="sxs-lookup"><span data-stu-id="5de26-114">Tutorial versions</span></span>
>
> <span data-ttu-id="5de26-115">OData версии 3, см. в разделе [действия OData в веб-API ASP.NET 2](../odata-v3/odata-actions.md).</span><span class="sxs-lookup"><span data-stu-id="5de26-115">For OData Version 3, see [OData Actions in ASP.NET Web API 2](../odata-v3/odata-actions.md).</span></span>

<span data-ttu-id="5de26-116">Разница между *действия* и *функции* действия может иметь побочные эффекты, а функции — нет.</span><span class="sxs-lookup"><span data-stu-id="5de26-116">The difference between *actions* and *functions* is that actions can have side effects, and functions do not.</span></span> <span data-ttu-id="5de26-117">Действия и функции могут возвращать данные.</span><span class="sxs-lookup"><span data-stu-id="5de26-117">Both actions and functions can return data.</span></span> <span data-ttu-id="5de26-118">Некоторые способы для действий:</span><span class="sxs-lookup"><span data-stu-id="5de26-118">Some uses for actions include:</span></span>

- <span data-ttu-id="5de26-119">Сложных транзакциях.</span><span class="sxs-lookup"><span data-stu-id="5de26-119">Complex transactions.</span></span>
- <span data-ttu-id="5de26-120">Управление несколько сущностей за один раз.</span><span class="sxs-lookup"><span data-stu-id="5de26-120">Manipulating several entities at once.</span></span>
- <span data-ttu-id="5de26-121">Разрешение обновлений только на определенные свойства сущности.</span><span class="sxs-lookup"><span data-stu-id="5de26-121">Allowing updates only to certain properties of an entity.</span></span>
- <span data-ttu-id="5de26-122">Отправка данных, который не является сущностью.</span><span class="sxs-lookup"><span data-stu-id="5de26-122">Sending data that is not an entity.</span></span>

<span data-ttu-id="5de26-123">Функции полезны для извлечения информации, не относящиеся непосредственно к сущности или коллекции.</span><span class="sxs-lookup"><span data-stu-id="5de26-123">Functions are useful for returning information that does not correspond directly to an entity or collection.</span></span>

<span data-ttu-id="5de26-124">Действие (или функции) могут быть предназначены единственную сущность или коллекцию.</span><span class="sxs-lookup"><span data-stu-id="5de26-124">An action (or function) can target a single entity or a collection.</span></span> <span data-ttu-id="5de26-125">В терминологии OData, это *привязки*.</span><span class="sxs-lookup"><span data-stu-id="5de26-125">In OData terminology, this is the *binding*.</span></span> <span data-ttu-id="5de26-126">Вы также можете &quot;несвязанного&quot; действий и функций, которые вызываются как статические операции службы.</span><span class="sxs-lookup"><span data-stu-id="5de26-126">You can also have &quot;unbound&quot; actions/functions, which are called as static operations on the service.</span></span>

## <a name="example-adding-an-action"></a><span data-ttu-id="5de26-127">Пример: Добавление действия</span><span class="sxs-lookup"><span data-stu-id="5de26-127">Example: Adding an Action</span></span>

<span data-ttu-id="5de26-128">Давайте определим действие, чтобы дать оценку продукта.</span><span class="sxs-lookup"><span data-stu-id="5de26-128">Let's define an action to rate a product.</span></span>

> [!NOTE]
> <span data-ttu-id="5de26-129">Этот учебник основан на учебнике [создания OData v4 конечной точки с помощью ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="5de26-129">This tutorial builds on the tutorial [Create an OData v4 Endpoint Using ASP.NET Web API 2](create-an-odata-v4-endpoint.md)</span></span>


<span data-ttu-id="5de26-130">Во-первых, добавьте `ProductRating` модели для представления оценки.</span><span class="sxs-lookup"><span data-stu-id="5de26-130">First, add a `ProductRating` model to represent the ratings.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample1.cs)]

<span data-ttu-id="5de26-131">Кроме того, добавить **DbSet** для `ProductsContext` класса, позволяя EF создаст таблицу оценок в базе данных.</span><span class="sxs-lookup"><span data-stu-id="5de26-131">Also add a **DbSet** to the `ProductsContext` class, so that EF will create a Ratings table in the database.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample2.cs)]

### <a name="add-the-action-to-the-edm"></a><span data-ttu-id="5de26-132">Добавление действия к модели EDM</span><span class="sxs-lookup"><span data-stu-id="5de26-132">Add the Action to the EDM</span></span>

<span data-ttu-id="5de26-133">В файле WebApiConfig.cs добавьте следующий код:</span><span class="sxs-lookup"><span data-stu-id="5de26-133">In WebApiConfig.cs, add the following code:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample3.cs)]

<span data-ttu-id="5de26-134">**EntityTypeConfiguration.Action** метод добавляет действие в модель данных сущности (модель EDM).</span><span class="sxs-lookup"><span data-stu-id="5de26-134">The **EntityTypeConfiguration.Action** method adds an action to the entity data model (EDM).</span></span> <span data-ttu-id="5de26-135">**Параметр** метод задает типизированный параметр для действия.</span><span class="sxs-lookup"><span data-stu-id="5de26-135">The **Parameter** method specifies a typed parameter for the action.</span></span>

<span data-ttu-id="5de26-136">Этот код также задает пространство имен для модели EDM.</span><span class="sxs-lookup"><span data-stu-id="5de26-136">This code also sets the namespace for the EDM.</span></span> <span data-ttu-id="5de26-137">Пространство имен имеет значение, так как URI для действия содержит действие, полное доменное имя:</span><span class="sxs-lookup"><span data-stu-id="5de26-137">The namespace matters because the URI for the action includes the fully-qualified action name:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample4.cmd)]

> [!NOTE]
> <span data-ttu-id="5de26-138">В типичной конфигурации IIS точка, в этот URL-адрес приведет к IIS, чтобы вернуть ошибку 404.</span><span class="sxs-lookup"><span data-stu-id="5de26-138">In a typical IIS configuration, the dot in this URL will cause IIS to return error 404.</span></span> <span data-ttu-id="5de26-139">Вы можете решить эту проблему, добавив следующий раздел в файл Web.Config:</span><span class="sxs-lookup"><span data-stu-id="5de26-139">You can resolve this by adding the following section to your Web.Config file:</span></span>

[!code-xml[Main](odata-actions-and-functions/samples/sample5.xml)]

### <a name="add-a-controller-method-for-the-action"></a><span data-ttu-id="5de26-140">Добавьте метод контроллера для действия</span><span class="sxs-lookup"><span data-stu-id="5de26-140">Add a Controller Method for the Action</span></span>

<span data-ttu-id="5de26-141">Чтобы включить &quot;скорость&quot; действие, добавьте следующий метод в `ProductsController`:</span><span class="sxs-lookup"><span data-stu-id="5de26-141">To enable the &quot;Rate&quot; action, add the following method to `ProductsController`:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample6.cs)]

<span data-ttu-id="5de26-142">Обратите внимание на то, что имя метода совпадает с именем действия.</span><span class="sxs-lookup"><span data-stu-id="5de26-142">Notice that the method name matches the action name.</span></span> <span data-ttu-id="5de26-143">**[HttpPost]** атрибут задает метод является методом HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="5de26-143">The **[HttpPost]** attribute specifies the method is an HTTP POST method.</span></span>

<span data-ttu-id="5de26-144">Для вызова действия, клиент отправляет запрос HTTP POST, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="5de26-144">To invoke the action, the client sends an HTTP POST request like the following:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample7.cmd)]

<span data-ttu-id="5de26-145">&quot;Скорость&quot; действие привязано к экземпляров продукта, поэтому URI для действия является имя полного действия, добавляемый в конец URI сущности.</span><span class="sxs-lookup"><span data-stu-id="5de26-145">The &quot;Rate&quot; action is bound to Product instances, so the URI for the action is the fully-qualified action name appended to the entity URI.</span></span> <span data-ttu-id="5de26-146">(Помните, что мы значение в пространстве имен EDM &quot;ProductService&quot;, поэтому действия полное доменное имя &quot;ProductService.Rate&quot;.)</span><span class="sxs-lookup"><span data-stu-id="5de26-146">(Recall that we set the EDM namespace to &quot;ProductService&quot;, so the fully-qualified action name is &quot;ProductService.Rate&quot;.)</span></span>

<span data-ttu-id="5de26-147">Текст запроса содержит параметры действий как полезные данные JSON.</span><span class="sxs-lookup"><span data-stu-id="5de26-147">The body of the request contains the action parameters as a JSON payload.</span></span> <span data-ttu-id="5de26-148">Веб-API автоматически преобразует полезные данные JSON для **ODataActionParameters** объект, который является просто словарь значений параметров.</span><span class="sxs-lookup"><span data-stu-id="5de26-148">Web API automatically converts the JSON payload to an **ODataActionParameters** object, which is just a dictionary of parameter values.</span></span> <span data-ttu-id="5de26-149">Этот словарь можно используйте для доступа к параметрам метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="5de26-149">Use this dictionary to access the parameters in your controller method.</span></span>

<span data-ttu-id="5de26-150">Если клиент отправляет параметры действий в неправильном форматирования, значения **ModelState.IsValid** имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="5de26-150">If the client sends the action parameters in the wrong format, the value of **ModelState.IsValid** is false.</span></span> <span data-ttu-id="5de26-151">Установите этот флажок, в методе контроллера и возвращает ошибку, если **IsValid** имеет значение false.</span><span class="sxs-lookup"><span data-stu-id="5de26-151">Check this flag in your controller method and return an error if **IsValid** is false.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample8.cs)]

## <a name="example-adding-a-function"></a><span data-ttu-id="5de26-152">Пример: Добавление функции</span><span class="sxs-lookup"><span data-stu-id="5de26-152">Example: Adding a Function</span></span>

<span data-ttu-id="5de26-153">Теперь давайте добавим функцию OData, который возвращает самых дорогих продуктов.</span><span class="sxs-lookup"><span data-stu-id="5de26-153">Now let's add an OData function that returns the most expensive product.</span></span> <span data-ttu-id="5de26-154">Как ранее, первым шагом является добавление функции модели EDM.</span><span class="sxs-lookup"><span data-stu-id="5de26-154">As before, the first step is adding the function to the EDM.</span></span> <span data-ttu-id="5de26-155">В файле WebApiConfig.cs добавьте следующий код.</span><span class="sxs-lookup"><span data-stu-id="5de26-155">In WebApiConfig.cs, add the following code.</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample9.cs)]

<span data-ttu-id="5de26-156">В этом случае функция привязана к коллекции продуктов, а не отдельных экземпляров продукта.</span><span class="sxs-lookup"><span data-stu-id="5de26-156">In this case, the function is bound to the Products collection, rather than individual Product instances.</span></span> <span data-ttu-id="5de26-157">Клиенты вызывать функцию, отправив запрос GET:</span><span class="sxs-lookup"><span data-stu-id="5de26-157">Clients invoke the function by sending a GET request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample10.cmd)]

<span data-ttu-id="5de26-158">Ниже приведен метод контроллера для этой функции.</span><span class="sxs-lookup"><span data-stu-id="5de26-158">Here is the controller method for this function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample11.cs)]

<span data-ttu-id="5de26-159">Обратите внимание на то, что имя метода соответствует имени функции.</span><span class="sxs-lookup"><span data-stu-id="5de26-159">Notice that the method name matches the function name.</span></span> <span data-ttu-id="5de26-160">**[HttpGet]** атрибут задает метод является методом HTTP GET.</span><span class="sxs-lookup"><span data-stu-id="5de26-160">The **[HttpGet]** attribute specifies the method is an HTTP GET method.</span></span>

<span data-ttu-id="5de26-161">Ниже приведен HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="5de26-161">Here is the HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample12.cmd)]

## <a name="example-adding-an-unbound-function"></a><span data-ttu-id="5de26-162">Пример: Добавление несвязанную функцию</span><span class="sxs-lookup"><span data-stu-id="5de26-162">Example: Adding an Unbound Function</span></span>

<span data-ttu-id="5de26-163">Предыдущий пример был привязан к коллекции функции.</span><span class="sxs-lookup"><span data-stu-id="5de26-163">The previous example was a function bound to a collection.</span></span> <span data-ttu-id="5de26-164">В этом примере мы создадим *несвязанного* функции.</span><span class="sxs-lookup"><span data-stu-id="5de26-164">In this next example, we'll create an *unbound* function.</span></span> <span data-ttu-id="5de26-165">Свободные функции вызываются как статические операции службы.</span><span class="sxs-lookup"><span data-stu-id="5de26-165">Unbound functions are called as static operations on the service.</span></span> <span data-ttu-id="5de26-166">В этом примере функция возвращает налог для данного почтового индекса.</span><span class="sxs-lookup"><span data-stu-id="5de26-166">The function in this example will return the sales tax for a given postal code.</span></span>

<span data-ttu-id="5de26-167">Добавьте функцию модели EDM в файле WebApiConfig:</span><span class="sxs-lookup"><span data-stu-id="5de26-167">In the WebApiConfig file, add the function to the EDM:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample13.cs)]

<span data-ttu-id="5de26-168">Обратите внимание, что мы называем **функция** непосредственно на **ODataModelBuilder**, а не типом сущности или коллекцией.</span><span class="sxs-lookup"><span data-stu-id="5de26-168">Notice that we are calling **Function** directly on the **ODataModelBuilder**, instead of the entity type or collection.</span></span> <span data-ttu-id="5de26-169">Это сообщает построитель модели, что функция является свободным.</span><span class="sxs-lookup"><span data-stu-id="5de26-169">This tells the model builder that the function is unbound.</span></span>

<span data-ttu-id="5de26-170">Ниже приведен метод контроллера, реализующий функцию.</span><span class="sxs-lookup"><span data-stu-id="5de26-170">Here is the controller method that implements the function:</span></span>

[!code-csharp[Main](odata-actions-and-functions/samples/sample14.cs)]

<span data-ttu-id="5de26-171">Неважно, какой контроллер веб-API, поместите этот метод в.</span><span class="sxs-lookup"><span data-stu-id="5de26-171">It does not matter which Web API controller you place this method in.</span></span> <span data-ttu-id="5de26-172">Вы можете задать для него `ProductsController`, или определить отдельный контроллер.</span><span class="sxs-lookup"><span data-stu-id="5de26-172">You could put it in `ProductsController`, or define a separate controller.</span></span> <span data-ttu-id="5de26-173">**[ODataRoute]** атрибут определяет шаблон URI для функции.</span><span class="sxs-lookup"><span data-stu-id="5de26-173">The **[ODataRoute]** attribute defines the URI template for the function.</span></span>

<span data-ttu-id="5de26-174">Ниже приведен пример запроса клиента.</span><span class="sxs-lookup"><span data-stu-id="5de26-174">Here is an example client request:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample15.cmd)]

<span data-ttu-id="5de26-175">HTTP-ответа:</span><span class="sxs-lookup"><span data-stu-id="5de26-175">The HTTP response:</span></span>

[!code-console[Main](odata-actions-and-functions/samples/sample16.cmd)]
