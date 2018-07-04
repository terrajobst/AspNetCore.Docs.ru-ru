---
uid: web-api/overview/web-api-routing-and-actions/routing-and-action-selection
title: Маршрутизация и выбор действий в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2012
ms.topic: article
ms.assetid: bcf2d223-cb7f-411e-be05-f43e96a14015
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-and-action-selection
msc.type: authoredcontent
ms.openlocfilehash: dc0edbdf62ceab1baf441301b47c0293de9a5c5d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388618"
---
<a name="routing-and-action-selection-in-aspnet-web-api"></a><span data-ttu-id="0b233-102">Маршрутизация и выбор действий в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0b233-102">Routing and Action Selection in ASP.NET Web API</span></span>
====================
<span data-ttu-id="0b233-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0b233-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0b233-104">В этой статье описывается, как веб-API ASP.NET направляет HTTP-запрос для определенного действия на контроллере.</span><span class="sxs-lookup"><span data-stu-id="0b233-104">This article describes how ASP.NET Web API routes an HTTP request to a particular action on a controller.</span></span>

> [!NOTE]
> <span data-ttu-id="0b233-105">Общий обзор маршрутизации, см. в разделе [маршрутизации в ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0b233-105">For a high-level overview of routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>


<span data-ttu-id="0b233-106">В этой статье рассматриваются аспекты процесс маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="0b233-106">This article looks at the details of the routing process.</span></span> <span data-ttu-id="0b233-107">Если вы создаете проект веб-API и проверили, что некоторые запросы не поймите маршрутизироваться должным образом, будем надеяться, что эта статья поможет.</span><span class="sxs-lookup"><span data-stu-id="0b233-107">If you create a Web API project and find that some requests don't get routed the way you expect, hopefully this article will help.</span></span>

<span data-ttu-id="0b233-108">Маршрутизация имеет три основных этапа:</span><span class="sxs-lookup"><span data-stu-id="0b233-108">Routing has three main phases:</span></span>

1. <span data-ttu-id="0b233-109">Сопоставлении по URI в шаблон маршрута.</span><span class="sxs-lookup"><span data-stu-id="0b233-109">Matching the URI to a route template.</span></span>
2. <span data-ttu-id="0b233-110">Выбор контроллера.</span><span class="sxs-lookup"><span data-stu-id="0b233-110">Selecting a controller.</span></span>
3. <span data-ttu-id="0b233-111">Выберите действие.</span><span class="sxs-lookup"><span data-stu-id="0b233-111">Selecting an action.</span></span>

<span data-ttu-id="0b233-112">Некоторые части процесса можно заменить собственные пользовательские поведения.</span><span class="sxs-lookup"><span data-stu-id="0b233-112">You can replace some parts of the process with your own custom behaviors.</span></span> <span data-ttu-id="0b233-113">В этой статье я опишу поведение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0b233-113">In this article, I describe the default behavior.</span></span> <span data-ttu-id="0b233-114">В итоге я Обратите внимание, местах, где можно настроить поведение.</span><span class="sxs-lookup"><span data-stu-id="0b233-114">At the end, I note the places where you can customize the behavior.</span></span>

## <a name="route-templates"></a><span data-ttu-id="0b233-115">Шаблоны маршрутов</span><span class="sxs-lookup"><span data-stu-id="0b233-115">Route Templates</span></span>

<span data-ttu-id="0b233-116">Шаблон маршрута будет выглядеть пути URI, но он может иметь значение заполнителя, обозначаются с помощью фигурных скобок:</span><span class="sxs-lookup"><span data-stu-id="0b233-116">A route template looks similar to a URI path, but it can have placeholder values, indicated with curly braces:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample1.ps1)]

<span data-ttu-id="0b233-117">При создании маршрута, можно предоставить значения по умолчанию для некоторых или всех заполнителей:</span><span class="sxs-lookup"><span data-stu-id="0b233-117">When you create a route, you can provide default values for some or all of the placeholders:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample2.cs)]

<span data-ttu-id="0b233-118">Также можно указать ограничения, которые ограничивают как сегмент URI может соответствовать заполнитель:</span><span class="sxs-lookup"><span data-stu-id="0b233-118">You can also provide constraints, which restrict how a URI segment can match a placeholder:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample3.js)]

<span data-ttu-id="0b233-119">Платформа framework пытается соответствовать сегменты в пути URI к шаблону.</span><span class="sxs-lookup"><span data-stu-id="0b233-119">The framework tries to match the segments in the URI path to the template.</span></span> <span data-ttu-id="0b233-120">Литералы в шаблоне должны точно совпадать.</span><span class="sxs-lookup"><span data-stu-id="0b233-120">Literals in the template must match exactly.</span></span> <span data-ttu-id="0b233-121">Заполнитель совпадает с любым значением, если не указать ограничения.</span><span class="sxs-lookup"><span data-stu-id="0b233-121">A placeholder matches any value, unless you specify constraints.</span></span> <span data-ttu-id="0b233-122">Другие части URI, например имя узла или параметры запроса не совпадает с платформой.</span><span class="sxs-lookup"><span data-stu-id="0b233-122">The framework does not match other parts of the URI, such as the host name or the query parameters.</span></span> <span data-ttu-id="0b233-123">Платформа framework выбирает первый маршрут в таблице маршрутов, соответствующий URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-123">The framework selects the first route in the route table that matches the URI.</span></span>

