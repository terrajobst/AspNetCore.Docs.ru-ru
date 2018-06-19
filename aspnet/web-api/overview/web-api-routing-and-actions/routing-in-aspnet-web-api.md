---
uid: web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
title: Маршрутизация в ASP.NET Web API | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/11/2012
ms.topic: article
ms.assetid: 0675bdc7-282f-4f47-b7f3-7e02133940ca
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api
msc.type: authoredcontent
ms.openlocfilehash: aa0ecc96029051fef6a81ac08f7fcf52a24de59c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26509263"
---
<a name="routing-in-aspnet-web-api"></a><span data-ttu-id="36eee-102">Маршрутизация в ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="36eee-102">Routing in ASP.NET Web API</span></span>
====================
<span data-ttu-id="36eee-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="36eee-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="36eee-104">В этой статье описывается, как веб-API ASP.NET направляет HTTP-запросы к контроллерам.</span><span class="sxs-lookup"><span data-stu-id="36eee-104">This article describes how ASP.NET Web API routes HTTP requests to controllers.</span></span>

> [!NOTE]
> <span data-ttu-id="36eee-105">Если вы знакомы с ASP.NET MVC, веб-API маршрутизации выполняется аналогично маршрутизации MVC.</span><span class="sxs-lookup"><span data-stu-id="36eee-105">If you are familiar with ASP.NET MVC, Web API routing is very similar to MVC routing.</span></span> <span data-ttu-id="36eee-106">Основное отличие заключается в что веб-API использует метод HTTP, а не пути URI, чтобы выбрать действие.</span><span class="sxs-lookup"><span data-stu-id="36eee-106">The main difference is that Web API uses the HTTP method, not the URI path, to select the action.</span></span> <span data-ttu-id="36eee-107">Также можно использовать стиль MVC маршрутизации в веб-API.</span><span class="sxs-lookup"><span data-stu-id="36eee-107">You can also use MVC-style routing in Web API.</span></span> <span data-ttu-id="36eee-108">В этой статье, не получает сведения о ASP.NET MVC.</span><span class="sxs-lookup"><span data-stu-id="36eee-108">This article does not assume any knowledge of ASP.NET MVC.</span></span>


## <a name="routing-tables"></a><span data-ttu-id="36eee-109">Таблицы маршрутизации</span><span class="sxs-lookup"><span data-stu-id="36eee-109">Routing Tables</span></span>

<span data-ttu-id="36eee-110">В ASP.NET Web API *контроллера* — это класс, который обрабатывает HTTP-запросы.</span><span class="sxs-lookup"><span data-stu-id="36eee-110">In ASP.NET Web API, a *controller* is a class that handles HTTP requests.</span></span> <span data-ttu-id="36eee-111">Открытые методы контроллера, называются *методы действий* или просто *действия*.</span><span class="sxs-lookup"><span data-stu-id="36eee-111">The public methods of the controller are called *action methods* or simply *actions*.</span></span> <span data-ttu-id="36eee-112">Платформа веб-API, получив запрос, запрос направляется на действие.</span><span class="sxs-lookup"><span data-stu-id="36eee-112">When the Web API framework receives a request, it routes the request to an action.</span></span>

<span data-ttu-id="36eee-113">Чтобы определить, какое действие следует вызвать, платформа использует *таблицу маршрутизации*.</span><span class="sxs-lookup"><span data-stu-id="36eee-113">To determine which action to invoke, the framework uses a *routing table*.</span></span> <span data-ttu-id="36eee-114">Шаблон проекта Visual Studio для веб-API создает маршрут по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="36eee-114">The Visual Studio project template for Web API creates a default route:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample1.cs)]

<span data-ttu-id="36eee-115">Этот маршрут определяется в файле WebApiConfig.cs, который помещается в приложении\_начального каталога:</span><span class="sxs-lookup"><span data-stu-id="36eee-115">This route is defined in the WebApiConfig.cs file, which is placed in the App\_Start directory:</span></span>

![](routing-in-aspnet-web-api/_static/image1.png)

