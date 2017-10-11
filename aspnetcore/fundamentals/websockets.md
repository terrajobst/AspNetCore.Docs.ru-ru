---
title: "Поддержка WebSockets в ASP.NET Core"
author: tdykstra
description: "Дополнительные сведения о начале работы с WebSockets в ASP.NET Core."
keywords: ASP.NET Core, WebSockets
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.assetid: 0e0fedcd-a7b4-4479-8ae0-36eab0229d7e
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 114d52d831668e5facd1142b5f9e5f68e7456f7e
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/01/2017
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="f433c-104">Общие сведения о WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="f433c-104">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="f433c-105">По [Tom Dykstra](https://github.com/tdykstra) и [Эндрю Stanton сиделка](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="f433c-105">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="f433c-106">В этой статье объясняется, как начало работы с WebSockets в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f433c-106">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="f433c-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, позволяющий двусторонний обмен данными постоянно каналы через TCP-подключений.</span><span class="sxs-lookup"><span data-stu-id="f433c-107">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="f433c-108">Он используется для приложений, например чат, каналам биржевых котировок, игры, в любое место в режиме реального времени функциональные возможности веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="f433c-108">It is used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="f433c-109">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="f433c-109">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="f433c-110">В разделе [дальнейшие действия](#next-steps) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="f433c-110">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="f433c-111">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="f433c-111">Prerequisites</span></span>

* <span data-ttu-id="f433c-112">ASP.NET Core 1.1 (не работает на 1.0)</span><span class="sxs-lookup"><span data-stu-id="f433c-112">ASP.NET Core 1.1 (does not run on 1.0)</span></span>
* <span data-ttu-id="f433c-113">Любой ОС, работающей в среде ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="f433c-113">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="f433c-114">Windows 7 или Windows Server 2008 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="f433c-114">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="f433c-115">Linux</span><span class="sxs-lookup"><span data-stu-id="f433c-115">Linux</span></span>
  * <span data-ttu-id="f433c-116">MacOS</span><span class="sxs-lookup"><span data-stu-id="f433c-116">macOS</span></span>

* <span data-ttu-id="f433c-117">**Исключение**: Если ваше приложение запускается в Windows с помощью IIS, или с WebListener, необходимо использовать:</span><span class="sxs-lookup"><span data-stu-id="f433c-117">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="f433c-118">Windows 8 и Windows Server 2012 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="f433c-118">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="f433c-119">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="f433c-119">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="f433c-120">WebSocket необходимо включить в службах IIS</span><span class="sxs-lookup"><span data-stu-id="f433c-120">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="f433c-121">Для поддерживаемых браузерах см. в разделе http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="f433c-121">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="f433c-122">Условия использования</span><span class="sxs-lookup"><span data-stu-id="f433c-122">When to use it</span></span>

<span data-ttu-id="f433c-123">Используйте WebSocket, когда требуется работать непосредственно с подключения к сокету.</span><span class="sxs-lookup"><span data-stu-id="f433c-123">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="f433c-124">Например может потребоваться наилучшую производительность для игр в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="f433c-124">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="f433c-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет всестороннее модель приложения для функций в режиме реального времени, но он работает только на ASP.NET, не ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f433c-125">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="f433c-126">Версия ядра SignalR находится в стадии разработки; ход его выполнения, разделе [репозитории GitHub для SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="f433c-126">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="f433c-127">Если не нужно ждать SignalR Core, вы можете использовать WebSocket прямо сейчас.</span><span class="sxs-lookup"><span data-stu-id="f433c-127">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="f433c-128">Но возможно, потребуется разрабатывать средства обеспечения SignalR бы, например:</span><span class="sxs-lookup"><span data-stu-id="f433c-128">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="f433c-129">Поддержка более широкий спектр версии браузеров с помощью автоматического возврата к транспорта альтернативные методы.</span><span class="sxs-lookup"><span data-stu-id="f433c-129">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="f433c-130">Автоматическое переподключение разрыва подключения.</span><span class="sxs-lookup"><span data-stu-id="f433c-130">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="f433c-131">Поддержка клиентов, вызывающих методы на сервере или наоборот.</span><span class="sxs-lookup"><span data-stu-id="f433c-131">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="f433c-132">Поддержка масштабирования на несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="f433c-132">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="f433c-133">Способ его использования</span><span class="sxs-lookup"><span data-stu-id="f433c-133">How to use it</span></span>

* <span data-ttu-id="f433c-134">Установка [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) пакета.</span><span class="sxs-lookup"><span data-stu-id="f433c-134">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="f433c-135">Настройки по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="f433c-135">Configure the middleware.</span></span>
* <span data-ttu-id="f433c-136">Принимает запросы WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f433c-136">Accept WebSocket requests.</span></span>
* <span data-ttu-id="f433c-137">Отправлять и получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="f433c-137">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="f433c-138">Настройки по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="f433c-138">Configure the middleware</span></span>

<span data-ttu-id="f433c-139">Добавить по промежуточного слоя WebSockets в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="f433c-139">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="f433c-140">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="f433c-140">The following settings can be configured:</span></span>

* <span data-ttu-id="f433c-141">`KeepAliveInterval`-Как часто следует отправлять клиенту, чтобы убедиться, что учетные записи-посредники сохранить открытым подключение «ping» кадры.</span><span class="sxs-lookup"><span data-stu-id="f433c-141">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="f433c-142">`ReceiveBufferSize`— Размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="f433c-142">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="f433c-143">Только опытным пользователям может потребоваться изменить его, для настройки производительности на основе размера свои данные.</span><span class="sxs-lookup"><span data-stu-id="f433c-143">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="f433c-144">Принимать запросы WebSocket</span><span class="sxs-lookup"><span data-stu-id="f433c-144">Accept WebSocket requests</span></span>

<span data-ttu-id="f433c-145">Где-либо более поздней версии в жизненном цикле запроса (Далее в `Configure` метод или действие MVC, например) Проверьте, является ли запрос WebSocket и принять запрос WebSocket.</span><span class="sxs-lookup"><span data-stu-id="f433c-145">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="f433c-146">Этот пример взят из более поздней версии в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="f433c-146">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="f433c-147">Запрос WebSocket могут приходить в любой URL-адрес, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="f433c-147">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="f433c-148">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="f433c-148">Send and receive messages</span></span>

<span data-ttu-id="f433c-149">`AcceptWebSocketAsync` Метод обновляет TCP-соединение до соединения WebSocket и дает [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) объекта.</span><span class="sxs-lookup"><span data-stu-id="f433c-149">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="f433c-150">Используйте объект WebSocket для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="f433c-150">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="f433c-151">Код, приведенный выше, которая принимает запрос WebSocket передает `WebSocket` объект `Echo` метод; вот `Echo` метод.</span><span class="sxs-lookup"><span data-stu-id="f433c-151">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="f433c-152">Код получает сообщение и немедленно отправляет одно сообщение.</span><span class="sxs-lookup"><span data-stu-id="f433c-152">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="f433c-153">Он остается в состоянии цикла, это сделать, пока клиент не закроет подключение.</span><span class="sxs-lookup"><span data-stu-id="f433c-153">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="f433c-154">Если принять WebSocket до начала этого цикла, завершает конвейера по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="f433c-154">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="f433c-155">После закрытия сокета, освобождает конвейера.</span><span class="sxs-lookup"><span data-stu-id="f433c-155">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="f433c-156">То есть запрос останавливается, продвижение вперед в конвейере при принятии WebSocket, как это будет при попадании действие MVC, например.</span><span class="sxs-lookup"><span data-stu-id="f433c-156">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="f433c-157">Однако по завершении этого цикла и закрыть сокет, запрос выполняется резервное копирование конвейера.</span><span class="sxs-lookup"><span data-stu-id="f433c-157">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="f433c-158">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="f433c-158">Next steps</span></span>

<span data-ttu-id="f433c-159">[Образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , сопровождающий это статья — это простые эхо приложение.</span><span class="sxs-lookup"><span data-stu-id="f433c-159">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="f433c-160">Он имеет веб-страницы, которая устанавливает соединения WebSocket, и сервер просто повторно отправляет клиенту все сообщения, которые он получает.</span><span class="sxs-lookup"><span data-stu-id="f433c-160">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="f433c-161">Запустите его из командной строки (она не включена для запуска из Visual Studio с IIS Express) и перейдите к http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="f433c-161">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="f433c-162">Веб-странице показано состояние подключения в верхнем левом углу.</span><span class="sxs-lookup"><span data-stu-id="f433c-162">The web page shows connection status at the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="f433c-164">Выберите **Connect** отправить запрос WebSocket показано URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="f433c-164">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="f433c-165">Введите тестовое сообщение и выберите **отправки**.</span><span class="sxs-lookup"><span data-stu-id="f433c-165">Enter a test message and select **Send**.</span></span> <span data-ttu-id="f433c-166">После этого выберите **закрыть сокет**.</span><span class="sxs-lookup"><span data-stu-id="f433c-166">When done, select **Close Socket**.</span></span> <span data-ttu-id="f433c-167">**Журнала связи** раздел сообщает каждого открытия, отправки и действие закрытия, как это происходит.</span><span class="sxs-lookup"><span data-stu-id="f433c-167">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)