<span data-ttu-id="0b233-124">Существуют два специальных заполнителя: «{controller}» и «{action}».</span><span class="sxs-lookup"><span data-stu-id="0b233-124">There are two special placeholders: "{controller}" and "{action}".</span></span>

- <span data-ttu-id="0b233-125">«{controller}» предоставляет имя контроллера.</span><span class="sxs-lookup"><span data-stu-id="0b233-125">"{controller}" provides the name of the controller.</span></span>
- <span data-ttu-id="0b233-126">«{action}» предоставляет имя действия.</span><span class="sxs-lookup"><span data-stu-id="0b233-126">"{action}" provides the name of the action.</span></span> <span data-ttu-id="0b233-127">В веб-API и обычных соглашений является пропуск «{action}».</span><span class="sxs-lookup"><span data-stu-id="0b233-127">In Web API, the usual convention is to omit "{action}".</span></span>

### <a name="defaults"></a><span data-ttu-id="0b233-128">расписания</span><span class="sxs-lookup"><span data-stu-id="0b233-128">Defaults</span></span>

<span data-ttu-id="0b233-129">Если вы предоставляете значения по умолчанию, маршрут будет соответствовать URI, который отсутствует в сегментах.</span><span class="sxs-lookup"><span data-stu-id="0b233-129">If you provide defaults, the route will match a URI that is missing those segments.</span></span> <span data-ttu-id="0b233-130">Пример:</span><span class="sxs-lookup"><span data-stu-id="0b233-130">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample4.cs)]

<span data-ttu-id="0b233-131">URI "`http://localhost/api/products`" будет соответствовать этому маршруту.</span><span class="sxs-lookup"><span data-stu-id="0b233-131">The URI "`http://localhost/api/products`" matches this route.</span></span> <span data-ttu-id="0b233-132">Сегмент «{category}» назначается значение по умолчанию «все».</span><span class="sxs-lookup"><span data-stu-id="0b233-132">The "{category}" segment is assigned the default value "all".</span></span>

### <a name="route-dictionary"></a><span data-ttu-id="0b233-133">Словарь маршрута</span><span class="sxs-lookup"><span data-stu-id="0b233-133">Route Dictionary</span></span>

<span data-ttu-id="0b233-134">Если платформа обнаруживает соответствие для URI, он создает словарь, содержащий значение для каждого местозаполнителя.</span><span class="sxs-lookup"><span data-stu-id="0b233-134">If the framework finds a match for a URI, it creates a dictionary that contains the value for each placeholder.</span></span> <span data-ttu-id="0b233-135">Ключи являются имена-заполнители, не включая фигурные скобки.</span><span class="sxs-lookup"><span data-stu-id="0b233-135">The keys are the placeholder names, not including the curly braces.</span></span> <span data-ttu-id="0b233-136">Значения берутся из пути URI или значения по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0b233-136">The values are taken from the URI path or from the defaults.</span></span> <span data-ttu-id="0b233-137">Словарь хранится в **IHttpRouteData** объекта.</span><span class="sxs-lookup"><span data-stu-id="0b233-137">The dictionary is stored in the **IHttpRouteData** object.</span></span>

<span data-ttu-id="0b233-138">На этом этапе сопоставление маршрутов специальными «{controller}» и «{action}» заполнители, обрабатываются так же, как другие заполнители.</span><span class="sxs-lookup"><span data-stu-id="0b233-138">During this route-matching phase, the special "{controller}" and "{action}" placeholders are treated just like the other placeholders.</span></span> <span data-ttu-id="0b233-139">Просто они хранятся в словаре с другими значениями.</span><span class="sxs-lookup"><span data-stu-id="0b233-139">They are simply stored in the dictionary with the other values.</span></span>

<span data-ttu-id="0b233-140">По умолчанию могут иметь особое значение **RouteParameter.Optional**.</span><span class="sxs-lookup"><span data-stu-id="0b233-140">A default can have the special value **RouteParameter.Optional**.</span></span> <span data-ttu-id="0b233-141">Если заполнитель получает это значение, значение не добавляется в словарь маршрута.</span><span class="sxs-lookup"><span data-stu-id="0b233-141">If a placeholder gets assigned this value, the value is not added to the route dictionary.</span></span> <span data-ttu-id="0b233-142">Пример:</span><span class="sxs-lookup"><span data-stu-id="0b233-142">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample5.cs)]

<span data-ttu-id="0b233-143">Для пути URI «api/продукты» будет содержать словарь маршрута:</span><span class="sxs-lookup"><span data-stu-id="0b233-143">For the URI path "api/products", the route dictionary will contain:</span></span>

- <span data-ttu-id="0b233-144">контроллер: «продукты»</span><span class="sxs-lookup"><span data-stu-id="0b233-144">controller: "products"</span></span>
- <span data-ttu-id="0b233-145">Категория: «все»</span><span class="sxs-lookup"><span data-stu-id="0b233-145">category: "all"</span></span>