<span data-ttu-id="36eee-116">Дополнительные сведения о **WebApiConfig** см. в описании [Настройка веб-API ASP.NET](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="36eee-116">For more information about the **WebApiConfig** class, see [Configuring ASP.NET Web API](../advanced/configuring-aspnet-web-api.md).</span></span>

<span data-ttu-id="36eee-117">Если резидентной веб-API, необходимо задать таблицу маршрутизации непосредственно на **HttpSelfHostConfiguration** объекта.</span><span class="sxs-lookup"><span data-stu-id="36eee-117">If you self-host Web API, you must set the routing table directly on the **HttpSelfHostConfiguration** object.</span></span> <span data-ttu-id="36eee-118">Дополнительные сведения см. в разделе [резидентной веб-API](../older-versions/self-host-a-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="36eee-118">For more information, see [Self-Host a Web API](../older-versions/self-host-a-web-api.md).</span></span>

<span data-ttu-id="36eee-119">Каждая запись в таблице маршрутизации содержит *шаблон маршрута*.</span><span class="sxs-lookup"><span data-stu-id="36eee-119">Each entry in the routing table contains a *route template*.</span></span> <span data-ttu-id="36eee-120">Шаблон маршрута по умолчанию для веб-API — &quot;api / {controller} / {id}&quot;.</span><span class="sxs-lookup"><span data-stu-id="36eee-120">The default route template for Web API is &quot;api/{controller}/{id}&quot;.</span></span> <span data-ttu-id="36eee-121">В этом шаблоне &quot;api&quot; сегмент литерального пути и {controller} и {id}-это переменные заполнителя.</span><span class="sxs-lookup"><span data-stu-id="36eee-121">In this template, &quot;api&quot; is a literal path segment, and {controller} and {id} are placeholder variables.</span></span>

<span data-ttu-id="36eee-122">Платформа веб-API, получив запрос HTTP, он пытается сопоставить URI для одного из шаблонов маршрута в таблице маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="36eee-122">When the Web API framework receives an HTTP request, it tries to match the URI against one of the route templates in the routing table.</span></span> <span data-ttu-id="36eee-123">Если ни один маршрут не соответствует, клиент получает сообщение об ошибке 404.</span><span class="sxs-lookup"><span data-stu-id="36eee-123">If no route matches, the client receives a 404 error.</span></span> <span data-ttu-id="36eee-124">Например следующий URL-адреса совпадают маршрут по умолчанию:</span><span class="sxs-lookup"><span data-stu-id="36eee-124">For example, the following URIs match the default route:</span></span>

- <span data-ttu-id="36eee-125">/ api/контактов</span><span class="sxs-lookup"><span data-stu-id="36eee-125">/api/contacts</span></span>
- <span data-ttu-id="36eee-126">/API/Contacts/1</span><span class="sxs-lookup"><span data-stu-id="36eee-126">/api/contacts/1</span></span>
- <span data-ttu-id="36eee-127">/API/Products/gizmo1</span><span class="sxs-lookup"><span data-stu-id="36eee-127">/api/products/gizmo1</span></span>

<span data-ttu-id="36eee-128">Тем не менее, следующий URI не соответствует, так как он не имеет &quot;api&quot; сегмента:</span><span class="sxs-lookup"><span data-stu-id="36eee-128">However, the following URI does not match, because it lacks the &quot;api&quot; segment:</span></span>

- <span data-ttu-id="36eee-129">/ contacts/1</span><span class="sxs-lookup"><span data-stu-id="36eee-129">/contacts/1</span></span>

> [!NOTE]
> <span data-ttu-id="36eee-130">Во избежание конфликтов с маршрутизация ASP.NET MVC — причина для использования «api» в маршруте.</span><span class="sxs-lookup"><span data-stu-id="36eee-130">The reason for using "api" in the route is to avoid collisions with ASP.NET MVC routing.</span></span> <span data-ttu-id="36eee-131">Таким образом, что &quot;/обращается&quot; перейдите на контроллер MVC и &quot;контактов/api/&quot; перейдите на контроллер веб-API.</span><span class="sxs-lookup"><span data-stu-id="36eee-131">That way, you can have &quot;/contacts&quot; go to an MVC controller, and &quot;/api/contacts&quot; go to a Web API controller.</span></span> <span data-ttu-id="36eee-132">Конечно Если вас не устраивает этому соглашению, можно изменить таблицу маршрутов по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="36eee-132">Of course, if you don't like this convention, you can change the default route table.</span></span>

<span data-ttu-id="36eee-133">После обнаружения совпадающий маршрут веб-API выбирает контроллера и действия:</span><span class="sxs-lookup"><span data-stu-id="36eee-133">Once a matching route is found, Web API selects the controller and the action:</span></span>

- <span data-ttu-id="36eee-134">Чтобы найти контроллер, добавляет веб-API &quot;контроллера&quot; значению *{controller}* переменной.</span><span class="sxs-lookup"><span data-stu-id="36eee-134">To find the controller, Web API adds &quot;Controller&quot; to the value of the *{controller}* variable.</span></span>
- <span data-ttu-id="36eee-135">Чтобы найти действие, веб-API рассматривается метод HTTP и выполняет для действия, имя которых начинается с таким именем метода HTTP.</span><span class="sxs-lookup"><span data-stu-id="36eee-135">To find the action, Web API looks at the HTTP method, and then looks for an action whose name begins with that HTTP method name.</span></span> <span data-ttu-id="36eee-136">Например, с помощью запроса GET веб-API ищет действие, которое начинается с &quot;получить... &quot;, такие как &quot;GetContact&quot; или &quot;GetAllContacts&quot;.</span><span class="sxs-lookup"><span data-stu-id="36eee-136">For example, with a GET request, Web API looks for an action that starts with &quot;Get...&quot;, such as &quot;GetContact&quot; or &quot;GetAllContacts&quot;.</span></span> <span data-ttu-id="36eee-137">Это соглашение применяется только к GET, POST, PUT и DELETE методы.</span><span class="sxs-lookup"><span data-stu-id="36eee-137">This convention applies only to GET, POST, PUT, and DELETE methods.</span></span> <span data-ttu-id="36eee-138">Другие методы HTTP можно включить с помощью атрибутов на контроллере.</span><span class="sxs-lookup"><span data-stu-id="36eee-138">You can enable other HTTP methods by using attributes on your controller.</span></span> <span data-ttu-id="36eee-139">Пример, мы рассмотрим позже.</span><span class="sxs-lookup"><span data-stu-id="36eee-139">We'll see an example of that later.</span></span>
- <span data-ttu-id="36eee-140">Другие заполнители в шаблон маршрута, таких как *{id}* соответствуют параметрам действия.</span><span class="sxs-lookup"><span data-stu-id="36eee-140">Other placeholder variables in the route template, such as *{id},* are mapped to action parameters.</span></span>

<span data-ttu-id="36eee-141">Давайте рассмотрим пример.</span><span class="sxs-lookup"><span data-stu-id="36eee-141">Let's look at an example.</span></span> <span data-ttu-id="36eee-142">Предположим, определены следующие контроллера:</span><span class="sxs-lookup"><span data-stu-id="36eee-142">Suppose that you define the following controller:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample2.cs)]

<span data-ttu-id="36eee-143">Ниже приведены некоторые возможные HTTP-запросов, а также действие, которая вызывается для каждого.</span><span class="sxs-lookup"><span data-stu-id="36eee-143">Here are some possible HTTP requests, along with the action that gets invoked for each:</span></span>

| <span data-ttu-id="36eee-144">Метод HTTP</span><span class="sxs-lookup"><span data-stu-id="36eee-144">HTTP Method</span></span> | <span data-ttu-id="36eee-145">Путь URI</span><span class="sxs-lookup"><span data-stu-id="36eee-145">URI Path</span></span> | <span data-ttu-id="36eee-146">Действие</span><span class="sxs-lookup"><span data-stu-id="36eee-146">Action</span></span> | <span data-ttu-id="36eee-147">Параметр</span><span class="sxs-lookup"><span data-stu-id="36eee-147">Parameter</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="36eee-148">GET</span><span class="sxs-lookup"><span data-stu-id="36eee-148">GET</span></span> | <span data-ttu-id="36eee-149">API и продуктов</span><span class="sxs-lookup"><span data-stu-id="36eee-149">api/products</span></span> | <span data-ttu-id="36eee-150">GetAllProducts</span><span class="sxs-lookup"><span data-stu-id="36eee-150">GetAllProducts</span></span> | <span data-ttu-id="36eee-151">*(нет)*</span><span class="sxs-lookup"><span data-stu-id="36eee-151">*(none)*</span></span> |
| <span data-ttu-id="36eee-152">GET</span><span class="sxs-lookup"><span data-stu-id="36eee-152">GET</span></span> | <span data-ttu-id="36eee-153">API/продукты/4</span><span class="sxs-lookup"><span data-stu-id="36eee-153">api/products/4</span></span> | <span data-ttu-id="36eee-154">GetProductById</span><span class="sxs-lookup"><span data-stu-id="36eee-154">GetProductById</span></span> | <span data-ttu-id="36eee-155">4</span><span class="sxs-lookup"><span data-stu-id="36eee-155">4</span></span> |
| <span data-ttu-id="36eee-156">DELETE</span><span class="sxs-lookup"><span data-stu-id="36eee-156">DELETE</span></span> | <span data-ttu-id="36eee-157">API/продукты/4</span><span class="sxs-lookup"><span data-stu-id="36eee-157">api/products/4</span></span> | <span data-ttu-id="36eee-158">DeleteProduct</span><span class="sxs-lookup"><span data-stu-id="36eee-158">DeleteProduct</span></span> | <span data-ttu-id="36eee-159">4</span><span class="sxs-lookup"><span data-stu-id="36eee-159">4</span></span> |
| <span data-ttu-id="36eee-160">ПОМЕСТИТЬ</span><span class="sxs-lookup"><span data-stu-id="36eee-160">POST</span></span> | <span data-ttu-id="36eee-161">API и продуктов</span><span class="sxs-lookup"><span data-stu-id="36eee-161">api/products</span></span> | <span data-ttu-id="36eee-162">*(не соответствий)*</span><span class="sxs-lookup"><span data-stu-id="36eee-162">*(no match)*</span></span> |  |

<span data-ttu-id="36eee-163">Обратите внимание, что *{id}* сегмент URI, если он имеется, сопоставляется *идентификатор* параметра действия.</span><span class="sxs-lookup"><span data-stu-id="36eee-163">Notice that the *{id}* segment of the URI, if present, is mapped to the *id* parameter of the action.</span></span> <span data-ttu-id="36eee-164">В этом примере определяются два GET методам: одному с в контроллере *идентификатор* параметр и без параметров.</span><span class="sxs-lookup"><span data-stu-id="36eee-164">In this example, the controller defines two GET methods, one with an *id* parameter and one with no parameters.</span></span>

<span data-ttu-id="36eee-165">Кроме того, обратите внимание, что запрос POST завершается ошибкой, поскольку контроллер не определяет &quot;Post... &quot; метод.</span><span class="sxs-lookup"><span data-stu-id="36eee-165">Also, note that the POST request will fail, because the controller does not define a &quot;Post...&quot; method.</span></span>

## <a name="routing-variations"></a><span data-ttu-id="36eee-166">Варианты маршрутизации</span><span class="sxs-lookup"><span data-stu-id="36eee-166">Routing Variations</span></span>

<span data-ttu-id="36eee-167">В предыдущем разделе речь базовый механизм маршрутизации ASP.NET Web API.</span><span class="sxs-lookup"><span data-stu-id="36eee-167">The previous section described the basic routing mechanism for ASP.NET Web API.</span></span> <span data-ttu-id="36eee-168">В этом разделе описываются некоторые варианты.</span><span class="sxs-lookup"><span data-stu-id="36eee-168">This section describes some variations.</span></span>

### <a name="http-methods"></a><span data-ttu-id="36eee-169">Методы HTTP</span><span class="sxs-lookup"><span data-stu-id="36eee-169">HTTP Methods</span></span>

<span data-ttu-id="36eee-170">Вместо того чтобы использовать соглашение об именовании для методов HTTP, явным образом можно указать метод HTTP для действия по метод действия с помощью **HttpGet**, **HttpPut**, **HttpPost** , или **HttpDelete** атрибута.</span><span class="sxs-lookup"><span data-stu-id="36eee-170">Instead of using the naming convention for HTTP methods, you can explicitly specify the HTTP method for an action by decorating the action method with the **HttpGet**, **HttpPut**, **HttpPost**, or **HttpDelete** attribute.</span></span>

<span data-ttu-id="36eee-171">В следующем примере метод FindProduct сопоставлено запросов GET:</span><span class="sxs-lookup"><span data-stu-id="36eee-171">In the following example, the FindProduct method is mapped to GET requests:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample3.cs)]

