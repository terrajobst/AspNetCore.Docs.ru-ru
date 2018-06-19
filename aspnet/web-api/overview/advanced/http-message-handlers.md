---
uid: web-api/overview/advanced/http-message-handlers
title: Обработчики сообщений HTTP в ASP.NET Web API | Документы Microsoft
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/13/2012
ms.topic: article
ms.assetid: 9002018b-3aa3-4358-bb1c-fbb5bc751d01
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/http-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 931ad3330d5e4dc2424ab37b8a6e2a3d123a8684
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
ms.locfileid: "26506953"
---
<a name="http-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="5ac9d-102">Обработчики HTTP-сообщения в веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="5ac9d-102">HTTP Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="5ac9d-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="5ac9d-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="5ac9d-104">Объект *обработчик сообщений* является классом, который получает HTTP-запрос и возвращает ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span> <span data-ttu-id="5ac9d-105">Обработчики сообщений являются производными от абстрактного **HttpMessageHandler** класса.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-105">Message handlers derive from the abstract **HttpMessageHandler** class.</span></span>

<span data-ttu-id="5ac9d-106">Как правило ряд обработчики сообщений соединяются друг с другом.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-106">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="5ac9d-107">Первый обработчик получает запрос HTTP, выполняет некоторую обработку и дает в обработчик следующий запрос.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-107">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="5ac9d-108">На некотором этапе ответ создается и возвращается в цепочке.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-108">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="5ac9d-109">Эта модель называется *делегирование* обработчика.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-109">This pattern is called a *delegating* handler.</span></span>

![](http-message-handlers/_static/image1.png)

## <a name="server-side-message-handlers"></a><span data-ttu-id="5ac9d-110">Обработчики сообщений на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="5ac9d-110">Server-Side Message Handlers</span></span>

<span data-ttu-id="5ac9d-111">На стороне сервера в конвейер веб-API использует обработчики для некоторых встроенных сообщений:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-111">On the server side, the Web API pipeline uses some built-in message handlers:</span></span>

- <span data-ttu-id="5ac9d-112">**Сервер HTTP** получает запрос от узла.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-112">**HttpServer** gets the request from the host.</span></span>
- <span data-ttu-id="5ac9d-113">**HttpRoutingDispatcher** отправляет запрос на основе маршрута.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-113">**HttpRoutingDispatcher** dispatches the request based on the route.</span></span>
- <span data-ttu-id="5ac9d-114">**HttpControllerDispatcher** отправляет запрос на контроллер веб-API.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-114">**HttpControllerDispatcher** sends the request to a Web API controller.</span></span>

<span data-ttu-id="5ac9d-115">Можно добавить пользовательские обработчики в конвейере.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-115">You can add custom handlers to the pipeline.</span></span> <span data-ttu-id="5ac9d-116">Обработчики сообщений хорошо подходят для решении общих задач, которые работают на уровне HTTP сообщения (а не действия контроллера).</span><span class="sxs-lookup"><span data-stu-id="5ac9d-116">Message handlers are good for cross-cutting concerns that operate at the level of HTTP messages (rather than controller actions).</span></span> <span data-ttu-id="5ac9d-117">Например обработчик сообщений могут:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-117">For example, a message handler might:</span></span>

- <span data-ttu-id="5ac9d-118">Чтение и изменение заголовков запроса.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-118">Read or modify request headers.</span></span>
- <span data-ttu-id="5ac9d-119">Добавьте заголовок ответа для ответов.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-119">Add a response header to responses.</span></span>
- <span data-ttu-id="5ac9d-120">Проверяет запросы, прежде чем они достигнут контроллера.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-120">Validate requests before they reach the controller.</span></span>

<span data-ttu-id="5ac9d-121">На этой диаграмме показаны две пользовательские обработчики, вставляемых в конвейере:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-121">This diagram shows two custom handlers inserted into the pipeline:</span></span>

![](http-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="5ac9d-122">На стороне клиента HttpClient также использует обработчики сообщений.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-122">On the client side, HttpClient also uses message handlers.</span></span> <span data-ttu-id="5ac9d-123">Дополнительные сведения см. в разделе [обработчики сообщений HttpClient](httpclient-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="5ac9d-123">For more information, see [HttpClient Message Handlers](httpclient-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="5ac9d-124">Обработчики пользовательских сообщений</span><span class="sxs-lookup"><span data-stu-id="5ac9d-124">Custom Message Handlers</span></span>

<span data-ttu-id="5ac9d-125">Чтобы написать обработчик пользовательского сообщения, являются производными от **System.Net.Http.DelegatingHandler** и Переопределите **SendAsync** метод.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-125">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="5ac9d-126">Этот метод имеет следующую сигнатуру:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-126">This method has the following signature:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample1.cs)]

