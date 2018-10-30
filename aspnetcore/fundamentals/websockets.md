---
title: Поддержка WebSockets в ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с WebSocket в ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/28/2018
uid: fundamentals/websockets
ms.openlocfilehash: b0f1aeff6c7a5777993459274293ba23f2d9dc12
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206744"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="e8170-103">Поддержка WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e8170-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="e8170-104">Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)</span><span class="sxs-lookup"><span data-stu-id="e8170-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="e8170-105">Эта статья описывает начало работы с WebSocket в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="e8170-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="e8170-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="e8170-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="e8170-107">Он используется в приложениях, где нужна быстрая связь в режиме реального времени, например в чатах, панелях мониторинга и играх.</span><span class="sxs-lookup"><span data-stu-id="e8170-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="e8170-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="e8170-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="e8170-109">Дополнительные сведения см. в разделе [Следующие шаги](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="e8170-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e8170-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="e8170-110">Prerequisites</span></span>

* <span data-ttu-id="e8170-111">ASP.NET Core 1.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="e8170-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="e8170-112">Любая ОС с поддержкой ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="e8170-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="e8170-113">Windows 7/Windows Server 2008 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="e8170-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="e8170-114">Linux</span><span class="sxs-lookup"><span data-stu-id="e8170-114">Linux</span></span>
  * <span data-ttu-id="e8170-115">macOS</span><span class="sxs-lookup"><span data-stu-id="e8170-115">macOS</span></span>
  
* <span data-ttu-id="e8170-116">Если приложение выполняется в Windows с IIS:</span><span class="sxs-lookup"><span data-stu-id="e8170-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="e8170-117">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="e8170-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="e8170-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="e8170-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="e8170-119">Необходимо включить WebSockets (см. раздел [Поддержка IIS и IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="e8170-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="e8170-120">Если приложение выполняется в [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="e8170-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="e8170-121">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="e8170-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="e8170-122">Список поддерживаемых обозревателей см. на странице https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="e8170-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="e8170-123">Условия использования WebSocket</span><span class="sxs-lookup"><span data-stu-id="e8170-123">When to use WebSockets</span></span>

<span data-ttu-id="e8170-124">Используйте WebSocket для работы с подключением через сокет напрямую.</span><span class="sxs-lookup"><span data-stu-id="e8170-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="e8170-125">Например, можно использовать WebSocket для оптимальной производительности игры в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="e8170-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="e8170-126">[ASP.NET Core SignalR](xref:signalr/introduction) — это библиотека, которая упрощает добавление веб-функций в приложения в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="e8170-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="e8170-127">Она использует WebSocket, когда это возможно.</span><span class="sxs-lookup"><span data-stu-id="e8170-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="e8170-128">Как использовать WebSocket</span><span class="sxs-lookup"><span data-stu-id="e8170-128">How to use WebSockets</span></span>

* <span data-ttu-id="e8170-129">Установка пакета [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="e8170-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="e8170-130">Настройка ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="e8170-130">Configure the middleware.</span></span>
* <span data-ttu-id="e8170-131">Принятие запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e8170-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="e8170-132">Отправка и получение сообщений.</span><span class="sxs-lookup"><span data-stu-id="e8170-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="e8170-133">Настройка ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="e8170-133">Configure the middleware</span></span>

<span data-ttu-id="e8170-134">Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="e8170-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

<span data-ttu-id="e8170-135">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="e8170-135">The following settings can be configured:</span></span>

* <span data-ttu-id="e8170-136">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="e8170-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span>
* <span data-ttu-id="e8170-137">`ReceiveBufferSize` — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="e8170-137">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="e8170-138">Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="e8170-138">Advanced users may need to change this for performance tuning based on the size of the data.</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="e8170-139">Принятие запросов WebSocket</span><span class="sxs-lookup"><span data-stu-id="e8170-139">Accept WebSocket requests</span></span>

<span data-ttu-id="e8170-140">На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или действии MVC) проверьте, относится ли запрос к WebSocket, и примите его.</span><span class="sxs-lookup"><span data-stu-id="e8170-140">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="e8170-141">Следующий пример взят из дальнейшей части метода `Configure`:</span><span class="sxs-lookup"><span data-stu-id="e8170-141">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="e8170-142">Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="e8170-142">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="e8170-143">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="e8170-143">Send and receive messages</span></span>

<span data-ttu-id="e8170-144">Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет объект [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="e8170-144">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="e8170-145">Используйте объект `WebSocket` для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="e8170-145">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="e8170-146">Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo`.</span><span class="sxs-lookup"><span data-stu-id="e8170-146">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="e8170-147">Код принимает сообщение и сразу отправляет такое же сообщение обратно.</span><span class="sxs-lookup"><span data-stu-id="e8170-147">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="e8170-148">Сообщения отправляются и получаются циклически, пока клиент не закроет подключение:</span><span class="sxs-lookup"><span data-stu-id="e8170-148">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="e8170-149">Если вы принимаете подключение WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается.</span><span class="sxs-lookup"><span data-stu-id="e8170-149">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="e8170-150">После закрытия сокета конвейер развертывается.</span><span class="sxs-lookup"><span data-stu-id="e8170-150">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="e8170-151">То есть запрос перестает перемещаться по конвейеру после принятия WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e8170-151">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="e8170-152">После завершения цикла и закрытия сокета запрос возвращается в конвейер.</span><span class="sxs-lookup"><span data-stu-id="e8170-152">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

## <a name="iisiis-express-support"></a><span data-ttu-id="e8170-153">Поддержка служб IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="e8170-153">IIS/IIS Express support</span></span>

<span data-ttu-id="e8170-154">Windows Server 2012 или более поздней версии и Windows 8 или более поздней версии с IIS и IIS Express 8 или более поздней версии поддерживают протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="e8170-154">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="e8170-155">Соединения WebSockets всегда включены при использовании IIS Express.</span><span class="sxs-lookup"><span data-stu-id="e8170-155">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="e8170-156">Включение WebSockets в службах IIS</span><span class="sxs-lookup"><span data-stu-id="e8170-156">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="e8170-157">Чтобы включить поддержку протокола WebSocket в Windows Server 2012 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="e8170-157">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="e8170-158">Эти действия не требуется выполнять при использовании IIS Express</span><span class="sxs-lookup"><span data-stu-id="e8170-158">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="e8170-159">В меню **Управление** запустите мастер **Добавить роли и компоненты** или в окне **Диспетчер серверов** щелкните соответствующую ссылку.</span><span class="sxs-lookup"><span data-stu-id="e8170-159">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="e8170-160">Выберите **Установка ролей или компонентов**.</span><span class="sxs-lookup"><span data-stu-id="e8170-160">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="e8170-161">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e8170-161">Select **Next**.</span></span>
1. <span data-ttu-id="e8170-162">Выберите подходящий сервер (по умолчанию выбирается локальный сервер).</span><span class="sxs-lookup"><span data-stu-id="e8170-162">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="e8170-163">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e8170-163">Select **Next**.</span></span>
1. <span data-ttu-id="e8170-164">Разверните **Веб-сервер (IIS)** в дереве **Роли**, разверните **Веб-сервер**, а затем **Разработка приложений**.</span><span class="sxs-lookup"><span data-stu-id="e8170-164">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="e8170-165">Выберите **протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="e8170-165">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="e8170-166">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e8170-166">Select **Next**.</span></span>
1. <span data-ttu-id="e8170-167">Если дополнительные функции не требуются, нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="e8170-167">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="e8170-168">Нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="e8170-168">Select **Install**.</span></span>
1. <span data-ttu-id="e8170-169">По завершении установки выберите **Закрыть**, чтобы выйти из мастера.</span><span class="sxs-lookup"><span data-stu-id="e8170-169">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="e8170-170">Чтобы включить поддержку протокола WebSocket в Windows 8 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="e8170-170">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="e8170-171">Эти действия не требуется выполнять при использовании IIS Express</span><span class="sxs-lookup"><span data-stu-id="e8170-171">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="e8170-172">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="e8170-172">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="e8170-173">Откройте следующе узлы: **IIS** > **Службы Интернета** > **Компоненты разработки приложений**.</span><span class="sxs-lookup"><span data-stu-id="e8170-173">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="e8170-174">Выберите компонент **Протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="e8170-174">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="e8170-175">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="e8170-175">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="e8170-176">Отключите WebSocket при использовании socket.io на Node.js</span><span class="sxs-lookup"><span data-stu-id="e8170-176">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="e8170-177">Если используется поддержка WebSocket в [socket.io](https://socket.io/) на [Node.js](https://nodejs.org/), отключите модуль WebSocket IIS по умолчанию с помощью элемента `webSocket` в *web.config* или *applicationHost.config*. Если не выполнить этот шаг, модуль IIS WebSocket попытается обработать соединение WebSocket, а не Node.js и приложение.</span><span class="sxs-lookup"><span data-stu-id="e8170-177">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="e8170-178">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="e8170-178">Next steps</span></span>

<span data-ttu-id="e8170-179">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) в этой статье — это эхо-приложение.</span><span class="sxs-lookup"><span data-stu-id="e8170-179">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="e8170-180">Оно имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер перенаправляет все полученные сообщения обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="e8170-180">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="e8170-181">Запустите приложение из командной строки (оно не предназначено для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="e8170-181">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="e8170-182">В верхнем левом углу веб-страницы отображается состояние подключения:</span><span class="sxs-lookup"><span data-stu-id="e8170-182">The web page shows the connection status in the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="e8170-184">Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="e8170-184">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="e8170-185">Введите тестовое сообщение и выберите **Send** (Отправить).</span><span class="sxs-lookup"><span data-stu-id="e8170-185">Enter a test message and select **Send**.</span></span> <span data-ttu-id="e8170-186">После этого выберите **Close Socket** (Закрыть сокет).</span><span class="sxs-lookup"><span data-stu-id="e8170-186">When done, select **Close Socket**.</span></span> <span data-ttu-id="e8170-187">В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.</span><span class="sxs-lookup"><span data-stu-id="e8170-187">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)