<span data-ttu-id="36eee-172">Чтобы разрешить несколько методов HTTP для действия или разрешить HTTP-методов, отличных от GET, PUT, POST и DELETE, используйте **AcceptVerbs** атрибут, который принимает список методов HTTP.</span><span class="sxs-lookup"><span data-stu-id="36eee-172">To allow multiple HTTP methods for an action, or to allow HTTP methods other than GET, PUT, POST, and DELETE, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample4.cs)]

<a id="routing_by_action_name"></a>
### <a name="routing-by-action-name"></a><span data-ttu-id="36eee-173">Маршрутизация по имени действия</span><span class="sxs-lookup"><span data-stu-id="36eee-173">Routing by Action Name</span></span>

<span data-ttu-id="36eee-174">Шаблон маршрутизации по умолчанию веб-API использует метод HTTP, чтобы выбрать действие.</span><span class="sxs-lookup"><span data-stu-id="36eee-174">With the default routing template, Web API uses the HTTP method to select the action.</span></span> <span data-ttu-id="36eee-175">Однако можно также создать маршрута, если имя действия включается в URI.</span><span class="sxs-lookup"><span data-stu-id="36eee-175">However, you can also create a route where the action name is included in the URI:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample5.cs)]

<span data-ttu-id="36eee-176">В этом шаблоне маршрута *{action}* имена параметров метода действия контроллера.</span><span class="sxs-lookup"><span data-stu-id="36eee-176">In this route template, the *{action}* parameter names the action method on the controller.</span></span> <span data-ttu-id="36eee-177">В этом стиле маршрутизации атрибуты можно используйте для указания разрешенные методы HTTP.</span><span class="sxs-lookup"><span data-stu-id="36eee-177">With this style of routing, use attributes to specify the allowed HTTP methods.</span></span> <span data-ttu-id="36eee-178">Например предположим, что контроллер содержит следующий метод:</span><span class="sxs-lookup"><span data-stu-id="36eee-178">For example, suppose your controller has the following method:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample6.cs)]

