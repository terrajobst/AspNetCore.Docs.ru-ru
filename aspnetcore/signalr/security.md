---
title: Вопросы безопасности в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать проверку подлинности и авторизации в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: anurse
ms.custom: mvc
ms.date: 06/29/2018
uid: signalr/security
ms.openlocfilehash: 98b5eb7be87920aacf7a941f76ff652ae7905303
ms.sourcegitcommit: f43f430a166a7ec137fcad12ded0372747227498
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/17/2018
ms.locfileid: "49391262"
---
# <a name="security-considerations-in-aspnet-core-signalr"></a><span data-ttu-id="a9c82-103">Вопросы безопасности в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a9c82-103">Security considerations in ASP.NET Core SignalR</span></span>

<span data-ttu-id="a9c82-104">По [Andrew Stanton медсестра](https://twitter.com/anurse)</span><span class="sxs-lookup"><span data-stu-id="a9c82-104">By [Andrew Stanton-Nurse](https://twitter.com/anurse)</span></span>

## <a name="overview"></a><span data-ttu-id="a9c82-105">Обзор</span><span class="sxs-lookup"><span data-stu-id="a9c82-105">Overview</span></span>

<span data-ttu-id="a9c82-106">SignalR обеспечивает ряд мер по обеспечению безопасности по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a9c82-106">SignalR provides a number of security protections by default.</span></span> <span data-ttu-id="a9c82-107">Важно понять, как настроить эти средства защиты.</span><span class="sxs-lookup"><span data-stu-id="a9c82-107">It's important to understand how to configure these protections.</span></span>

### <a name="cross-origin-resource-sharing"></a><span data-ttu-id="a9c82-108">Общий доступ к ресурсам независимо от источника</span><span class="sxs-lookup"><span data-stu-id="a9c82-108">Cross-origin resource sharing</span></span>

<span data-ttu-id="a9c82-109">[Кросс-совместного использования ресурсов (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) может использоваться для подключений SignalR независимо от источника в браузере.</span><span class="sxs-lookup"><span data-stu-id="a9c82-109">[Cross-origin resource sharing (CORS)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing) can be used to allow cross-origin SignalR connections in the browser.</span></span> <span data-ttu-id="a9c82-110">Если код JavaScript размещается в другое доменное имя из приложения SignalR, необходимо включить [по промежуточного слоя ASP.NET Core CORS](xref:security/cors) чтобы разрешить подключение.</span><span class="sxs-lookup"><span data-stu-id="a9c82-110">If your JavaScript code is hosted on a different domain name from your SignalR app, you have to enable the [ASP.NET Core CORS middleware](xref:security/cors) in order to allow the connection.</span></span> <span data-ttu-id="a9c82-111">Как правило позволяют запросов о происхождении только из доменов, контролируемых вами.</span><span class="sxs-lookup"><span data-stu-id="a9c82-111">In general, allow cross-origin requests only from domains you control.</span></span> <span data-ttu-id="a9c82-112">Например, если ваш сайт размещается по `http://www.example.com` и SignalR приложение размещено на `http://signalr.example.com`, настраивайте CORS в приложении SignalR, разрешая только источник `www.example.com`.</span><span class="sxs-lookup"><span data-stu-id="a9c82-112">For example, if your site is hosted on `http://www.example.com` and your SignalR app is hosted on `http://signalr.example.com`, you should configure CORS in your SignalR app to only allow the origin `www.example.com`.</span></span>

<span data-ttu-id="a9c82-113">Дополнительные сведения о настройке CORS см. в разделе [в документации по ASP.NET Core CORS](xref:security/cors).</span><span class="sxs-lookup"><span data-stu-id="a9c82-113">For more information on configuring CORS, see [the documentation on ASP.NET Core CORS](xref:security/cors).</span></span> <span data-ttu-id="a9c82-114">SignalR требуются следующие политики CORS для правильной работы:</span><span class="sxs-lookup"><span data-stu-id="a9c82-114">SignalR requires the following CORS policies in order to operate correctly:</span></span>

* <span data-ttu-id="a9c82-115">Политика должна разрешать определенных источников, ожидается, или разрешить любые источники (не рекомендуется).</span><span class="sxs-lookup"><span data-stu-id="a9c82-115">The policy must allow the specific origins you expect, or allow any origin (not recommended).</span></span>
* <span data-ttu-id="a9c82-116">Методы HTTP `GET` и `POST` должны быть разрешены.</span><span class="sxs-lookup"><span data-stu-id="a9c82-116">HTTP methods `GET` and `POST` must be allowed.</span></span>
* <span data-ttu-id="a9c82-117">Необходимо включить учетные данные, даже в том случае, если вы не используете проверку подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9c82-117">Credentials must be enabled, even when you aren't using authentication.</span></span>

<span data-ttu-id="a9c82-118">Например, следующая политика CORS позволяет клиенту браузера SignalR, размещенных на `http://example.com` для доступа к приложению SignalR:</span><span class="sxs-lookup"><span data-stu-id="a9c82-118">For example, the following CORS policy allows a SignalR browser client hosted on `http://example.com` to access your SignalR app:</span></span>

```csharp
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Make sure the CORS middleware is ahead of SignalR.
    app.UseCors(builder => {
        builder.WithOrigins("http://example.com")
            .AllowAnyHeader()
            .WithMethods("GET", "POST")
            .AllowCredentials();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="a9c82-119">SignalR не совместим с встроенной функции CORS в службе приложений Azure.</span><span class="sxs-lookup"><span data-stu-id="a9c82-119">SignalR is not compatible with the built-in CORS feature in Azure App Service.</span></span>

### <a name="websocket-origin-restriction"></a><span data-ttu-id="a9c82-120">Ограничение WebSocket Origin</span><span class="sxs-lookup"><span data-stu-id="a9c82-120">WebSocket Origin Restriction</span></span>

<span data-ttu-id="a9c82-121">Защиты, предоставляемые CORS не применяются к WebSockets.</span><span class="sxs-lookup"><span data-stu-id="a9c82-121">The protections provided by CORS do not apply to WebSockets.</span></span> <span data-ttu-id="a9c82-122">Браузеры не требуют предварительных запросов CORS, а также они учитывают ограничений, указанных в `Access-Control` заголовки при составлении запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a9c82-122">Browsers do not perform CORS pre-flight requests, nor do they respect the restrictions specified in `Access-Control` headers when making WebSocket requests.</span></span> <span data-ttu-id="a9c82-123">Однако браузеры отправляют `Origin` заголовка при выдаче запросов WebSocket.</span><span class="sxs-lookup"><span data-stu-id="a9c82-123">However, browsers do send the `Origin` header when issuing WebSocket requests.</span></span> <span data-ttu-id="a9c82-124">Следует настроить приложение для проверки этих заголовков, чтобы убедиться, что только WebSockets, поступающие от источники, которые предполагается, что разрешены.</span><span class="sxs-lookup"><span data-stu-id="a9c82-124">You should configure your application to validate these headers in order to ensure that only WebSockets coming from the origins you expect are allowed.</span></span>

<span data-ttu-id="a9c82-125">В ASP.NET Core 2.1, это можно сделать с помощью пользовательского по промежуточного слоя, можно поместить **выше `UseSignalR`и любое по промежуточного слоя проверки подлинности** в вашей `Configure` метод:</span><span class="sxs-lookup"><span data-stu-id="a9c82-125">In ASP.NET Core 2.1, this can be achieved using a custom middleware you can place **above `UseSignalR`, and any authentication middleware** in your `Configure` method:</span></span>

```csharp
// In your Startup class, add a static field listing the allowed Origin values:
private static readonly HashSet<string> _allowedOrigins = new HashSet<string>()
{
    // Add allowed origins here. For example:
    "http://www.mysite.com",
    "http://mysite.com",
};

// In your Configure method:
public void Configure(IApplicationBuilder app)
{
    // ... other middleware ...

    // Validate Origin header on WebSocket requests to prevent unexpected cross-site WebSocket requests
    app.Use((context, next) =>
    {
        // Check for a WebSocket request.
        if(string.Equals(context.Request.Headers["Upgrade"], "websocket"))
        {
            var origin = context.Request.Headers["Origin"];

            // If there is no origin header, or if the origin header doesn't match an allowed value:
            if(string.IsNullOrEmpty(origin) && !_allowedOrigins.Contains(origin))
            {
                // The origin is not allowed, reject the request
                context.Response.StatusCode = StatusCodes.Status400BadRequest;
                return Task.CompletedTask;
            }
        }

        // The request is not a WebSocket request or is a valid Origin, so let it continue
        return next();
    });

    // ... other middleware ...

    app.UseSignalR();

    // ... other middleware ...
}
```

> [!NOTE]
> <span data-ttu-id="a9c82-126">`Origin` Заголовка полностью контролируется с помощью клиента и, подобно `Referer` заголовка, можно подделать.</span><span class="sxs-lookup"><span data-stu-id="a9c82-126">The `Origin` header is completely controlled by the client and, like the `Referer` header, can be faked.</span></span> <span data-ttu-id="a9c82-127">Эти заголовки никогда не должен использоваться как механизм проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="a9c82-127">These headers should never be used as an authentication mechanism.</span></span>

### <a name="access-token-logging"></a><span data-ttu-id="a9c82-128">Ведение журнала для маркера доступа</span><span class="sxs-lookup"><span data-stu-id="a9c82-128">Access token logging</span></span>

<span data-ttu-id="a9c82-129">При использовании WebSockets или Server-Sent события, браузер клиент отправляет маркер доступа в строке запроса.</span><span class="sxs-lookup"><span data-stu-id="a9c82-129">When using WebSockets or Server-Sent Events, the browser client sends the access token in the query string.</span></span> <span data-ttu-id="a9c82-130">Это обычно так безопасен, как с помощью стандарта `Authorization` заголовка, однако многие веб-серверы входа URL-адрес для каждого запроса, включая строку запроса.</span><span class="sxs-lookup"><span data-stu-id="a9c82-130">This is generally as secure as using the standard `Authorization` header, however many web servers log the URL for each request, including the query string.</span></span> <span data-ttu-id="a9c82-131">Это означает, что маркер доступа, которые могут быть включены в журналах.</span><span class="sxs-lookup"><span data-stu-id="a9c82-131">This means the access token may be included in logs.</span></span> <span data-ttu-id="a9c82-132">Просмотрите параметры ведения журнала веб сервера во избежание ведение журнала этой информации.</span><span class="sxs-lookup"><span data-stu-id="a9c82-132">Consider reviewing the web server's logging settings to avoid logging this information.</span></span>

### <a name="exceptions"></a><span data-ttu-id="a9c82-133">Исключения</span><span class="sxs-lookup"><span data-stu-id="a9c82-133">Exceptions</span></span>

<span data-ttu-id="a9c82-134">Сообщения об исключениях, как правило, являются конфиденциальных данных, которые не следует раскрывать для клиента.</span><span class="sxs-lookup"><span data-stu-id="a9c82-134">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="a9c82-135">По умолчанию SignalR не отправляет сведения о исключения, вызванного метода концентратора клиенту.</span><span class="sxs-lookup"><span data-stu-id="a9c82-135">By default, SignalR doesn't send the details of an exception thrown by a hub method to the client.</span></span> <span data-ttu-id="a9c82-136">Вместо этого клиент получает универсальное сообщение, указывающее, что произошла ошибка.</span><span class="sxs-lookup"><span data-stu-id="a9c82-136">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="a9c82-137">Это поведение можно переопределить, задав [ `EnableDetailedErrors` ](xref:signalr/configuration#configure-server-options) параметр.</span><span class="sxs-lookup"><span data-stu-id="a9c82-137">You can override this behavior by setting the [`EnableDetailedErrors`](xref:signalr/configuration#configure-server-options) setting.</span></span>

### <a name="buffer-management"></a><span data-ttu-id="a9c82-138">Управление буферами</span><span class="sxs-lookup"><span data-stu-id="a9c82-138">Buffer management</span></span>

<span data-ttu-id="a9c82-139">SignalR использует буферы подключения для управления входящих и исходящих сообщений.</span><span class="sxs-lookup"><span data-stu-id="a9c82-139">SignalR uses per-connection buffers in order to manage incoming and outgoing messages.</span></span> <span data-ttu-id="a9c82-140">По умолчанию SignalR ограничивает эти буферы, до 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="a9c82-140">By default, SignalR limits these buffers to 32KB.</span></span> <span data-ttu-id="a9c82-141">Это означает, что наибольшее возможные сообщения, которое клиент или сервер может отправлять составляет 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="a9c82-141">This means the largest possible message a client or server can send is 32KB.</span></span> <span data-ttu-id="a9c82-142">Это также означает, что максимальный объем памяти, занимаемой подключение для сообщений составляет 32 КБ.</span><span class="sxs-lookup"><span data-stu-id="a9c82-142">This also means the maximum amount of memory consumed by a connection for messages is 32KB.</span></span> <span data-ttu-id="a9c82-143">Если вы знаете, что сообщения всегда меньше, чем это ограничение, можно уменьшить этот размер, чтобы клиент не сможет отправить сообщение из большего размера и заставить сервер для выделения памяти, чтобы принять его.</span><span class="sxs-lookup"><span data-stu-id="a9c82-143">If you know your messages are always smaller than this limit, you can reduce this size to prevent a client from being able to send a larger message and force the server to allocate memory to accept it.</span></span> <span data-ttu-id="a9c82-144">Аналогично Если известно, что сообщения превышают этот предел, может быть увеличен.</span><span class="sxs-lookup"><span data-stu-id="a9c82-144">Similarly, if you know your messages are larger than this limit, you can increase it.</span></span> <span data-ttu-id="a9c82-145">Однако имейте в виду, что увеличение этого значения означает, что клиент может вызвать сервер должен выделить дополнительную память и может сократить число одновременных подключений, которые приложение сможет обработать.</span><span class="sxs-lookup"><span data-stu-id="a9c82-145">However, be aware that increasing this limit means that the client is able to cause the server to allocate additional memory and may reduce the number of concurrent connections your app can handle.</span></span>

<span data-ttu-id="a9c82-146">Существуют отдельные ограничения для входящих и исходящих сообщений, как можно настроить на [ `HttpConnectionDispatcherOptions` ](xref:signalr/configuration#configure-server-options) объект, настроенный в `MapHub`:</span><span class="sxs-lookup"><span data-stu-id="a9c82-146">There are separate limits for incoming and outgoing messages, both can be configured on the [`HttpConnectionDispatcherOptions`](xref:signalr/configuration#configure-server-options) object configured in `MapHub`:</span></span>

* <span data-ttu-id="a9c82-147">`ApplicationMaxBufferSize` Представляет максимальное число байтов от клиента, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="a9c82-147">`ApplicationMaxBufferSize` represents the maximum number of bytes from the client that the server buffers.</span></span> <span data-ttu-id="a9c82-148">Если клиент пытается отправить сообщение, размер которых превышает этот предел, соединение может быть закрыт.</span><span class="sxs-lookup"><span data-stu-id="a9c82-148">If the client attempts to send a message larger than this limit, the connection may be closed.</span></span>
* <span data-ttu-id="a9c82-149">`TransportMaxBufferSize` Представляет максимальное число байтов, которые сервер может отправлять.</span><span class="sxs-lookup"><span data-stu-id="a9c82-149">`TransportMaxBufferSize` represents the maximum number of bytes the server can send.</span></span> <span data-ttu-id="a9c82-150">Если сервер предпринимает попытку отправки сообщения (включая возвращаемые значения методов концентратора) превышает этот предел, будет вызвано исключение.</span><span class="sxs-lookup"><span data-stu-id="a9c82-150">If the server attempts to send a message (includes return values from hub methods) larger than this limit, an exception will be thrown.</span></span>

<span data-ttu-id="a9c82-151">Если установленное `0` полностью отключает ограничение.</span><span class="sxs-lookup"><span data-stu-id="a9c82-151">Setting the limit to `0` disables the limit entirely.</span></span> <span data-ttu-id="a9c82-152">Тем не менее это должен сделать очень осторожно.</span><span class="sxs-lookup"><span data-stu-id="a9c82-152">However, this should be done with extreme caution.</span></span> <span data-ttu-id="a9c82-153">Удаление ограничения позволяет клиенту отправлять сообщения из любого размера.</span><span class="sxs-lookup"><span data-stu-id="a9c82-153">Removing the limit allows a client to send a message of any size.</span></span> <span data-ttu-id="a9c82-154">Это может использоваться со стороны вредоносного клиента заставить памяти для выделения, что может значительно снизить число одновременных подключений, которые ваше приложение может поддерживать.</span><span class="sxs-lookup"><span data-stu-id="a9c82-154">This could be used by a malicious client to cause excess memory to be allocated, which could dramatically reduce the number of concurrent connections your app can support.</span></span>
