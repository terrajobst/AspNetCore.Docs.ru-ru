---
uid: web-api/overview/advanced/httpclient-message-handlers
title: "Обработчики сообщений HttpClient веб-API ASP.NET | Документы Microsoft"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/01/2012
ms.topic: article
ms.assetid: 5a4b6c80-b2e9-4710-8969-d5076f7f82b8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/advanced/httpclient-message-handlers
msc.type: authoredcontent
ms.openlocfilehash: 805741b0ac682b7479ce82127df48b1b9a49a427
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/10/2017
---
<a name="httpclient-message-handlers-in-aspnet-web-api"></a><span data-ttu-id="9f315-102">Обработчики сообщений HttpClient веб-API ASP.NET</span><span class="sxs-lookup"><span data-stu-id="9f315-102">HttpClient Message Handlers in ASP.NET Web API</span></span>
====================
<span data-ttu-id="9f315-103">по [Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="9f315-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="9f315-104">Объект *обработчик сообщений* является классом, который получает HTTP-запрос и возвращает ответ HTTP.</span><span class="sxs-lookup"><span data-stu-id="9f315-104">A *message handler* is a class that receives an HTTP request and returns an HTTP response.</span></span>

<span data-ttu-id="9f315-105">Как правило ряд обработчики сообщений соединяются друг с другом.</span><span class="sxs-lookup"><span data-stu-id="9f315-105">Typically, a series of message handlers are chained together.</span></span> <span data-ttu-id="9f315-106">Первый обработчик получает запрос HTTP, выполняет некоторую обработку и дает в обработчик следующий запрос.</span><span class="sxs-lookup"><span data-stu-id="9f315-106">The first handler receives an HTTP request, does some processing, and gives the request to the next handler.</span></span> <span data-ttu-id="9f315-107">На некотором этапе ответ создается и возвращается в цепочке.</span><span class="sxs-lookup"><span data-stu-id="9f315-107">At some point, the response is created and goes back up the chain.</span></span> <span data-ttu-id="9f315-108">Эта модель называется *делегирование* обработчика.</span><span class="sxs-lookup"><span data-stu-id="9f315-108">This pattern is called a *delegating* handler.</span></span>

![](httpclient-message-handlers/_static/image1.png)

<span data-ttu-id="9f315-109">На стороне клиента **HttpClient** класс использует обработчик сообщений для обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="9f315-109">On the client side, the **HttpClient** class uses a message handler to process requests.</span></span> <span data-ttu-id="9f315-110">Обработчик по умолчанию является **HttpClientHandler**, который отправляет запрос по сети и получает ответ от сервера.</span><span class="sxs-lookup"><span data-stu-id="9f315-110">The default handler is **HttpClientHandler**, which sends the request over the network and gets the response from the server.</span></span> <span data-ttu-id="9f315-111">Можно вставить обработчики пользовательских сообщений в конвейер клиента:</span><span class="sxs-lookup"><span data-stu-id="9f315-111">You can insert custom message handlers into the client pipeline:</span></span>

![](httpclient-message-handlers/_static/image2.png)

> [!NOTE]
> <span data-ttu-id="9f315-112">Веб-API ASP.NET также использует обработчики сообщений на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="9f315-112">ASP.NET Web API also uses message handlers on the server side.</span></span> <span data-ttu-id="9f315-113">Дополнительные сведения см. в разделе [обработчиков сообщений HTTP](http-message-handlers.md).</span><span class="sxs-lookup"><span data-stu-id="9f315-113">For more information, see [HTTP Message Handlers](http-message-handlers.md).</span></span>


## <a name="custom-message-handlers"></a><span data-ttu-id="9f315-114">Обработчики пользовательских сообщений</span><span class="sxs-lookup"><span data-stu-id="9f315-114">Custom Message Handlers</span></span>

<span data-ttu-id="9f315-115">Чтобы написать обработчик пользовательского сообщения, являются производными от **System.Net.Http.DelegatingHandler** и Переопределите **SendAsync** метод.</span><span class="sxs-lookup"><span data-stu-id="9f315-115">To write a custom message handler, derive from **System.Net.Http.DelegatingHandler** and override the **SendAsync** method.</span></span> <span data-ttu-id="9f315-116">Ниже представлена подпись метода:</span><span class="sxs-lookup"><span data-stu-id="9f315-116">Here is the method signature:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample1.cs)]