<span data-ttu-id="0b233-146">Для «api/продукты/toys/123» Однако словарь маршрута будет содержать:</span><span class="sxs-lookup"><span data-stu-id="0b233-146">For "api/products/toys/123", however, the route dictionary will contain:</span></span>

- <span data-ttu-id="0b233-147">контроллер: «продукты»</span><span class="sxs-lookup"><span data-stu-id="0b233-147">controller: "products"</span></span>
- <span data-ttu-id="0b233-148">Категория: «toys»</span><span class="sxs-lookup"><span data-stu-id="0b233-148">category: "toys"</span></span>
- <span data-ttu-id="0b233-149">Идентификатор: «123»</span><span class="sxs-lookup"><span data-stu-id="0b233-149">id: "123"</span></span>

<span data-ttu-id="0b233-150">Значения по умолчанию также может включать значение, которое не встречается в любом месте в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="0b233-150">The defaults can also include a value that does not appear anywhere in the route template.</span></span> <span data-ttu-id="0b233-151">Если этот маршрут соответствует, это значение хранится в словаре.</span><span class="sxs-lookup"><span data-stu-id="0b233-151">If the route matches, that value is stored in the dictionary.</span></span> <span data-ttu-id="0b233-152">Пример:</span><span class="sxs-lookup"><span data-stu-id="0b233-152">For example:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample6.cs)]

<span data-ttu-id="0b233-153">Если пути URI «корневой/api/8», словарь будет содержать два значения:</span><span class="sxs-lookup"><span data-stu-id="0b233-153">If the URI path is "api/root/8", the dictionary will contain two values:</span></span>

- <span data-ttu-id="0b233-154">контроллер: «customers»</span><span class="sxs-lookup"><span data-stu-id="0b233-154">controller: "customers"</span></span>
- <span data-ttu-id="0b233-155">Идентификатор: «8»</span><span class="sxs-lookup"><span data-stu-id="0b233-155">id: "8"</span></span>

## <a name="selecting-a-controller"></a><span data-ttu-id="0b233-156">Выбор контроллера</span><span class="sxs-lookup"><span data-stu-id="0b233-156">Selecting a Controller</span></span>

<span data-ttu-id="0b233-157">Выбор контроллера обрабатывается **IHttpControllerSelector.SelectController** метод.</span><span class="sxs-lookup"><span data-stu-id="0b233-157">Controller selection is handled by the **IHttpControllerSelector.SelectController** method.</span></span> <span data-ttu-id="0b233-158">Этот метод принимает **HttpRequestMessage** экземпляра и возвращает **HttpControllerDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="0b233-158">This method takes an **HttpRequestMessage** instance and returns an **HttpControllerDescriptor**.</span></span> <span data-ttu-id="0b233-159">Реализация по умолчанию предоставляется **DefaultHttpControllerSelector** класса.</span><span class="sxs-lookup"><span data-stu-id="0b233-159">The default implementation is provided by the **DefaultHttpControllerSelector** class.</span></span> <span data-ttu-id="0b233-160">Этот класс использует простой алгоритм:</span><span class="sxs-lookup"><span data-stu-id="0b233-160">This class uses a straightforward algorithm:</span></span>

1. <span data-ttu-id="0b233-161">Искать в словаре маршрутов для ключа «controller».</span><span class="sxs-lookup"><span data-stu-id="0b233-161">Look in the route dictionary for the key "controller".</span></span>
2. <span data-ttu-id="0b233-162">Значение для этого ключа, добавьте строку «Controller», чтобы получить имя типа контроллера.</span><span class="sxs-lookup"><span data-stu-id="0b233-162">Take the value for this key and append the string "Controller" to get the controller type name.</span></span>
3. <span data-ttu-id="0b233-163">Найдите контроллер Web API с таким именем типа.</span><span class="sxs-lookup"><span data-stu-id="0b233-163">Look for a Web API controller with this type name.</span></span>

<span data-ttu-id="0b233-164">Например если маршрут словарь содержит пары "ключ значение"«controller» = «продукты», тип контроллера является «ProductsController».</span><span class="sxs-lookup"><span data-stu-id="0b233-164">For example, if the route dictionary contains the key-value pair "controller" = "products", then the controller type is "ProductsController".</span></span> <span data-ttu-id="0b233-165">Если отсутствует соответствующий тип или несколько соответствий, платформа возвращает ошибку клиенту.</span><span class="sxs-lookup"><span data-stu-id="0b233-165">If there is no matching type, or multiple matches, the framework returns an error to the client.</span></span>

<span data-ttu-id="0b233-166">Для шага 3 **DefaultHttpControllerSelector** использует **IHttpControllerTypeResolver** интерфейса для получения списка типов контроллера веб-API.</span><span class="sxs-lookup"><span data-stu-id="0b233-166">For step 3, **DefaultHttpControllerSelector** uses the **IHttpControllerTypeResolver** interface to get the list of Web API controller types.</span></span> <span data-ttu-id="0b233-167">Реализация по умолчанию **IHttpControllerTypeResolver** возвращает все открытые классы, реализующие (a) **IHttpController**, (б): не абстрактный и (c) иметь имя, которое заканчивается на «Controller».</span><span class="sxs-lookup"><span data-stu-id="0b233-167">The default implementation of **IHttpControllerTypeResolver** returns all public classes that (a) implement **IHttpController**, (b) are not abstract, and (c) have a name that ends in "Controller".</span></span>

