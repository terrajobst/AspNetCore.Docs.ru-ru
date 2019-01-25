---
title: Вопросы безопасности в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как использовать проверку подлинности и авторизации в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: 52cfac6be8e61572acdf0b19dab574b607314d97
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836068"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="d7d12-103">Вопросы безопасности в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="d7d12-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="d7d12-104">По [Andrew Stanton медсестра](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="d7d12-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="d7d12-105">В этой статье сведения об обеспечении безопасности SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d12-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="d7d12-106">Общий доступ к ресурсам независимо от источника</span><span class="sxs-lookup"><span data-stu-id="d7d12-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="d7d12-107">[Кросс-совместного использования ресурсов (CORS)](https://www.w3.org/TR/cors/) может использоваться для подключений SignalR независимо от источника в браузере.</span><span class="sxs-lookup"><span data-stu-id="d7d12-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="d7d12-108">Если код JavaScript размещается в домене, отличном от приложения SignalR, [по промежуточного слоя CORS](xref:security/cors) необходимо включить, чтобы разрешить JavaScript для подключения к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="d7d12-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="d7d12-109">Разрешить запросы независимо от источника только из доменов, которым вы доверяете или элемента управления.</span><span class="sxs-lookup"><span data-stu-id="d7d12-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="d7d12-110">Пример:</span><span class="sxs-lookup"><span data-stu-id="d7d12-110">For example:</span></span>

* <span data-ttu-id="d7d12-111">На узле размещается на `http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="d7d12-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="d7d12-112">SignalR приложение размещено на `http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="d7d12-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="d7d12-113">CORS необходимо настроить в приложении SignalR, разрешая только источник `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="d7d12-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="d7d12-114">Дополнительные сведения о настройке CORS см. в разделе [Включение запросов независимо от источника (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="d7d12-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="d7d12-115">SignalR **требует** следующие политики CORS:</span><span class="sxs-lookup"><span data-stu-id="d7d12-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="d7d12-116">Разрешить определенные ожидаемые источников.</span><span class="sxs-lookup"><span data-stu-id="d7d12-116">Allow the specific expected origins.</span></span> <span data-ttu-id="d7d12-117">Позволяя любого источника возможна, но является **не** безопасное или рекомендуемые.</span><span class="sxs-lookup"><span data-stu-id="d7d12-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="d7d12-118">Методы HTTP `GET` и `POST` должны быть разрешены.</span><span class="sxs-lookup"><span data-stu-id="d7d12-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="d7d12-119">Необходимо включить учетные данные, даже в том случае, если не используется проверка подлинности.</span><span class="sxs-lookup"><span data-stu-id="d7d12-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="d7d12-120">Например, следующая политика CORS позволяет клиенту браузера SignalR, размещенных на `https://example.com` для доступа к приложению SignalR, размещенных на `https://signalr.example.com`:</span><span class="sxs-lookup"><span data-stu-id="d7d12-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

> [!NOTE]
> <span data-ttu-id="d7d12-121">SignalR не совместим с встроенной функции CORS в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="d7d12-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="d7d12-122">Ограничение WebSocket Origin</span><span class="sxs-lookup"><span data-stu-id="d7d12-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d7d12-123">Варианты защиты, предоставляемые CORS, не применяются к WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d7d12-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="d7d12-124">Origin ограничение на WebSockets, чтение [WebSockets origin ограничение](xref:fundamentals/websockets#websocket-origin-restriction).</span><span class="sxs-lookup"><span data-stu-id="d7d12-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="d7d12-125">Варианты защиты, предоставляемые CORS, не применяются к WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d7d12-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="d7d12-126">Браузеры **не** поддерживают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="d7d12-126">Browsers do **not**:</span></span>

* <span data-ttu-id="d7d12-127">выполнение предварительных запросов CORS;</span><span class="sxs-lookup"><span data-stu-id="d7d12-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="d7d12-128">использование ограничений, указанных в заголовках `Access-Control`, при выполнении запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d7d12-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="d7d12-129">Однако браузеры отправляют заголовок `Origin` при выпуске запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d7d12-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="d7d12-130">Приложения должны быть настроены для проверки этих заголовков, чтобы использовались только WebSocket из ожидаемых источников.</span><span class="sxs-lookup"><span data-stu-id="d7d12-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="d7d12-131">В ASP.NET Core 2.1 и более поздних версиях проверка заголовков можно добиться, используя пользовательские по промежуточного слоя поместить **перед `UseSignalR`и по промежуточного слоя проверки подлинности** в `Configure`:</span><span class="sxs-lookup"><span data-stu-id="d7d12-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="d7d12-132">Заголовок `Origin` контролируется клиентом и, как и заголовок `Referer`, может быть подделан.</span><span class="sxs-lookup"><span data-stu-id="d7d12-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="d7d12-133">Эти заголовки должен **не** использоваться в качестве механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d7d12-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="d7d12-134">Ведение журнала для маркера доступа</span><span class="sxs-lookup"><span data-stu-id="d7d12-134">Access token logging</span></span>

<span data-ttu-id="d7d12-135">При использовании WebSockets или Server-Sent события, браузер клиент отправляет маркер доступа в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="d7d12-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="d7d12-136">Получение маркера доступа с помощью строки запроса является обычно так безопасен, как с помощью стандарта `Authorization` заголовка.</span><span class="sxs-lookup"><span data-stu-id="d7d12-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="d7d12-137">Следует всегда использовать протокол HTTPS, чтобы обеспечить безопасное подключение между клиентом и сервером end-to-end.</span><span class="sxs-lookup"><span data-stu-id="d7d12-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="d7d12-138">Многие веб-серверы входа URL-адрес для каждого запроса, включая строку запроса.</span><span class="sxs-lookup"><span data-stu-id="d7d12-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="d7d12-139">Ведение журнала URL-адреса могут вносить в журнал маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="d7d12-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="d7d12-140">ASP.NET Core регистрирует URL-адрес для каждого запроса по умолчанию, который будет включать строку запроса.</span><span class="sxs-lookup"><span data-stu-id="d7d12-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="d7d12-141">Пример:</span><span class="sxs-lookup"><span data-stu-id="d7d12-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="d7d12-142">Если у вас есть вопросы о ведении журнала эти данные с сервера журналов, можно отключить ведение журнала полностью, настроив `Microsoft.AspNetCore.Hosting` средство ведения журнала `Warning` уровне или выше (эти сообщения записываются в `Info` уровень).</span><span class="sxs-lookup"><span data-stu-id="d7d12-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="d7d12-143">См. в документации на [фильтрации журнала](xref:fundamentals/logging/index#log-filtering) Дополнительные сведения.</span><span class="sxs-lookup"><span data-stu-id="d7d12-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="d7d12-144">Если вы хотите по-прежнему журнал определенные сведения запроса, вы можете [записи по промежуточного слоя](xref:fundamentals/middleware/index#write-middleware) данные требовать и отфильтровать `access_token` значения строки запроса (при его наличии).</span><span class="sxs-lookup"><span data-stu-id="d7d12-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/index#write-middleware) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="d7d12-145">Исключения</span><span class="sxs-lookup"><span data-stu-id="d7d12-145">Exceptions</span></span>

<span data-ttu-id="d7d12-146">Сообщения об исключениях, как правило, являются конфиденциальных данных, которые не следует раскрывать для клиента.</span><span class="sxs-lookup"><span data-stu-id="d7d12-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="d7d12-147">По умолчанию SignalR не отправляет сведения о исключения, вызванного метода концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="d7d12-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="d7d12-148">Вместо этого клиент получает универсальное сообщение, указывающее, что произошла ошибка.</span><span class="sxs-lookup"><span data-stu-id="d7d12-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="d7d12-149">Доставка сообщений исключения клиенту может быть заменено (например в среде разработки или тестирования) [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options).</span><span class="sxs-lookup"><span data-stu-id="d7d12-149">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="d7d12-150">Сообщения об исключениях не должны предоставляться клиенту в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="d7d12-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="d7d12-151">Управление буферами</span><span class="sxs-lookup"><span data-stu-id="d7d12-151">Buffer management</span></span>

<span data-ttu-id="d7d12-152">SignalR использует буферы подключения для управления входящих и исходящих сообщений.</span><span class="sxs-lookup"><span data-stu-id="d7d12-152">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="d7d12-153">По умолчанию SignalR ограничивает эти буферы, до 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="d7d12-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="d7d12-154">Наибольшее сообщение, которое клиент или сервер может отправить составляет 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="d7d12-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="d7d12-155">Максимальная память, занятая подключение для сообщений составляет 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="d7d12-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="d7d12-156">Если сообщения всегда меньше 32 КБ, можно уменьшить предел, который:</span><span class="sxs-lookup"><span data-stu-id="d7d12-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="d7d12-157">Запрещает клиента отправлять сообщения большего размера.</span><span class="sxs-lookup"><span data-stu-id="d7d12-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="d7d12-158">Сервер никогда не потребуется выделить большие буферы для приема сообщений.</span><span class="sxs-lookup"><span data-stu-id="d7d12-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="d7d12-159">Если сообщения превышает 32 КБ, можно увеличить предел.</span><span class="sxs-lookup"><span data-stu-id="d7d12-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="d7d12-160">Увеличение этого значения означает, что:</span><span class="sxs-lookup"><span data-stu-id="d7d12-160">Increasing this limit means:</span></span>

* <span data-ttu-id="d7d12-161">Клиент может вызвать сервер должен выделить большие буферы памяти.</span><span class="sxs-lookup"><span data-stu-id="d7d12-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="d7d12-162">Распределение Server больших буферов может уменьшить количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="d7d12-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="d7d12-163">Существуют ограничения для входящих и исходящих сообщений, как можно настроить на [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) объект, настроенный в `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="d7d12-163">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="d7d12-164">`ApplicationMaxBufferSize` Представляет максимальное число байтов от клиента, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="d7d12-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="d7d12-165">Если клиент пытается отправить сообщение, размер которых превышает этот предел, соединение может быть закрыт.</span><span class="sxs-lookup"><span data-stu-id="d7d12-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="d7d12-166">`TransportMaxBufferSize` Представляет максимальное число байтов, которые сервер может отправлять.</span><span class="sxs-lookup"><span data-stu-id="d7d12-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="d7d12-167">Если сервер пытается отправить сообщение (включая возвращаемые значения методов концентратора), размер которых превышает этот предел, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="d7d12-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="d7d12-168">Если установленное `0` отключает ограничение.</span><span class="sxs-lookup"><span data-stu-id="d7d12-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="d7d12-169">Удаление ограничения позволяет клиенту отправлять сообщения из любого размера.</span><span class="sxs-lookup"><span data-stu-id="d7d12-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="d7d12-170">Вредоносные клиенты при отправке больших сообщений может привести к памяти для выделения.</span><span class="sxs-lookup"><span data-stu-id="d7d12-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="d7d12-171">Использование памяти может значительно снизить количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="d7d12-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
