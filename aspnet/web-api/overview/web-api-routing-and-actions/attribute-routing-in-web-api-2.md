---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "Атрибут маршрутизации в ASP.NET Web API 2 | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 173add73a150d3e13ae243d6548463da912dadee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="331ce-102">Атрибут маршрутизации в ASP.NET Web API 2</span><span class="sxs-lookup"><span data-stu-id="331ce-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="331ce-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="331ce-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="331ce-104">*Маршрутизация* как веб-API соответствует URI-адрес действия.</span><span class="sxs-lookup"><span data-stu-id="331ce-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="331ce-105">Веб-API 2 поддерживает новый тип маршрутизации, вызывается *маршрутизацией атрибутов*.</span><span class="sxs-lookup"><span data-stu-id="331ce-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="331ce-106">Как следует из имен, атрибут маршрутизации использует атрибуты для определения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="331ce-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="331ce-107">Атрибут маршрутизации предоставляет больший контроль над URL-адреса в веб-API.</span><span class="sxs-lookup"><span data-stu-id="331ce-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="331ce-108">Например можно легко создать URI, которые описывают иерархии ресурсов.</span><span class="sxs-lookup"><span data-stu-id="331ce-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="331ce-109">Ранее стиль маршрутизации, вызывается на основе соглашений маршрутизации, по-прежнему полностью поддерживается.</span><span class="sxs-lookup"><span data-stu-id="331ce-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="331ce-110">На самом деле можно сочетать оба способа, в том же проекте.</span><span class="sxs-lookup"><span data-stu-id="331ce-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="331ce-111">В этом разделе показано, как включить маршрутизацию атрибута и описывает различные параметры для атрибута маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="331ce-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="331ce-112">End-to-end учебник, в котором используется атрибут маршрутизация, см. [создать REST API с маршрутизацией атрибутов в веб-API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="331ce-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>


## <a name="prerequisites"></a><span data-ttu-id="331ce-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="331ce-113">Prerequisites</span></span>

<span data-ttu-id="331ce-114">[Visual Studio 2017 г](https://www.visualstudio.com/vs/) Community, Professional или Enterprise Edition</span><span class="sxs-lookup"><span data-stu-id="331ce-114">[Visual Studio 2017](https://www.visualstudio.com/vs/) Community, Professional, or Enterprise Edition</span></span>

<span data-ttu-id="331ce-115">Можно также установить необходимые пакеты с помощью диспетчера пакетов NuGet.</span><span class="sxs-lookup"><span data-stu-id="331ce-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="331ce-116">Из **средства** в Visual Studio, выберите пункт меню **диспетчер пакетов библиотеки**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="331ce-116">From the **Tools** menu in Visual Studio, select **Library Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="331ce-117">В окне консоли диспетчера пакетов введите следующую команду:</span><span class="sxs-lookup"><span data-stu-id="331ce-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="331ce-118">Почему атрибута маршрутизации?</span><span class="sxs-lookup"><span data-stu-id="331ce-118">Why Attribute Routing?</span></span>

<span data-ttu-id="331ce-119">В первом выпуске используется веб-API *соглашению* маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="331ce-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="331ce-120">В типа маршрутизации определить один или несколько шаблонов маршрута, которые по сути являются параметризованные строки.</span><span class="sxs-lookup"><span data-stu-id="331ce-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="331ce-121">Платформа получает запрос, сопоставляет URI с шаблоном маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="331ce-122">(Дополнительные сведения о маршрутизации на основе соглашения см. в разделе [маршрутизации в ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="331ce-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="331ce-123">Одно из преимуществ маршрутизации на основе соглашений — шаблоны определены в одном месте, что правила маршрутизации применяется последовательно на всех контроллерах.</span><span class="sxs-lookup"><span data-stu-id="331ce-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="331ce-124">К сожалению маршрутизация на основе соглашения о становится трудно поддерживать определенные шаблоны URI, которые являются общими в API RESTful.</span><span class="sxs-lookup"><span data-stu-id="331ce-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="331ce-125">Например, часто содержат ресурсы дочерние ресурсы: клиенты имеют заказы, фильмы имеют субъекты, книг у авторов и т. д.</span><span class="sxs-lookup"><span data-stu-id="331ce-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="331ce-126">Логично, для создания URI, которые отражают таких отношений:</span><span class="sxs-lookup"><span data-stu-id="331ce-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="331ce-127">Такой тип URI сложно создать с помощью маршрутизации на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="331ce-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="331ce-128">Несмотря на то, что это можно сделать, результаты не масштабируются также в том случае, если у вас есть много контроллеров или типы ресурсов.</span><span class="sxs-lookup"><span data-stu-id="331ce-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="331ce-129">При маршрутизации атрибута является тривиальным, чтобы определить маршрут для данного URI.</span><span class="sxs-lookup"><span data-stu-id="331ce-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="331ce-130">Просто добавьте атрибут к действию контроллера:</span><span class="sxs-lookup"><span data-stu-id="331ce-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="331ce-131">Ниже приведены некоторые шаблоны, которые атрибута маршрутизации позволяет легко.</span><span class="sxs-lookup"><span data-stu-id="331ce-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="331ce-132">**Управление версиями API**</span><span class="sxs-lookup"><span data-stu-id="331ce-132">**API versioning**</span></span>

<span data-ttu-id="331ce-133">В этом примере «/ api/v1/продуктов» будет перенаправляться в другой контроллер, чем «/ v2/api/продуктов».</span><span class="sxs-lookup"><span data-stu-id="331ce-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`  
`/api/v2/products`

<span data-ttu-id="331ce-134">**Перегруженные сегментов URI-адреса**</span><span class="sxs-lookup"><span data-stu-id="331ce-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="331ce-135">В этом примере «1» является номер заказа, но «ожидание» сопоставляемый с коллекцией.</span><span class="sxs-lookup"><span data-stu-id="331ce-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`  
`/orders/pending`

<span data-ttu-id="331ce-136">**Несколькими типами параметров**</span><span class="sxs-lookup"><span data-stu-id="331ce-136">**Multiple parameter types**</span></span>

<span data-ttu-id="331ce-137">В этом примере «1» является номер заказа, но дата «2013-06/16".</span><span class="sxs-lookup"><span data-stu-id="331ce-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="331ce-138">Включение атрибута маршрутизации</span><span class="sxs-lookup"><span data-stu-id="331ce-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="331ce-139">Чтобы включить атрибут маршрутизации, вызовите **MapHttpAttributeRoutes** во время настройки.</span><span class="sxs-lookup"><span data-stu-id="331ce-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="331ce-140">Этот метод расширения определен в **System.Web.Http.HttpConfigurationExtensions** класса.</span><span class="sxs-lookup"><span data-stu-id="331ce-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="331ce-141">Атрибут маршрутизации может сочетаться с [соглашению](routing-in-aspnet-web-api.md) маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="331ce-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="331ce-142">Определение маршрутов на основе соглашений, вызовите **MapHttpRoute** метод.</span><span class="sxs-lookup"><span data-stu-id="331ce-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="331ce-143">Дополнительные сведения о настройке веб-API см. в разделе [Настройка ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="331ce-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="331ce-144">Примечание: Переход с веб-API 1</span><span class="sxs-lookup"><span data-stu-id="331ce-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="331ce-145">Перед веб-API 2 шаблоны проектов веб-API создан следующий код:</span><span class="sxs-lookup"><span data-stu-id="331ce-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="331ce-146">Если включен атрибут маршрутизации, этот код вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="331ce-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="331ce-147">При обновлении существующего проекта веб-API, чтобы использовать атрибут маршрутизации, необходимо обязательно обновить код конфигурации следующее:</span><span class="sxs-lookup"><span data-stu-id="331ce-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="331ce-148">Дополнительные сведения см. в разделе [Настройка веб-API с размещением ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="331ce-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="331ce-149">Добавление атрибутов маршрута</span><span class="sxs-lookup"><span data-stu-id="331ce-149">Adding Route Attributes</span></span>

<span data-ttu-id="331ce-150">Ниже приведен пример маршрута, определенного с помощью атрибута:</span><span class="sxs-lookup"><span data-stu-id="331ce-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="331ce-151">Строка &quot;клиентов / {customerId} / упорядочивает&quot; шаблон URI для маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="331ce-152">Веб-API предпринимается попытка сопоставить URI запроса в шаблон.</span><span class="sxs-lookup"><span data-stu-id="331ce-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="331ce-153">В этом примере «заказчики» и «orders» являются литерала сегментов и «{customerId}» является параметром-переменной.</span><span class="sxs-lookup"><span data-stu-id="331ce-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="331ce-154">Следующие URI будет соответствовать этому шаблону:</span><span class="sxs-lookup"><span data-stu-id="331ce-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="331ce-155">Вы можете ограничить соответствия с помощью [ограничения](#constraints), описанных далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="331ce-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="331ce-156">Обратите внимание, что &quot;{customerId}&quot; параметры в шаблоне маршрута совпадает с именем *customerId* параметра в методе.</span><span class="sxs-lookup"><span data-stu-id="331ce-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="331ce-157">Когда веб-API вызывает действия контроллера, он пытается выполнить привязку параметров маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="331ce-158">Например, если URL-адрес является `http://example.com/customers/1/orders`, веб-API пытается привязать значение «1» *customerId* параметр в действие.</span><span class="sxs-lookup"><span data-stu-id="331ce-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="331ce-159">Шаблон URI может иметь несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="331ce-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="331ce-160">Методы контроллера, которые имеют атрибут маршрута используйте маршрутизации на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="331ce-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="331ce-161">Таким образом, можно сочетать оба типа маршрутизации в том же проекте.</span><span class="sxs-lookup"><span data-stu-id="331ce-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="331ce-162">Методы HTTP</span><span class="sxs-lookup"><span data-stu-id="331ce-162">HTTP Methods</span></span>

<span data-ttu-id="331ce-163">Веб-API также выбирает действия на основе метода HTTP запроса (GET, POST, и т. д.).</span><span class="sxs-lookup"><span data-stu-id="331ce-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="331ce-164">По умолчанию веб-API ищет совпадение без учета регистра с начала имени метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="331ce-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="331ce-165">Например, метод контроллера с именем `PutCustomers` соответствует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="331ce-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="331ce-166">Это соглашение можно переопределить посредством декорирования метод с любым следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="331ce-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="331ce-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="331ce-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="331ce-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="331ce-168">**[HttpGet]**</span></span>
- <span data-ttu-id="331ce-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="331ce-169">**[HttpHead]**</span></span>
- <span data-ttu-id="331ce-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="331ce-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="331ce-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="331ce-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="331ce-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="331ce-172">**[HttpPost]**</span></span>
- <span data-ttu-id="331ce-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="331ce-173">**[HttpPut]**</span></span>

<span data-ttu-id="331ce-174">Следующий пример сопоставляет CreateBook метод HTTP-запросы POST.</span><span class="sxs-lookup"><span data-stu-id="331ce-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="331ce-175">Для всех других методов HTTP, включая стандартным методы используйте **AcceptVerbs** атрибут, который принимает список методов HTTP.</span><span class="sxs-lookup"><span data-stu-id="331ce-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="331ce-176">Префиксов маршрутов</span><span class="sxs-lookup"><span data-stu-id="331ce-176">Route Prefixes</span></span>

<span data-ttu-id="331ce-177">Часто маршруты в контроллере, начинающихся с тем же префиксом.</span><span class="sxs-lookup"><span data-stu-id="331ce-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="331ce-178">Пример:</span><span class="sxs-lookup"><span data-stu-id="331ce-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="331ce-179">Можно задать Общий префикс для всей контроллера с помощью **[RoutePrefix]** атрибута:</span><span class="sxs-lookup"><span data-stu-id="331ce-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="331ce-180">Используйте атрибут method тильды (~) для переопределения префикс маршрута:</span><span class="sxs-lookup"><span data-stu-id="331ce-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="331ce-181">Префикс маршрута может включать параметры:</span><span class="sxs-lookup"><span data-stu-id="331ce-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="331ce-182">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="331ce-182">Route Constraints</span></span>

<span data-ttu-id="331ce-183">Ограничения маршрута позволяют ограничить сопоставления параметров в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="331ce-184">Общий синтаксис &quot;{параметр: ограничение}&quot;.</span><span class="sxs-lookup"><span data-stu-id="331ce-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="331ce-185">Пример:</span><span class="sxs-lookup"><span data-stu-id="331ce-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="331ce-186">Здесь первый маршрут выбирается только если &quot;идентификатор&quot; сегмент URI должно быть целым числом.</span><span class="sxs-lookup"><span data-stu-id="331ce-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="331ce-187">В противном случае будет выбран второй маршрут.</span><span class="sxs-lookup"><span data-stu-id="331ce-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="331ce-188">В следующей таблице перечислены ограничения, которые поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="331ce-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="331ce-189">Ограничение</span><span class="sxs-lookup"><span data-stu-id="331ce-189">Constraint</span></span> | <span data-ttu-id="331ce-190">Описание:</span><span class="sxs-lookup"><span data-stu-id="331ce-190">Description</span></span> | <span data-ttu-id="331ce-191">Пример</span><span class="sxs-lookup"><span data-stu-id="331ce-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="331ce-192">Альфа-канал</span><span class="sxs-lookup"><span data-stu-id="331ce-192">alpha</span></span> | <span data-ttu-id="331ce-193">Соответствует прописные или строчные буквы латинского алфавита (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="331ce-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="331ce-194">{x:alpha}</span><span class="sxs-lookup"><span data-stu-id="331ce-194">{x:alpha}</span></span> |
| <span data-ttu-id="331ce-195">bool</span><span class="sxs-lookup"><span data-stu-id="331ce-195">bool</span></span> | <span data-ttu-id="331ce-196">Соответствует логическое значение.</span><span class="sxs-lookup"><span data-stu-id="331ce-196">Matches a Boolean value.</span></span> | <span data-ttu-id="331ce-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="331ce-197">{x:bool}</span></span> |
| <span data-ttu-id="331ce-198">datetime</span><span class="sxs-lookup"><span data-stu-id="331ce-198">datetime</span></span> | <span data-ttu-id="331ce-199">Соответствует **DateTime** значение.</span><span class="sxs-lookup"><span data-stu-id="331ce-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="331ce-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="331ce-200">{x:datetime}</span></span> |
| <span data-ttu-id="331ce-201">decimal</span><span class="sxs-lookup"><span data-stu-id="331ce-201">decimal</span></span> | <span data-ttu-id="331ce-202">Совпадает с десятичным значением.</span><span class="sxs-lookup"><span data-stu-id="331ce-202">Matches a decimal value.</span></span> | <span data-ttu-id="331ce-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="331ce-203">{x:decimal}</span></span> |
| <span data-ttu-id="331ce-204">double</span><span class="sxs-lookup"><span data-stu-id="331ce-204">double</span></span> | <span data-ttu-id="331ce-205">Совпадает с 64-разрядное значение с плавающей запятой.</span><span class="sxs-lookup"><span data-stu-id="331ce-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="331ce-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="331ce-206">{x:double}</span></span> |
| <span data-ttu-id="331ce-207">float</span><span class="sxs-lookup"><span data-stu-id="331ce-207">float</span></span> | <span data-ttu-id="331ce-208">Совпадает с 32-разрядное значение с плавающей запятой.</span><span class="sxs-lookup"><span data-stu-id="331ce-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="331ce-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="331ce-209">{x:float}</span></span> |
| <span data-ttu-id="331ce-210">guid</span><span class="sxs-lookup"><span data-stu-id="331ce-210">guid</span></span> | <span data-ttu-id="331ce-211">Совпадает со значением GUID.</span><span class="sxs-lookup"><span data-stu-id="331ce-211">Matches a GUID value.</span></span> | <span data-ttu-id="331ce-212">{x:guid}</span><span class="sxs-lookup"><span data-stu-id="331ce-212">{x:guid}</span></span> |
| <span data-ttu-id="331ce-213">int</span><span class="sxs-lookup"><span data-stu-id="331ce-213">int</span></span> | <span data-ttu-id="331ce-214">Совпадает со значением 32-разрядное целое число.</span><span class="sxs-lookup"><span data-stu-id="331ce-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="331ce-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="331ce-215">{x:int}</span></span> |
| <span data-ttu-id="331ce-216">длина</span><span class="sxs-lookup"><span data-stu-id="331ce-216">length</span></span> | <span data-ttu-id="331ce-217">Соответствует строке, указанной длины или в указанном диапазоне длин.</span><span class="sxs-lookup"><span data-stu-id="331ce-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="331ce-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="331ce-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="331ce-219">long</span><span class="sxs-lookup"><span data-stu-id="331ce-219">long</span></span> | <span data-ttu-id="331ce-220">Совпадает со значением 64-разрядное целое число.</span><span class="sxs-lookup"><span data-stu-id="331ce-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="331ce-221">{x: long}</span><span class="sxs-lookup"><span data-stu-id="331ce-221">{x:long}</span></span> |
| <span data-ttu-id="331ce-222">max</span><span class="sxs-lookup"><span data-stu-id="331ce-222">max</span></span> | <span data-ttu-id="331ce-223">Совпадает со значением максимальное целое число.</span><span class="sxs-lookup"><span data-stu-id="331ce-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="331ce-224">{x: max(10)}</span><span class="sxs-lookup"><span data-stu-id="331ce-224">{x:max(10)}</span></span> |
| <span data-ttu-id="331ce-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="331ce-225">maxlength</span></span> | <span data-ttu-id="331ce-226">Соответствует строке с максимальной длиной.</span><span class="sxs-lookup"><span data-stu-id="331ce-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="331ce-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="331ce-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="331ce-228">min</span><span class="sxs-lookup"><span data-stu-id="331ce-228">min</span></span> | <span data-ttu-id="331ce-229">Соответствует целое минимальное значение.</span><span class="sxs-lookup"><span data-stu-id="331ce-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="331ce-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="331ce-230">{x:min(10)}</span></span> |
| <span data-ttu-id="331ce-231">minLength</span><span class="sxs-lookup"><span data-stu-id="331ce-231">minlength</span></span> | <span data-ttu-id="331ce-232">Со строкой с минимальной длиной.</span><span class="sxs-lookup"><span data-stu-id="331ce-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="331ce-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="331ce-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="331ce-234">range</span><span class="sxs-lookup"><span data-stu-id="331ce-234">range</span></span> | <span data-ttu-id="331ce-235">Соответствует целым числом в диапазоне значений.</span><span class="sxs-lookup"><span data-stu-id="331ce-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="331ce-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="331ce-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="331ce-237">regex</span><span class="sxs-lookup"><span data-stu-id="331ce-237">regex</span></span> | <span data-ttu-id="331ce-238">Соответствует регулярному выражению.</span><span class="sxs-lookup"><span data-stu-id="331ce-238">Matches a regular expression.</span></span> | <span data-ttu-id="331ce-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="331ce-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="331ce-240">Обратите внимание на то что некоторые ограничения, такие как &quot;min&quot;, принимают аргументы в круглые скобки.</span><span class="sxs-lookup"><span data-stu-id="331ce-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="331ce-241">Можно применить несколько ограничений для параметров, разделенных точкой с запятой.</span><span class="sxs-lookup"><span data-stu-id="331ce-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="331ce-242">Ограничения пользовательских маршрута</span><span class="sxs-lookup"><span data-stu-id="331ce-242">Custom Route Constraints</span></span>

<span data-ttu-id="331ce-243">Можно создать настраиваемый маршрут ограничения, реализовав **IHttpRouteConstraint** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="331ce-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="331ce-244">Например следующее ограничение ограничивает ненулевое целочисленное значение параметра.</span><span class="sxs-lookup"><span data-stu-id="331ce-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="331ce-245">Ниже показано, как зарегистрировать ограничение:</span><span class="sxs-lookup"><span data-stu-id="331ce-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="331ce-246">Теперь можно применить ограничение в свои маршруты:</span><span class="sxs-lookup"><span data-stu-id="331ce-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="331ce-247">Также можно заменить весь **DefaultInlineConstraintResolver** класса путем реализации **IInlineConstraintResolver** интерфейса.</span><span class="sxs-lookup"><span data-stu-id="331ce-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="331ce-248">Это заменит все встроенные ограничения, если реализация **IInlineConstraintResolver** добавит их специально.</span><span class="sxs-lookup"><span data-stu-id="331ce-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="331ce-249">Необязательный URI параметры и значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="331ce-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="331ce-250">Параметр URI можно сделать необязательным путем добавления вопросительный знак параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="331ce-251">Если параметр маршрута является необязательным, необходимо определить значения по умолчанию для параметра метода.</span><span class="sxs-lookup"><span data-stu-id="331ce-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="331ce-252">В этом примере `/api/books/locale/1033` и `/api/books/locale` возвращать тот же ресурс.</span><span class="sxs-lookup"><span data-stu-id="331ce-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="331ce-253">Кроме того можно указать значение по умолчанию в шаблоне маршрута, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="331ce-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="331ce-254">Это аналогично предыдущему примеру, но есть незначительные различия поведения, когда применяется значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="331ce-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="331ce-255">В первом примере («{lcid?}») значение по умолчанию 1033 назначается непосредственно в параметр метода, параметр будет иметь точное значение.</span><span class="sxs-lookup"><span data-stu-id="331ce-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="331ce-256">Во втором примере («{lcid = 1033}»), значение по умолчанию «1033» проходит через процесс привязки модели.</span><span class="sxs-lookup"><span data-stu-id="331ce-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="331ce-257">Связыватель модели по умолчанию преобразует «1033» в числовое значение 1033.</span><span class="sxs-lookup"><span data-stu-id="331ce-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="331ce-258">Тем не менее можно подключить настраиваемый связыватель модели, что может сделать нечто иное.</span><span class="sxs-lookup"><span data-stu-id="331ce-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="331ce-259">(В большинстве случаев при отсутствии связыватели моделей, настраиваемые в ваш конвейер две формы будет эквивалентное.)</span><span class="sxs-lookup"><span data-stu-id="331ce-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="331ce-260">Имена маршрутов</span><span class="sxs-lookup"><span data-stu-id="331ce-260">Route Names</span></span>

<span data-ttu-id="331ce-261">В веб-API каждый имеет имя.</span><span class="sxs-lookup"><span data-stu-id="331ce-261">In Web API, every route has a name.</span></span> <span data-ttu-id="331ce-262">Имена маршрутов полезны для создания ссылок, так что можно включить ссылку в HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="331ce-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="331ce-263">Чтобы указать имя маршрута, задайте **имя** атрибута.</span><span class="sxs-lookup"><span data-stu-id="331ce-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="331ce-264">Следующий пример показывает, как задать имя маршрута, а также как использовать имя маршрута, при создании ссылки.</span><span class="sxs-lookup"><span data-stu-id="331ce-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="331ce-265">Порядок маршрута</span><span class="sxs-lookup"><span data-stu-id="331ce-265">Route Order</span></span>

<span data-ttu-id="331ce-266">Когда платформа предпринимается попытка сопоставить URI с маршрутом, она оценивает маршруты в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="331ce-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="331ce-267">Чтобы указать порядок, задайте **RouteOrder** маршрут атрибута.</span><span class="sxs-lookup"><span data-stu-id="331ce-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="331ce-268">Более низкие значения вычисляются первыми.</span><span class="sxs-lookup"><span data-stu-id="331ce-268">Lower values are evaluated first.</span></span> <span data-ttu-id="331ce-269">Значение порядка по умолчанию равно нулю.</span><span class="sxs-lookup"><span data-stu-id="331ce-269">The default order value is zero.</span></span>

<span data-ttu-id="331ce-270">Вот, как определяется общего упорядочения.</span><span class="sxs-lookup"><span data-stu-id="331ce-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="331ce-271">Сравнение **RouteOrder** маршрут атрибута.</span><span class="sxs-lookup"><span data-stu-id="331ce-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="331ce-272">Рассмотрим каждый сегмент URI в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="331ce-273">Для каждого сегмента заказов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="331ce-273">For each segment, order as follows:</span></span> 

    1. <span data-ttu-id="331ce-274">Литерал сегментов.</span><span class="sxs-lookup"><span data-stu-id="331ce-274">Literal segments.</span></span>
    2. <span data-ttu-id="331ce-275">Параметры маршрута с ограничениями.</span><span class="sxs-lookup"><span data-stu-id="331ce-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="331ce-276">Параметры маршрута без ограничений.</span><span class="sxs-lookup"><span data-stu-id="331ce-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="331ce-277">Сегменты с подстановочными знаками параметр с ограничениями.</span><span class="sxs-lookup"><span data-stu-id="331ce-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="331ce-278">Сегменты с подстановочными знаками параметр без ограничений.</span><span class="sxs-lookup"><span data-stu-id="331ce-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="331ce-279">В случае одинаковых значений, маршруты, упорядоченные по сравнения строк по порядковому номеру без учета регистра ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="331ce-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="331ce-280">Пример.</span><span class="sxs-lookup"><span data-stu-id="331ce-280">Here is an example.</span></span> <span data-ttu-id="331ce-281">Предположим, что определение следующего контроллера:</span><span class="sxs-lookup"><span data-stu-id="331ce-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="331ce-282">Эти маршруты содержат, упорядочены следующим образом.</span><span class="sxs-lookup"><span data-stu-id="331ce-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="331ce-283">заказы подробности</span><span class="sxs-lookup"><span data-stu-id="331ce-283">orders/details</span></span>
2. <span data-ttu-id="331ce-284">заказы / {id}</span><span class="sxs-lookup"><span data-stu-id="331ce-284">orders/{id}</span></span>
3. <span data-ttu-id="331ce-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="331ce-285">orders/{customerName}</span></span>
4. <span data-ttu-id="331ce-286">заказы / {\*date}</span><span class="sxs-lookup"><span data-stu-id="331ce-286">orders/{\*date}</span></span>
5. <span data-ttu-id="331ce-287">заказы / ожидание</span><span class="sxs-lookup"><span data-stu-id="331ce-287">orders/pending</span></span>

<span data-ttu-id="331ce-288">Обратите внимание, что «подробности» является сегментом литерала находится перед «{id}», но «ожидается» отображается последнее, так как **RouteOrder** свойства является 1.</span><span class="sxs-lookup"><span data-stu-id="331ce-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="331ce-289">(Предполагается, что ни один клиент именуются «подробности» или «Ожидание».</span><span class="sxs-lookup"><span data-stu-id="331ce-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="331ce-290">В целом Старайтесь избегать неоднозначных маршрутов.</span><span class="sxs-lookup"><span data-stu-id="331ce-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="331ce-291">В этом примере лучше шаблон маршрута для `GetByCustomer` — «клиентов / {customerName}»)</span><span class="sxs-lookup"><span data-stu-id="331ce-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