<span data-ttu-id="9f315-117">Этот метод принимает **HttpRequestMessage** как входные данные и асинхронно возвращает **HttpResponseMessage**.</span><span class="sxs-lookup"><span data-stu-id="9f315-117">The method takes an **HttpRequestMessage** as input and asynchronously returns an **HttpResponseMessage**.</span></span> <span data-ttu-id="9f315-118">Типичная реализация выполняет следующие задачи.</span><span class="sxs-lookup"><span data-stu-id="9f315-118">A typical implementation does the following:</span></span>

1. <span data-ttu-id="9f315-119">Обрабатывает сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="9f315-119">Process the request message.</span></span>
2. <span data-ttu-id="9f315-120">Вызовите `base.SendAsync` отправлять запрос внутреннему обработчику.</span><span class="sxs-lookup"><span data-stu-id="9f315-120">Call `base.SendAsync` to send the request to the inner handler.</span></span>
3. <span data-ttu-id="9f315-121">Внутренний обработчик возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="9f315-121">The inner handler returns a response message.</span></span> <span data-ttu-id="9f315-122">(Этот шаг является асинхронной.)</span><span class="sxs-lookup"><span data-stu-id="9f315-122">(This step is asynchronous.)</span></span>
4. <span data-ttu-id="9f315-123">Обработать ответ и возвращается вызывающему объекту.</span><span class="sxs-lookup"><span data-stu-id="9f315-123">Process the response and return it to the caller.</span></span>

<span data-ttu-id="9f315-124">В следующем примере обработчик сообщений, который необходимо добавить пользовательский заголовок в исходящий запрос:</span><span class="sxs-lookup"><span data-stu-id="9f315-124">The following example shows a message handler that adds a custom header to the outgoing request:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample2.cs)]

<span data-ttu-id="9f315-125">Вызов `base.SendAsync` является асинхронным.</span><span class="sxs-lookup"><span data-stu-id="9f315-125">The call to `base.SendAsync` is asynchronous.</span></span> <span data-ttu-id="9f315-126">Если обработчик не выполняет никаких действий после этого вызова, используйте **await** ключевое слово, чтобы возобновить выполнение после завершения метода.</span><span class="sxs-lookup"><span data-stu-id="9f315-126">If the handler does any work after this call, use the **await** keyword to resume execution after the method completes.</span></span> <span data-ttu-id="9f315-127">Следующий пример показывает обработчик, который регистрирует коды ошибок.</span><span class="sxs-lookup"><span data-stu-id="9f315-127">The following example shows a handler that logs error codes.</span></span> <span data-ttu-id="9f315-128">Ведение журнала, сам не представляют интереса, но в примере показано, как получать в ответ внутри обработчика.</span><span class="sxs-lookup"><span data-stu-id="9f315-128">The logging itself is not very interesting, but the example shows how to get at the response inside the handler.</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample3.cs?highlight=10,13)]

## <a name="adding-message-handlers-to-the-client-pipeline"></a><span data-ttu-id="9f315-129">Добавление обработчиков сообщений в конвейер клиента</span><span class="sxs-lookup"><span data-stu-id="9f315-129">Adding Message Handlers to the Client Pipeline</span></span>

<span data-ttu-id="9f315-130">Чтобы добавить пользовательские обработчики для **HttpClient**, используйте **HttpClientFactory.Create** метод:</span><span class="sxs-lookup"><span data-stu-id="9f315-130">To add custom handlers to **HttpClient**, use the **HttpClientFactory.Create** method:</span></span>

[!code-csharp[Main](httpclient-message-handlers/samples/sample4.cs)]

<span data-ttu-id="9f315-131">Обработчики сообщений вызываются в порядке, в котором можно передать их в **создать** метод.</span><span class="sxs-lookup"><span data-stu-id="9f315-131">Message handlers are called in the order that you pass them into the **Create** method.</span></span> <span data-ttu-id="9f315-132">Поскольку обработчики являются вложенными, ответное сообщение перемещается в обратном направлении.</span><span class="sxs-lookup"><span data-stu-id="9f315-132">Because handlers are nested, the response message travels in the other direction.</span></span> <span data-ttu-id="9f315-133">Последним обработчиком является первым получает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="9f315-133">That is, the last handler is the first to get the response message.</span></span>