## <a name="action-selection"></a><span data-ttu-id="0b233-168">Выбор действия</span><span class="sxs-lookup"><span data-stu-id="0b233-168">Action Selection</span></span>

<span data-ttu-id="0b233-169">После выбора контроллера, платформа framework выбирает действие, вызвав **IHttpActionSelector.SelectAction** метод.</span><span class="sxs-lookup"><span data-stu-id="0b233-169">After selecting the controller, the framework selects the action by calling the **IHttpActionSelector.SelectAction** method.</span></span> <span data-ttu-id="0b233-170">Этот метод принимает **HttpControllerContext** и возвращает **HttpActionDescriptor**.</span><span class="sxs-lookup"><span data-stu-id="0b233-170">This method takes an **HttpControllerContext** and returns an **HttpActionDescriptor**.</span></span>

<span data-ttu-id="0b233-171">Реализация по умолчанию предоставляется **ApiControllerActionSelector** класса.</span><span class="sxs-lookup"><span data-stu-id="0b233-171">The default implementation is provided by the **ApiControllerActionSelector** class.</span></span> <span data-ttu-id="0b233-172">Чтобы выбрать действие, она проверяет следующее:</span><span class="sxs-lookup"><span data-stu-id="0b233-172">To select an action, it looks at the following:</span></span>

- <span data-ttu-id="0b233-173">Метод HTTP запроса.</span><span class="sxs-lookup"><span data-stu-id="0b233-173">The HTTP method of the request.</span></span>
- <span data-ttu-id="0b233-174">Заполнитель «{action}» в шаблоне маршрута, если он имеется.</span><span class="sxs-lookup"><span data-stu-id="0b233-174">The "{action}" placeholder in the route template, if present.</span></span>
- <span data-ttu-id="0b233-175">Параметры действия на контроллере.</span><span class="sxs-lookup"><span data-stu-id="0b233-175">The parameters of the actions on the controller.</span></span>

<span data-ttu-id="0b233-176">Перед тем как рассмотреть алгоритм выбора, мы должны знать кое о действий контроллера.</span><span class="sxs-lookup"><span data-stu-id="0b233-176">Before looking at the selection algorithm, we need to understand some things about controller actions.</span></span>