<span data-ttu-id="5ac9d-127">Этот метод принимает **HttpRequestMessage** как входные данные и асинхронно возвращает **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-127">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="5ac9d-128">Типичная реализация выполняет следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-128">A typical implementation does the following:</span></span>

1. <span data-ttu-id="5ac9d-129">Обрабатывает сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-129">Process the request message.</span></span>
2. <span data-ttu-id="5ac9d-130">Вызовите `base.SendAsync` отправлять запрос внутреннему обработчику.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-130">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="5ac9d-131">Внутренний обработчик возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-131">The inner handler returns a response message.</span></span> <span data-ttu-id="5ac9d-132">(Этот шаг является асинхронной.)</span><span class="sxs-lookup"><span data-stu-id="5ac9d-132">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="5ac9d-133">Обработать ответ и возвращается вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-133">Process the response and return it to the caller.</span></span>

<span data-ttu-id="5ac9d-134">Вот упрощенного примера:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-134">Here is a trivial example:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="5ac9d-135">Вызов `base.SendAsync` является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-135">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="5ac9d-136">Если обработчик не выполняет никаких действий после этого вызова, используйте **await** ключевое слово, как показано.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-136">If the handler does any work after this call, use the **await** keyword, as shown.</span></span>


<span data-ttu-id="5ac9d-137">Делегирование обработчик можно также пропустить внутренний обработчик и непосредственного создания ответа:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-137">A delegating handler can also skip the inner handler and directly create the response:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample3.cs)]

<span data-ttu-id="5ac9d-138">Если делегирование обработчик создает ответ без вызова `base.SendAsync`, запрос пропускает оставшуюся часть конвейера.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-138">If a delegating handler creates the response without calling `base.SendAsync`, the request skips the rest of the pipeline.</span></span> <span data-ttu-id="5ac9d-139">Это может быть полезно для обработчик, который проверяет запрос (Создание ответ с ошибкой).</span><span class="sxs-lookup"><span data-stu-id="5ac9d-139">This can be useful for a handler that validates the request (creating an error response).</span></span>

![](http-message-handlers/_static/image3.png)

## <a name="adding-a-handler-to-the-pipeline"></a><span data-ttu-id="5ac9d-140">Добавление обработчика в конвейере</span><span class="sxs-lookup"><span data-stu-id="5ac9d-140">Adding a Handler to the Pipeline</span></span>

<span data-ttu-id="5ac9d-141">Чтобы добавить обработчик сообщений на стороне сервера, добавьте обработчик **HttpConfiguration.MessageHandlers** коллекции.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-141">To add a message handler on the server side, add the handler to the **HttpConfiguration.MessageHandlers** collection.</span></span> <span data-ttu-id="5ac9d-142">Если шаблон «ASP.NET MVC 4 веб-приложения» используется для создания проекта, это можно сделать это внутри **WebApiConfig** класса:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-142">If you used the "ASP.NET MVC 4 Web Application" template to create the project, you can do this inside the **WebApiConfig** class:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample4.cs)]

<span data-ttu-id="5ac9d-143">Обработчики сообщений вызываются в том же порядке, в котором они появляются в **MessageHandlers** коллекции.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-143">Message handlers are called in the same order that they appear in **MessageHandlers** collection.</span></span> <span data-ttu-id="5ac9d-144">Поскольку они вкладываются ответное сообщение перемещается в обратном направлении.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-144">Because they are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="5ac9d-145">Последним обработчиком является первым получает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-145">That is, the last handler is the first to get the response message.</span></span>

<span data-ttu-id="5ac9d-146">Обратите внимание, что не требуется задать внутренних обработчиков; Платформа веб-API автоматически подключается обработчиков сообщений.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-146">Notice that you don't need to set the inner handlers; the Web API framework automatically connects the message handlers.</span></span>

<span data-ttu-id="5ac9d-147">Если вы являетесь [резидентным](../older-versions/self-host-a-web-api.md), создайте экземпляр класса **HttpSelfHostConfiguration** и добавьте обработчики для **MessageHandlers** коллекции.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-147">If you are [self-hosting](../older-versions/self-host-a-web-api.md), create an instance of the **HttpSelfHostConfiguration** class and add the handlers to the **MessageHandlers** collection.</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample5.cs)]

<span data-ttu-id="5ac9d-148">Теперь давайте рассмотрим некоторые примеры обработчики пользовательских сообщений.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-148">Now let's look at some examples of custom message handlers.</span></span>

