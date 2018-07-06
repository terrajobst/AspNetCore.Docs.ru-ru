---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Маршрутизация в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/11/2012
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: ed9c575c448563307a0657e734076962fe067164
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825412"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="f1320-102">Маршрутизация в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="f1320-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="f1320-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="f1320-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="f1320-104">В этой статье описывается, как веб-API ASP.NET направляет HTTP-запросы к контроллерам.</span><span class="sxs-lookup"><span data-stu-id="f1320-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="f1320-105">Если вы знакомы с ASP.NET MVC, веб-API маршрутизацию значительной степени аналогична маршрутизации MVC.</span><span class="sxs-lookup"><span data-stu-id="f1320-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="f1320-106">Основное различие заключается в том, что веб-API использует метод HTTP, а не пути URI, выберите действие.</span><span class="sxs-lookup"><span data-stu-id="f1320-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="f1320-107">Можно также использовать Маршрутизация в стиле MVC в веб-API.</span><span class="sxs-lookup"><span data-stu-id="f1320-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="f1320-108">В этой статье не предполагают, что любой ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="f1320-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="f1320-109">Таблицы маршрутизации</span><span class="sxs-lookup"><span data-stu-id="f1320-109">Routing Tables</span></span>

<span data-ttu-id="f1320-110">В веб-API ASP.NET *контроллера* является классом, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="f1320-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="f1320-111">Открытые методы контроллера вызываются *методы действий* или просто *действия*.</span><span class="sxs-lookup"><span data-stu-id="f1320-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="f1320-112">Когда платформа веб-API получает запрос, он направляет запрос к действию.</span><span class="sxs-lookup"><span data-stu-id="f1320-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="f1320-113">Чтобы определить действие, вызываемое, инфраструктура использует *таблицу маршрутизации*.</span><span class="sxs-lookup"><span data-stu-id="f1320-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="f1320-114">Шаблон проекта Visual Studio для веб-API создает маршрут по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="f1320-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="f1320-115">Этот маршрут, определенный в файле WebApiConfig.cs, который помещается в приложении\_начальный каталог:</span><span class="sxs-lookup"><span data-stu-id="f1320-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="f1320-116">Дополнительные сведения о **WebApiConfig** , представлена в разделе [Настройка веб-API ASP.NET](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f1320-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="f1320-117">Если вы самостоятельно размещаете веб-API, необходимо задать таблицу маршрутизации непосредственно на **HttpSelfHostConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="f1320-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="f1320-118">Дополнительные сведения см. в разделе [резидентного размещения веб-API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="f1320-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="f1320-119">Каждая запись в таблице маршрутизации содержит *шаблон маршрута*.</span><span class="sxs-lookup"><span data-stu-id="f1320-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="f1320-120">Шаблон маршрута по умолчанию для веб-API является &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="f1320-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="f1320-121">В этом шаблоне &quot;api&quot; — это сегмент литерального пути и {controller} и {id} — это заполнитель переменные.</span><span class="sxs-lookup"><span data-stu-id="f1320-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="f1320-122">Когда платформа веб-API получает запрос HTTP, она пытается сопоставить URI для одного из шаблонов маршрута в таблице маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="f1320-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="f1320-123">Если ни один маршрут не соответствует, клиент получает сообщение об ошибке 404.</span><span class="sxs-lookup"><span data-stu-id="f1320-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="f1320-124">Например следующие URI соответствует маршрут по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="f1320-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="f1320-125">/ api/contacts</span><span class="sxs-lookup"><span data-stu-id="f1320-125">/api/contacts</span></span>
- <span data-ttu-id="f1320-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="f1320-126">/api/contacts/1</span></span>
- <span data-ttu-id="f1320-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="f1320-127">/api/products/gizmo1</span></span>

<span data-ttu-id="f1320-128">Тем не менее, следующий URI не соответствует, так как у него нет &quot;api&quot; сегмент:</span><span class="sxs-lookup"><span data-stu-id="f1320-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="f1320-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="f1320-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="f1320-130">Во избежание конфликтов с маршрутизация ASP.NET MVC является причиной использования «api» в маршруте.</span><span class="sxs-lookup"><span data-stu-id="f1320-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="f1320-131">Таким образом, может иметь &quot;/обращается&quot; перейдите к контроллеру MVC и &quot;/api/contacts&quot; перейдите к контроллеру веб-API.</span><span class="sxs-lookup"><span data-stu-id="f1320-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="f1320-132">Конечно Если вам не нравится это соглашение, можно изменить таблицы маршрутизации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="f1320-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="f1320-133">После обнаружения подходящего маршрута веб-API выбирает контроллер и действие:</span><span class="sxs-lookup"><span data-stu-id="f1320-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="f1320-134">Чтобы найти контроллер, добавляет веб-API &quot;контроллера&quot; значению *{controller}* переменной.</span><span class="sxs-lookup"><span data-stu-id="f1320-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="f1320-135">Чтобы найти действия, веб-API рассматривает метод HTTP и выполняет для действия, имя которых начинается с метода HTTP с таким именем.</span><span class="sxs-lookup"><span data-stu-id="f1320-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="f1320-136">Например, с помощью запроса GET, веб-API выглядит для действия, которое начинается с &quot;получить... &quot;, такие как &quot;GetContact&quot; или &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="f1320-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="f1320-137">Это соглашение применяется только к GET, POST, PUT и DELETE методы.</span><span class="sxs-lookup"><span data-stu-id="f1320-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="f1320-138">Вы можете включить другие методы HTTP с помощью атрибутов на контроллере.</span><span class="sxs-lookup"><span data-stu-id="f1320-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="f1320-139">Пример, мы рассмотрим позже.</span><span class="sxs-lookup"><span data-stu-id="f1320-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="f1320-140">Другие переменные заполнителей в шаблоне маршрута, такие как *{id},* сопоставляются с параметрами действия.</span><span class="sxs-lookup"><span data-stu-id="f1320-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="f1320-141">Давайте рассмотрим пример.</span><span class="sxs-lookup"><span data-stu-id="f1320-141">Let's look at an example.</span></span> <span data-ttu-id="f1320-142">Предположим, что определить следующий контроллер:</span><span class="sxs-lookup"><span data-stu-id="f1320-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="f1320-143">Ниже приведены некоторые возможные HTTP-запросов, а также действие, которое вызывается для каждого.</span><span class="sxs-lookup"><span data-stu-id="f1320-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="f1320-144">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="f1320-144">HTTP Method</span></span> | <span data-ttu-id="f1320-145">Путь URI</span><span class="sxs-lookup"><span data-stu-id="f1320-145">URI Path</span></span> | <span data-ttu-id="f1320-146">Действие</span><span class="sxs-lookup"><span data-stu-id="f1320-146">Action</span></span> | <span data-ttu-id="f1320-147">Параметр</span><span class="sxs-lookup"><span data-stu-id="f1320-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="f1320-148">GET</span><span class="sxs-lookup"><span data-stu-id="f1320-148">GET</span></span> | <span data-ttu-id="f1320-149">API/products</span><span class="sxs-lookup"><span data-stu-id="f1320-149">api/products</span></span> | <span data-ttu-id="f1320-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="f1320-150">GetAllProducts</span></span> | <span data-ttu-id="f1320-151">*(нет)*</span><span class="sxs-lookup"><span data-stu-id="f1320-151">*(none)*</span></span> |
| <span data-ttu-id="f1320-152">GET</span><span class="sxs-lookup"><span data-stu-id="f1320-152">GET</span></span> | <span data-ttu-id="f1320-153">API/продукты/4</span><span class="sxs-lookup"><span data-stu-id="f1320-153">api/products/4</span></span> | <span data-ttu-id="f1320-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="f1320-154">GetProductById</span></span> | <span data-ttu-id="f1320-155">4</span><span class="sxs-lookup"><span data-stu-id="f1320-155">4</span></span> |
| <span data-ttu-id="f1320-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="f1320-156">DELETE</span></span> | <span data-ttu-id="f1320-157">API/продукты/4</span><span class="sxs-lookup"><span data-stu-id="f1320-157">api/products/4</span></span> | <span data-ttu-id="f1320-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="f1320-158">DeleteProduct</span></span> | <span data-ttu-id="f1320-159">4</span><span class="sxs-lookup"><span data-stu-id="f1320-159">4</span></span> |
| <span data-ttu-id="f1320-160">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="f1320-160">POST</span></span> | <span data-ttu-id="f1320-161">API/products</span><span class="sxs-lookup"><span data-stu-id="f1320-161">api/products</span></span> | <span data-ttu-id="f1320-162">*(не соответствий)*</span><span class="sxs-lookup"><span data-stu-id="f1320-162">*(no match)*</span></span> |  |

<span data-ttu-id="f1320-163">Обратите внимание, что *{id}* сегмент URI, если он имеется, сопоставляется с *идентификатор* параметра действия.</span><span class="sxs-lookup"><span data-stu-id="f1320-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="f1320-164">В этом примере контроллер определяет два метода GET, одна с *идентификатор* параметр и один без параметров.</span><span class="sxs-lookup"><span data-stu-id="f1320-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="f1320-165">Кроме того, обратите внимание, что запрос POST завершается ошибкой, так как контроллер определяет &quot;Post... &quot; метод.</span><span class="sxs-lookup"><span data-stu-id="f1320-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="f1320-166">Варианты маршрутизации</span><span class="sxs-lookup"><span data-stu-id="f1320-166">Routing Variations</span></span>

<span data-ttu-id="f1320-167">В предыдущем разделе описан базовый механизм маршрутизации веб-API ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="f1320-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="f1320-168">В этом разделе описаны некоторые различия.</span><span class="sxs-lookup"><span data-stu-id="f1320-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="f1320-169">Методы HTTP</span><span class="sxs-lookup"><span data-stu-id="f1320-169">HTTP Methods</span></span>

<span data-ttu-id="f1320-170">Вместо того чтобы использовать соглашение об именовании для методов HTTP, можно явно указать метод HTTP для действия, дополняя метод действия с **HttpGet**, **HttpPut**, **HttpPost** , или **HttpDelete** атрибута.</span><span class="sxs-lookup"><span data-stu-id="f1320-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="f1320-171">В следующем примере метод FindProduct сопоставляется запросов GET.</span><span class="sxs-lookup"><span data-stu-id="f1320-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="f1320-172">Чтобы разрешить несколько методов HTTP для действия или разрешить HTTP-методы, отличные от GET, PUT, POST и DELETE, используйте **AcceptVerbs** атрибут, который принимает список методов HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1320-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="f1320-173">Маршрутизация по имени действия</span><span class="sxs-lookup"><span data-stu-id="f1320-173">Routing by Action Name</span></span>

<span data-ttu-id="f1320-174">С шаблоном маршрута по умолчанию веб-API использует метод HTTP, чтобы выбрать действие.</span><span class="sxs-lookup"><span data-stu-id="f1320-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="f1320-175">Тем не менее можно также создать маршрут, если имя действия включается в URI.</span><span class="sxs-lookup"><span data-stu-id="f1320-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="f1320-176">В этом шаблоне маршрута *{action}* имена параметров метода действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="f1320-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="f1320-177">При использовании этого стиля маршрута используйте атрибуты, чтобы указать разрешенные методы HTTP.</span><span class="sxs-lookup"><span data-stu-id="f1320-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="f1320-178">Например предположим, что устройство имеет следующий метод:</span><span class="sxs-lookup"><span data-stu-id="f1320-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="f1320-179">В этом случае запрос GET для «api/продукты/сведения/1» сопоставляется метод сведения.</span><span class="sxs-lookup"><span data-stu-id="f1320-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="f1320-180">Этот стиль маршрутизации похож на ASP.NET MVC и может подойти для API в стиле RPC.</span><span class="sxs-lookup"><span data-stu-id="f1320-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="f1320-181">Имя действия можно переопределить с помощью **ActionName** атрибута.</span><span class="sxs-lookup"><span data-stu-id="f1320-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="f1320-182">В следующем примере есть два действия, которые сопоставляются с &quot;api/продукты/эскиз/*идентификатор*. Один поддерживает GET, а другой поддерживает POST:</span><span class="sxs-lookup"><span data-stu-id="f1320-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="f1320-183">Без действий</span><span class="sxs-lookup"><span data-stu-id="f1320-183">Non-Actions</span></span>

<span data-ttu-id="f1320-184">Чтобы предотвратить метода вызывается как действие, используйте **NonAction** атрибута.</span><span class="sxs-lookup"><span data-stu-id="f1320-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="f1320-185">Это указывает на Framework что метод не действие, даже если это бы в противном случае соответствует правилам маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="f1320-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="f1320-186">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="f1320-186">Further Reading</span></span>

<span data-ttu-id="f1320-187">В этом разделе предоставляются высокоуровневый обзор маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="f1320-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="f1320-188">Для получения дополнительных сведений см. в разделе [Маршрутизация и Выбор действия](routing-and-action-selection.md), которая описывает точно как платформа соответствует URI с маршрутом, выбирает контроллер, а затем выбирает действие, которое вызывается.</span><span class="sxs-lookup"><span data-stu-id="f1320-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