<span data-ttu-id="0b233-177">**Какие методы на контроллере, считаются «действия»?**</span><span class="sxs-lookup"><span data-stu-id="0b233-177">**Which methods on the controller are considered "actions"?**</span></span> <span data-ttu-id="0b233-178">При выборе действия, платформа считывает только открытые методы экземпляра на контроллере.</span><span class="sxs-lookup"><span data-stu-id="0b233-178">When selecting an action, the framework only looks at public instance methods on the controller.</span></span> <span data-ttu-id="0b233-179">Кроме того, он исключает [«специальным именем»](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) методы (конструкторы, события, перегрузки операторов и т. д.) и методы, унаследованные от **ApiController** класса.</span><span class="sxs-lookup"><span data-stu-id="0b233-179">Also, it excludes ["special name"](https://msdn.microsoft.com/library/system.reflection.methodbase.isspecialname) methods (constructors, events, operator overloads, and so forth), and methods inherited from the **ApiController** class.</span></span>

<span data-ttu-id="0b233-180">**Методы HTTP.**</span><span class="sxs-lookup"><span data-stu-id="0b233-180">**HTTP Methods.**</span></span> <span data-ttu-id="0b233-181">Платформа framework выбирает только действий, которые соответствуют метод HTTP запроса, определяется следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0b233-181">The framework only chooses actions that match the HTTP method of the request, determined as follows:</span></span>

1. <span data-ttu-id="0b233-182">Можно указать метод HTTP с атрибутом: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**,  **HttpOptions**, **HttpPatch**, **HttpPost**, или **HttpPut**.</span><span class="sxs-lookup"><span data-stu-id="0b233-182">You can specify the HTTP method with an attribute: **AcceptVerbs**, **HttpDelete**, **HttpGet**, **HttpHead**, **HttpOptions**, **HttpPatch**, **HttpPost**, or **HttpPut**.</span></span>
2. <span data-ttu-id="0b233-183">В противном случае если имя метода контроллера начинается с «Get», «Post», «Put», «Удалить», «Head», «Параметры» или «Исправления», затем по соглашению, поддерживаемые действием этого метода HTTP.</span><span class="sxs-lookup"><span data-stu-id="0b233-183">Otherwise, if the name of the controller method starts with "Get", "Post", "Put", "Delete", "Head", "Options", or "Patch", then by convention the action supports that HTTP method.</span></span>
3. <span data-ttu-id="0b233-184">Если ни один из перечисленных выше, поддерживает метод POST.</span><span class="sxs-lookup"><span data-stu-id="0b233-184">If none of the above, the method supports POST.</span></span>

<span data-ttu-id="0b233-185">**Привязки параметров.**</span><span class="sxs-lookup"><span data-stu-id="0b233-185">**Parameter Bindings.**</span></span> <span data-ttu-id="0b233-186">Привязка параметра имеет, как веб-API создает значение для параметра.</span><span class="sxs-lookup"><span data-stu-id="0b233-186">A parameter binding is how Web API creates a value for a parameter.</span></span> <span data-ttu-id="0b233-187">Вот правило по умолчанию для привязки параметров:</span><span class="sxs-lookup"><span data-stu-id="0b233-187">Here is the default rule for parameter binding:</span></span>

- <span data-ttu-id="0b233-188">Простые типы, взяты из URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-188">Simple types are taken from the URI.</span></span>
- <span data-ttu-id="0b233-189">Сложные типы, взяты из текста запроса.</span><span class="sxs-lookup"><span data-stu-id="0b233-189">Complex types are taken from the request body.</span></span>

<span data-ttu-id="0b233-190">Простые типы включают все [простые типы .NET Framework](https://msdn.microsoft.com/library/system.type.isprimitive), а также **даты и времени**, **десятичное**, **Guid**, **строка** , и **TimeSpan**.</span><span class="sxs-lookup"><span data-stu-id="0b233-190">Simple types include all of the [.NET Framework primitive types](https://msdn.microsoft.com/library/system.type.isprimitive), plus **DateTime**, **Decimal**, **Guid**, **String**, and **TimeSpan**.</span></span> <span data-ttu-id="0b233-191">Для каждого действия не более одного параметра может считывать текст запроса.</span><span class="sxs-lookup"><span data-stu-id="0b233-191">For each action, at most one parameter can read the request body.</span></span>

> [!NOTE]
> <span data-ttu-id="0b233-192">Это можно переопределить правила привязки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0b233-192">It is possible to override the default binding rules.</span></span> <span data-ttu-id="0b233-193">См. в разделе [привязки параметров веб-API за кулисами](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span><span class="sxs-lookup"><span data-stu-id="0b233-193">See [WebAPI Parameter binding under the hood](https://blogs.msdn.com/b/jmstall/archive/2012/05/11/webapi-parameter-binding-under-the-hood.aspx).</span></span>


<span data-ttu-id="0b233-194">С учетом этого Вот алгоритм выбора действия.</span><span class="sxs-lookup"><span data-stu-id="0b233-194">With that background, here is the action selection algorithm.</span></span>

1. <span data-ttu-id="0b233-195">Создайте список всех действий на контроллере, который соответствует метод запроса HTTP.</span><span class="sxs-lookup"><span data-stu-id="0b233-195">Create a list of all actions on the controller that match the HTTP request method.</span></span>
2. <span data-ttu-id="0b233-196">Если словарь маршрута имеет запись «действия», удалите действия, имя которого соответствует ли это значение.</span><span class="sxs-lookup"><span data-stu-id="0b233-196">If the route dictionary has an "action" entry, remove actions whose name does not match this value.</span></span>
3. <span data-ttu-id="0b233-197">Повторите для сопоставления параметров действия на URI следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0b233-197">Try to match action parameters to the URI, as follows:</span></span> 

    1. <span data-ttu-id="0b233-198">Для каждого действия получение списка параметров, которые являются простой тип, где привязка получает параметр из URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-198">For each action, get a list of the parameters that are a simple type, where the binding gets the parameter from the URI.</span></span> <span data-ttu-id="0b233-199">Исключите необязательные параметры.</span><span class="sxs-lookup"><span data-stu-id="0b233-199">Exclude optional parameters.</span></span>
    2. <span data-ttu-id="0b233-200">В этом списке попробуйте найти совпадения для каждого параметра, словарь маршрута или в строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-200">From this list, try to find a match for each parameter name, either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="0b233-201">Совпадения учитывается регистр и не зависят от порядка параметров.</span><span class="sxs-lookup"><span data-stu-id="0b233-201">Matches are case insensitive and do not depend on the parameter order.</span></span>
    3. <span data-ttu-id="0b233-202">Выберите действие, где каждый параметр в списке, имеет соответствие в URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-202">Select an action where every parameter in the list has a match in the URI.</span></span>
    4. <span data-ttu-id="0b233-203">Если более одного действия соответствует этим критериям, выберите любой из с большинство параметров соответствия.</span><span class="sxs-lookup"><span data-stu-id="0b233-203">If more that one action meets these criteria, pick the one with the most parameter matches.</span></span>
4. <span data-ttu-id="0b233-204">Игнорировать действия с **[NonAction]** атрибута.</span><span class="sxs-lookup"><span data-stu-id="0b233-204">Ignore actions with the **[NonAction]** attribute.</span></span>

<span data-ttu-id="0b233-205">Шаг #3 является, вероятно, наиболее путаницу.</span><span class="sxs-lookup"><span data-stu-id="0b233-205">Step #3 is probably the most confusing.</span></span> <span data-ttu-id="0b233-206">Основная идея заключается в том, что его значение можно получить параметр из URI, из текста запроса или из пользовательской привязки.</span><span class="sxs-lookup"><span data-stu-id="0b233-206">The basic idea is that a parameter can get its value either from the URI, from the request body, or from a custom binding.</span></span> <span data-ttu-id="0b233-207">Для параметров, полученных из URI мы хотим убедиться, что URI фактически содержит значение для этого параметра, либо в пути (через словарь маршрута), либо в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="0b233-207">For parameters that come from the URI, we want to ensure that the URI actually contains a value for that parameter, either in the path (via the route dictionary) or in the query string.</span></span>

<span data-ttu-id="0b233-208">Например рассмотрим следующее действие:</span><span class="sxs-lookup"><span data-stu-id="0b233-208">For example, consider the following action:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample7.cs)]