## <a name="example-x-http-method-override"></a><span data-ttu-id="5ac9d-149">Пример: X-HTTP-Method-Override</span><span class="sxs-lookup"><span data-stu-id="5ac9d-149">Example: X-HTTP-Method-Override</span></span>

<span data-ttu-id="5ac9d-150">X-HTTP-Method-Override является стандартным заголовком HTTP.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-150">X-HTTP-Method-Override is a non-standard HTTP header.</span></span> <span data-ttu-id="5ac9d-151">Она предназначена для клиентов, которые не отправляют определенные типы HTTP-запросов, такой как PUT или DELETE.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-151">It is designed for clients that cannot send certain HTTP request types, such as PUT or DELETE.</span></span> <span data-ttu-id="5ac9d-152">Вместо этого клиент отправляет запрос POST и задает заголовок X-HTTP-Method-Override для нужного метода.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-152">Instead, the client sends a POST request and sets the X-HTTP-Method-Override header to the desired method.</span></span> <span data-ttu-id="5ac9d-153">Пример:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-153">For example:</span></span>

[!code-console[Main](http-message-handlers/samples/sample6.cmd)]

<span data-ttu-id="5ac9d-154">Ниже приведен обработчик сообщений, который добавляет поддержку для X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-154">Here is a message handler that adds support for X-HTTP-Method-Override:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample7.cs)]

<span data-ttu-id="5ac9d-155">В **SendAsync** метод, обработчик проверяет, находится ли сообщение-запрос POST-запрос и содержит ли заголовок X-HTTP-Method-Override.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-155">In the **SendAsync** method, the handler checks whether the request message is a POST request, and whether it contains the X-HTTP-Method-Override header.</span></span> <span data-ttu-id="5ac9d-156">В этом случае он проверяет значение заголовка и затем изменяет метод запроса.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-156">If so, it validates the header value, and then modifies the request method.</span></span> <span data-ttu-id="5ac9d-157">Наконец, обработчик вызывает `base.SendAsync` для передачи сообщения обработчику Далее.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-157">Finally, the handler calls `base.SendAsync` to pass the message to the next handler.</span></span>

<span data-ttu-id="5ac9d-158">Когда запрос достигает **HttpControllerDispatcher** класса **HttpControllerDispatcher** направит запрос на основе метода обновленный запрос.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-158">When the request reaches the **HttpControllerDispatcher** class, **HttpControllerDispatcher** will route the request based on the updated request method.</span></span>

## <a name="example-adding-a-custom-response-header"></a><span data-ttu-id="5ac9d-159">Пример: Добавление настраиваемого заголовка ответа</span><span class="sxs-lookup"><span data-stu-id="5ac9d-159">Example: Adding a Custom Response Header</span></span>

<span data-ttu-id="5ac9d-160">Ниже приведен обработчик сообщений, который добавляет пользовательский заголовок в каждое сообщение ответа.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-160">Here is a message handler that adds a custom header to every response message:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample8.cs)]

<span data-ttu-id="5ac9d-161">Во-первых, обработчик вызывает `base.SendAsync` для передачи запроса внутренний обработчик сообщений.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-161">First, the handler calls `base.SendAsync` to pass the request to the inner message handler.</span></span> <span data-ttu-id="5ac9d-162">Внутренний обработчик возвращает ответное сообщение, но не асинхронно с помощью **задачи&lt;T&gt;**  объекта.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-162">The inner handler returns a response message, but it does so asynchronously using a **Task&lt;T&gt;** object.</span></span> <span data-ttu-id="5ac9d-163">Ответное сообщение недоступно до `base.SendAsync` завершения асинхронно.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-163">The response message is not available until `base.SendAsync` completes asynchronously.</span></span>

<span data-ttu-id="5ac9d-164">В этом примере используется **await** ключевое слово для выполнения работы асинхронно после `SendAsync` завершения.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-164">This example uses the **await** keyword to perform work asynchronously after `SendAsync` completes.</span></span> <span data-ttu-id="5ac9d-165">Если вы используете .NET Framework 4.0, используйте **задачи**&lt;T&gt;**. ContinueWith** метод:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-165">If you are targeting .NET Framework 4.0, use the **Task**&lt;T&gt;**.ContinueWith** method:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample9.cs)]

## <a name="example-checking-for-an-api-key"></a><span data-ttu-id="5ac9d-166">Пример: Поиск ключа API</span><span class="sxs-lookup"><span data-stu-id="5ac9d-166">Example: Checking for an API Key</span></span>

<span data-ttu-id="5ac9d-167">Некоторые веб-службы требуется клиентов включить ключ API в свой запрос.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-167">Some web services require clients to include an API key in their request.</span></span> <span data-ttu-id="5ac9d-168">В следующем примере показано, как обработчик сообщений можно проверить запросы действительного ключа API:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-168">The following example shows how a message handler can check requests for a valid API key:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample10.cs)]

