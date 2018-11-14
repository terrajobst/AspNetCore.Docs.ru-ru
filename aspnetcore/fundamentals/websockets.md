---
title: Поддержка WebSockets в ASP.NET Core
author: rick-anderson
description: Сведения о начале работы с WebSocket в ASP.NET Core.
monikerRange: '>= aspnetcore-1.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/06/2018
uid: fundamentals/websockets
ms.openlocfilehash: 3a649f88699d61636d9aa7fbfe4468ca67b3b018
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225412"
---
# <a name="websockets-support-in-aspnet-core"></a><span data-ttu-id="3c714-103">Поддержка WebSockets в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3c714-103">WebSockets support in ASP.NET Core</span></span>

<span data-ttu-id="3c714-104">Авторы: [Tom Dykstra](https://github.com/tdykstra) (Том Дикстра) и [Andrew Stanton-Nurse](https://github.com/anurse) (Эндрю Стэнтон-Нёрс)</span><span class="sxs-lookup"><span data-stu-id="3c714-104">By [Tom Dykstra](https://github.com/tdykstra) and [Andrew Stanton-Nurse](https://github.com/anurse)</span></span>

<span data-ttu-id="3c714-105">Эта статья описывает начало работы с WebSocket в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3c714-105">This article explains how to get started with WebSockets in ASP.NET Core.</span></span> <span data-ttu-id="3c714-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) — это протокол, предоставляющий сохраняемые двусторонние каналы связи по TCP-подключениям.</span><span class="sxs-lookup"><span data-stu-id="3c714-106">[WebSocket](https://wikipedia.org/wiki/WebSocket) ([RFC 6455](https://tools.ietf.org/html/rfc6455)) is a protocol that enables two-way persistent communication channels over TCP connections.</span></span> <span data-ttu-id="3c714-107">Он используется в приложениях, где нужна быстрая связь в режиме реального времени, например в чатах, панелях мониторинга и играх.</span><span class="sxs-lookup"><span data-stu-id="3c714-107">It's used in apps that benefit from fast, real-time communication, such as chat, dashboard, and game apps.</span></span>

<span data-ttu-id="3c714-108">[Просмотреть или скачать пример кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="3c714-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span> <span data-ttu-id="3c714-109">Дополнительные сведения см. в разделе [Следующие шаги](#next-steps).</span><span class="sxs-lookup"><span data-stu-id="3c714-109">See the [Next steps](#next-steps) section for more information.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3c714-110">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="3c714-110">Prerequisites</span></span>

* <span data-ttu-id="3c714-111">ASP.NET Core 1.1 или более поздней версии</span><span class="sxs-lookup"><span data-stu-id="3c714-111">ASP.NET Core 1.1 or later</span></span>
* <span data-ttu-id="3c714-112">Любая ОС с поддержкой ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3c714-112">Any OS that supports ASP.NET Core:</span></span>
  
  * <span data-ttu-id="3c714-113">Windows 7/Windows Server 2008 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="3c714-113">Windows 7 / Windows Server 2008 or later</span></span>
  * <span data-ttu-id="3c714-114">Linux</span><span class="sxs-lookup"><span data-stu-id="3c714-114">Linux</span></span>
  * <span data-ttu-id="3c714-115">macOS</span><span class="sxs-lookup"><span data-stu-id="3c714-115">macOS</span></span>
  
* <span data-ttu-id="3c714-116">Если приложение выполняется в Windows с IIS:</span><span class="sxs-lookup"><span data-stu-id="3c714-116">If the app runs on Windows with IIS:</span></span>

  * <span data-ttu-id="3c714-117">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="3c714-117">Windows 8 / Windows Server 2012 or later</span></span>
  * <span data-ttu-id="3c714-118">IIS 8/IIS 8 Express</span><span class="sxs-lookup"><span data-stu-id="3c714-118">IIS 8 / IIS 8 Express</span></span>
  * <span data-ttu-id="3c714-119">Необходимо включить WebSockets (см. раздел [Поддержка IIS и IIS Express](#iisiis-express-support)).</span><span class="sxs-lookup"><span data-stu-id="3c714-119">WebSockets must be enabled (See the [IIS/IIS Express support](#iisiis-express-support) section.).</span></span>
  
* <span data-ttu-id="3c714-120">Если приложение выполняется в [HTTP.sys](xref:fundamentals/servers/httpsys):</span><span class="sxs-lookup"><span data-stu-id="3c714-120">If the app runs on [HTTP.sys](xref:fundamentals/servers/httpsys):</span></span>

  * <span data-ttu-id="3c714-121">Windows 8/Windows Server 2012 или более поздних версий</span><span class="sxs-lookup"><span data-stu-id="3c714-121">Windows 8 / Windows Server 2012 or later</span></span>

* <span data-ttu-id="3c714-122">Список поддерживаемых обозревателей см. на странице https://caniuse.com/#feat=websockets.</span><span class="sxs-lookup"><span data-stu-id="3c714-122">For supported browsers, see https://caniuse.com/#feat=websockets.</span></span>

## <a name="when-to-use-websockets"></a><span data-ttu-id="3c714-123">Условия использования WebSocket</span><span class="sxs-lookup"><span data-stu-id="3c714-123">When to use WebSockets</span></span>

<span data-ttu-id="3c714-124">Используйте WebSocket для работы с подключением через сокет напрямую.</span><span class="sxs-lookup"><span data-stu-id="3c714-124">Use WebSockets to work directly with a socket connection.</span></span> <span data-ttu-id="3c714-125">Например, можно использовать WebSocket для оптимальной производительности игры в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="3c714-125">For example, use WebSockets for the best possible performance with a real-time game.</span></span>

<span data-ttu-id="3c714-126">[ASP.NET Core SignalR](xref:signalr/introduction) — это библиотека, которая упрощает добавление веб-функций в приложения в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="3c714-126">[ASP.NET Core SignalR](xref:signalr/introduction) is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="3c714-127">Она использует WebSocket, когда это возможно.</span><span class="sxs-lookup"><span data-stu-id="3c714-127">It uses WebSockets whenever possible.</span></span>

## <a name="how-to-use-websockets"></a><span data-ttu-id="3c714-128">Как использовать WebSocket</span><span class="sxs-lookup"><span data-stu-id="3c714-128">How to use WebSockets</span></span>

* <span data-ttu-id="3c714-129">Установка пакета [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/).</span><span class="sxs-lookup"><span data-stu-id="3c714-129">Install the [Microsoft.AspNetCore.WebSockets](https://www.nuget.org/packages/Microsoft.AspNetCore.WebSockets/) package.</span></span>
* <span data-ttu-id="3c714-130">Настройка ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="3c714-130">Configure the middleware.</span></span>
* <span data-ttu-id="3c714-131">Принятие запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-131">Accept WebSocket requests.</span></span>
* <span data-ttu-id="3c714-132">Отправка и получение сообщений.</span><span class="sxs-lookup"><span data-stu-id="3c714-132">Send and receive messages.</span></span>

### <a name="configure-the-middleware"></a><span data-ttu-id="3c714-133">Настройка ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="3c714-133">Configure the middleware</span></span>

<span data-ttu-id="3c714-134">Добавьте ПО промежуточного слоя WebSocket в метод `Configure` класса `Startup`:</span><span class="sxs-lookup"><span data-stu-id="3c714-134">Add the WebSockets middleware in the `Configure` method of the `Startup` class:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSockets)]

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="3c714-135">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="3c714-135">The following settings can be configured:</span></span>

* <span data-ttu-id="3c714-136">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="3c714-136">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="3c714-137">Значение по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="3c714-137">The default is two minutes.</span></span>
* <span data-ttu-id="3c714-138">`ReceiveBufferSize` — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="3c714-138">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="3c714-139">Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="3c714-139">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="3c714-140">Значение по умолчанию — 4 КБ.</span><span class="sxs-lookup"><span data-stu-id="3c714-140">The default is 4 KB.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3c714-141">Можно настроить следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="3c714-141">The following settings can be configured:</span></span>

* <span data-ttu-id="3c714-142">`KeepAliveInterval` — как часто нужно отправлять клиенту кадры проверки связи, чтобы прокси-серверы удерживали соединение открытым.</span><span class="sxs-lookup"><span data-stu-id="3c714-142">`KeepAliveInterval` - How frequently to send "ping" frames to the client to ensure proxies keep the connection open.</span></span> <span data-ttu-id="3c714-143">Значение по умолчанию — две минуты.</span><span class="sxs-lookup"><span data-stu-id="3c714-143">The default is two minutes.</span></span>
* <span data-ttu-id="3c714-144">`ReceiveBufferSize` — размер буфера, используемого для получения данных.</span><span class="sxs-lookup"><span data-stu-id="3c714-144">`ReceiveBufferSize` - The size of the buffer used to receive data.</span></span> <span data-ttu-id="3c714-145">Опытные пользователи могут изменить этот параметр для настройки производительности с учетом размера данных.</span><span class="sxs-lookup"><span data-stu-id="3c714-145">Advanced users may need to change this for performance tuning based on the size of the data.</span></span> <span data-ttu-id="3c714-146">Значение по умолчанию — 4 КБ.</span><span class="sxs-lookup"><span data-stu-id="3c714-146">The default is 4 KB.</span></span>
* <span data-ttu-id="3c714-147">`AllowedOrigins` — список допустимых значений заголовка Origin для запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-147">`AllowedOrigins` - A list of allowed Origin header values for WebSocket requests.</span></span> <span data-ttu-id="3c714-148">По умолчанию разрешены все источники.</span><span class="sxs-lookup"><span data-stu-id="3c714-148">By default, all origins are allowed.</span></span> <span data-ttu-id="3c714-149">Дополнительные сведения см. в разделе "Ограничения для источников WebSocket" ниже.</span><span class="sxs-lookup"><span data-stu-id="3c714-149">See "WebSocket origin restriction" below for details.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=UseWebSocketsOptions)]

::: moniker-end

### <a name="accept-websocket-requests"></a><span data-ttu-id="3c714-150">Принятие запросов WebSocket</span><span class="sxs-lookup"><span data-stu-id="3c714-150">Accept WebSocket requests</span></span>

<span data-ttu-id="3c714-151">На более позднем этапе жизненного цикла запроса (например, далее в методе `Configure` или действии MVC) проверьте, относится ли запрос к WebSocket, и примите его.</span><span class="sxs-lookup"><span data-stu-id="3c714-151">Somewhere later in the request life cycle (later in the `Configure` method or in an MVC action, for example) check if it's a WebSocket request and accept the WebSocket request.</span></span>

<span data-ttu-id="3c714-152">Следующий пример взят из дальнейшей части метода `Configure`:</span><span class="sxs-lookup"><span data-stu-id="3c714-152">The following example is from later in the `Configure` method:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=AcceptWebSocket&highlight=7)]

::: moniker-end

<span data-ttu-id="3c714-153">Запрос WebSocket может поступить по любому URL-адресу, но этот пример кода принимает только запросы для `/ws`.</span><span class="sxs-lookup"><span data-stu-id="3c714-153">A WebSocket request could come in on any URL, but this sample code only accepts requests for `/ws`.</span></span>

### <a name="send-and-receive-messages"></a><span data-ttu-id="3c714-154">Отправка и получение сообщений</span><span class="sxs-lookup"><span data-stu-id="3c714-154">Send and receive messages</span></span>

<span data-ttu-id="3c714-155">Метод `AcceptWebSocketAsync` обновляет TCP-соединение до соединения WebSocket и предоставляет объект [WebSocket](/dotnet/core/api/system.net.websockets.websocket).</span><span class="sxs-lookup"><span data-stu-id="3c714-155">The `AcceptWebSocketAsync` method upgrades the TCP connection to a WebSocket connection and provides a [WebSocket](/dotnet/core/api/system.net.websockets.websocket) object.</span></span> <span data-ttu-id="3c714-156">Используйте объект `WebSocket` для отправки и получения сообщений.</span><span class="sxs-lookup"><span data-stu-id="3c714-156">Use the `WebSocket` object to send and receive messages.</span></span>

<span data-ttu-id="3c714-157">Приведенный выше код, который принимает запрос WebSocket, передает объект `WebSocket` в метод `Echo`.</span><span class="sxs-lookup"><span data-stu-id="3c714-157">The code shown earlier that accepts the WebSocket request passes the `WebSocket` object to an `Echo` method.</span></span> <span data-ttu-id="3c714-158">Код принимает сообщение и сразу отправляет такое же сообщение обратно.</span><span class="sxs-lookup"><span data-stu-id="3c714-158">The code receives a message and immediately sends back the same message.</span></span> <span data-ttu-id="3c714-159">Сообщения отправляются и получаются циклически, пока клиент не закроет подключение:</span><span class="sxs-lookup"><span data-stu-id="3c714-159">Messages are sent and received in a loop until the client closes the connection:</span></span>

::: moniker range=">= aspnetcore-2.0"

[!code-csharp[](websockets/samples/2.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

::: moniker range="< aspnetcore-2.0"

[!code-csharp[](websockets/samples/1.x/WebSocketsSample/Startup.cs?name=Echo)]

::: moniker-end

<span data-ttu-id="3c714-160">Если вы принимаете подключение WebSocket до начала этого цикла, конвейер ПО промежуточного слоя завершается.</span><span class="sxs-lookup"><span data-stu-id="3c714-160">When accepting the WebSocket connection before beginning the loop, the middleware pipeline ends.</span></span> <span data-ttu-id="3c714-161">После закрытия сокета конвейер развертывается.</span><span class="sxs-lookup"><span data-stu-id="3c714-161">Upon closing the socket, the pipeline unwinds.</span></span> <span data-ttu-id="3c714-162">То есть запрос перестает перемещаться по конвейеру после принятия WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-162">That is, the request stops moving forward in the pipeline when the WebSocket is accepted.</span></span> <span data-ttu-id="3c714-163">После завершения цикла и закрытия сокета запрос возвращается в конвейер.</span><span class="sxs-lookup"><span data-stu-id="3c714-163">When the loop is finished and the socket is closed, the request proceeds back up the pipeline.</span></span>

::: moniker range=">= aspnetcore-2.2"

### <a name="websocket-origin-restriction"></a><span data-ttu-id="3c714-164">Ограничения для источников WebSocket</span><span class="sxs-lookup"><span data-stu-id="3c714-164">WebSocket origin restriction</span></span>

<span data-ttu-id="3c714-165">Варианты защиты, предоставляемые CORS, не применяются к WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-165">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="3c714-166">Браузеры **не** поддерживают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="3c714-166">Browsers do **not**:</span></span>

* <span data-ttu-id="3c714-167">выполнение предварительных запросов CORS;</span><span class="sxs-lookup"><span data-stu-id="3c714-167">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="3c714-168">использование ограничений, указанных в заголовках `Access-Control`, при выполнении запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-168">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="3c714-169">Однако браузеры отправляют заголовок `Origin` при выпуске запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-169">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="3c714-170">Приложения должны быть настроены для проверки этих заголовков, чтобы использовались только WebSocket из ожидаемых источников.</span><span class="sxs-lookup"><span data-stu-id="3c714-170">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="3c714-171">Если вы размещаете сервер по адресу "https://server.com", а клиент — по адресу "https://client.com", добавьте "https://client.com" в список `AllowedOrigins` подлежащих проверке WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-171">If you're hosting your server on "https://server.com" and hosting your client on "https://client.com", add "https://client.com" to the `AllowedOrigins` list for WebSockets to verify.</span></span>

```csharp
app.UseWebSockets(new WebSocketOptions()
{
    AllowedOrigins.Add("https://client.com");
    AllowedOrigins.Add("https://www.client.com");
});
```

> [!NOTE]
> <span data-ttu-id="3c714-172">Заголовок `Origin` контролируется клиентом и, как и заголовок `Referer`, может быть подделан.</span><span class="sxs-lookup"><span data-stu-id="3c714-172">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="3c714-173">**Не** используйте эти заголовки в качестве механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="3c714-173">Do **not** use these headers as an authentication mechanism.</span></span>

::: moniker-end

## <a name="iisiis-express-support"></a><span data-ttu-id="3c714-174">Поддержка служб IIS/IIS Express</span><span class="sxs-lookup"><span data-stu-id="3c714-174">IIS/IIS Express support</span></span>

<span data-ttu-id="3c714-175">Windows Server 2012 или более поздней версии и Windows 8 или более поздней версии с IIS и IIS Express 8 или более поздней версии поддерживают протокол WebSocket.</span><span class="sxs-lookup"><span data-stu-id="3c714-175">Windows Server 2012 or later and Windows 8 or later with IIS/IIS Express 8 or later has support for the WebSocket protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="3c714-176">Соединения WebSockets всегда включены при использовании IIS Express.</span><span class="sxs-lookup"><span data-stu-id="3c714-176">WebSockets are always enabled when using IIS Express.</span></span>

### <a name="enabling-websockets-on-iis"></a><span data-ttu-id="3c714-177">Включение WebSockets в службах IIS</span><span class="sxs-lookup"><span data-stu-id="3c714-177">Enabling WebSockets on IIS</span></span>

<span data-ttu-id="3c714-178">Чтобы включить поддержку протокола WebSocket в Windows Server 2012 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="3c714-178">To enable support for the WebSocket protocol on Windows Server 2012 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="3c714-179">Эти действия не требуется выполнять при использовании IIS Express</span><span class="sxs-lookup"><span data-stu-id="3c714-179">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="3c714-180">В меню **Управление** запустите мастер **Добавить роли и компоненты** или в окне **Диспетчер серверов** щелкните соответствующую ссылку.</span><span class="sxs-lookup"><span data-stu-id="3c714-180">Use the **Add Roles and Features** wizard from the **Manage** menu or the link in **Server Manager**.</span></span>
1. <span data-ttu-id="3c714-181">Выберите **Установка ролей или компонентов**.</span><span class="sxs-lookup"><span data-stu-id="3c714-181">Select **Role-based or Feature-based Installation**.</span></span> <span data-ttu-id="3c714-182">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3c714-182">Select **Next**.</span></span>
1. <span data-ttu-id="3c714-183">Выберите подходящий сервер (по умолчанию выбирается локальный сервер).</span><span class="sxs-lookup"><span data-stu-id="3c714-183">Select the appropriate server (the local server is selected by default).</span></span> <span data-ttu-id="3c714-184">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3c714-184">Select **Next**.</span></span>
1. <span data-ttu-id="3c714-185">Разверните **Веб-сервер (IIS)** в дереве **Роли**, разверните **Веб-сервер**, а затем **Разработка приложений**.</span><span class="sxs-lookup"><span data-stu-id="3c714-185">Expand **Web Server (IIS)** in the **Roles** tree, expand **Web Server**, and then expand **Application Development**.</span></span>
1. <span data-ttu-id="3c714-186">Выберите **протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="3c714-186">Select **WebSocket Protocol**.</span></span> <span data-ttu-id="3c714-187">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3c714-187">Select **Next**.</span></span>
1. <span data-ttu-id="3c714-188">Если дополнительные функции не требуются, нажмите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="3c714-188">If additional features aren't needed, select **Next**.</span></span>
1. <span data-ttu-id="3c714-189">Нажмите кнопку **Установить**.</span><span class="sxs-lookup"><span data-stu-id="3c714-189">Select **Install**.</span></span>
1. <span data-ttu-id="3c714-190">По завершении установки выберите **Закрыть**, чтобы выйти из мастера.</span><span class="sxs-lookup"><span data-stu-id="3c714-190">When the installation completes, select **Close** to exit the wizard.</span></span>

<span data-ttu-id="3c714-191">Чтобы включить поддержку протокола WebSocket в Windows 8 или более поздней версии:</span><span class="sxs-lookup"><span data-stu-id="3c714-191">To enable support for the WebSocket protocol on Windows 8 or later:</span></span>

> [!NOTE]
> <span data-ttu-id="3c714-192">Эти действия не требуется выполнять при использовании IIS Express</span><span class="sxs-lookup"><span data-stu-id="3c714-192">These steps are not required when using IIS Express</span></span>

1. <span data-ttu-id="3c714-193">Последовательно выберите **Панель управления** > **Программы** > **Программы и компоненты** > **Включение или отключение компонентов Windows** (в левой части экрана).</span><span class="sxs-lookup"><span data-stu-id="3c714-193">Navigate to **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off** (left side of the screen).</span></span>
1. <span data-ttu-id="3c714-194">Откройте следующе узлы: **IIS** > **Службы Интернета** > **Компоненты разработки приложений**.</span><span class="sxs-lookup"><span data-stu-id="3c714-194">Open the following nodes: **Internet Information Services** > **World Wide Web Services** > **Application Development Features**.</span></span>
1. <span data-ttu-id="3c714-195">Выберите компонент **Протокол WebSocket**.</span><span class="sxs-lookup"><span data-stu-id="3c714-195">Select the **WebSocket Protocol** feature.</span></span> <span data-ttu-id="3c714-196">Нажмите кнопку **ОК**.</span><span class="sxs-lookup"><span data-stu-id="3c714-196">Select **OK**.</span></span>

### <a name="disable-websocket-when-using-socketio-on-nodejs"></a><span data-ttu-id="3c714-197">Отключите WebSocket при использовании socket.io на Node.js</span><span class="sxs-lookup"><span data-stu-id="3c714-197">Disable WebSocket when using socket.io on Node.js</span></span>

<span data-ttu-id="3c714-198">Если используется поддержка WebSocket в [socket.io](https://socket.io/) на [Node.js](https://nodejs.org/), отключите модуль WebSocket IIS по умолчанию с помощью элемента `webSocket` в *web.config* или *applicationHost.config*. Если не выполнить этот шаг, модуль IIS WebSocket попытается обработать соединение WebSocket, а не Node.js и приложение.</span><span class="sxs-lookup"><span data-stu-id="3c714-198">If using the WebSocket support in [socket.io](https://socket.io/) on [Node.js](https://nodejs.org/), disable the default IIS WebSocket module using the `webSocket` element in *web.config* or *applicationHost.config*. If this step isn't performed, the IIS WebSocket module attempts to handle the WebSocket communication rather than Node.js and the app.</span></span>

```xml
<system.webServer>
  <webSocket enabled="false" />
</system.webServer>
```

## <a name="next-steps"></a><span data-ttu-id="3c714-199">Следующие шаги</span><span class="sxs-lookup"><span data-stu-id="3c714-199">Next steps</span></span>

<span data-ttu-id="3c714-200">[Пример приложения](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) в этой статье — это эхо-приложение.</span><span class="sxs-lookup"><span data-stu-id="3c714-200">The [sample app](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/websockets/samples) that accompanies this article is an echo app.</span></span> <span data-ttu-id="3c714-201">Оно имеет веб-страницу, которая устанавливает соединения WebSocket, а сервер перенаправляет все полученные сообщения обратно клиенту.</span><span class="sxs-lookup"><span data-stu-id="3c714-201">It has a web page that makes WebSocket connections, and the server resends any messages it receives back to the client.</span></span> <span data-ttu-id="3c714-202">Запустите приложение из командной строки (оно не предназначено для запуска из Visual Studio с IIS Express) и перейдите по адресу http://localhost:5000.</span><span class="sxs-lookup"><span data-stu-id="3c714-202">Run the app from a command prompt (it's not set up to run from Visual Studio with IIS Express) and navigate to http://localhost:5000.</span></span> <span data-ttu-id="3c714-203">В верхнем левом углу веб-страницы отображается состояние подключения:</span><span class="sxs-lookup"><span data-stu-id="3c714-203">The web page shows the connection status in the upper left:</span></span>

![Начальное состояние веб-страницы](websockets/_static/start.png)

<span data-ttu-id="3c714-205">Выберите **Connect** (Подключить), чтобы отправить запрос WebSocket на показанный URL-адрес.</span><span class="sxs-lookup"><span data-stu-id="3c714-205">Select **Connect** to send a WebSocket request to the URL shown.</span></span> <span data-ttu-id="3c714-206">Введите тестовое сообщение и выберите **Send** (Отправить).</span><span class="sxs-lookup"><span data-stu-id="3c714-206">Enter a test message and select **Send**.</span></span> <span data-ttu-id="3c714-207">После этого выберите **Close Socket** (Закрыть сокет).</span><span class="sxs-lookup"><span data-stu-id="3c714-207">When done, select **Close Socket**.</span></span> <span data-ttu-id="3c714-208">В разделе **Communication Log** (Журнал связи) выводится каждое выполняемое действие открытия, отправки и закрытия.</span><span class="sxs-lookup"><span data-stu-id="3c714-208">The **Communication Log** section reports each open, send, and close action as it happens.</span></span>

![Начальное состояние веб-страницы](websockets/_static/end.png)