<span data-ttu-id="0b233-209">*Идентификатор* привязывается параметр URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-209">The *id* parameter binds to the URI.</span></span> <span data-ttu-id="0b233-210">Таким образом это действие может соответствовать только URI, содержащий значение для «id», либо в словаре маршрутов, либо в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="0b233-210">Therefore, this action can only match a URI that contains a value for "id", either in the route dictionary or in the query string.</span></span>

<span data-ttu-id="0b233-211">Необязательные параметры являются исключением, так как они являются необязательными.</span><span class="sxs-lookup"><span data-stu-id="0b233-211">Optional parameters are an exception, because they are optional.</span></span> <span data-ttu-id="0b233-212">Для необязательного параметра это нормально привязку невозможно получить значение из URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-212">For an optional parameter, it's OK if the binding can't get the value from the URI.</span></span>

<span data-ttu-id="0b233-213">Сложные типы являются исключением по другой причине.</span><span class="sxs-lookup"><span data-stu-id="0b233-213">Complex types are an exception for a different reason.</span></span> <span data-ttu-id="0b233-214">Сложный тип может быть привязан только к URL-АДРЕСУ через пользовательскую привязку.</span><span class="sxs-lookup"><span data-stu-id="0b233-214">A complex type can only bind to the URI through a custom binding.</span></span> <span data-ttu-id="0b233-215">Но в этом случае платформа не может знать заранее ли параметр будет привязан к определенному URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-215">But in that case, the framework cannot know in advance whether the parameter would bind to a particular URI.</span></span> <span data-ttu-id="0b233-216">Чтобы узнать, ей потребуется вызывать привязку.</span><span class="sxs-lookup"><span data-stu-id="0b233-216">To find out, it would need to invoke the binding.</span></span> <span data-ttu-id="0b233-217">Алгоритм выбора призван выберите действие из статических описания, перед вызовом любой привязки.</span><span class="sxs-lookup"><span data-stu-id="0b233-217">The goal of the selection algorithm is to select an action from the static description, before invoking any bindings.</span></span> <span data-ttu-id="0b233-218">Таким образом сложные типы, исключаются из алгоритм сопоставления.</span><span class="sxs-lookup"><span data-stu-id="0b233-218">Therefore, complex types are excluded from the matching algorithm.</span></span>

<span data-ttu-id="0b233-219">После выбора действия вызываются все привязки параметров.</span><span class="sxs-lookup"><span data-stu-id="0b233-219">After the action is selected, all parameter bindings are invoked.</span></span>

<span data-ttu-id="0b233-220">Сводка:</span><span class="sxs-lookup"><span data-stu-id="0b233-220">Summary:</span></span>

- <span data-ttu-id="0b233-221">Действие должно соответствовать метод HTTP запроса.</span><span class="sxs-lookup"><span data-stu-id="0b233-221">The action must match the HTTP method of the request.</span></span>
- <span data-ttu-id="0b233-222">Имя действия должно соответствовать «действие» запись в словаре маршрутов, при его наличии.</span><span class="sxs-lookup"><span data-stu-id="0b233-222">The action name must match the "action" entry in the route dictionary, if present.</span></span>
- <span data-ttu-id="0b233-223">Для каждого параметра действия Если параметр взят из URI, затем имя параметра необходимо найти в словаре маршрутов или в строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-223">For every parameter of the action, if the parameter is taken from the URI, then the parameter name must be found either in the route dictionary or in the URI query string.</span></span> <span data-ttu-id="0b233-224">(Необязательные параметры и параметры со сложными типами, исключаются.)</span><span class="sxs-lookup"><span data-stu-id="0b233-224">(Optional parameters and parameters with complex types are excluded.)</span></span>
- <span data-ttu-id="0b233-225">Попробуйте в соответствии с максимальным числом параметров.</span><span class="sxs-lookup"><span data-stu-id="0b233-225">Try to match the most number of parameters.</span></span> <span data-ttu-id="0b233-226">Возможно, наиболее подходящий метод без параметров.</span><span class="sxs-lookup"><span data-stu-id="0b233-226">The best match might be a method with no parameters.</span></span>

## <a name="extended-example"></a><span data-ttu-id="0b233-227">Развернутый пример</span><span class="sxs-lookup"><span data-stu-id="0b233-227">Extended Example</span></span>

<span data-ttu-id="0b233-228">Маршруты:</span><span class="sxs-lookup"><span data-stu-id="0b233-228">Routes:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample8.cs)]