<span data-ttu-id="36eee-179">В этом случае метод сведения будут сопоставлены запрос GET для «api/продукты/сведения-1».</span><span class="sxs-lookup"><span data-stu-id="36eee-179">In this case, a GET request for "api/products/details/1" would map to the Details method.</span></span> <span data-ttu-id="36eee-180">Этот стиль маршрутизации похож на ASP.NET MVC и может подходить для API стиле RPC.</span><span class="sxs-lookup"><span data-stu-id="36eee-180">This style of routing is similar to ASP.NET MVC, and may be appropriate for an RPC-style API.</span></span>

<span data-ttu-id="36eee-181">Имя действия можно переопределить с помощью **ActionName** атрибута.</span><span class="sxs-lookup"><span data-stu-id="36eee-181">You can override the action name by using the **ActionName** attribute.</span></span> <span data-ttu-id="36eee-182">В следующем примере имеются два действия, которые сопоставляются &quot;api, продукты или эскиз/*идентификатор*. Одно из них поддерживает GET, а другой поддерживает POST:</span><span class="sxs-lookup"><span data-stu-id="36eee-182">In the following example, there are two actions that map to &quot;api/products/thumbnail/*id*. One supports GET and the other supports POST:</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample7.cs)]

### <a name="non-actions"></a><span data-ttu-id="36eee-183">Без действия</span><span class="sxs-lookup"><span data-stu-id="36eee-183">Non-Actions</span></span>

<span data-ttu-id="36eee-184">Чтобы предотвратить получение вызывается в качестве действия метода, используйте **NonAction** атрибута.</span><span class="sxs-lookup"><span data-stu-id="36eee-184">To prevent a method from getting invoked as an action, use the **NonAction** attribute.</span></span> <span data-ttu-id="36eee-185">Это сигнализирует платформа метод не является действие, даже если бы в противном случае соответствие правила маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="36eee-185">This signals to the framework that the method is not an action, even if it would otherwise match the routing rules.</span></span>

[!code-csharp[Main](routing-in-aspnet-web-api/samples/sample8.cs)]

## <a name="further-reading"></a><span data-ttu-id="36eee-186">Дополнительные сведения</span><span class="sxs-lookup"><span data-stu-id="36eee-186">Further Reading</span></span>

<span data-ttu-id="36eee-187">В этом разделе предоставлены высокоуровневое представление маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="36eee-187">This topic provided a high-level view of routing.</span></span> <span data-ttu-id="36eee-188">Дополнительные сведения см. в разделе [Маршрутизация и Выбор действия](routing-and-action-selection.md), описывающей точно как платформа соответствует URI-адрес маршрута, выбирает контроллер, а затем выбирает действие для вызова.</span><span class="sxs-lookup"><span data-stu-id="36eee-188">For more detail, see [Routing and Action Selection](routing-and-action-selection.md), which describes exactly how the framework matches a URI to a route, selects a controller, and then selects the action to invoke.</span></span>
