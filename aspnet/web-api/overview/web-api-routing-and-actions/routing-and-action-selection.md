---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Маршрутизация и выбор действий в ASP.NET Web API | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: 997582263bd48590b74434ee0ffc6be928fa1e08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="6da6d-102">Маршрутизация и выбор действий в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="6da6d-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="6da6d-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="6da6d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="6da6d-104">В этой статье описывается, как веб-API ASP.NET маршрутизирует запрос HTTP для определенного действия на контроллере.</span><span class="sxs-lookup"><span data-stu-id="6da6d-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="6da6d-105">Высокоуровневый обзор маршрутизации см. в разделе [маршрутизации в ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="6da6d-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="6da6d-106">В этой статье рассматриваются сведения процесса маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="6da6d-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="6da6d-107">Если необходимо создать проект веб-API и найти том, что некоторые запросы не получить направлено должным образом, надеемся Эта статья поможет.</span><span class="sxs-lookup"><span data-stu-id="6da6d-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="6da6d-108">Маршрутизация имеет три основных этапа:</span><span class="sxs-lookup"><span data-stu-id="6da6d-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="6da6d-109">Сопоставления URI для шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="6da6d-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="6da6d-110">При выборе контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-110">Selecting a controller.</span></span>
3. <span data-ttu-id="6da6d-111">Выберите действие.</span><span class="sxs-lookup"><span data-stu-id="6da6d-111">Selecting an action.</span></span>

<span data-ttu-id="6da6d-112">Некоторые части процесса можно заменить собственные пользовательские поведения.</span><span class="sxs-lookup"><span data-stu-id="6da6d-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="6da6d-113">В этой статье описывается поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6da6d-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="6da6d-114">В конце я Обратите внимание, везде, где можно настроить поведение.</span><span class="sxs-lookup"><span data-stu-id="6da6d-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="6da6d-115">Шаблоны маршрута</span><span class="sxs-lookup"><span data-stu-id="6da6d-115">Route Templates</span></span>

<span data-ttu-id="6da6d-116">Шаблон маршрута выглядит аналогично пути URI, но может иметь значения заполнителя, указав фигурные скобки:</span><span class="sxs-lookup"><span data-stu-id="6da6d-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="6da6d-117">При создании маршрута можно предоставить значения по умолчанию для некоторых или всех заполнителей:</span><span class="sxs-lookup"><span data-stu-id="6da6d-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="6da6d-118">Можно также ввести ограничения, которые ограничивают как сегмент URI можно сопоставить заполнитель:</span><span class="sxs-lookup"><span data-stu-id="6da6d-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="6da6d-119">Платформа ищет совпадение сегментов в пути URI к шаблону.</span><span class="sxs-lookup"><span data-stu-id="6da6d-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="6da6d-120">Литералы в шаблоне должны точно совпадать.</span><span class="sxs-lookup"><span data-stu-id="6da6d-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="6da6d-121">Заполнитель совпадает с любым значением, если не указать ограничения.</span><span class="sxs-lookup"><span data-stu-id="6da6d-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="6da6d-122">Платформа не соответствует другие части URI, таких как имя узла или параметры запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="6da6d-123">Платформа выбирает первый маршрут в таблице маршрутов, который соответствует URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="6da6d-124">Существует два специальных заполнителей: «{controller}» и «{action}».</span><span class="sxs-lookup"><span data-stu-id="6da6d-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="6da6d-125">«{controller}» предоставляет имя контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="6da6d-126">«{action}» содержит имя действия.</span><span class="sxs-lookup"><span data-stu-id="6da6d-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="6da6d-127">В веб-API обычных соглашений — пропустить «{action}».</span><span class="sxs-lookup"><span data-stu-id="6da6d-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="6da6d-128">расписания</span><span class="sxs-lookup"><span data-stu-id="6da6d-128">Defaults</span></span>

<span data-ttu-id="6da6d-129">При указании значения по умолчанию маршрута будет соответствовать URI, который не удалось обнаружить эти сегменты.</span><span class="sxs-lookup"><span data-stu-id="6da6d-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="6da6d-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="6da6d-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="6da6d-131">URI "`http://localhost/api/products`" будет соответствовать этому маршруту.</span><span class="sxs-lookup"><span data-stu-id="6da6d-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="6da6d-132">Сегмент {«категория»} назначается значение по умолчанию «все».</span><span class="sxs-lookup"><span data-stu-id="6da6d-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="6da6d-133">Словарь маршрута</span><span class="sxs-lookup"><span data-stu-id="6da6d-133">Route Dictionary</span></span>

<span data-ttu-id="6da6d-134">Если платформа находит соответствие для URI, он создает словарь, содержащий значение для каждого заполнителя.</span><span class="sxs-lookup"><span data-stu-id="6da6d-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="6da6d-135">Ключи являются именами заполнителей, не включая фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="6da6d-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="6da6d-136">Значения берутся из пути URI или значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6da6d-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="6da6d-137">Словарь хранится в **IHttpRouteData** объекта.</span><span class="sxs-lookup"><span data-stu-id="6da6d-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="6da6d-138">На этом этапе сопоставление маршрутов специальной «{controller}» и «{action}» заполнители, обрабатываются так же, как другие заполнители.</span><span class="sxs-lookup"><span data-stu-id="6da6d-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="6da6d-139">Просто они хранятся в словаре с другими значениями.</span><span class="sxs-lookup"><span data-stu-id="6da6d-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="6da6d-140">Значение по умолчанию имеют специальное значение **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="6da6d-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="6da6d-141">Если заполнитель возвращает это значение, значение не добавляется в словарь маршрута.</span><span class="sxs-lookup"><span data-stu-id="6da6d-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="6da6d-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="6da6d-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="6da6d-143">Словарь маршрута для пути URI «api и продукты» будет содержать следующее:</span><span class="sxs-lookup"><span data-stu-id="6da6d-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="6da6d-144">контроллер: «продукты»</span><span class="sxs-lookup"><span data-stu-id="6da6d-144">controller: "products"</span></span>
- <span data-ttu-id="6da6d-145">Категория: «все»</span><span class="sxs-lookup"><span data-stu-id="6da6d-145">category: "all"</span></span>

<span data-ttu-id="6da6d-146">«Api/продукты/toys/123» Однако словарь маршрута будет содержать:</span><span class="sxs-lookup"><span data-stu-id="6da6d-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="6da6d-147">контроллер: «продукты»</span><span class="sxs-lookup"><span data-stu-id="6da6d-147">controller: "products"</span></span>
- <span data-ttu-id="6da6d-148">Категория: «toys»</span><span class="sxs-lookup"><span data-stu-id="6da6d-148">category: "toys"</span></span>
- <span data-ttu-id="6da6d-149">Идентификатор: «123»</span><span class="sxs-lookup"><span data-stu-id="6da6d-149">id: "123"</span></span>

<span data-ttu-id="6da6d-150">Значения по умолчанию можно также включить значение, которое не отображается в любом месте в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="6da6d-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="6da6d-151">Если этот маршрут соответствует, это значение хранится в словаре.</span><span class="sxs-lookup"><span data-stu-id="6da6d-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="6da6d-152">Пример:</span><span class="sxs-lookup"><span data-stu-id="6da6d-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="6da6d-153">Если пути URI «корневой/api/8», словарь будет содержать два значения:</span><span class="sxs-lookup"><span data-stu-id="6da6d-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="6da6d-154">контроллер: «заказчики»</span><span class="sxs-lookup"><span data-stu-id="6da6d-154">controller: "customers"</span></span>
- <span data-ttu-id="6da6d-155">Идентификатор: «8»</span><span class="sxs-lookup"><span data-stu-id="6da6d-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="6da6d-156">При выборе контроллера</span><span class="sxs-lookup"><span data-stu-id="6da6d-156">Selecting a Controller</span></span>

<span data-ttu-id="6da6d-157">Выбор контроллера обрабатывается **IHttpControllerSelector.SelectController** метод.</span><span class="sxs-lookup"><span data-stu-id="6da6d-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="6da6d-158">Этот метод принимает **HttpRequestMessage** экземпляр и возвращает **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="6da6d-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="6da6d-159">Реализация по умолчанию обеспечивается **DefaultHttpControllerSelector** класса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="6da6d-160">Этот класс будет использоваться алгоритм прост:</span><span class="sxs-lookup"><span data-stu-id="6da6d-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="6da6d-161">Искать в словарь маршрута для ключа «controller».</span><span class="sxs-lookup"><span data-stu-id="6da6d-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="6da6d-162">Использовано значение для этого ключа и добавить строку «Controller», чтобы получить имя типа контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="6da6d-163">Ищите контроллер веб-API с таким именем типа.</span><span class="sxs-lookup"><span data-stu-id="6da6d-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="6da6d-164">Например если маршрут словарь содержит пару ключ значение «controller» = «продукты», «ProductsController» имеет тип контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="6da6d-165">Если отсутствует соответствующий тип или несколько соответствий, платформа возвращает ошибку для клиента.</span><span class="sxs-lookup"><span data-stu-id="6da6d-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="6da6d-166">На шаге 3 **DefaultHttpControllerSelector** использует **IHttpControllerTypeResolver** интерфейс для получения списка типов контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="6da6d-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="6da6d-167">Реализация по умолчанию **IHttpControllerTypeResolver** возвращает все открытые классы, реализующие (a) **IHttpController**, (б) являются не абстрактный и (c) иметь имя, которое заканчивается на «Controller».</span><span class="sxs-lookup"><span data-stu-id="6da6d-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="6da6d-168">Выбор действия</span><span class="sxs-lookup"><span data-stu-id="6da6d-168">Action Selection</span></span>

<span data-ttu-id="6da6d-169">После выбора контроллера, платформа выбирает действие, вызвав **IHttpActionSelector.SelectAction** метод.</span><span class="sxs-lookup"><span data-stu-id="6da6d-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="6da6d-170">Этот метод принимает **HttpControllerContext** и возвращает **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="6da6d-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="6da6d-171">Реализация по умолчанию обеспечивается **ApiControllerActionSelector** класса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="6da6d-172">Чтобы выбрать действие, он выполняет поиск следующих:</span><span class="sxs-lookup"><span data-stu-id="6da6d-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="6da6d-173">Метод HTTP запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="6da6d-174">Заполнитель «{action}» в шаблон маршрута, если он имеется.</span><span class="sxs-lookup"><span data-stu-id="6da6d-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="6da6d-175">Параметры действий в контроллере.</span><span class="sxs-lookup"><span data-stu-id="6da6d-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="6da6d-176">Перед рассмотрением алгоритм выбора, мы должны понять следующее о действиях контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="6da6d-177">**Какие именно методы на контроллере, считаются «действия»?**</span><span class="sxs-lookup"><span data-stu-id="6da6d-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="6da6d-178">При выборе действия, платформа считывает только открытые методы экземпляра на контроллере.</span><span class="sxs-lookup"><span data-stu-id="6da6d-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="6da6d-179">Кроме того, он исключает [«специальным именем»](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) методы (конструкторы, события, перегрузки операторов и т. д.) и методы, унаследованные от **ApiController** класса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="6da6d-180">**Методы HTTP.**</span><span class="sxs-lookup"><span data-stu-id="6da6d-180">**HTTP Methods.**</span></span> <span data-ttu-id="6da6d-181">Платформа выбирает только действий, которые соответствуют метод HTTP запроса определяется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6da6d-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="6da6d-182">Можно указать метод HTTP с атрибутом: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, или **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="6da6d-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="6da6d-183">В противном случае если имя метода контроллера начинается с «Get», «Post», «Put», «Delete», «Заголовок», «Параметры» или «Исправление», затем по соглашению действие поддерживает, HTTP-метод.</span><span class="sxs-lookup"><span data-stu-id="6da6d-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="6da6d-184">Если ни один из перечисленных выше поддерживает метод POST.</span><span class="sxs-lookup"><span data-stu-id="6da6d-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="6da6d-185">**Привязки параметров.**</span><span class="sxs-lookup"><span data-stu-id="6da6d-185">**Parameter Bindings.**</span></span> <span data-ttu-id="6da6d-186">Привязки параметра состоит в том, как веб-API создает значения для параметра.</span><span class="sxs-lookup"><span data-stu-id="6da6d-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="6da6d-187">Вот правило по умолчанию для привязки параметра.</span><span class="sxs-lookup"><span data-stu-id="6da6d-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="6da6d-188">Простые типы, взяты из URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="6da6d-189">Сложные типы, взяты из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="6da6d-190">Простые типы включают все [простые типы .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), плюс **DateTime**, **десятичное**, **Guid**, **строки** , и **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="6da6d-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="6da6d-191">Для каждого действия не более одного параметра может считывать текст запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="6da6d-192">Это можно переопределить правила привязки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="6da6d-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="6da6d-193">В разделе [привязки параметра WebAPI за кулисами](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="6da6d-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="6da6d-194">С помощью данного цвета фона Вот алгоритм выбора действия.</span><span class="sxs-lookup"><span data-stu-id="6da6d-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="6da6d-195">Создайте список всех действий в контроллере, соответствующий метод HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="6da6d-196">Если словарь маршрута есть запись «действия», удалите действия, имя которого не соответствует это значение.</span><span class="sxs-lookup"><span data-stu-id="6da6d-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="6da6d-197">Повторите для сопоставления параметров действия на URI следующим образом:</span><span class="sxs-lookup"><span data-stu-id="6da6d-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="6da6d-198">Для каждого действия получите список параметров, которые простого типа, где привязка получает параметр из URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="6da6d-199">Исключите необязательные параметры.</span><span class="sxs-lookup"><span data-stu-id="6da6d-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="6da6d-200">В этом списке попробуйте найти совпадения для каждого параметра, словарь маршрута или в строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="6da6d-201">Совпадения не зависят от регистра и не зависят от порядка параметров.</span><span class="sxs-lookup"><span data-stu-id="6da6d-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="6da6d-202">Выберите действие, где каждый параметр в списке имеет соответствие в URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="6da6d-203">Если более одного действия не отвечает этим критериям, выберите один с большинство параметров соответствия.</span><span class="sxs-lookup"><span data-stu-id="6da6d-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="6da6d-204">Игнорировать действия с **[NonAction]** атрибута.</span><span class="sxs-lookup"><span data-stu-id="6da6d-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="6da6d-205">Шаг #3 — вероятно, наиболее путаницу.</span><span class="sxs-lookup"><span data-stu-id="6da6d-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="6da6d-206">Основная идея заключается в том, что его значение можно получить параметр из URI, в тексте запроса или из пользовательской привязки.</span><span class="sxs-lookup"><span data-stu-id="6da6d-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="6da6d-207">Для параметров, полученные из URI мы хотим убедиться, что URI фактически содержит значение для этого параметра в пути (через словарь маршрута) или в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="6da6d-208">Например рассмотрим следующее действие:</span><span class="sxs-lookup"><span data-stu-id="6da6d-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="6da6d-209">*Идентификатор* привязка параметра к URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="6da6d-210">Таким образом это действие может соответствовать только URI, который содержит значение «идентификатор», или в строке запроса словарь маршрута.</span><span class="sxs-lookup"><span data-stu-id="6da6d-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="6da6d-211">Необязательные параметры представляют собой исключение, так как они не являются обязательными.</span><span class="sxs-lookup"><span data-stu-id="6da6d-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="6da6d-212">Для необязательного параметра является работоспособным, если привязка не удалось получить значение из URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="6da6d-213">Исключением являются сложные типы по другой причине.</span><span class="sxs-lookup"><span data-stu-id="6da6d-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="6da6d-214">Сложный тип может быть привязан только к URI посредством пользовательской привязки.</span><span class="sxs-lookup"><span data-stu-id="6da6d-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="6da6d-215">Однако в этом случае платформа не может заранее знать, будет ли параметр привязан определенный URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="6da6d-216">Чтобы узнать, его потребуется вызвать привязки.</span><span class="sxs-lookup"><span data-stu-id="6da6d-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="6da6d-217">Алгоритм выбора предназначена для выбора действия из статических описания, перед вызовом любых привязок.</span><span class="sxs-lookup"><span data-stu-id="6da6d-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="6da6d-218">Таким образом сложные типы, исключаются из алгоритм сопоставления.</span><span class="sxs-lookup"><span data-stu-id="6da6d-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="6da6d-219">После выбора действия вызываются все привязки параметров.</span><span class="sxs-lookup"><span data-stu-id="6da6d-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="6da6d-220">Сводка:</span><span class="sxs-lookup"><span data-stu-id="6da6d-220">Summary:</span></span>

- <span data-ttu-id="6da6d-221">Действие должно соответствовать метод HTTP запроса.</span><span class="sxs-lookup"><span data-stu-id="6da6d-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="6da6d-222">Имя действия должна совпадать с записью «действие» в словарь маршрута, при его наличии.</span><span class="sxs-lookup"><span data-stu-id="6da6d-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="6da6d-223">Для каждого параметра действия Если параметр берется из URI, затем имя параметра необходимо найти в словарь маршрута или в строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="6da6d-224">(Исключаются необязательные параметры и параметры со сложными типами).</span><span class="sxs-lookup"><span data-stu-id="6da6d-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="6da6d-225">Пытаются найти наибольшее число параметров.</span><span class="sxs-lookup"><span data-stu-id="6da6d-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="6da6d-226">Оптимальная может быть метод без параметров.</span><span class="sxs-lookup"><span data-stu-id="6da6d-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="6da6d-227">Подробный пример</span><span class="sxs-lookup"><span data-stu-id="6da6d-227">Extended Example</span></span>

<span data-ttu-id="6da6d-228">Маршруты:</span><span class="sxs-lookup"><span data-stu-id="6da6d-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="6da6d-229">Контроллер:</span><span class="sxs-lookup"><span data-stu-id="6da6d-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="6da6d-230">HTTP-запроса:</span><span class="sxs-lookup"><span data-stu-id="6da6d-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="6da6d-231">Маршрутизации в компоненте</span><span class="sxs-lookup"><span data-stu-id="6da6d-231">Route Matching</span></span>

<span data-ttu-id="6da6d-232">URI, совпадает с именем «DefaultApi» маршрут.</span><span class="sxs-lookup"><span data-stu-id="6da6d-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="6da6d-233">Словарь маршрута содержит следующие записи:</span><span class="sxs-lookup"><span data-stu-id="6da6d-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="6da6d-234">контроллер: «продукты»</span><span class="sxs-lookup"><span data-stu-id="6da6d-234">controller: "products"</span></span>
- <span data-ttu-id="6da6d-235">Идентификатор: «1»</span><span class="sxs-lookup"><span data-stu-id="6da6d-235">id: "1"</span></span>

<span data-ttu-id="6da6d-236">Словарь маршрута не содержит параметров строки запроса, «версия» и «подробности», но они по-прежнему будут учитываться во время выбора действия.</span><span class="sxs-lookup"><span data-stu-id="6da6d-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="6da6d-237">Выбор контроллера</span><span class="sxs-lookup"><span data-stu-id="6da6d-237">Controller Selection</span></span>

<span data-ttu-id="6da6d-238">Из записи «controller» в словарь маршрута, является тип контроллера `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="6da6d-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="6da6d-239">Выбор действия</span><span class="sxs-lookup"><span data-stu-id="6da6d-239">Action Selection</span></span>

<span data-ttu-id="6da6d-240">HTTP-запрос — это запрос GET.</span><span class="sxs-lookup"><span data-stu-id="6da6d-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="6da6d-241">Действия контроллера, поддерживающих GET являются `GetAll`, `GetById`, и `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="6da6d-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="6da6d-242">Словарь маршрута не содержит запись для «действия», поэтому нам не нужен для сопоставления имени действия.</span><span class="sxs-lookup"><span data-stu-id="6da6d-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="6da6d-243">Затем выполняется попытка сопоставить имена параметров для действий, просмотрев операции GET.</span><span class="sxs-lookup"><span data-stu-id="6da6d-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="6da6d-244">Действие</span><span class="sxs-lookup"><span data-stu-id="6da6d-244">Action</span></span> | <span data-ttu-id="6da6d-245">Параметры соответствия</span><span class="sxs-lookup"><span data-stu-id="6da6d-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="6da6d-246">Нет</span><span class="sxs-lookup"><span data-stu-id="6da6d-246">none</span></span> |
| `GetById` | <span data-ttu-id="6da6d-247">«Идентификатор»</span><span class="sxs-lookup"><span data-stu-id="6da6d-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="6da6d-248">«Имя»</span><span class="sxs-lookup"><span data-stu-id="6da6d-248">"name"</span></span> |

<span data-ttu-id="6da6d-249">Обратите внимание, что *версии* параметр `GetById` не учитываются, поскольку он является необязательным параметром.</span><span class="sxs-lookup"><span data-stu-id="6da6d-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="6da6d-250">`GetAll` Элементарно соответствующий метод.</span><span class="sxs-lookup"><span data-stu-id="6da6d-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="6da6d-251">`GetById` Также соответствует метод, так как «id» содержит словарь маршрута.</span><span class="sxs-lookup"><span data-stu-id="6da6d-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="6da6d-252">`FindProductsByName` Метод не совпадает.</span><span class="sxs-lookup"><span data-stu-id="6da6d-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="6da6d-253">`GetById` Побеждает метод, так как он соответствует один параметр и без параметров для `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="6da6d-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="6da6d-254">Метод вызывается со следующими значениями параметров:</span><span class="sxs-lookup"><span data-stu-id="6da6d-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="6da6d-255">*id* = 1</span><span class="sxs-lookup"><span data-stu-id="6da6d-255">*id* = 1</span></span>
- <span data-ttu-id="6da6d-256">*версия* = 1.5</span><span class="sxs-lookup"><span data-stu-id="6da6d-256">*version* = 1.5</span></span>

<span data-ttu-id="6da6d-257">Обратите внимание, что даже если *версии* не использовался в алгоритм выбора, значение параметра берется из строки запроса URI.</span><span class="sxs-lookup"><span data-stu-id="6da6d-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="6da6d-258">Точки расширения</span><span class="sxs-lookup"><span data-stu-id="6da6d-258">Extension Points</span></span>

<span data-ttu-id="6da6d-259">Веб-API предоставляет точки расширения для некоторых частей процесса маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="6da6d-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="6da6d-260">Интерфейс</span><span class="sxs-lookup"><span data-stu-id="6da6d-260">Interface</span></span> | <span data-ttu-id="6da6d-261">Описание:</span><span class="sxs-lookup"><span data-stu-id="6da6d-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="6da6d-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="6da6d-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="6da6d-263">Выбирает контроллер.</span><span class="sxs-lookup"><span data-stu-id="6da6d-263">Selects the controller.</span></span> |
| <span data-ttu-id="6da6d-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="6da6d-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="6da6d-265">Получает список типов контроллеров.</span><span class="sxs-lookup"><span data-stu-id="6da6d-265">Gets the list of controller types.</span></span> <span data-ttu-id="6da6d-266">**DefaultHttpControllerSelector** выбирает тип контроллера из этого списка.</span><span class="sxs-lookup"><span data-stu-id="6da6d-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="6da6d-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="6da6d-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="6da6d-268">Возвращает список сборок, проект.</span><span class="sxs-lookup"><span data-stu-id="6da6d-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="6da6d-269">**IHttpControllerTypeResolver** интерфейс использует этот список, чтобы найти типы контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="6da6d-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="6da6d-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="6da6d-271">Создает новый экземпляр контроллера.</span><span class="sxs-lookup"><span data-stu-id="6da6d-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="6da6d-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="6da6d-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="6da6d-273">Выбирает действие.</span><span class="sxs-lookup"><span data-stu-id="6da6d-273">Selects the action.</span></span> |
| <span data-ttu-id="6da6d-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="6da6d-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="6da6d-275">Вызывает действие.</span><span class="sxs-lookup"><span data-stu-id="6da6d-275">Invokes the action.</span></span> |

<span data-ttu-id="6da6d-276">Чтобы предоставить собственную реализацию для любого из этих интерфейсов, используйте **службы** коллекции на **HttpConfiguration** объекта:</span><span class="sxs-lookup"><span data-stu-id="6da6d-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