<span data-ttu-id="0b233-229">Контроллер:</span><span class="sxs-lookup"><span data-stu-id="0b233-229">Controller:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample9.cs)]

<span data-ttu-id="0b233-230">HTTP-запроса:</span><span class="sxs-lookup"><span data-stu-id="0b233-230">HTTP request:</span></span>

[!code-console[Main](routing-and-action-selection/samples/sample10.cmd)]

### <a name="route-matching"></a><span data-ttu-id="0b233-231">Маршрутизации в компоненте</span><span class="sxs-lookup"><span data-stu-id="0b233-231">Route Matching</span></span>

<span data-ttu-id="0b233-232">URI соответствует маршрут с именем «DefaultApi».</span><span class="sxs-lookup"><span data-stu-id="0b233-232">The URI matches the route named "DefaultApi".</span></span> <span data-ttu-id="0b233-233">Словарь маршрута содержит следующие записи:</span><span class="sxs-lookup"><span data-stu-id="0b233-233">The route dictionary contains the following entries:</span></span>

- <span data-ttu-id="0b233-234">контроллер: «продукты»</span><span class="sxs-lookup"><span data-stu-id="0b233-234">controller: "products"</span></span>
- <span data-ttu-id="0b233-235">Идентификатор: «1»</span><span class="sxs-lookup"><span data-stu-id="0b233-235">id: "1"</span></span>

<span data-ttu-id="0b233-236">Словарь маршрута не содержит параметры строки запроса, «версия» и «подробности», но они по-прежнему будут учитываться во время выбора действия.</span><span class="sxs-lookup"><span data-stu-id="0b233-236">The route dictionary does not contain the query string parameters, "version" and "details", but these will still be considered during action selection.</span></span>

### <a name="controller-selection"></a><span data-ttu-id="0b233-237">Выбор контроллера</span><span class="sxs-lookup"><span data-stu-id="0b233-237">Controller Selection</span></span>

<span data-ttu-id="0b233-238">Из записи «controller» в словаре маршрутов, является тип контроллера `ProductsController`.</span><span class="sxs-lookup"><span data-stu-id="0b233-238">From the "controller" entry in the route dictionary, the controller type is `ProductsController`.</span></span>

### <a name="action-selection"></a><span data-ttu-id="0b233-239">Выбор действия</span><span class="sxs-lookup"><span data-stu-id="0b233-239">Action Selection</span></span>

<span data-ttu-id="0b233-240">HTTP-запрос является запросом GET.</span><span class="sxs-lookup"><span data-stu-id="0b233-240">The HTTP request is a GET request.</span></span> <span data-ttu-id="0b233-241">Действия контроллера, которые поддерживают GET являются `GetAll`, `GetById`, и `FindProductsByName`.</span><span class="sxs-lookup"><span data-stu-id="0b233-241">The controller actions that support GET are `GetAll`, `GetById`, and `FindProductsByName`.</span></span> <span data-ttu-id="0b233-242">Словарь маршрута не содержит запись для «действия», так что мы не должны совпадать с именем действия.</span><span class="sxs-lookup"><span data-stu-id="0b233-242">The route dictionary does not contain an entry for "action", so we don't need to match the action name.</span></span>

<span data-ttu-id="0b233-243">Далее попытка сопоставить имена параметров для действий, искать только действия GET.</span><span class="sxs-lookup"><span data-stu-id="0b233-243">Next, we try to match parameter names for the actions, looking only at the GET actions.</span></span>

| <span data-ttu-id="0b233-244">Действие</span><span class="sxs-lookup"><span data-stu-id="0b233-244">Action</span></span> | <span data-ttu-id="0b233-245">Параметры соответствия</span><span class="sxs-lookup"><span data-stu-id="0b233-245">Parameters to Match</span></span> |
| --- | --- |
| `GetAll` | <span data-ttu-id="0b233-246">Нет</span><span class="sxs-lookup"><span data-stu-id="0b233-246">none</span></span> |
| `GetById` | <span data-ttu-id="0b233-247">«Идентификатор»</span><span class="sxs-lookup"><span data-stu-id="0b233-247">"id"</span></span> |
| `FindProductsByName` | <span data-ttu-id="0b233-248">«name»</span><span class="sxs-lookup"><span data-stu-id="0b233-248">"name"</span></span> |

<span data-ttu-id="0b233-249">Обратите внимание, что *версии* параметр `GetById` не является, так как это необязательный параметр.</span><span class="sxs-lookup"><span data-stu-id="0b233-249">Notice that the *version* parameter of `GetById` is not considered, because it is an optional parameter.</span></span>

<span data-ttu-id="0b233-250">`GetAll` Тривиально соответствующий метод.</span><span class="sxs-lookup"><span data-stu-id="0b233-250">The `GetAll` method matches trivially.</span></span> <span data-ttu-id="0b233-251">`GetById` Метод также соответствует, так как «id» содержит словарь маршрута.</span><span class="sxs-lookup"><span data-stu-id="0b233-251">The `GetById` method also matches, because the route dictionary contains "id".</span></span> <span data-ttu-id="0b233-252">`FindProductsByName` Метод не соответствует.</span><span class="sxs-lookup"><span data-stu-id="0b233-252">The `FindProductsByName` method does not match.</span></span>

