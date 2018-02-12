---
title: "Поддержка WebSockets в ASP.NET Core"
author: tdykstra
description: "Сведения о начале работы с WebSocket в ASP.NET Core."
manager: wpickett
ms.author: tdykstra
ms.date: 03/25/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: 306eca28b9f1f66e1ccaf185ccae87db8dea1b01
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/30/2018
---
# <a name="introduction-to-websockets-in-aspnet-core"></a><span data-ttu-id="0ffee-103">Общие сведения о WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0ffee-103">Introduction to WebSockets in ASP.NET Core</span></span>

<span data-ttu-id="0ffee-104">Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)</span><span class="sxs-lookup"><span data-stu-id="0ffee-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="0ffee-105">Эта статья описывает начало работы с WebSocket в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0ffee-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="0ffee-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="0ffee-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="0ffee-107">Он используется для таких приложений, как чаты, биржевые тикеры, игры, а также везде, где в веб-приложении нужны функции реального времени.</span><span class="sxs-lookup"><span data-stu-id="0ffee-107">It's used for applications such as chat, stock tickers, games, anywhere you want real-time functionality in a web application.</span></span>

<span data-ttu-id="0ffee-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="0ffee-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="0ffee-109">Дополнительные сведения см. в разделе [Следующие шаги](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="0ffee-109">See the [Next Steps](#next-steps) section for more information.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="0ffee-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="0ffee-110">Prerequisites</span></span>

* <span data-ttu-id="0ffee-111">ASP.NET Core 1.1 (не запускается в версии 1.0)</span><span class="sxs-lookup"><span data-stu-id="0ffee-111">ASP.NET Core 1.1 (doesn't run on 1.0)</span></span>
* <span data-ttu-id="0ffee-112">Любая ОС, где работает ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0ffee-112">Any OS that ASP.NET Core runs on:</span></span>
  
  * <span data-ttu-id="0ffee-113">Windows 7/Windows Server 2008 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="0ffee-113">Windows 7 / Windows Server 2008 and later</span></span>
  * <span data-ttu-id="0ffee-114">Linux</span><span class="sxs-lookup"><span data-stu-id="0ffee-114">Linux</span></span>
  * <span data-ttu-id="0ffee-115">macOS</span><span class="sxs-lookup"><span data-stu-id="0ffee-115">macOS</span></span>

* <span data-ttu-id="0ffee-116">**Исключение**: если ваше приложение выполняется в Windows с использованием IIS или WebListener, нужно использовать:</span><span class="sxs-lookup"><span data-stu-id="0ffee-116">**Exception**: If your app runs on Windows with IIS, or with WebListener, you must use:</span></span>

  * <span data-ttu-id="0ffee-117">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="0ffee-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="0ffee-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="0ffee-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="0ffee-119">Требуется включить WebSocket в IIS</span><span class="sxs-lookup"><span data-stu-id="0ffee-119">WebSocket must be enabled in IIS</span></span>

* <span data-ttu-id="0ffee-120">Поддерживаемые браузеры см. на странице http://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="0ffee-120">For supported browsers, see http://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-it"></a><span data-ttu-id="0ffee-121">Условия использования</span><span class="sxs-lookup"><span data-stu-id="0ffee-121">When to use it</span></span>

<span data-ttu-id="0ffee-122">Используйте WebSocket, когда требуется работать непосредственно с подключением через сокет.</span><span class="sxs-lookup"><span data-stu-id="0ffee-122">Use WebSockets when you need to work directly with a socket connection.</span></span> <span data-ttu-id="0ffee-123">Например, вам может потребоваться обеспечить наилучшую производительность для игры в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="0ffee-123">For example, you might need the best possible performance for a real-time game.</span></span>

<span data-ttu-id="0ffee-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет расширенную модель приложения для функций реального времени, однако работает только в ASP.NET, но не в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="0ffee-124">[ASP.NET SignalR](https://docs.microsoft.com/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer application model for real-time functionality, but it runs only on ASP.NET, not ASP.NET Core.</span></span> <span data-ttu-id="0ffee-125">Версия SignalR Core еще разрабатывается. Проследить за ходом разработки можно в [репозитории GitHub для SignalR Core](https://github.com/aspnet/SignalR).</span><span class="sxs-lookup"><span data-stu-id="0ffee-125">A Core version of SignalR is under development; to follow its progress, see the [GitHub repository for SignalR Core](https://github.com/aspnet/SignalR).</span></span>

<span data-ttu-id="0ffee-126">Если вы не хотите ждать SignalR Core, можете использовать WebSocket уже сейчас.</span><span class="sxs-lookup"><span data-stu-id="0ffee-126">If you don't want to wait for SignalR Core, you can use WebSockets directly now.</span></span> <span data-ttu-id="0ffee-127">Однако вам может потребоваться разработать функции, которые будет предоставлять SignalR, например:</span><span class="sxs-lookup"><span data-stu-id="0ffee-127">But you might have to develop features that SignalR would provide, such as:</span></span>

* <span data-ttu-id="0ffee-128">Поддержка расширенного набора версий браузеров с автоматическим возвратом к альтернативным методам передачи.</span><span class="sxs-lookup"><span data-stu-id="0ffee-128">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="0ffee-129">Автоматическое переподключение в случае разрыва соединения.</span><span class="sxs-lookup"><span data-stu-id="0ffee-129">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="0ffee-130">Поддержка методов вызова клиента на сервере или наоборот.</span><span class="sxs-lookup"><span data-stu-id="0ffee-130">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="0ffee-131">Поддержка масштабирования до нескольких серверов.</span><span class="sxs-lookup"><span data-stu-id="0ffee-131">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="0ffee-132">Использование</span><span class="sxs-lookup"><span data-stu-id="0ffee-132">How to use it</span></span>

* <span data-ttu-id="0ffee-133">Установка пакета [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="0ffee-133">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="0ffee-134">Настройка ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="0ffee-134">Configure the middleware.</span></span>
* <span data-ttu-id="0ffee-135">Принятие запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="0ffee-135">Accept WebSocket requests.</span></span>
* <span data-ttu-id="0ffee-136">Отправка и получение сообщений.</span><span class="sxs-lookup"><span data-stu-id="0ffee-136">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="0ffee-137">Настройка ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="0ffee-137">Configure the middleware</span></span>

<span data-ttu-id="0ffee-138">Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`.</span><span class="sxs-lookup"><span data-stu-id="0ffee-138">Add the WebSockets middleware in the `Configure` method of the `Startup` class.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="0ffee-139">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="0ffee-139">The following settings can be configured:</span></span>

* <span data-ttu-id="0ffee-140">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="0ffee-140">`KeepAliveInterval` - How frequently to send "ping" frames to the client, to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="0ffee-141">`ReceiveBufferSize` — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="0ffee-141">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="0ffee-142">Изменение этого параметра может потребоваться лишь опытным пользователям и позволяет настроить производительность с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="0ffee-142">Only advanced users would need to change this, for performance tuning based on the size of their data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="0ffee-143">Принятие запросов WebSocket</span><span class="sxs-lookup"><span data-stu-id="0ffee-143">Accept WebSocket requests</span></span>

<span data-ttu-id="0ffee-144">На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или действии MVC) проверьте, относится ли запрос к WebSocket, и примите его.</span><span class="sxs-lookup"><span data-stu-id="0ffee-144">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="0ffee-145">Этот пример взят из дальнейшей части метода `Configure`.</span><span class="sxs-lookup"><span data-stu-id="0ffee-145">This example is from later in the `Configure` method.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="0ffee-146">Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="0ffee-146">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="0ffee-147">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="0ffee-147">Send and receive messages</span></span>

<span data-ttu-id="0ffee-148">Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет вам объект [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="0ffee-148">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and gives you a [WebSocket](https://docs.microsoft.com/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="0ffee-149">Используйте этот объект для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="0ffee-149">Use the WebSocket object to send and receive messages.</span></span>

<span data-ttu-id="0ffee-150">Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo` (здесь это именно метод `Echo`).</span><span class="sxs-lookup"><span data-stu-id="0ffee-150">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method; here's the `Echo` method.</span></span> <span data-ttu-id="0ffee-151">Код принимает сообщение и сразу отправляет такое же сообщение обратно.</span><span class="sxs-lookup"><span data-stu-id="0ffee-151">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="0ffee-152">Он выполняет этот цикл, пока клиент не закроет соединение.</span><span class="sxs-lookup"><span data-stu-id="0ffee-152">It stays in a loop doing that until the client closes the connection.</span></span> 

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="0ffee-153">Если вы принимаете WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается.</span><span class="sxs-lookup"><span data-stu-id="0ffee-153">When you accept the WebSocket before beginning this loop, the middleware pipeline ends.</span></span>  <span data-ttu-id="0ffee-154">После закрытия сокета конвейер развертывается.</span><span class="sxs-lookup"><span data-stu-id="0ffee-154">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="0ffee-155">То есть при принятии WebSocket запрос прекращает продвигаться вперед по конвейеру, как это случается, например, при попадании на действие MVC.</span><span class="sxs-lookup"><span data-stu-id="0ffee-155">That is, the request stops moving forward in the pipeline when you accept a WebSocket, just as it would when you hit an MVC action, for example.</span></span>  <span data-ttu-id="0ffee-156">Однако когда вы завершите этот цикл и закроете сокет, запрос продолжит движение по конвейеру.</span><span class="sxs-lookup"><span data-stu-id="0ffee-156">But when you finish this loop and close the socket, the request proceeds back up the pipeline.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0ffee-157">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="0ffee-157">Next steps</span></span>

<span data-ttu-id="0ffee-158">К этой статье прилагается [пример](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) в виде простого приложения вывода на экран.</span><span class="sxs-lookup"><span data-stu-id="0ffee-158">The [sample application](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is a simple echo application.</span></span> <span data-ttu-id="0ffee-159">Он имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер просто перенаправляет клиенту все полученные сообщения.</span><span class="sxs-lookup"><span data-stu-id="0ffee-159">It has a web page that makes WebSocket connections, and the server just resends back to the client any messages it receives.</span></span> <span data-ttu-id="0ffee-160">Запустите его из командной строки (он не предназначен для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="0ffee-160">Run it from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="0ffee-161">В верхнем левом углу веб-страницы отображается состояние подключения:</span><span class="sxs-lookup"><span data-stu-id="0ffee-161">The web page shows connection status at the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="0ffee-163">Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="0ffee-163">Select **Connect** to send a WebSocket request to the URL shown.</span></span>  <span data-ttu-id="0ffee-164">Введите тестовое сообщение и выберите **Send** (Отправить).</span><span class="sxs-lookup"><span data-stu-id="0ffee-164">Enter a test message and select **Send**.</span></span> <span data-ttu-id="0ffee-165">После этого выберите **Close Socket** (Закрыть сокет).</span><span class="sxs-lookup"><span data-stu-id="0ffee-165">When done, select **Close Socket**.</span></span> <span data-ttu-id="0ffee-166">В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.</span><span class="sxs-lookup"><span data-stu-id="0ffee-166">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)
