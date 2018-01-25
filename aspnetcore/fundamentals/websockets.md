---
title: "Поддержка WebSockets в ASP.NET Core"
author: tdykstra
description: "Дополнительные сведения о начале работы с WebSockets в ASP.NET Core."
ms.author: tdykstra
manager: wpickett
ms.date: 03/25/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: fundamentals/websockets
ms.openlocfilehash: 6f335376c72cd0c68f4667cf0e661a25bf448980
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="51bd3-103">Общие сведения о WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="51bd3-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="51bd3-104">По [Tom Dykstra](https://github.com/tdykstra) и [Эндрю Stanton сиделка](https://github.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="51bd3-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="51bd3-105">В этой статье объясняется, как начало работы с WebSockets в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="51bd3-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="51bd3-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="51bd3-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="51bd3-107">Он используется для приложений, например чат, каналам биржевых котировок, игры, в любое место в режиме реального времени функциональные возможности веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="51bd3-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="51bd3-108">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([загрузке](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="51bd3-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="51bd3-109">В разделе [дальнейшие действия](#next-steps) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="51bd3-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="51bd3-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="51bd3-110">Prerequisites</span></span>

* <span data-ttu-id="51bd3-111">ASP.NET Core 1.1 (не запускается на 1.0)</span><span class="sxs-lookup"><span data-stu-id="51bd3-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="51bd3-112">Любой ОС, работающей в среде ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="51bd3-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="51bd3-113">Windows 7 или Windows Server 2008 и более поздних версий</span><span class="sxs-lookup"><span data-stu-id="51bd3-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="51bd3-114">Linux</span><span class="sxs-lookup"><span data-stu-id="51bd3-114">Linux</span></span>
  * <span data-ttu-id="51bd3-115">MacOS</span><span class="sxs-lookup"><span data-stu-id="51bd3-115">macOS</span></span>

* <span data-ttu-id="51bd3-116">**Исключение**: Если ваше приложение запускается в Windows с помощью IIS, или с WebListener, необходимо использовать:</span><span class="sxs-lookup"><span data-stu-id="51bd3-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="51bd3-117">Windows 8 и Windows Server 2012 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="51bd3-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="51bd3-118">IIS 8 / IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="51bd3-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="51bd3-119">WebSocket необходимо включить в службах IIS</span><span class="sxs-lookup"><span data-stu-id="51bd3-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="51bd3-120">Для поддерживаемых браузерах см. в разделе http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="51bd3-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="51bd3-121">Условия использования</span><span class="sxs-lookup"><span data-stu-id="51bd3-121">When to use it</span></span>

<span data-ttu-id="51bd3-122">Используйте WebSocket, когда требуется работать непосредственно с подключения к сокету.</span><span class="sxs-lookup"><span data-stu-id="51bd3-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="51bd3-123">Например может потребоваться наилучшую производительность для игр в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="51bd3-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="51bd3-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет всестороннее модель приложения для функций в режиме реального времени, но он работает только на ASP.NET, не ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="51bd3-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="51bd3-125">Версия ядра SignalR находится в стадии разработки; ход его выполнения, разделе [репозитории GitHub для SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="51bd3-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="51bd3-126">Если не нужно ждать SignalR Core, вы можете использовать WebSocket прямо сейчас.</span><span class="sxs-lookup"><span data-stu-id="51bd3-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="51bd3-127">Но возможно, потребуется разрабатывать средства обеспечения SignalR бы, например:</span><span class="sxs-lookup"><span data-stu-id="51bd3-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="51bd3-128">Поддержка более широкий спектр версии браузеров с помощью автоматического возврата к транспорта альтернативные методы.</span><span class="sxs-lookup"><span data-stu-id="51bd3-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="51bd3-129">Автоматическое переподключение разрыва подключения.</span><span class="sxs-lookup"><span data-stu-id="51bd3-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="51bd3-130">Поддержка клиентов, вызывающих методы на сервере или наоборот.</span><span class="sxs-lookup"><span data-stu-id="51bd3-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="51bd3-131">Поддержка масштабирования на несколько серверов.</span><span class="sxs-lookup"><span data-stu-id="51bd3-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="51bd3-132">Использование</span><span class="sxs-lookup"><span data-stu-id="51bd3-132">How to use it</span></span>

* <span data-ttu-id="51bd3-133">Установка [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) пакета.</span><span class="sxs-lookup"><span data-stu-id="51bd3-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="51bd3-134">Настройки по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="51bd3-134">Configure the middleware.</span></span>
* <span data-ttu-id="51bd3-135">Принимает запросы WebSocket.</span><span class="sxs-lookup"><span data-stu-id="51bd3-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="51bd3-136">Отправлять и получать сообщения.</span><span class="sxs-lookup"><span data-stu-id="51bd3-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="51bd3-137">Настройки по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="51bd3-137">Configure the middleware</span></span>

<span data-ttu-id="51bd3-138">Добавить по промежуточного слоя WebSockets в `Configure` метод `Startup` класса.</span><span class="sxs-lookup"><span data-stu-id="51bd3-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="51bd3-139">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="51bd3-139">The following settings can be configured:</span></span>

* <span data-ttu-id="51bd3-140">`KeepAliveInterval`-Как часто следует отправлять клиенту, чтобы убедиться, что учетные записи-посредники сохранить открытым подключение «ping» кадры.</span><span class="sxs-lookup"><span data-stu-id="51bd3-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="51bd3-141">`ReceiveBufferSize`— Размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="51bd3-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="51bd3-142">Только опытным пользователям может потребоваться изменить его, для настройки производительности на основе размера свои данные.</span><span class="sxs-lookup"><span data-stu-id="51bd3-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="51bd3-143">Принимать запросы WebSocket</span><span class="sxs-lookup"><span data-stu-id="51bd3-143">Accept WebSocket requests</span></span>

<span data-ttu-id="51bd3-144">Где-либо более поздней версии в жизненном цикле запроса (Далее в `Configure` метод или действие MVC, например) Проверьте, является ли запрос WebSocket и принять запрос WebSocket.</span><span class="sxs-lookup"><span data-stu-id="51bd3-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="51bd3-145">Этот пример взят из более поздней версии в `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="51bd3-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="51bd3-146">Запрос WebSocket могут приходить в любой URL-адрес, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="51bd3-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="51bd3-147">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="51bd3-147">Send and receive messages</span></span>

<span data-ttu-id="51bd3-148">`AcceptWebSocketAsync` Метод обновляет TCP-соединение до соединения WebSocket и дает [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) объекта.</span><span class="sxs-lookup"><span data-stu-id="51bd3-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="51bd3-149">Используйте объект WebSocket для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="51bd3-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="51bd3-150">Код, приведенный выше, которая принимает запрос WebSocket передает `WebSocket` объект `Echo` метод; вот `Echo` метод.</span><span class="sxs-lookup"><span data-stu-id="51bd3-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="51bd3-151">Код получает сообщение и немедленно отправляет одно сообщение.</span><span class="sxs-lookup"><span data-stu-id="51bd3-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="51bd3-152">Он остается в состоянии цикла, это сделать, пока клиент не закроет подключение.</span><span class="sxs-lookup"><span data-stu-id="51bd3-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="51bd3-153">Если принять WebSocket до начала этого цикла, завершает конвейера по промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="51bd3-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="51bd3-154">После закрытия сокета, освобождает конвейера.</span><span class="sxs-lookup"><span data-stu-id="51bd3-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="51bd3-155">То есть запрос останавливается, продвижение вперед в конвейере при принятии WebSocket, как это будет при попадании действие MVC, например.</span><span class="sxs-lookup"><span data-stu-id="51bd3-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="51bd3-156">Однако по завершении этого цикла и закрыть сокет, запрос выполняется резервное копирование конвейера.</span><span class="sxs-lookup"><span data-stu-id="51bd3-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="51bd3-157">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="51bd3-157">Next steps</span></span>

<span data-ttu-id="51bd3-158">[Образец приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) , сопровождающий это статья — это простые эхо приложение.</span><span class="sxs-lookup"><span data-stu-id="51bd3-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="51bd3-159">Он имеет веб-страницы, которая устанавливает соединения WebSocket, и сервер просто повторно отправляет клиенту все сообщения, которые он получает.</span><span class="sxs-lookup"><span data-stu-id="51bd3-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="51bd3-160">Запустите его из командной строки (она не включена для запуска из Visual Studio с IIS Express) и перейдите к http://localhost: 5000.</span><span class="sxs-lookup"><span data-stu-id="51bd3-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="51bd3-161">Веб-странице показано состояние подключения в верхнем левом углу.</span><span class="sxs-lookup"><span data-stu-id="51bd3-161">The web page shows connection status at the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="51bd3-163">Выберите **Connect** отправить запрос WebSocket показано URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="51bd3-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="51bd3-164">Введите тестовое сообщение и выберите **отправки**.</span><span class="sxs-lookup"><span data-stu-id="51bd3-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="51bd3-165">После этого выберите **закрыть сокет**.</span><span class="sxs-lookup"><span data-stu-id="51bd3-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="51bd3-166">**Журнала связи** раздел сообщает каждого открытия, отправки и действие закрытия, как это происходит.</span><span class="sxs-lookup"><span data-stu-id="51bd3-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)
