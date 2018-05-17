---
title: Поддержка WebSockets в ASP.NET Core
author: tdykstra
description: Сведения о начале работы с WebSocket в ASP.NET Core.
manager: wpickett
ms.author: tdykstra
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/websockets
ms.openlocfilehash: e744ab5b81ff85f48edb012a86b55003cc74929c
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/22/2018
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="aa4a5-103">Поддержка WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="aa4a5-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="aa4a5-104">Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)</span><span class="sxs-lookup"><span data-stu-id="aa4a5-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="aa4a5-105">Эта статья описывает начало работы с WebSocket в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="aa4a5-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="aa4a5-107">Он используется в приложениях, где нужна быстрая связь в режиме реального времени, например в чатах, панелях мониторинга и играх.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="aa4a5-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([описание скачивания](xref:tutorials/index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample)).</span></span> <span data-ttu-id="aa4a5-109">Дополнительные сведения см. в разделе [Следующие шаги](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="aa4a5-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="aa4a5-110">Prerequisites</span></span>

* <span data-ttu-id="aa4a5-111">ASP.NET Core 1.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="aa4a5-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="aa4a5-112">Любая ОС с поддержкой ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="aa4a5-113">Windows 7/Windows Server 2008 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="aa4a5-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="aa4a5-114">Linux</span><span class="sxs-lookup"><span data-stu-id="aa4a5-114">Linux</span></span>
  * <span data-ttu-id="aa4a5-115">macOS</span><span class="sxs-lookup"><span data-stu-id="aa4a5-115">macOS</span></span>
  
* <span data-ttu-id="aa4a5-116">Если приложение выполняется в Windows с IIS:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="aa4a5-117">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="aa4a5-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="aa4a5-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="aa4a5-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="aa4a5-119">В службах IIS необходимо включить WebSockets (см. раздел [Поддержка IIS и IIS Express](#iisiis-express-support).)</span><span class="sxs-lookup"><span data-stu-id="aa4a5-119">WebSockets must be enabled in IIS (See the [IIS/IIS Express support](#iisiis-express-support) section.)</span></span>
  
* <span data-ttu-id="aa4a5-120">Если приложение выполняется в [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="aa4a5-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="aa4a5-121">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="aa4a5-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="aa4a5-122">Список поддерживаемых обозревателей см. на странице https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="aa4a5-123">Условия использования WebSocket</span><span class="sxs-lookup"><span data-stu-id="aa4a5-123">When to use WebSockets</span></span>

<span data-ttu-id="aa4a5-124">Используйте WebSocket для работы с подключением через сокет напрямую.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="aa4a5-125">Например, можно использовать WebSocket для оптимальной производительности игры в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="aa4a5-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) предоставляет расширенную модель приложения для функций реального времени, однако работает только в ASP.NET 4.x, но не в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-126">[ASP.NET SignalR](/aspnet/signalr/overview/getting-started/introduction-to-signalr) provides a richer app model for real-time functionality, but it only runs on ASP.NET 4.x, not ASP.NET Core.</span></span> <span data-ttu-id="aa4a5-127">Версия SignalR в ASP.NET Core будет выпущена в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-127">An ASP.NET Core version of SignalR is scheduled for release with ASP.NET Core 2.1.</span></span> <span data-ttu-id="aa4a5-128">См. [Планирование высокого уровня в ASP.NET Core 2.1](https://github.com/aspnet/Announcements/issues/288).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-128">See [ASP.NET Core 2.1 high-level planning](https://github.com/aspnet/Announcements/issues/288).</span></span>

<span data-ttu-id="aa4a5-129">До выпуска SignalR Core можно использовать WebSocket.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-129">Until SignalR Core is released, WebSockets can be used.</span></span> <span data-ttu-id="aa4a5-130">Но функции, предоставляемые SignalR, должны предоставляться и поддерживаться разработчиком.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-130">However, features that SignalR provides must be provided and supported by the developer.</span></span> <span data-ttu-id="aa4a5-131">Пример:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-131">For example:</span></span>

* <span data-ttu-id="aa4a5-132">Поддержка расширенного набора версий браузеров с автоматическим возвратом к альтернативным методам передачи.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-132">Support for a broader range of browser versions by using automatic fallback to alternative transport methods.</span></span>
* <span data-ttu-id="aa4a5-133">Автоматическое переподключение в случае разрыва соединения.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-133">Automatic reconnection when a connection drops.</span></span>
* <span data-ttu-id="aa4a5-134">Поддержка методов вызова клиента на сервере или наоборот.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-134">Support for clients calling methods on the server or vice versa.</span></span>
* <span data-ttu-id="aa4a5-135">Поддержка масштабирования до нескольких серверов.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-135">Support for scaling to multiple servers.</span></span>

## <a name="how-to-use-it"></a><span data-ttu-id="aa4a5-136">Использование</span><span class="sxs-lookup"><span data-stu-id="aa4a5-136">How to use it</span></span>

* <span data-ttu-id="aa4a5-137">Установка пакета [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-137">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="aa4a5-138">Настройка ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-138">Configure the middleware.</span></span>
* <span data-ttu-id="aa4a5-139">Принятие запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-139">Accept WebSocket requests.</span></span>
* <span data-ttu-id="aa4a5-140">Отправка и получение сообщений.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-140">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="aa4a5-141">Настройка ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="aa4a5-141">Configure the middleware</span></span>

<span data-ttu-id="aa4a5-142">Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-142">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSockets)]

<span data-ttu-id="aa4a5-143">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-143">The following settings can be configured:</span></span>

* <span data-ttu-id="aa4a5-144">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-144">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="aa4a5-145">`ReceiveBufferSize` — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-145">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="aa4a5-146">Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-146">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=UseWebSocketsOptions)]

### <a name="accept-websocket-requests"></a><span data-ttu-id="aa4a5-147">Принятие запросов WebSocket</span><span class="sxs-lookup"><span data-stu-id="aa4a5-147">Accept WebSocket requests</span></span>

<span data-ttu-id="aa4a5-148">На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или действии MVC) проверьте, относится ли запрос к WebSocket, и примите его.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-148">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="aa4a5-149">Следующий пример взят из дальнейшей части метода `Configure`:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-149">The following example is from later in the `Configure` method:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=AcceptWebSocket&highlight=7)]

<span data-ttu-id="aa4a5-150">Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-150">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="aa4a5-151">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="aa4a5-151">Send and receive messages</span></span>

<span data-ttu-id="aa4a5-152">Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет объект [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-152">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="aa4a5-153">Используйте объект `WebSocket` для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-153">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="aa4a5-154">Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo`.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-154">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="aa4a5-155">Код принимает сообщение и сразу отправляет такое же сообщение обратно.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-155">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="aa4a5-156">Сообщения отправляются и получаются циклически, пока клиент не закроет подключение:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-156">Messages are sent and received in a loop until the client closes the connection:</span></span>

[!code-csharp[](websockets/sample/Startup.cs?name=Echo)]

<span data-ttu-id="aa4a5-157">Если вы принимаете подключение WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-157">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="aa4a5-158">После закрытия сокета конвейер развертывается.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-158">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="aa4a5-159">То есть запрос перестает перемещаться по конвейеру после принятия WebSocket.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-159">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="aa4a5-160">После завершения цикла и закрытия сокета запрос возвращается в конвейер.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-160">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="aa4a5-161">Поддержка служб IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="aa4a5-161">IIS/IIS Express support</span></span>

<span data-ttu-id="aa4a5-162">Windows Server 2012 или более поздней версии и Windows 8 или более поздней версии с IIS и IIS Express 8 или более поздней версии поддерживают протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-162">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

<span data-ttu-id="aa4a5-163">Чтобы включить поддержку протокола WebSocket в Windows Server 2012 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-163">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

1. <span data-ttu-id="aa4a5-164">В меню **Управление** запустите мастер **Добавить роли и компоненты** или в окне **Диспетчер серверов** щелкните соответствующую ссылку.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-164">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="aa4a5-165">Выберите **Установка ролей или компонентов**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-165">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="aa4a5-166">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-166">Select **Next**.</span></span>
1. <span data-ttu-id="aa4a5-167">Выберите подходящий сервер (по умолчанию выбирается локальный сервер).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-167">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="aa4a5-168">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-168">Select **Next**.</span></span>
1. <span data-ttu-id="aa4a5-169">Разверните **Веб-сервер (IIS)** в дереве **Роли**, разверните **Веб-сервер**, а затем **Разработка приложений**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-169">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="aa4a5-170">Выберите **протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-170">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="aa4a5-171">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-171">Select **Next**.</span></span>
1. <span data-ttu-id="aa4a5-172">Если дополнительные функции не требуются, нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-172">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="aa4a5-173">Нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-173">Select **Install**.</span></span>
1. <span data-ttu-id="aa4a5-174">По завершении установки выберите **Закрыть**, чтобы выйти из мастера.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-174">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="aa4a5-175">Чтобы включить поддержку протокола WebSocket в Windows 8 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-175">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

1. <span data-ttu-id="aa4a5-176">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-176">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="aa4a5-177">Откройте следующе узлы: **IIS** > **Службы Интернета** > **Компоненты разработки приложений**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-177">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="aa4a5-178">Выберите компонент **Протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-178">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="aa4a5-179">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-179">Select **OK**.</span></span>

<span data-ttu-id="aa4a5-180">**Отключите WebSocket при использовании socket.io на node.js**</span><span class="sxs-lookup"><span data-stu-id="aa4a5-180">**Disable WebSocket when using socket.io on node.js**</span></span>

<span data-ttu-id="aa4a5-181">Если используется поддержка WebSocket в [socket.io](https://socket.io/) на [Node.js](https://nodejs.org/), отключите модуль WebSocket IIS по умолчанию с помощью элемента `webSocket` в *web.config* или *applicationHost.config*. Если не выполнить этот шаг, модуль IIS WebSocket попытается обработать соединение WebSocket, а не Node.js и приложение.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-181">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="aa4a5-182">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="aa4a5-182">Next steps</span></span>

<span data-ttu-id="aa4a5-183">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) в этой статье — это эхо-приложение.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-183">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/sample) that accompanies this article is an echo app.</span></span> <span data-ttu-id="aa4a5-184">Оно имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер перенаправляет все полученные сообщения обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-184">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="aa4a5-185">Запустите приложение из командной строки (оно не предназначено для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-185">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="aa4a5-186">В верхнем левом углу веб-страницы отображается состояние подключения:</span><span class="sxs-lookup"><span data-stu-id="aa4a5-186">The web page shows the connection status in the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="aa4a5-188">Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-188">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="aa4a5-189">Введите тестовое сообщение и выберите **Send** (Отправить).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-189">Enter a test message and select **Send**.</span></span> <span data-ttu-id="aa4a5-190">После этого выберите **Close Socket** (Закрыть сокет).</span><span class="sxs-lookup"><span data-stu-id="aa4a5-190">When done, select **Close Socket**.</span></span> <span data-ttu-id="aa4a5-191">В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.</span><span class="sxs-lookup"><span data-stu-id="aa4a5-191">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)