<span data-ttu-id="5ac9d-169">Этот обработчик ищет ключ API в строке запроса URI.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-169">This handler looks for the API key in the URI query string.</span></span> <span data-ttu-id="5ac9d-170">(В этом примере предполагается, что ключ является статическая строка.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-170">(For this example, we assume that the key is a static string.</span></span> <span data-ttu-id="5ac9d-171">Фактической реализации, скорее всего, использовать более сложные проверки.) Если строка запроса содержит ключ, обработчик передает запрос внутреннему обработчику.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-171">A real implementation would probably use more complex validation.) If the query string contains the key, the handler passes the request to the inner handler.</span></span>

<span data-ttu-id="5ac9d-172">Если запрос не имеет допустимый ключ, обработчик создает ответное сообщение с состояния 403, запрещено.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-172">If the request does not have a valid key, the handler creates a response message with status 403, Forbidden.</span></span> <span data-ttu-id="5ac9d-173">В этом случае обработчик вызывает `base.SendAsync`, поэтому внутренний обработчик не получает запрос, а также контроллер.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-173">In this case, the handler does not call `base.SendAsync`, so the inner handler never receives the request, nor does the controller.</span></span> <span data-ttu-id="5ac9d-174">Таким образом контроллер может предполагается, что все входящие запросы допустимый ключ API.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-174">Therefore, the controller can assume that all incoming requests have a valid API key.</span></span>

> [!NOTE]
> <span data-ttu-id="5ac9d-175">Если ключ API применяется только к определенным действиям контроллера, можно использовать фильтр действий вместо обработчик сообщений.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-175">If the API key applies only to certain controller actions, consider using an action filter instead of a message handler.</span></span> <span data-ttu-id="5ac9d-176">Фильтры действий выполняются после выполняется маршрутизация URI.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-176">Action filters run after URI routing is performed.</span></span>


## <a name="per-route-message-handlers"></a><span data-ttu-id="5ac9d-177">Обработчики сообщений на маршрут</span><span class="sxs-lookup"><span data-stu-id="5ac9d-177">Per-Route Message Handlers</span></span>

<span data-ttu-id="5ac9d-178">Обработчики в **HttpConfiguration.MessageHandlers** коллекции применяются глобально.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-178">Handlers in the **HttpConfiguration.MessageHandlers** collection apply globally.</span></span>

<span data-ttu-id="5ac9d-179">Кроме того можно добавить обработчик сообщений для конкретного маршрута при определении маршрута:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-179">Alternatively, you can add a message handler to a specific route when you define the route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample11.cs?highlight=16)]

<span data-ttu-id="5ac9d-180">В этом примере, если URI запроса совпадает с «Route2», запрос отправляется `MessageHandler2`.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-180">In this example, if the request URI matches "Route2", the request is dispatched to `MessageHandler2`.</span></span> <span data-ttu-id="5ac9d-181">В примере ниже показан конвейер для этих маршрутов:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-181">The following diagram shows the pipeline for these two routes:</span></span>

![](http-message-handlers/_static/image4.png)

<span data-ttu-id="5ac9d-182">Обратите внимание, что `MessageHandler2` заменяет значение по умолчанию **HttpControllerDispatcher**.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-182">Notice that `MessageHandler2` replaces the default **HttpControllerDispatcher**.</span></span> <span data-ttu-id="5ac9d-183">В этом примере `MessageHandler2` создает ответ и запросы, которые соответствуют «Route2» никогда не переходить к контроллеру.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-183">In this example, `MessageHandler2` creates the response, and requests that match "Route2" never go to a controller.</span></span> <span data-ttu-id="5ac9d-184">Это позволяет заменить весь механизм контроллера веб-API с пользовательской конечной точкой.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-184">This lets you replace the entire Web API controller mechanism with your own custom endpoint.</span></span>

<span data-ttu-id="5ac9d-185">Кроме того, можно делегировать обработчик сообщений-маршрута **HttpControllerDispatcher**, который затем перенаправляется в контроллере.</span><span class="sxs-lookup"><span data-stu-id="5ac9d-185">Alternatively, a per-route message handler can delegate to **HttpControllerDispatcher**, which then dispatches to a controller.</span></span>

![](http-message-handlers/_static/image5.png)

<span data-ttu-id="5ac9d-186">Ниже показано, как настройка этого маршрута:</span><span class="sxs-lookup"><span data-stu-id="5ac9d-186">The following code shows how to configure this route:</span></span>

[!code-csharp[Main](http-message-handlers/samples/sample12.cs)]