<span data-ttu-id="0b233-253">`GetById` Метод wins, так как он соответствует один параметр, и без параметров для `GetAll`.</span><span class="sxs-lookup"><span data-stu-id="0b233-253">The `GetById` method wins, because it matches one parameter, versus no parameters for `GetAll`.</span></span> <span data-ttu-id="0b233-254">Метод вызывается с приведенными ниже значениями параметров:</span><span class="sxs-lookup"><span data-stu-id="0b233-254">The method is invoked with the following parameter values:</span></span>

- <span data-ttu-id="0b233-255">*Идентификатор* = 1</span><span class="sxs-lookup"><span data-stu-id="0b233-255">*id* = 1</span></span>
- <span data-ttu-id="0b233-256">*версия* = 1.5</span><span class="sxs-lookup"><span data-stu-id="0b233-256">*version* = 1.5</span></span>

<span data-ttu-id="0b233-257">Обратите внимание, что даже если *версии* не использовался в алгоритм выбора, значение параметра берется из строки запроса URI.</span><span class="sxs-lookup"><span data-stu-id="0b233-257">Notice that even though *version* was not used in the selection algorithm, the value of the parameter comes from the URI query string.</span></span>

## <a name="extension-points"></a><span data-ttu-id="0b233-258">Точки расширения</span><span class="sxs-lookup"><span data-stu-id="0b233-258">Extension Points</span></span>

<span data-ttu-id="0b233-259">Веб-API предоставляет точки расширения для некоторых этапов процесса маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="0b233-259">Web API provides extension points for some parts of the routing process.</span></span>

| <span data-ttu-id="0b233-260">Интерфейс</span><span class="sxs-lookup"><span data-stu-id="0b233-260">Interface</span></span> | <span data-ttu-id="0b233-261">Описание:</span><span class="sxs-lookup"><span data-stu-id="0b233-261">Description</span></span> |
| --- | --- |
| <span data-ttu-id="0b233-262">**IHttpControllerSelector**</span><span class="sxs-lookup"><span data-stu-id="0b233-262">**IHttpControllerSelector**</span></span> | <span data-ttu-id="0b233-263">Выбирает контроллер.</span><span class="sxs-lookup"><span data-stu-id="0b233-263">Selects the controller.</span></span> |
| <span data-ttu-id="0b233-264">**IHttpControllerTypeResolver**</span><span class="sxs-lookup"><span data-stu-id="0b233-264">**IHttpControllerTypeResolver**</span></span> | <span data-ttu-id="0b233-265">Получает список типов контроллеров.</span><span class="sxs-lookup"><span data-stu-id="0b233-265">Gets the list of controller types.</span></span> <span data-ttu-id="0b233-266">**DefaultHttpControllerSelector** выбирает тип контроллера из этого списка.</span><span class="sxs-lookup"><span data-stu-id="0b233-266">The **DefaultHttpControllerSelector** chooses the controller type from this list.</span></span> |
| <span data-ttu-id="0b233-267">**IAssembliesResolver**</span><span class="sxs-lookup"><span data-stu-id="0b233-267">**IAssembliesResolver**</span></span> | <span data-ttu-id="0b233-268">Возвращает список сборок, проект.</span><span class="sxs-lookup"><span data-stu-id="0b233-268">Gets the list of project assemblies.</span></span> <span data-ttu-id="0b233-269">**IHttpControllerTypeResolver** интерфейс использует этот список для поиска типов контроллеров.</span><span class="sxs-lookup"><span data-stu-id="0b233-269">The **IHttpControllerTypeResolver** interface uses this list to find the controller types.</span></span> |
| <span data-ttu-id="0b233-270">**IHttpControllerActivator**</span><span class="sxs-lookup"><span data-stu-id="0b233-270">**IHttpControllerActivator**</span></span> | <span data-ttu-id="0b233-271">Создает новые экземпляры объектов controller.</span><span class="sxs-lookup"><span data-stu-id="0b233-271">Creates new controller instances.</span></span> |
| <span data-ttu-id="0b233-272">**IHttpActionSelector**</span><span class="sxs-lookup"><span data-stu-id="0b233-272">**IHttpActionSelector**</span></span> | <span data-ttu-id="0b233-273">Выбирает действие.</span><span class="sxs-lookup"><span data-stu-id="0b233-273">Selects the action.</span></span> |
| <span data-ttu-id="0b233-274">**IHttpActionInvoker**</span><span class="sxs-lookup"><span data-stu-id="0b233-274">**IHttpActionInvoker**</span></span> | <span data-ttu-id="0b233-275">Вызывает действие.</span><span class="sxs-lookup"><span data-stu-id="0b233-275">Invokes the action.</span></span> |

<span data-ttu-id="0b233-276">Чтобы предоставить собственную реализацию для любого из этих интерфейсов, используйте **служб** коллекции **HttpConfiguration** объекта:</span><span class="sxs-lookup"><span data-stu-id="0b233-276">To provide your own implementation for any of these interfaces, use the **Services** collection on the **HttpConfiguration** object:</span></span>

[!code-csharp[Main](routing-and-action-selection/samples/sample11.cs)]
