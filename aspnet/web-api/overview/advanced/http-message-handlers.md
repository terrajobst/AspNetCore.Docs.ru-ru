---
uid: web-api/overview/advanced/http-message-handlers
title: Обработчики сообщений HTTP в веб-API ASP.NET | Документация Майкрософт
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 02/13/2012
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 5694845d6f7f9d868c7b3f83785fddda863756d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/05/2018
ms.locfileid: "37821801"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="eb983-102">Обработчики сообщений HTTP в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="eb983-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="eb983-103">по [Майк Уоссон](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="eb983-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="eb983-104">Объект *обработчик сообщений* — это класс, который получает HTTP-запрос и возвращает ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="eb983-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="eb983-105">Обработчики сообщений являются производными от абстрактного **HttpMessageHandler** класса.</span><span class="sxs-lookup"><span data-stu-id="eb983-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="eb983-106">Как правило ряд обработчиков сообщений соединяются друг с другом.</span><span class="sxs-lookup"><span data-stu-id="eb983-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="eb983-107">Первый обработчик получает запрос HTTP, обрабатывает и предоставляет запрос к следующий обработчик.</span><span class="sxs-lookup"><span data-stu-id="eb983-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="eb983-108">Рано или поздно ответа создается и возвращается в цепочке.</span><span class="sxs-lookup"><span data-stu-id="eb983-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="eb983-109">Такая модель называется *делегирование* обработчика.</span><span class="sxs-lookup"><span data-stu-id="eb983-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="eb983-110">Обработчики сообщений на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="eb983-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="eb983-111">На стороне сервера конвейер веб-API использует некоторые обработчики встроенных сообщений:</span><span class="sxs-lookup"><span data-stu-id="eb983-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="eb983-112">**Сервер HTTP** получает запрос от узла.</span><span class="sxs-lookup"><span data-stu-id="eb983-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="eb983-113">**HttpRoutingDispatcher** отправляет запрос на основе маршрута.</span><span class="sxs-lookup"><span data-stu-id="eb983-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="eb983-114">**HttpControllerDispatcher** отправляет запрос контроллеру веб-API.</span><span class="sxs-lookup"><span data-stu-id="eb983-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="eb983-115">Можно добавить пользовательские обработчики в конвейере.</span><span class="sxs-lookup"><span data-stu-id="eb983-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="eb983-116">Обработчики сообщений хорошо подходят для сквозных функций, которые работают на уровне HTTP сообщения (а не действий контроллера).</span><span class="sxs-lookup"><span data-stu-id="eb983-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="eb983-117">Например обработчик сообщений может:</span><span class="sxs-lookup"><span data-stu-id="eb983-117">For example, a message handler might:</span></span>

- <span data-ttu-id="eb983-118">Чтение и изменение заголовков запроса.</span><span class="sxs-lookup"><span data-stu-id="eb983-118">Read or modify request headers.</span></span>
- <span data-ttu-id="eb983-119">Добавьте заголовок ответа для ответов.</span><span class="sxs-lookup"><span data-stu-id="eb983-119">Add a response header to responses.</span></span>
- <span data-ttu-id="eb983-120">Проверьте запросы, прежде чем они достигнут контроллера.</span><span class="sxs-lookup"><span data-stu-id="eb983-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="eb983-121">На этой диаграмме показаны два пользовательских обработчиков, которые вставлены в конвейер:</span><span class="sxs-lookup"><span data-stu-id="eb983-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="eb983-122">На стороне клиента HttpClient также использует обработчики сообщений.</span><span class="sxs-lookup"><span data-stu-id="eb983-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="eb983-123">Дополнительные сведения см. в разделе [обработчики сообщений HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="eb983-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="eb983-124">Обработчики пользовательских сообщений</span><span class="sxs-lookup"><span data-stu-id="eb983-124">Custom Message Handlers</span></span>

