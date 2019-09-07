---
title: Вопросы безопасности в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как использовать проверку подлинности и авторизацию в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 11/06/2018
uid: signalr/security
ms.openlocfilehash: a52db2ff51c55f7299d63aa3c7398f99727e0694
ms.sourcegitcommit: 387cf29f5d5addef2cbc70670a11d612806b36b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70746559"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="27208-103">Вопросы безопасности в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="27208-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="27208-104">[Эндрю Стантон-медперсонала](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="27208-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

<span data-ttu-id="27208-105">Эта статья содержит сведения о защите SignalR.</span><span class="sxs-lookup"><span data-stu-id="27208-105">This article provides information on securing SignalR.</span></span>

## <a name="cross-origin-resource-sharing"></a><span data-ttu-id="27208-106">Общий доступ к ресурсам между источниками</span><span class="sxs-lookup"><span data-stu-id="27208-106">Cross-origin resource sharing</span></span>

<span data-ttu-id="27208-107">[Общий доступ к ресурсам в разных источниках (CORS)](https://www.w3.org/TR/cors/) можно использовать, чтобы разрешить подключения SignalR в браузере.</span><span class="sxs-lookup"><span data-stu-id="27208-107">[Cross-origin resource sharing (CORS)](https://www.w3.org/TR/cors/) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="27208-108">Если код JavaScript размещен в другом домене из приложения SignalR, необходимо включить по [промежуточного слоя CORS](xref:security/cors) , чтобы разрешить JavaScript подключаться к приложению SignalR.</span><span class="sxs-lookup"><span data-stu-id="27208-108">If JavaScript code is hosted on a different domain from the SignalR app, [CORS middleware](xref:security/cors) must be enabled to allow the JavaScript to connect to the SignalR app.</span></span> <span data-ttu-id="27208-109">Разрешать запросы между источниками только из доменов, которым вы доверяете или контролируете.</span><span class="sxs-lookup"><span data-stu-id="27208-109">Allow cross-origin requests only from domains you trust or control.</span></span> <span data-ttu-id="27208-110">Например:</span><span class="sxs-lookup"><span data-stu-id="27208-110">For example:</span></span>

* <span data-ttu-id="27208-111">Ваш сайт размещен на`http://www.example.com`</span><span class="sxs-lookup"><span data-stu-id="27208-111">Your site is hosted on `http://www.example.com`</span></span>
* <span data-ttu-id="27208-112">Ваше приложение SignalR размещено на`http://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="27208-112">Your SignalR app is hosted on `http://signalr.example.com`</span></span>

<span data-ttu-id="27208-113">CORS следует настроить в приложении SignalR, чтобы разрешить только источник `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="27208-113">CORS should be configured in the SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="27208-114">Дополнительные сведения о настройке CORS см. в разделе [Включение запросов между источниками (CORS)](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="27208-114">For more information on configuring CORS, see [Enable Cross-Origin Requests (CORS)](xref:security/cors).</span></span> <span data-ttu-id="27208-115">Для SignalR **требуются** следующие политики CORS:</span><span class="sxs-lookup"><span data-stu-id="27208-115">SignalR **requires** the following CORS policies:</span></span>

* <span data-ttu-id="27208-116">Разрешить конкретные ожидаемые источники.</span><span class="sxs-lookup"><span data-stu-id="27208-116">Allow the specific expected origins.</span></span> <span data-ttu-id="27208-117">Разрешение любого источника возможно, но **не** является безопасным или рекомендуемым.</span><span class="sxs-lookup"><span data-stu-id="27208-117">Allowing any origin is possible but is **not** secure or recommended.</span></span>
* <span data-ttu-id="27208-118">Методы `GET` HTTP и `POST` должны быть разрешены.</span><span class="sxs-lookup"><span data-stu-id="27208-118">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="27208-119">Учетные данные должны быть включены, даже если проверка подлинности не используется.</span><span class="sxs-lookup"><span data-stu-id="27208-119">Credentials must be enabled, even when authentication is not used.</span></span>

<span data-ttu-id="27208-120">Например, следующая политика CORS позволяет клиенту обозревателя SignalR, размещенному `https://example.com` в, получать доступ к приложению SignalR, размещенному на: `https://signalr.example.com`</span><span class="sxs-lookup"><span data-stu-id="27208-120">For example, the following CORS policy allows a SignalR browser client hosted on `https://example.com` to access the SignalR app hosted on `https://signalr.example.com`:</span></span>

::: moniker range=">= aspnetcore-3.0"

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder =>
    {
        builder.WithOrigins("https://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<ChatHub>("/chatHub");
    });

    // ... other middleware ...
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

[!code-csharp[Main](security/sample/Startup.cs?name=snippet1)]

::: moniker-end

> [!NOTE]
> <span data-ttu-id="27208-121">SignalR несовместим со встроенным компонентом CORS в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="27208-121">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

## <a name="websocket-origin-restriction"></a><span data-ttu-id="27208-122">Ограничение источника WebSocket</span><span class="sxs-lookup"><span data-stu-id="27208-122">WebSocket Origin Restriction</span></span>

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="27208-123">Варианты защиты, предоставляемые CORS, не применяются к WebSocket.</span><span class="sxs-lookup"><span data-stu-id="27208-123">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="27208-124">Ограничение [по источнику](xref:fundamentals/websockets#websocket-origin-restriction)для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="27208-124">For origin restriction on WebSockets, read [WebSockets origin restriction](xref:fundamentals/websockets#websocket-origin-restriction).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.2"

<span data-ttu-id="27208-125">Варианты защиты, предоставляемые CORS, не применяются к WebSocket.</span><span class="sxs-lookup"><span data-stu-id="27208-125">The protections provided by CORS don't apply to WebSockets.</span></span> <span data-ttu-id="27208-126">Браузеры **не** поддерживают следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="27208-126">Browsers do **not**:</span></span>

* <span data-ttu-id="27208-127">выполнение предварительных запросов CORS;</span><span class="sxs-lookup"><span data-stu-id="27208-127">Perform CORS pre-flight requests.</span></span>
* <span data-ttu-id="27208-128">использование ограничений, указанных в заголовках `Access-Control`, при выполнении запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="27208-128">Respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span>

<span data-ttu-id="27208-129">Однако браузеры отправляют заголовок `Origin` при выпуске запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="27208-129">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="27208-130">Приложения должны быть настроены для проверки этих заголовков, чтобы использовались только WebSocket из ожидаемых источников.</span><span class="sxs-lookup"><span data-stu-id="27208-130">Applications should be configured to validate these headers to ensure that only WebSockets coming from the expected origins are allowed.</span></span>

<span data-ttu-id="27208-131">В ASP.NET Core 2,1 и более поздних версиях проверка заголовков может быть достигнута с помощью пользовательского промежуточного слоя `Configure`, размещенного **до `UseSignalR`, и по промежуточного слоя** в:</span><span class="sxs-lookup"><span data-stu-id="27208-131">In ASP.NET Core 2.1 and later, header validation can be achieved using a custom middleware placed **before `UseSignalR`, and authentication middleware** in `Configure`:</span></span>

[!code-csharp[Main](security/sample/Startup.cs?name=snippet2)]

> [!NOTE]
> <span data-ttu-id="27208-132">Заголовок `Origin` контролируется клиентом и, как и заголовок `Referer`, может быть подделан.</span><span class="sxs-lookup"><span data-stu-id="27208-132">The `Origin` header is controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="27208-133">Эти заголовки **не** следует использовать в качестве механизма проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="27208-133">These headers should **not** be used as an authentication mechanism.</span></span>

::: moniker-end

## <a name="access-token-logging"></a><span data-ttu-id="27208-134">Ведение журнала маркеров доступа</span><span class="sxs-lookup"><span data-stu-id="27208-134">Access token logging</span></span>

<span data-ttu-id="27208-135">При использовании веб-сокетов или событий, отправленных сервером, клиент браузера отправляет маркер доступа в строку запроса.</span><span class="sxs-lookup"><span data-stu-id="27208-135">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="27208-136">Получение маркера доступа через строку запроса, как правило, защищено с помощью `Authorization` стандартного заголовка.</span><span class="sxs-lookup"><span data-stu-id="27208-136">Receiving the access token via query string is generally as secure as using the standard `Authorization` header.</span></span> <span data-ttu-id="27208-137">Для обеспечения безопасного сквозного подключения между клиентом и сервером всегда следует использовать протокол HTTPS.</span><span class="sxs-lookup"><span data-stu-id="27208-137">You should always use HTTPS to ensure a secure end-to-end connection between the client and the server.</span></span> <span data-ttu-id="27208-138">Многие веб-серверы заключают в журнал URL-адрес каждого запроса, включая строку запроса.</span><span class="sxs-lookup"><span data-stu-id="27208-138">Many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="27208-139">Ведение журнала URL-адресов может регистрировать маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="27208-139">Logging the URLs may log the access token.</span></span> <span data-ttu-id="27208-140">ASP.NET Core записывает URL-адрес для каждого запроса по умолчанию, который будет включать строку запроса.</span><span class="sxs-lookup"><span data-stu-id="27208-140">ASP.NET Core logs the URL for each request by default, which will include the query string.</span></span> <span data-ttu-id="27208-141">Например:</span><span class="sxs-lookup"><span data-stu-id="27208-141">For example:</span></span>

```
info: Microsoft.AspNetCore.Hosting.Internal.WebHost[1]
      Request starting HTTP/1.1 GET http://localhost:5000/myhub?access_token=1234
```

<span data-ttu-id="27208-142">Если у вас есть вопросы о записи этих данных в журналы сервера, можно полностью отключить это ведение журнала, настроив `Microsoft.AspNetCore.Hosting` средство ведения журнала `Warning` на уровень или выше (эти сообщения записываются на `Info` уровне).</span><span class="sxs-lookup"><span data-stu-id="27208-142">If you have concerns about logging this data with your server logs, you can disable this logging entirely by configuring the `Microsoft.AspNetCore.Hosting` logger to the `Warning` level or above (these messages are written at `Info` level).</span></span> <span data-ttu-id="27208-143">Дополнительные сведения см. в документации по [фильтрации журналов](xref:fundamentals/logging/index#log-filtering) .</span><span class="sxs-lookup"><span data-stu-id="27208-143">See the documentation on [Log Filtering](xref:fundamentals/logging/index#log-filtering) for more information.</span></span> <span data-ttu-id="27208-144">Если вы по-прежнему хотите заносить в журнал определенные сведения о запросе, можно [написать по промежуточного слоя](xref:fundamentals/middleware/write) для записи нужных `access_token` данных и отфильтровать значение строки запроса (при наличии).</span><span class="sxs-lookup"><span data-stu-id="27208-144">If you still want to log certain request information, you can [write a middleware](xref:fundamentals/middleware/write) to log the data you require and filter out the `access_token` query string value (if present).</span></span>

## <a name="exceptions"></a><span data-ttu-id="27208-145">Исключения</span><span class="sxs-lookup"><span data-stu-id="27208-145">Exceptions</span></span>

<span data-ttu-id="27208-146">Сообщения об исключениях обычно считаются конфиденциальными данными, которые не должны быть раскрыты для клиента.</span><span class="sxs-lookup"><span data-stu-id="27208-146">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="27208-147">По умолчанию SignalR не отправляет клиенту сведения об исключении, вызываемом методом концентратора.</span><span class="sxs-lookup"><span data-stu-id="27208-147">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="27208-148">Вместо этого клиент получает универсальное сообщение, указывающее на возникновение ошибки.</span><span class="sxs-lookup"><span data-stu-id="27208-148">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="27208-149">Доставка сообщений об исключениях клиенту может быть переопределена (например, в разработке или тестировании [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options)) с помощью.</span><span class="sxs-lookup"><span data-stu-id="27208-149">Exception message delivery to the client can be overridden (for example in development or test) with [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options).</span></span> <span data-ttu-id="27208-150">Сообщения об исключениях не должны предоставляться клиенту в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="27208-150">Exception messages should not be exposed to the client in production apps.</span></span>

## <a name="buffer-management"></a><span data-ttu-id="27208-151">Управление буферами</span><span class="sxs-lookup"><span data-stu-id="27208-151">Buffer management</span></span>

<span data-ttu-id="27208-152">SignalR использует буферы каждого подключения для управления входящими и исходящими сообщениями.</span><span class="sxs-lookup"><span data-stu-id="27208-152">SignalR uses per-connection buffers to manage incoming and outgoing messages.</span></span> <span data-ttu-id="27208-153">По умолчанию SignalR ограничивает эти буферы до 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="27208-153">By default, SignalR limits these buffers to 32 KB.</span></span> <span data-ttu-id="27208-154">Максимальное сообщение, которое клиент или сервер может отправить, — 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="27208-154">The largest message a client or server can send is 32 KB.</span></span> <span data-ttu-id="27208-155">Максимальный объем памяти, потребляемый соединением для сообщений, составляет 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="27208-155">The maximum memory consumed by a connection for messages is 32 KB.</span></span> <span data-ttu-id="27208-156">Если размер сообщений всегда меньше 32 КБ, можно уменьшить ограничение, которое:</span><span class="sxs-lookup"><span data-stu-id="27208-156">If your messages are always smaller than 32 KB, you can reduce the limit, which:</span></span>

* <span data-ttu-id="27208-157">Запрещает клиенту отправить сообщение большего размера.</span><span class="sxs-lookup"><span data-stu-id="27208-157">Prevents a client from being able to send a larger message.</span></span>
* <span data-ttu-id="27208-158">Серверу не придется распределять большие буферы для приема сообщений.</span><span class="sxs-lookup"><span data-stu-id="27208-158">The server will never need to allocate large buffers to accept messages.</span></span>

<span data-ttu-id="27208-159">Если размер сообщений превышает 32 КБ, можно увеличить это ограничение.</span><span class="sxs-lookup"><span data-stu-id="27208-159">If your messages are larger than 32 KB, you can increase the limit.</span></span> <span data-ttu-id="27208-160">Увеличение этого предела означает:</span><span class="sxs-lookup"><span data-stu-id="27208-160">Increasing this limit means:</span></span>

* <span data-ttu-id="27208-161">Клиент может привести к выделению сервером больших буферов памяти.</span><span class="sxs-lookup"><span data-stu-id="27208-161">The client can cause the server to allocate large memory buffers.</span></span>
* <span data-ttu-id="27208-162">Выделение сервером больших буферов может сократить число одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="27208-162">Server allocation of large buffers may reduce the number of concurrent connections.</span></span>

<span data-ttu-id="27208-163">Существуют ограничения для входящих и исходящих сообщений. они могут быть настроены [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) для объекта, настроенного в: `MapHub`</span><span class="sxs-lookup"><span data-stu-id="27208-163">There are limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="27208-164">`ApplicationMaxBufferSize`представляет максимальное число байтов от клиента, на котором находятся буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="27208-164">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="27208-165">Если клиент пытается отправить сообщение, размер которого превышает это ограничение, соединение может быть закрыто.</span><span class="sxs-lookup"><span data-stu-id="27208-165">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="27208-166">`TransportMaxBufferSize`представляет максимальное число байтов, которое может быть отправлено сервером.</span><span class="sxs-lookup"><span data-stu-id="27208-166">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="27208-167">Если сервер пытается отправить сообщение (включая возвращаемые значения из методов концентратора), превышающие это ограничение, будет создано исключение.</span><span class="sxs-lookup"><span data-stu-id="27208-167">If the server attempts to send a message (including return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="27208-168">Установка ограничения `0` отключает ограничение.</span><span class="sxs-lookup"><span data-stu-id="27208-168">Setting the limit to `0` disables the limit.</span></span> <span data-ttu-id="27208-169">Удаление ограничения позволяет клиенту отправить сообщение любого размера.</span><span class="sxs-lookup"><span data-stu-id="27208-169">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="27208-170">Вредоносные клиенты, отправляющие большие сообщения, могут вызвать чрезмерное выделение памяти.</span><span class="sxs-lookup"><span data-stu-id="27208-170">Malicious clients sending large messages can cause excess memory to be allocated.</span></span> <span data-ttu-id="27208-171">Чрезмерное использование памяти может значительно сократить количество одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="27208-171">Excess memory usage can significantly reduce the number of concurrent connections.</span></span>
