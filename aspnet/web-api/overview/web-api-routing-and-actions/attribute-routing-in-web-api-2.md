---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: Маршрутизация в ASP.NET Web API 2 атрибутов | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 22eb2fd748d52ec95e813ada8b1bf3b4826ad573
ms.sourcegitcommit: 6e6002de467cd135a69e5518d4ba9422d693132a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/16/2018
ms.locfileid: "49348485"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="0ca17-102">Маршрутизация в ASP.NET Web API 2 с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="0ca17-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="0ca17-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="0ca17-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="0ca17-104">*Маршрутизация* является как веб-API сопоставляет URI с действием.</span><span class="sxs-lookup"><span data-stu-id="0ca17-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="0ca17-105">Веб-API 2 поддерживает новый тип маршрутизации, вызывается *маршрутизации с помощью атрибутов*.</span><span class="sxs-lookup"><span data-stu-id="0ca17-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="0ca17-106">Как и предполагает название, маршрутизация с помощью атрибутов использует атрибуты для определения маршрутов.</span><span class="sxs-lookup"><span data-stu-id="0ca17-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="0ca17-107">Маршрутизация с помощью атрибутов предоставляет больший контроль над коды URI в веб-API.</span><span class="sxs-lookup"><span data-stu-id="0ca17-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="0ca17-108">Например можно легко создать URI, которые описывают иерархии ресурсов.</span><span class="sxs-lookup"><span data-stu-id="0ca17-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="0ca17-109">Более ранние стиль маршрутизации, вызывается на основе соглашений маршрутизации, по-прежнему полностью поддерживается.</span><span class="sxs-lookup"><span data-stu-id="0ca17-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="0ca17-110">На самом деле можно сочетать оба способа в том же проекте.</span><span class="sxs-lookup"><span data-stu-id="0ca17-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="0ca17-111">В этом разделе показано, как включить маршрутизацию атрибутов и описываются различные параметры для маршрутизации с помощью атрибутов.</span><span class="sxs-lookup"><span data-stu-id="0ca17-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="0ca17-112">End-to-end учебник, в котором используется маршрутизация с помощью атрибутов, см. в разделе [Создание REST API с помощью маршрутизации с помощью атрибутов в веб-API 2](create-a-rest-api-with-attribute-routing.md).</span><span class="sxs-lookup"><span data-stu-id="0ca17-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0ca17-113">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0ca17-113">Prerequisites</span></span>

<span data-ttu-id="0ca17-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional или Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="0ca17-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="0ca17-115">Кроме того можно используйте диспетчер пакетов NuGet для установки необходимых пакетов.</span><span class="sxs-lookup"><span data-stu-id="0ca17-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="0ca17-116">Из **средства** меню в Visual Studio, выберите пункт **диспетчер пакетов NuGet**, а затем выберите **консоль диспетчера пакетов**.</span><span class="sxs-lookup"><span data-stu-id="0ca17-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="0ca17-117">Введите следующую команду в окне консоли диспетчера пакетов:</span><span class="sxs-lookup"><span data-stu-id="0ca17-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="0ca17-118">Почему Маршрутизация атрибутов?</span><span class="sxs-lookup"><span data-stu-id="0ca17-118">Why Attribute Routing?</span></span>