<span data-ttu-id="eb983-125">Чтобы написать обработчик пользовательского сообщения, являются производными от **System.Net.Http.DelegatingHandler** и переопределить **SendAsync** метод.</span><span class="sxs-lookup"><span data-stu-id="eb983-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="eb983-126">Этот метод имеет следующую сигнатуру:</span><span class="sxs-lookup"><span data-stu-id="eb983-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="eb983-127">Этот метод принимает **HttpRequestMessage** как входных данных и асинхронно возвращает **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="eb983-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="eb983-128">Типичной реализации выполняет следующие функции:</span><span class="sxs-lookup"><span data-stu-id="eb983-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="eb983-129">Процесс сообщения запроса.</span><span class="sxs-lookup"><span data-stu-id="eb983-129">Process the request message.</span></span>
2. <span data-ttu-id="eb983-130">Вызовите `base.SendAsync` для отправки запроса на внутренний обработчик.</span><span class="sxs-lookup"><span data-stu-id="eb983-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="eb983-131">Внутренний обработчик возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="eb983-131">The inner handler returns a response message.</span></span> <span data-ttu-id="eb983-132">(Этот шаг выполняется асинхронно).</span><span class="sxs-lookup"><span data-stu-id="eb983-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="eb983-133">Обработать ответ и вернуть его вызывающей стороне.</span><span class="sxs-lookup"><span data-stu-id="eb983-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="eb983-134">Вот тривиальный пример:</span><span class="sxs-lookup"><span data-stu-id="eb983-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="eb983-135">Вызов `base.SendAsync` является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="eb983-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="eb983-136">Если обработчик не выполняет никакой работы после этого вызова, используйте **await** ключевое слово, как показано.</span><span class="sxs-lookup"><span data-stu-id="eb983-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="eb983-137">Делегирование обработчик можно также пропустить внутренний обработчик и напрямую создать ответ:</span><span class="sxs-lookup"><span data-stu-id="eb983-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="eb983-138">Если делегирование обработчик создает ответ без вызова `base.SendAsync`, запрос пропускает остальной части конвейера.</span><span class="sxs-lookup"><span data-stu-id="eb983-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="eb983-139">Это может быть полезно для обработчика, который проверяет запрос (Создание сообщение об ошибке).</span><span class="sxs-lookup"><span data-stu-id="eb983-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="eb983-140">Добавление обработчика в конвейер</span><span class="sxs-lookup"><span data-stu-id="eb983-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="eb983-141">Чтобы добавить обработчик сообщений на стороне сервера, добавьте обработчик **HttpConfiguration.MessageHandlers** коллекции.</span><span class="sxs-lookup"><span data-stu-id="eb983-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="eb983-142">Если вы использовали шаблон «ASP.NET MVC 4 веб-приложение» для создания проекта, это можно сделать это внутри **WebApiConfig** класса:</span><span class="sxs-lookup"><span data-stu-id="eb983-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="eb983-143">Обработчики сообщений вызываются в том же порядке, в котором они расположены в **MessageHandlers** коллекции.</span><span class="sxs-lookup"><span data-stu-id="eb983-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="eb983-144">Так как они являются вложенными, ответное сообщение перемещается в обратном направлении.</span><span class="sxs-lookup"><span data-stu-id="eb983-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="eb983-145">Последним обработчиком является первым ответного сообщения.</span><span class="sxs-lookup"><span data-stu-id="eb983-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="eb983-146">Обратите внимание на то, что вам не нужно задавать внутренних обработчиков; Платформа веб-API автоматически подключается обработчиков сообщений.</span><span class="sxs-lookup"><span data-stu-id="eb983-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="eb983-147">Если вы являетесь [размещение на собственном сервере](../older-versions/self-host-a-web-api.md), создайте экземпляр **HttpSelfHostConfiguration** класса и добавьте обработчики для **MessageHandlers** коллекции.</span><span class="sxs-lookup"><span data-stu-id="eb983-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="eb983-148">Теперь давайте рассмотрим несколько примеров обработчики пользовательских сообщений.</span><span class="sxs-lookup"><span data-stu-id="eb983-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="eb983-149">Пример: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="eb983-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="eb983-150">X-HTTP-Method-Override является стандартным заголовком HTTP.</span><span class="sxs-lookup"><span data-stu-id="eb983-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="eb983-151">Она предназначена для клиентов, которые не удается отправить определенные типы HTTP-запросов, таких как PUT или DELETE.</span><span class="sxs-lookup"><span data-stu-id="eb983-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="eb983-152">Вместо этого клиент отправляет запрос POST и задает заголовок X-HTTP-Method-Override для нужного метода.</span><span class="sxs-lookup"><span data-stu-id="eb983-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="eb983-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="eb983-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="eb983-154">Ниже приведен обработчик сообщений, который добавляет поддержку для X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="eb983-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="eb983-155">В **SendAsync** метод, обработчик проверяет, является ли сообщение запроса запрос POST и содержит ли он заголовок X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="eb983-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="eb983-156">Если Да, он проверяет значение заголовка и затем изменяет метод запроса.</span><span class="sxs-lookup"><span data-stu-id="eb983-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="eb983-157">Наконец, обработчик вызывает `base.SendAsync` для передачи сообщения следующего обработчика.</span><span class="sxs-lookup"><span data-stu-id="eb983-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="eb983-158">Когда запрос достигает **HttpControllerDispatcher** класс, **HttpControllerDispatcher** направит запрос на основе метода обновленный запрос.</span><span class="sxs-lookup"><span data-stu-id="eb983-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="eb983-159">Пример: Добавление настраиваемого заголовка ответа</span><span class="sxs-lookup"><span data-stu-id="eb983-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="eb983-160">Вот обработчик сообщений, который добавляет пользовательский заголовок к каждому сообщению ответа:</span><span class="sxs-lookup"><span data-stu-id="eb983-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="eb983-161">Во-первых, обработчик вызывает `base.SendAsync` для передачи запроса внутренний обработчик сообщений.</span><span class="sxs-lookup"><span data-stu-id="eb983-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="eb983-162">Внутренний обработчик возвращает ответное сообщение, но не асинхронно с помощью **задачи&lt;T&gt;**  объекта.</span><span class="sxs-lookup"><span data-stu-id="eb983-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="eb983-163">Ответное сообщение недоступно до `base.SendAsync` завершения асинхронно.</span><span class="sxs-lookup"><span data-stu-id="eb983-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="eb983-164">В этом примере используется **await** ключевое слово, чтобы выполнять работу асинхронно после того, как `SendAsync` завершения.</span><span class="sxs-lookup"><span data-stu-id="eb983-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="eb983-165">Если вы ориентируетесь на .NET Framework 4.0, используйте **задачи**&lt;T&gt;**. ContinueWith** метод:</span><span class="sxs-lookup"><span data-stu-id="eb983-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="eb983-166">Пример: Проверка наличия ключа API</span><span class="sxs-lookup"><span data-stu-id="eb983-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="eb983-167">Некоторые веб-службы требуется клиентам содержать ключ API в свой запрос.</span><span class="sxs-lookup"><span data-stu-id="eb983-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="eb983-168">В следующем примере показано, как обработчик сообщений можно проверить запросы на допустимый ключ API:</span><span class="sxs-lookup"><span data-stu-id="eb983-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="eb983-169">Этот обработчик выполняет поиск ключа API в строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="eb983-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="eb983-170">(В этом примере предполагается, что ключ является статическая строка.</span><span class="sxs-lookup"><span data-stu-id="eb983-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="eb983-171">Фактической реализации используются более сложные проверки.) Если строка запроса содержит ключ, обработчик передает запрос внутреннему обработчику.</span><span class="sxs-lookup"><span data-stu-id="eb983-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="eb983-172">Если запрос не имеет допустимый ключ, обработчик создает ответное сообщение с состояния 403, запрещено.</span><span class="sxs-lookup"><span data-stu-id="eb983-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="eb983-173">В этом случае обработчик не вызывает `base.SendAsync`, поэтому внутренний обработчик никогда не получает запрос, а также контроллер.</span><span class="sxs-lookup"><span data-stu-id="eb983-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="eb983-174">Таким образом контроллер может рассчитывать на все входящие запросы действительный ключ API.</span><span class="sxs-lookup"><span data-stu-id="eb983-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="eb983-175">Если ключ API применяется только к определенным действиям контроллера, рассмотрите возможность использования фильтра действий вместо обработчика сообщений.</span><span class="sxs-lookup"><span data-stu-id="eb983-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="eb983-176">Фильтры действий выполняются после выполняется маршрутизация URI.</span><span class="sxs-lookup"><span data-stu-id="eb983-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="eb983-177">Обработчики сообщений-Route</span><span class="sxs-lookup"><span data-stu-id="eb983-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="eb983-178">Обработчики в **HttpConfiguration.MessageHandlers** коллекции применяются глобально.</span><span class="sxs-lookup"><span data-stu-id="eb983-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="eb983-179">Кроме того можно добавить обработчик сообщений для определенного маршрута при определении маршрута:</span><span class="sxs-lookup"><span data-stu-id="eb983-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="eb983-180">В этом примере, если URI запроса совпадает с «Route2», запрос отправляется `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="eb983-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="eb983-181">В примере ниже показан конвейер для этих маршрутов:</span><span class="sxs-lookup"><span data-stu-id="eb983-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="eb983-182">Обратите внимание, что `MessageHandler2` заменяет значение по умолчанию **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="eb983-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="eb983-183">В этом примере `MessageHandler2` создает ответ, и запросы, которые соответствуют «Route2» не переходить к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="eb983-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="eb983-184">Это позволяет заменить весь механизм контроллера веб-API с пользовательской конечной точке.</span><span class="sxs-lookup"><span data-stu-id="eb983-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="eb983-185">Кроме того, можно делегировать обработчик сообщений-route **HttpControllerDispatcher**, который затем отправляет к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="eb983-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="eb983-186">Ниже показано, как настроить этот маршрут:</span><span class="sxs-lookup"><span data-stu-id="eb983-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