<span data-ttu-id="0ca17-119">Первая версия веб-API используется *соглашениям* маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="0ca17-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="0ca17-120">В этом типе маршрутизации определить один или дополнительные шаблоны маршрутов, которые по сути являются параметризованные строки.</span><span class="sxs-lookup"><span data-stu-id="0ca17-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="0ca17-121">Когда платформа получает запрос, он соответствует URI с шаблоном маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="0ca17-122">(Дополнительные сведения о маршрутизации на основе соглашения, см. в разделе [маршрутизации в ASP.NET Web API](routing-in-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0ca17-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="0ca17-123">Одним из преимуществ маршрутизации на основе соглашения является определены шаблоны в одном месте, что согласованное применение правила маршрутизации на всех контроллерах.</span><span class="sxs-lookup"><span data-stu-id="0ca17-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="0ca17-124">К сожалению маршрутизация на основе соглашения об затрудняет для поддержки определенных шаблонов URI, которые являются общими в API-интерфейсов RESTful.</span><span class="sxs-lookup"><span data-stu-id="0ca17-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="0ca17-125">Например, ресурсы часто содержат дочерние ресурсы: клиенты размещают заказы, субъекты имеют фильмы, книги у авторов и т. д.</span><span class="sxs-lookup"><span data-stu-id="0ca17-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="0ca17-126">Логично, чтобы создать URI, которые отражают следующие связи:</span><span class="sxs-lookup"><span data-stu-id="0ca17-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="0ca17-127">Такой тип URI трудно создать с помощью маршрутизации на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="0ca17-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="0ca17-128">Несмотря на то, что это можно сделать, результаты не масштабируются также в том случае, если у вас много контроллеров или типов ресурсов.</span><span class="sxs-lookup"><span data-stu-id="0ca17-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="0ca17-129">С помощью маршрутизации с помощью атрибутов, это просто определить маршрут для данного универсального кода Ресурса.</span><span class="sxs-lookup"><span data-stu-id="0ca17-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="0ca17-130">Нужно просто добавить атрибут действие контроллера:</span><span class="sxs-lookup"><span data-stu-id="0ca17-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="0ca17-131">Ниже приведены некоторые шаблоны, которые атрибуту маршрутизации легко.</span><span class="sxs-lookup"><span data-stu-id="0ca17-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="0ca17-132">**Управление версиями API**</span><span class="sxs-lookup"><span data-stu-id="0ca17-132">**API versioning**</span></span>

<span data-ttu-id="0ca17-133">В этом примере «/ api/v1/products» будет перенаправляться в другом контроллере, чем «/ api/v2/products».</span><span class="sxs-lookup"><span data-stu-id="0ca17-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="0ca17-134">**Перегруженный сегментов URI-адреса**</span><span class="sxs-lookup"><span data-stu-id="0ca17-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="0ca17-135">В этом примере «1» — это номер заказа, но «ожидание», которому сопоставлен коллекции.</span><span class="sxs-lookup"><span data-stu-id="0ca17-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="0ca17-136">**Несколькими типами параметров**</span><span class="sxs-lookup"><span data-stu-id="0ca17-136">**Multiple parameter types**</span></span>

<span data-ttu-id="0ca17-137">В этом примере «1» — это номер заказа, но дата «2013/06/16".</span><span class="sxs-lookup"><span data-stu-id="0ca17-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="0ca17-138">Включение маршрутизации с помощью атрибутов</span><span class="sxs-lookup"><span data-stu-id="0ca17-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="0ca17-139">Чтобы включить маршрутизацию атрибутов, вызовите **MapHttpAttributeRoutes** во время настройки.</span><span class="sxs-lookup"><span data-stu-id="0ca17-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="0ca17-140">Этот метод расширения определен в **System.Web.Http.HttpConfigurationExtensions** класса.</span><span class="sxs-lookup"><span data-stu-id="0ca17-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="0ca17-141">Маршрутизация с помощью атрибутов можно сочетать с [соглашениям](routing-in-aspnet-web-api.md) маршрутизации.</span><span class="sxs-lookup"><span data-stu-id="0ca17-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="0ca17-142">Чтобы определить маршруты на основе соглашения, вызовите **MapHttpRoute** метод.</span><span class="sxs-lookup"><span data-stu-id="0ca17-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="0ca17-143">Дополнительные сведения о настройке веб-API, см. в разделе [Настройка ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span><span class="sxs-lookup"><span data-stu-id="0ca17-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="0ca17-144">Примечание: Миграция с веб-API 1</span><span class="sxs-lookup"><span data-stu-id="0ca17-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="0ca17-145">Прежде чем веб-API 2 шаблоны проектов веб-API созданный код следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0ca17-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="0ca17-146">Если включен маршрутизации с помощью атрибутов, этот код вызовет исключение.</span><span class="sxs-lookup"><span data-stu-id="0ca17-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="0ca17-147">Если вы обновляете существующий проект веб-API для использования маршрутизации с помощью атрибутов, не забудьте обновить этот код конфигурации следующим:</span><span class="sxs-lookup"><span data-stu-id="0ca17-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="0ca17-148">Дополнительные сведения см. в разделе [Настройка веб-API с размещением ASP.NET](../advanced/configuring-aspnet-web-api.md#webhost).</span><span class="sxs-lookup"><span data-stu-id="0ca17-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="0ca17-149">Добавление атрибутов маршрута</span><span class="sxs-lookup"><span data-stu-id="0ca17-149">Adding Route Attributes</span></span>

<span data-ttu-id="0ca17-150">Вот пример маршрута, определенных с помощью атрибута:</span><span class="sxs-lookup"><span data-stu-id="0ca17-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="0ca17-151">Строка &quot;клиентов / {customerId} / orders&quot; шаблон URI для маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="0ca17-152">Веб-API пытается сопоставить запрос URI в шаблон.</span><span class="sxs-lookup"><span data-stu-id="0ca17-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="0ca17-153">В этом примере «customers» и «orders» являются литерала сегментов, а «{customerId}» является параметром переменной.</span><span class="sxs-lookup"><span data-stu-id="0ca17-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="0ca17-154">Этот шаблон соответствует следующих URI:</span><span class="sxs-lookup"><span data-stu-id="0ca17-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="0ca17-155">Вы можете ограничить соответствия с помощью [ограничения](#constraints), описанные далее в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="0ca17-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="0ca17-156">Обратите внимание, что &quot;{customerId}&quot; параметра в шаблоне маршрута совпадает с именем *customerId* параметра метода.</span><span class="sxs-lookup"><span data-stu-id="0ca17-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="0ca17-157">Когда веб-API вызывает действие контроллера, он пытается привязать параметры маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="0ca17-158">Например, если URL-адрес является `http://example.com/customers/1/orders`, веб-API пытается привязать значение «1» для *customerId* параметра в действии.</span><span class="sxs-lookup"><span data-stu-id="0ca17-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="0ca17-159">Шаблон URI может иметь несколько параметров.</span><span class="sxs-lookup"><span data-stu-id="0ca17-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="0ca17-160">Методы контроллера, у которых нет атрибута маршрута используйте маршрутизации на основе соглашения.</span><span class="sxs-lookup"><span data-stu-id="0ca17-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="0ca17-161">Таким образом, вы можете сочетать оба типа маршрутизации в том же проекте.</span><span class="sxs-lookup"><span data-stu-id="0ca17-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="0ca17-162">Методы HTTP</span><span class="sxs-lookup"><span data-stu-id="0ca17-162">HTTP Methods</span></span>

<span data-ttu-id="0ca17-163">Веб-API также выбирает действия на основе метода HTTP запроса (GET, POST, и т.д.).</span><span class="sxs-lookup"><span data-stu-id="0ca17-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="0ca17-164">По умолчанию веб-API ищет совпадение без учета регистра, с запуском имя метода контроллера.</span><span class="sxs-lookup"><span data-stu-id="0ca17-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="0ca17-165">Например, метод контроллера с именем `PutCustomers` соответствует запрос HTTP PUT.</span><span class="sxs-lookup"><span data-stu-id="0ca17-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="0ca17-166">Вы можете переопределить это соглашение, дополняя метод с любым следующие атрибуты:</span><span class="sxs-lookup"><span data-stu-id="0ca17-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="0ca17-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="0ca17-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-168">**[HttpGet]**</span></span>
- <span data-ttu-id="0ca17-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-169">**[HttpHead]**</span></span>
- <span data-ttu-id="0ca17-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="0ca17-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="0ca17-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-172">**[HttpPost]**</span></span>
- <span data-ttu-id="0ca17-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="0ca17-173">**[HttpPut]**</span></span>

<span data-ttu-id="0ca17-174">Следующий пример сопоставляет метод CreateBook на запросы HTTP POST.</span><span class="sxs-lookup"><span data-stu-id="0ca17-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="0ca17-175">Для всех остальных методов HTTP, включая нестандартными способами, используйте **AcceptVerbs** атрибут, который принимает список методов HTTP.</span><span class="sxs-lookup"><span data-stu-id="0ca17-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="0ca17-176">Префиксов маршрутов</span><span class="sxs-lookup"><span data-stu-id="0ca17-176">Route Prefixes</span></span>

<span data-ttu-id="0ca17-177">Часто маршруты в контроллере, начинающихся с одинаковым префиксом.</span><span class="sxs-lookup"><span data-stu-id="0ca17-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="0ca17-178">Пример:</span><span class="sxs-lookup"><span data-stu-id="0ca17-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="0ca17-179">Можно задать Общий префикс для всего контроллера с помощью **[RoutePrefix]** атрибут:</span><span class="sxs-lookup"><span data-stu-id="0ca17-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="0ca17-180">Используйте тильды (~) на атрибут method, чтобы переопределить префикс маршрута:</span><span class="sxs-lookup"><span data-stu-id="0ca17-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="0ca17-181">Префикс маршрута может включать параметры:</span><span class="sxs-lookup"><span data-stu-id="0ca17-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="0ca17-182">Ограничения маршрута</span><span class="sxs-lookup"><span data-stu-id="0ca17-182">Route Constraints</span></span>

<span data-ttu-id="0ca17-183">Ограничения маршрута позволяют ограничить возможности сопоставления параметров в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="0ca17-184">Общий синтаксис &quot;{параметр: ограничение}&quot;.</span><span class="sxs-lookup"><span data-stu-id="0ca17-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="0ca17-185">Пример:</span><span class="sxs-lookup"><span data-stu-id="0ca17-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="0ca17-186">Здесь первый маршрут выбирается только если &quot;идентификатор&quot; сегмент URI должно быть целым числом.</span><span class="sxs-lookup"><span data-stu-id="0ca17-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="0ca17-187">В противном случае будет выбран второй маршрут.</span><span class="sxs-lookup"><span data-stu-id="0ca17-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="0ca17-188">В следующей таблице перечислены ограничения, которые поддерживаются.</span><span class="sxs-lookup"><span data-stu-id="0ca17-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="0ca17-189">Ограничение</span><span class="sxs-lookup"><span data-stu-id="0ca17-189">Constraint</span></span> | <span data-ttu-id="0ca17-190">Описание</span><span class="sxs-lookup"><span data-stu-id="0ca17-190">Description</span></span> | <span data-ttu-id="0ca17-191">Пример</span><span class="sxs-lookup"><span data-stu-id="0ca17-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="0ca17-192">альфа-канала</span><span class="sxs-lookup"><span data-stu-id="0ca17-192">alpha</span></span> | <span data-ttu-id="0ca17-193">Совпадения прописные или строчные буквы латинского алфавита (a-z, A-Z)</span><span class="sxs-lookup"><span data-stu-id="0ca17-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="0ca17-194">{x: альфа-канал}</span><span class="sxs-lookup"><span data-stu-id="0ca17-194">{x:alpha}</span></span> |
| <span data-ttu-id="0ca17-195">bool</span><span class="sxs-lookup"><span data-stu-id="0ca17-195">bool</span></span> | <span data-ttu-id="0ca17-196">Сопоставляет логическое значение.</span><span class="sxs-lookup"><span data-stu-id="0ca17-196">Matches a Boolean value.</span></span> | <span data-ttu-id="0ca17-197">{x: bool}</span><span class="sxs-lookup"><span data-stu-id="0ca17-197">{x:bool}</span></span> |
| <span data-ttu-id="0ca17-198">datetime</span><span class="sxs-lookup"><span data-stu-id="0ca17-198">datetime</span></span> | <span data-ttu-id="0ca17-199">Совпадения **DateTime** значение.</span><span class="sxs-lookup"><span data-stu-id="0ca17-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="0ca17-200">{x: datetime}</span><span class="sxs-lookup"><span data-stu-id="0ca17-200">{x:datetime}</span></span> |
| <span data-ttu-id="0ca17-201">decimal</span><span class="sxs-lookup"><span data-stu-id="0ca17-201">decimal</span></span> | <span data-ttu-id="0ca17-202">Соответствует значение десятичного числа.</span><span class="sxs-lookup"><span data-stu-id="0ca17-202">Matches a decimal value.</span></span> | <span data-ttu-id="0ca17-203">{x: decimal}</span><span class="sxs-lookup"><span data-stu-id="0ca17-203">{x:decimal}</span></span> |
| <span data-ttu-id="0ca17-204">double</span><span class="sxs-lookup"><span data-stu-id="0ca17-204">double</span></span> | <span data-ttu-id="0ca17-205">Совпадает со значением с плавающей запятой 64-разрядной.</span><span class="sxs-lookup"><span data-stu-id="0ca17-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="0ca17-206">{x: double}</span><span class="sxs-lookup"><span data-stu-id="0ca17-206">{x:double}</span></span> |
| <span data-ttu-id="0ca17-207">float</span><span class="sxs-lookup"><span data-stu-id="0ca17-207">float</span></span> | <span data-ttu-id="0ca17-208">Совпадает с 32-разрядное значение с плавающей запятой.</span><span class="sxs-lookup"><span data-stu-id="0ca17-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="0ca17-209">{x: float}</span><span class="sxs-lookup"><span data-stu-id="0ca17-209">{x:float}</span></span> |
| <span data-ttu-id="0ca17-210">guid</span><span class="sxs-lookup"><span data-stu-id="0ca17-210">guid</span></span> | <span data-ttu-id="0ca17-211">Совпадает со значением GUID.</span><span class="sxs-lookup"><span data-stu-id="0ca17-211">Matches a GUID value.</span></span> | <span data-ttu-id="0ca17-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="0ca17-212">{x:guid}</span></span> |
| <span data-ttu-id="0ca17-213">int</span><span class="sxs-lookup"><span data-stu-id="0ca17-213">int</span></span> | <span data-ttu-id="0ca17-214">Совпадает со значением 32-разрядное целое число.</span><span class="sxs-lookup"><span data-stu-id="0ca17-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="0ca17-215">{x: int}</span><span class="sxs-lookup"><span data-stu-id="0ca17-215">{x:int}</span></span> |
| <span data-ttu-id="0ca17-216">длина</span><span class="sxs-lookup"><span data-stu-id="0ca17-216">length</span></span> | <span data-ttu-id="0ca17-217">Соответствует строке с указанным значением длины или в указанном диапазоне длин.</span><span class="sxs-lookup"><span data-stu-id="0ca17-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="0ca17-218">{x: length(6)} {x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="0ca17-219">long</span><span class="sxs-lookup"><span data-stu-id="0ca17-219">long</span></span> | <span data-ttu-id="0ca17-220">Совпадает со значением 64-разрядное целое число.</span><span class="sxs-lookup"><span data-stu-id="0ca17-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="0ca17-221">{x: long}</span><span class="sxs-lookup"><span data-stu-id="0ca17-221">{x:long}</span></span> |
| <span data-ttu-id="0ca17-222">max</span><span class="sxs-lookup"><span data-stu-id="0ca17-222">max</span></span> | <span data-ttu-id="0ca17-223">Соответствует целое максимальное значение.</span><span class="sxs-lookup"><span data-stu-id="0ca17-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="0ca17-224">{x: max(10)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-224">{x:max(10)}</span></span> |
| <span data-ttu-id="0ca17-225">MaxLength</span><span class="sxs-lookup"><span data-stu-id="0ca17-225">maxlength</span></span> | <span data-ttu-id="0ca17-226">Соответствует строке с максимальной длиной.</span><span class="sxs-lookup"><span data-stu-id="0ca17-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="0ca17-227">{x: maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="0ca17-228">min</span><span class="sxs-lookup"><span data-stu-id="0ca17-228">min</span></span> | <span data-ttu-id="0ca17-229">Соответствует целое число с минимальным значением.</span><span class="sxs-lookup"><span data-stu-id="0ca17-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="0ca17-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-230">{x:min(10)}</span></span> |
| <span data-ttu-id="0ca17-231">minLength</span><span class="sxs-lookup"><span data-stu-id="0ca17-231">minlength</span></span> | <span data-ttu-id="0ca17-232">Соответствует строке минимальной длины.</span><span class="sxs-lookup"><span data-stu-id="0ca17-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="0ca17-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="0ca17-234">range</span><span class="sxs-lookup"><span data-stu-id="0ca17-234">range</span></span> | <span data-ttu-id="0ca17-235">Соответствует целым числом в диапазоне значений.</span><span class="sxs-lookup"><span data-stu-id="0ca17-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="0ca17-236">{x: range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="0ca17-237">regex</span><span class="sxs-lookup"><span data-stu-id="0ca17-237">regex</span></span> | <span data-ttu-id="0ca17-238">Соответствует регулярному выражению.</span><span class="sxs-lookup"><span data-stu-id="0ca17-238">Matches a regular expression.</span></span> | <span data-ttu-id="0ca17-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="0ca17-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="0ca17-240">Обратите внимание на то что некоторые ограничения, такие как &quot;min&quot;, принимают аргументы в круглые скобки.</span><span class="sxs-lookup"><span data-stu-id="0ca17-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="0ca17-241">Можно применить несколько ограничений для параметра, разделенные двоеточием.</span><span class="sxs-lookup"><span data-stu-id="0ca17-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="0ca17-242">Ограничения пользовательских маршрутов</span><span class="sxs-lookup"><span data-stu-id="0ca17-242">Custom Route Constraints</span></span>

<span data-ttu-id="0ca17-243">Ограничения пользовательских маршрутов можно создать путем реализации **IHttpRouteConstraint** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="0ca17-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="0ca17-244">Например следующее ограничение ограничивает параметр ненулевое целочисленное значение.</span><span class="sxs-lookup"><span data-stu-id="0ca17-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="0ca17-245">Ниже показано, как зарегистрировать ограничение:</span><span class="sxs-lookup"><span data-stu-id="0ca17-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="0ca17-246">Теперь можно применить ограничения в свои маршруты:</span><span class="sxs-lookup"><span data-stu-id="0ca17-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="0ca17-247">Также можно заменить весь **DefaultInlineConstraintResolver** класса путем реализации **IInlineConstraintResolver** интерфейс.</span><span class="sxs-lookup"><span data-stu-id="0ca17-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="0ca17-248">Это заменит все встроенные ограничения, если ваша реализация **IInlineConstraintResolver** специально добавляет их.</span><span class="sxs-lookup"><span data-stu-id="0ca17-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="0ca17-249">Дополнительный URI параметры и значения по умолчанию</span><span class="sxs-lookup"><span data-stu-id="0ca17-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="0ca17-250">Можно сделать необязательным параметра URI, добавив вопросительным знаком для параметра маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="0ca17-251">Если параметр маршрута является необязательным, необходимо определить значение по умолчанию для параметра метода.</span><span class="sxs-lookup"><span data-stu-id="0ca17-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="0ca17-252">В этом примере `/api/books/locale/1033` и `/api/books/locale` возвращает тот же ресурс.</span><span class="sxs-lookup"><span data-stu-id="0ca17-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="0ca17-253">Кроме того можно указать значение по умолчанию в шаблоне маршрута, следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0ca17-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="0ca17-254">Это почти так же, как в предыдущем примере, но есть незначительные различия поведения, когда применяется значение по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="0ca17-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="0ca17-255">В первом примере («{lcid?}») значение по умолчанию 1033 назначается непосредственно параметр метода, поэтому параметр будет иметь точное значение.</span><span class="sxs-lookup"><span data-stu-id="0ca17-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="0ca17-256">Во втором примере ("{lcid = 1033}»), значение по умолчанию «1033» проходит через процесс привязки модели.</span><span class="sxs-lookup"><span data-stu-id="0ca17-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="0ca17-257">Связыватель модели по умолчанию преобразует «1033» в числовое значение 1033.</span><span class="sxs-lookup"><span data-stu-id="0ca17-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="0ca17-258">Тем не менее можно подключить в настраиваемый связыватель модели, который может делать нечто иное.</span><span class="sxs-lookup"><span data-stu-id="0ca17-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="0ca17-259">(В большинстве случаев при отсутствии пользовательские связыватели модели в конвейере, две формы будет эквивалентное.)</span><span class="sxs-lookup"><span data-stu-id="0ca17-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="0ca17-260">Имена маршрутов</span><span class="sxs-lookup"><span data-stu-id="0ca17-260">Route Names</span></span>

<span data-ttu-id="0ca17-261">В веб-API каждый маршрут имеет имя.</span><span class="sxs-lookup"><span data-stu-id="0ca17-261">In Web API, every route has a name.</span></span> <span data-ttu-id="0ca17-262">Имена маршрутов можно использовать для создания ссылок, таким образом, можно добавить ссылку в HTTP-ответа.</span><span class="sxs-lookup"><span data-stu-id="0ca17-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="0ca17-263">Чтобы указать имя маршрута, задайте **имя** атрибута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="0ca17-264">Следующий пример показывает, как задать имя маршрута, а также как использовать имя маршрута, при создании ссылки.</span><span class="sxs-lookup"><span data-stu-id="0ca17-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="0ca17-265">Порядок маршрута</span><span class="sxs-lookup"><span data-stu-id="0ca17-265">Route Order</span></span>

<span data-ttu-id="0ca17-266">Когда платформа пытается соответствует URI с маршрутом, она проверяет маршруты в определенном порядке.</span><span class="sxs-lookup"><span data-stu-id="0ca17-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="0ca17-267">Чтобы указать порядок, задайте **порядок** атрибута маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-267">To specify the order, set the **Order** property on the route attribute.</span></span> <span data-ttu-id="0ca17-268">Более низкие значения вычисляются первыми.</span><span class="sxs-lookup"><span data-stu-id="0ca17-268">Lower values are evaluated first.</span></span> <span data-ttu-id="0ca17-269">Значение порядка по умолчанию равно нулю.</span><span class="sxs-lookup"><span data-stu-id="0ca17-269">The default order value is zero.</span></span>

<span data-ttu-id="0ca17-270">Вот, как определяется общем порядке:</span><span class="sxs-lookup"><span data-stu-id="0ca17-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="0ca17-271">Сравнение **порядок** свойство атрибута маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-271">Compare the **Order** property of the route attribute.</span></span>
2. <span data-ttu-id="0ca17-272">Рассмотрим каждый сегмент URI в шаблоне маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="0ca17-273">Для каждого сегмента заказов следующим образом:</span><span class="sxs-lookup"><span data-stu-id="0ca17-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="0ca17-274">Литерал сегменты.</span><span class="sxs-lookup"><span data-stu-id="0ca17-274">Literal segments.</span></span>
    2. <span data-ttu-id="0ca17-275">Параметры маршрута с ограничениями.</span><span class="sxs-lookup"><span data-stu-id="0ca17-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="0ca17-276">Параметры маршрута без ограничений.</span><span class="sxs-lookup"><span data-stu-id="0ca17-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="0ca17-277">Сегменты с подстановочными знаками параметр с ограничениями.</span><span class="sxs-lookup"><span data-stu-id="0ca17-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="0ca17-278">Сегменты с подстановочными знаками параметр без ограничений.</span><span class="sxs-lookup"><span data-stu-id="0ca17-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="0ca17-279">В случае равенства преимущество получает маршруты упорядочиваются по сравнение строк по порядковому номеру без учета регистра ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) шаблона маршрута.</span><span class="sxs-lookup"><span data-stu-id="0ca17-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="0ca17-280">Пример.</span><span class="sxs-lookup"><span data-stu-id="0ca17-280">Here is an example.</span></span> <span data-ttu-id="0ca17-281">Предположим, вы определяете следующего контроллера:</span><span class="sxs-lookup"><span data-stu-id="0ca17-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="0ca17-282">Эти маршруты упорядочены следующим образом.</span><span class="sxs-lookup"><span data-stu-id="0ca17-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="0ca17-283">заказы и сведения</span><span class="sxs-lookup"><span data-stu-id="0ca17-283">orders/details</span></span>
2. <span data-ttu-id="0ca17-284">заказы / {id}</span><span class="sxs-lookup"><span data-stu-id="0ca17-284">orders/{id}</span></span>
3. <span data-ttu-id="0ca17-285">заказы / {customerName}</span><span class="sxs-lookup"><span data-stu-id="0ca17-285">orders/{customerName}</span></span>
4. <span data-ttu-id="0ca17-286">заказы / {\*Дата}</span><span class="sxs-lookup"><span data-stu-id="0ca17-286">orders/{\*date}</span></span>
5. <span data-ttu-id="0ca17-287">заказы / ожидание</span><span class="sxs-lookup"><span data-stu-id="0ca17-287">orders/pending</span></span>

<span data-ttu-id="0ca17-288">Обратите внимание, что «подробности» является сегментом литерала и появляется перед «{id}», но «Ожидание» отображается последнее, так как **порядок** свойства является 1.</span><span class="sxs-lookup"><span data-stu-id="0ca17-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **Order** property is 1.</span></span> <span data-ttu-id="0ca17-289">(В этом примере предполагается, что существует то заказчики с именем «подробности» или «Ожидание».</span><span class="sxs-lookup"><span data-stu-id="0ca17-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="0ca17-290">Как правило Старайтесь избегать неоднозначных маршрутов.</span><span class="sxs-lookup"><span data-stu-id="0ca17-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="0ca17-291">В этом примере лучше шаблон маршрута для `GetByCustomer` является «клиентов / {customerName}»)</span><span class="sxs-lookup"><span data-stu-id="0ca17-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
