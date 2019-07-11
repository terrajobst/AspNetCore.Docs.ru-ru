---
title: Конфигурация ASP.NET Core SignalR
author: bradygaster
description: Сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/03/2019
uid: signalr/configuration
ms.openlocfilehash: 8c9fcaecb04555718f5da6a42a8e56c258e795af
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/11/2019
ms.locfileid: "67813451"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="eac5c-103">Конфигурация ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="eac5c-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="eac5c-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="eac5c-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="eac5c-105">ASP.NET Core SignalR поддерживает два протокола для кодировки сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="eac5c-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="eac5c-106">Каждый протокол имеет параметры конфигурации для сериализации.</span><span class="sxs-lookup"><span data-stu-id="eac5c-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="eac5c-107">Сериализация JSON можно настроить на сервере, используя [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) метод расширения, которую можно добавить после [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в вашей `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="eac5c-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="eac5c-108">`AddJsonProtocol` Метод принимает делегат, который получает `options` объекта.</span><span class="sxs-lookup"><span data-stu-id="eac5c-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="eac5c-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) свойство на этот объект является JSON.NET `JsonSerializerSettings` объект, который может использоваться для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="eac5c-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="eac5c-110">См. в разделе [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="eac5c-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="eac5c-111">Например чтобы настроить сериализатор, используемый имена свойств «PascalCase», а не имена «camelCase» по умолчанию, используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="eac5c-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="eac5c-112">В клиента .NET, так же `AddJsonProtocol` метод расширения существует на [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="eac5c-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="eac5c-113">`Microsoft.Extensions.DependencyInjection` Пространства имен должны импортироваться разрешить метод расширения:</span><span class="sxs-lookup"><span data-stu-id="eac5c-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="eac5c-114">Не поддерживается для настройки сериализации JSON в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="eac5c-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="eac5c-115">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="eac5c-115">MessagePack serialization options</span></span>

<span data-ttu-id="eac5c-116">MessagePack сериализации можно настроить, предоставьте делегат для [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) вызова.</span><span class="sxs-lookup"><span data-stu-id="eac5c-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="eac5c-117">См. в разделе [MessagePack в SignalR](xref:signalr/messagepackhubprotocol) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="eac5c-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="eac5c-118">Не поддерживается для настройки сериализации MessagePack в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="eac5c-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="eac5c-119">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="eac5c-119">Configure server options</span></span>

<span data-ttu-id="eac5c-120">В следующей таблице описаны параметры для настройки концентраторов SignalR:</span><span class="sxs-lookup"><span data-stu-id="eac5c-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="eac5c-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-121">Option</span></span> | <span data-ttu-id="eac5c-122">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-122">Default Value</span></span> | <span data-ttu-id="eac5c-123">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="eac5c-124">30 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-124">30 seconds</span></span> | <span data-ttu-id="eac5c-125">Сервер будет рассматривать клиента отключен, если он еще не получил сообщение (включая keep-alive) в этом интервале.</span><span class="sxs-lookup"><span data-stu-id="eac5c-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="eac5c-126">Может занять больше времени, чем этот интервал времени ожидания для клиента, фактически помечается как отключенный, из-за, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="eac5c-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="eac5c-127">Рекомендуемое значение — double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="eac5c-128">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-128">15 seconds</span></span> | <span data-ttu-id="eac5c-129">Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="eac5c-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="eac5c-130">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="eac5c-131">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="eac5c-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="eac5c-132">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-132">15 seconds</span></span> | <span data-ttu-id="eac5c-133">Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="eac5c-134">При изменении `KeepAliveInterval`, изменить `ServerTimeout` / `serverTimeoutInMilliseconds` параметр на клиенте.</span><span class="sxs-lookup"><span data-stu-id="eac5c-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="eac5c-135">Минимальная рекомендуемая `ServerTimeout` / `serverTimeoutInMilliseconds` значение double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="eac5c-136">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="eac5c-136">All installed protocols</span></span> | <span data-ttu-id="eac5c-137">Протоколы, поддерживаемые этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="eac5c-137">Protocols supported by this hub.</span></span> <span data-ttu-id="eac5c-138">По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="eac5c-139">Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="eac5c-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="eac5c-140">По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="eac5c-141">Максимальное количество элементов, которые можно сохранить в буфере для клиента отправьте потоков.</span><span class="sxs-lookup"><span data-stu-id="eac5c-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="eac5c-142">Если этот предел будет достигнут, обработку вызовов будет заблокирована, пока сервер обрабатывает поток элементов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="eac5c-143">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-143">Option</span></span> | <span data-ttu-id="eac5c-144">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-144">Default Value</span></span> | <span data-ttu-id="eac5c-145">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-145">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="eac5c-146">30 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-146">30 seconds</span></span> | <span data-ttu-id="eac5c-147">Сервер будет рассматривать клиента отключен, если он еще не получил сообщение (включая keep-alive) в этом интервале.</span><span class="sxs-lookup"><span data-stu-id="eac5c-147">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="eac5c-148">Может занять больше времени, чем этот интервал времени ожидания для клиента, фактически помечается как отключенный, из-за, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="eac5c-148">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="eac5c-149">Рекомендуемое значение — double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-149">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="eac5c-150">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-150">15 seconds</span></span> | <span data-ttu-id="eac5c-151">Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="eac5c-151">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="eac5c-152">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-152">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="eac5c-153">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="eac5c-153">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="eac5c-154">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-154">15 seconds</span></span> | <span data-ttu-id="eac5c-155">Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-155">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="eac5c-156">При изменении `KeepAliveInterval`, изменить `ServerTimeout` / `serverTimeoutInMilliseconds` параметр на клиенте.</span><span class="sxs-lookup"><span data-stu-id="eac5c-156">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="eac5c-157">Минимальная рекомендуемая `ServerTimeout` / `serverTimeoutInMilliseconds` значение double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-157">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="eac5c-158">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="eac5c-158">All installed protocols</span></span> | <span data-ttu-id="eac5c-159">Протоколы, поддерживаемые этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="eac5c-159">Protocols supported by this hub.</span></span> <span data-ttu-id="eac5c-160">По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-160">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="eac5c-161">Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="eac5c-161">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="eac5c-162">По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-162">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="eac5c-163">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-163">Option</span></span> | <span data-ttu-id="eac5c-164">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-164">Default Value</span></span> | <span data-ttu-id="eac5c-165">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="eac5c-166">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-166">15 seconds</span></span> | <span data-ttu-id="eac5c-167">Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="eac5c-167">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="eac5c-168">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-168">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="eac5c-169">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="eac5c-169">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="eac5c-170">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-170">15 seconds</span></span> | <span data-ttu-id="eac5c-171">Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-171">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="eac5c-172">При изменении `KeepAliveInterval`, изменить `ServerTimeout` / `serverTimeoutInMilliseconds` параметр на клиенте.</span><span class="sxs-lookup"><span data-stu-id="eac5c-172">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="eac5c-173">Минимальная рекомендуемая `ServerTimeout` / `serverTimeoutInMilliseconds` значение double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-173">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="eac5c-174">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="eac5c-174">All installed protocols</span></span> | <span data-ttu-id="eac5c-175">Протоколы, поддерживаемые этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="eac5c-175">Protocols supported by this hub.</span></span> <span data-ttu-id="eac5c-176">По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-176">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="eac5c-177">Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="eac5c-177">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="eac5c-178">По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-178">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="eac5c-179">Можно настроить параметры для всех концентраторов, предоставляя делегат параметры для `AddSignalR` вызов в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="eac5c-179">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

<span data-ttu-id="eac5c-180">Параметры для одного центра переопределить глобальные параметры, указанные на `AddSignalR` и могут настраиваться с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="eac5c-180">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="eac5c-181">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="eac5c-181">Advanced HTTP configuration options</span></span>

<span data-ttu-id="eac5c-182">Используйте `HttpConnectionDispatcherOptions` для расширенной настройки, относящиеся к транспортов и управление буфером памяти.</span><span class="sxs-lookup"><span data-stu-id="eac5c-182">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="eac5c-183">Эти параметры настраиваются путем передачи делегата для [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="eac5c-183">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

<span data-ttu-id="eac5c-184">В следующей таблице описаны параметры для настройки дополнительных параметров HTTP ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="eac5c-184">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="eac5c-185">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-185">Option</span></span> | <span data-ttu-id="eac5c-186">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-186">Default Value</span></span> | <span data-ttu-id="eac5c-187">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-187">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="eac5c-188">32 КБ</span><span class="sxs-lookup"><span data-stu-id="eac5c-188">32 KB</span></span> | <span data-ttu-id="eac5c-189">Максимальное число байтов, полученных от клиента, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="eac5c-189">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="eac5c-190">Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="eac5c-190">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="eac5c-191">Данные, собранные из автоматически `Authorize` атрибутов, примененных к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="eac5c-191">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="eac5c-192">Список [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="eac5c-192">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="eac5c-193">32 КБ</span><span class="sxs-lookup"><span data-stu-id="eac5c-193">32 KB</span></span> | <span data-ttu-id="eac5c-194">Максимальное число байтов, отправленных в приложение, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="eac5c-194">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="eac5c-195">Увеличение этого значения позволяет серверу для отправки сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="eac5c-195">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="eac5c-196">Включены все транспорты.</span><span class="sxs-lookup"><span data-stu-id="eac5c-196">All Transports are enabled.</span></span> | <span data-ttu-id="eac5c-197">Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-197">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="eac5c-198">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="eac5c-198">See below.</span></span> | <span data-ttu-id="eac5c-199">Дополнительные параметры, характерные для транспорта длинный опрос.</span><span class="sxs-lookup"><span data-stu-id="eac5c-199">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="eac5c-200">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="eac5c-200">See below.</span></span> | <span data-ttu-id="eac5c-201">Дополнительные параметры, характерные для транспорта WebSockets.</span><span class="sxs-lookup"><span data-stu-id="eac5c-201">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="eac5c-202">Транспорт длинный опрос имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:</span><span class="sxs-lookup"><span data-stu-id="eac5c-202">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="eac5c-203">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-203">Option</span></span> | <span data-ttu-id="eac5c-204">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-204">Default Value</span></span> | <span data-ttu-id="eac5c-205">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-205">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="eac5c-206">90 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-206">90 seconds</span></span> | <span data-ttu-id="eac5c-207">Максимальное время ожидания сервером ответа сообщение, отправляемое клиенту перед завершением работы запрос на единый опроса.</span><span class="sxs-lookup"><span data-stu-id="eac5c-207">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="eac5c-208">Уменьшение этого значения приводит к клиентам выполнять чаще, новые запросы опроса.</span><span class="sxs-lookup"><span data-stu-id="eac5c-208">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="eac5c-209">Транспорт WebSocket имеет дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:</span><span class="sxs-lookup"><span data-stu-id="eac5c-209">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="eac5c-210">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-210">Option</span></span> | <span data-ttu-id="eac5c-211">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-211">Default Value</span></span> | <span data-ttu-id="eac5c-212">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-212">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="eac5c-213">5 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-213">5 seconds</span></span> | <span data-ttu-id="eac5c-214">После завершения работы сервера, если клиенту не удается закрыть в течение этого времени интервала, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="eac5c-214">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="eac5c-215">Делегат, который может использоваться для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-215">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="eac5c-216">Делегат получает значения, запрошенной клиентом в качестве входных данных и должен возвращать необходимое значение.</span><span class="sxs-lookup"><span data-stu-id="eac5c-216">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="eac5c-217">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="eac5c-217">Configure client options</span></span>

<span data-ttu-id="eac5c-218">Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступна в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="eac5c-218">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="eac5c-219">Она также доступна в клиенте Java, но `HttpHubConnectionBuilder` производным классом является то, что содержит построитель параметры конфигурации, а также на `HubConnection` сам.</span><span class="sxs-lookup"><span data-stu-id="eac5c-219">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="eac5c-220">Настройка ведения журнала</span><span class="sxs-lookup"><span data-stu-id="eac5c-220">Configure logging</span></span>

<span data-ttu-id="eac5c-221">Настроить ведение журнала с помощью клиента .NET `ConfigureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="eac5c-221">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="eac5c-222">Ведение журнала, поставщики и фильтры могут быть зарегистрированы в так же как при работе на сервере.</span><span class="sxs-lookup"><span data-stu-id="eac5c-222">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="eac5c-223">См. в разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="eac5c-223">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="eac5c-224">Чтобы зарегистрировать поставщики ведения журналов, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="eac5c-224">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="eac5c-225">См. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) разделе документы с полным списком.</span><span class="sxs-lookup"><span data-stu-id="eac5c-225">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="eac5c-226">Например, чтобы включить ведение журнала консоли, установить `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="eac5c-226">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="eac5c-227">Вызовите `AddConsole` метод расширения:</span><span class="sxs-lookup"><span data-stu-id="eac5c-227">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="eac5c-228">В клиенте JavaScript, аналогичное `configureLogging` метод существует.</span><span class="sxs-lookup"><span data-stu-id="eac5c-228">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="eac5c-229">Укажите `LogLevel` значение, указывающее минимальный уровень журнала сообщений для получения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-229">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="eac5c-230">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="eac5c-230">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="eac5c-231">Вместо `LogLevel` значение, вы также можете предоставить `string` значение, представляющее имя уровня журнала.</span><span class="sxs-lookup"><span data-stu-id="eac5c-231">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="eac5c-232">Это полезно при настройке SignalR, ведение журнала в средах, где у вас нет доступа к `LogLevel` константы.</span><span class="sxs-lookup"><span data-stu-id="eac5c-232">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="eac5c-233">Ниже перечислены уровни ведения журнала доступны.</span><span class="sxs-lookup"><span data-stu-id="eac5c-233">The following table lists the available log levels.</span></span> <span data-ttu-id="eac5c-234">Значение, вы предоставляете `configureLogging` задает **минимальное** уровень, который будет записываться ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="eac5c-234">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="eac5c-235">Сообщения, регистрируемые на этом уровне **или уровни, перечисленных в таблице после него**, будут регистрироваться.</span><span class="sxs-lookup"><span data-stu-id="eac5c-235">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="eac5c-236">string</span><span class="sxs-lookup"><span data-stu-id="eac5c-236">string</span></span> | <span data-ttu-id="eac5c-237">LogLevel</span><span class="sxs-lookup"><span data-stu-id="eac5c-237">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="eac5c-238">`"info"` **Или** `"information"`</span><span class="sxs-lookup"><span data-stu-id="eac5c-238">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="eac5c-239">`"warn"` **Или** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="eac5c-239">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="eac5c-240">Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="eac5c-240">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="eac5c-241">Дополнительные сведения о ведении журнала см. в разделе [документации диагностики SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="eac5c-241">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="eac5c-242">Клиент SignalR Java использует [SLF4J](https://www.slf4j.org/) библиотеки для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="eac5c-242">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="eac5c-243">Это API высокого уровня ведения журнала, который позволяет пользователям библиотеки выбрал свою собственную реализацию определенного ведения журнала, Собрав воедино в зависимость от конкретных ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="eac5c-243">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="eac5c-244">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="eac5c-244">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="eac5c-245">Если не настроить ведение журнала в зависимости, SLF4J загружает средство ведения журнала по умолчанию без операции с следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="eac5c-245">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="eac5c-246">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="eac5c-246">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="eac5c-247">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="eac5c-247">Configure allowed transports</span></span>

<span data-ttu-id="eac5c-248">Можно настроить транспорт, используемый с SignalR в `WithUrl` вызова (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="eac5c-248">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="eac5c-249">Побитовое или-значения `HttpTransportType` может использоваться для ограничения клиента на использование только указанного транспорта.</span><span class="sxs-lookup"><span data-stu-id="eac5c-249">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="eac5c-250">Все транспорты включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="eac5c-250">All transports are enabled by default.</span></span>

<span data-ttu-id="eac5c-251">Например чтобы отключить события Server-Sent транспорта, но подключений WebSockets и длинный опрос:</span><span class="sxs-lookup"><span data-stu-id="eac5c-251">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="eac5c-252">В клиенте JavaScript транспортов настраиваются, задав `transport` на объект параметры, передаваемые `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="eac5c-252">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="eac5c-253">В этой версии Java клиента websockets-это доступно только транспорт.</span><span class="sxs-lookup"><span data-stu-id="eac5c-253">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="eac5c-254">В клиенте Java, следует выбрать транспорт `withTransport` метод `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="eac5c-254">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="eac5c-255">В клиенте Java по умолчанию с помощью транспорта WebSockets.</span><span class="sxs-lookup"><span data-stu-id="eac5c-255">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="eac5c-256">Клиент SignalR Java не поддерживает резервный транспорт.</span><span class="sxs-lookup"><span data-stu-id="eac5c-256">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="eac5c-257">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="eac5c-257">Configure bearer authentication</span></span>

<span data-ttu-id="eac5c-258">Чтобы указать данные проверки подлинности, а также SignalR запросов, используйте `AccessTokenProvider` параметр (`accessTokenFactory` в JavaScript) для указания функция, которая возвращает маркер необходимый тип доступа.</span><span class="sxs-lookup"><span data-stu-id="eac5c-258">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="eac5c-259">В клиенте .NET, этот маркер передается как HTTP «Проверки подлинности носителя» токена (с помощью `Authorization` заголовка с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="eac5c-259">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="eac5c-260">В клиенте JavaScript используется маркер доступа в качестве токена носителя, **за исключением** в некоторых случаях, где обозреватель API-интерфейсы ограничить возможность применения заголовки (в частности, в запросы событий Server-Sent и WebSockets).</span><span class="sxs-lookup"><span data-stu-id="eac5c-260">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="eac5c-261">В этих случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="eac5c-261">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="eac5c-262">В клиенте .NET `AccessTokenProvider` параметр может быть указан с помощью делегата параметры в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="eac5c-262">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="eac5c-263">В клиенте JavaScript настроен маркер доступа, задав `accessTokenFactory` на объект параметров в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="eac5c-263">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

<span data-ttu-id="eac5c-264">В клиенте SignalR Java, можно настроить токен носителя для проверки подлинности, предоставляя фабрику токена доступа для [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="eac5c-264">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="eac5c-265">Используйте [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [RxJava](https://github.com/ReactiveX/RxJava) [единый\<строка >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="eac5c-265">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="eac5c-266">С помощью вызова [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), можно написать логику для получения маркеров доступа для вашего клиента.</span><span class="sxs-lookup"><span data-stu-id="eac5c-266">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="eac5c-267">Настройка времени ожидания и параметры проверки активности</span><span class="sxs-lookup"><span data-stu-id="eac5c-267">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="eac5c-268">Доступны дополнительные параметры для настройки времени ожидания и поведение активности на `HubConnection` сам объект:</span><span class="sxs-lookup"><span data-stu-id="eac5c-268">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="eac5c-269">.NET</span><span class="sxs-lookup"><span data-stu-id="eac5c-269">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="eac5c-270">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-270">Option</span></span> | <span data-ttu-id="eac5c-271">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-271">Default value</span></span> | <span data-ttu-id="eac5c-272">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-272">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="eac5c-273">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="eac5c-273">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="eac5c-274">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="eac5c-274">Timeout for server activity.</span></span> <span data-ttu-id="eac5c-275">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="eac5c-275">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="eac5c-276">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="eac5c-276">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="eac5c-277">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-277">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="eac5c-278">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-278">15 seconds</span></span> | <span data-ttu-id="eac5c-279">Время ожидания подтверждения исходным сервером.</span><span class="sxs-lookup"><span data-stu-id="eac5c-279">Timeout for initial server handshake.</span></span> <span data-ttu-id="eac5c-280">Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="eac5c-280">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="eac5c-281">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-281">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="eac5c-282">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="eac5c-282">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="eac5c-283">В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-283">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="eac5c-284">JavaScript</span><span class="sxs-lookup"><span data-stu-id="eac5c-284">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="eac5c-285">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-285">Option</span></span> | <span data-ttu-id="eac5c-286">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-286">Default value</span></span> | <span data-ttu-id="eac5c-287">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-287">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="eac5c-288">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="eac5c-288">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="eac5c-289">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="eac5c-289">Timeout for server activity.</span></span> <span data-ttu-id="eac5c-290">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `onclose` событий.</span><span class="sxs-lookup"><span data-stu-id="eac5c-290">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="eac5c-291">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="eac5c-291">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="eac5c-292">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-292">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="eac5c-293">Java</span><span class="sxs-lookup"><span data-stu-id="eac5c-293">Java</span></span>](#tab/java)

| <span data-ttu-id="eac5c-294">Параметр</span><span class="sxs-lookup"><span data-stu-id="eac5c-294">Option</span></span> | <span data-ttu-id="eac5c-295">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-295">Default value</span></span> | <span data-ttu-id="eac5c-296">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-296">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="eac5c-297">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="eac5c-297">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="eac5c-298">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="eac5c-298">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="eac5c-299">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="eac5c-299">Timeout for server activity.</span></span> <span data-ttu-id="eac5c-300">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `onClose` событий.</span><span class="sxs-lookup"><span data-stu-id="eac5c-300">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="eac5c-301">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="eac5c-301">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="eac5c-302">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-302">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="eac5c-303">15 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-303">15 seconds</span></span> | <span data-ttu-id="eac5c-304">Время ожидания подтверждения исходным сервером.</span><span class="sxs-lookup"><span data-stu-id="eac5c-304">Timeout for initial server handshake.</span></span> <span data-ttu-id="eac5c-305">Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `onClose` событий.</span><span class="sxs-lookup"><span data-stu-id="eac5c-305">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="eac5c-306">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="eac5c-306">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="eac5c-307">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="eac5c-307">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="eac5c-308">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="eac5c-308">Configure additional options</span></span>

<span data-ttu-id="eac5c-309">Дополнительные параметры можно настроить в `WithUrl` (`withUrl` в JavaScript) метод `HubConnectionBuilder` или на различных интерфейсов API настройки на `HttpHubConnectionBuilder` в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="eac5c-309">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="eac5c-310">.NET</span><span class="sxs-lookup"><span data-stu-id="eac5c-310">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="eac5c-311">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="eac5c-311">.NET Option</span></span> |  <span data-ttu-id="eac5c-312">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-312">Default value</span></span> | <span data-ttu-id="eac5c-313">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-313">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="eac5c-314">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-314">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="eac5c-315">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="eac5c-315">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="eac5c-316">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="eac5c-316">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="eac5c-317">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="eac5c-317">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="eac5c-318">Empty</span><span class="sxs-lookup"><span data-stu-id="eac5c-318">Empty</span></span> | <span data-ttu-id="eac5c-319">Коллекция TLS-сертификатов, отправляемых для проверки подлинности запросов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-319">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="eac5c-320">Empty</span><span class="sxs-lookup"><span data-stu-id="eac5c-320">Empty</span></span> | <span data-ttu-id="eac5c-321">Коллекция файлов cookie HTTP для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="eac5c-321">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="eac5c-322">Empty</span><span class="sxs-lookup"><span data-stu-id="eac5c-322">Empty</span></span> | <span data-ttu-id="eac5c-323">Учетные данные для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="eac5c-323">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="eac5c-324">5 секунд</span><span class="sxs-lookup"><span data-stu-id="eac5c-324">5 seconds</span></span> | <span data-ttu-id="eac5c-325">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="eac5c-325">WebSockets only.</span></span> <span data-ttu-id="eac5c-326">Максимальное количество времени, клиент ожидает после закрывающего тега для сервера подтвердить запрос на закрытие.</span><span class="sxs-lookup"><span data-stu-id="eac5c-326">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="eac5c-327">Если сервер не подтвердил закрытия в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="eac5c-327">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="eac5c-328">Empty</span><span class="sxs-lookup"><span data-stu-id="eac5c-328">Empty</span></span> | <span data-ttu-id="eac5c-329">Сопоставление дополнительных HTTP-заголовков для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="eac5c-329">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="eac5c-330">Делегат, который может использоваться для настройки или замените `HttpMessageHandler` используется для отправки запросов HTTP.</span><span class="sxs-lookup"><span data-stu-id="eac5c-330">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="eac5c-331">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="eac5c-331">Not used for WebSocket connections.</span></span> <span data-ttu-id="eac5c-332">Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="eac5c-332">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="eac5c-333">Измените параметры на это значение по умолчанию и вернуть его или возвращают новый `HttpMessageHandler` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="eac5c-333">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="eac5c-334">**Если заменить обработчик обязательно скопируйте параметры, которые вы хотите сохранить из обработчика, в противном случае настроенные параметры (например, файлы cookie и заголовки) нельзя применить новый обработчик.**</span><span class="sxs-lookup"><span data-stu-id="eac5c-334">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="eac5c-335">HTTP-прокси для использования при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-335">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="eac5c-336">Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="eac5c-336">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="eac5c-337">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="eac5c-337">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="eac5c-338">Делегат, который может использоваться для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="eac5c-338">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="eac5c-339">Получает экземпляр класса [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="eac5c-339">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="eac5c-340">JavaScript</span><span class="sxs-lookup"><span data-stu-id="eac5c-340">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="eac5c-341">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="eac5c-341">JavaScript Option</span></span> | <span data-ttu-id="eac5c-342">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-342">Default Value</span></span> | <span data-ttu-id="eac5c-343">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-343">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="eac5c-344">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-344">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="eac5c-345">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="eac5c-345">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="eac5c-346">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="eac5c-346">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="eac5c-347">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="eac5c-347">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="eac5c-348">Java</span><span class="sxs-lookup"><span data-stu-id="eac5c-348">Java</span></span>](#tab/java)

| <span data-ttu-id="eac5c-349">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="eac5c-349">Java Option</span></span> | <span data-ttu-id="eac5c-350">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="eac5c-350">Default Value</span></span> | <span data-ttu-id="eac5c-351">Описание</span><span class="sxs-lookup"><span data-stu-id="eac5c-351">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="eac5c-352">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="eac5c-352">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="eac5c-353">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="eac5c-353">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="eac5c-354">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="eac5c-354">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="eac5c-355">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="eac5c-355">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="eac5c-356">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="eac5c-356">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="eac5c-357">Empty</span><span class="sxs-lookup"><span data-stu-id="eac5c-357">Empty</span></span> | <span data-ttu-id="eac5c-358">Сопоставление дополнительных HTTP-заголовков для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="eac5c-358">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="eac5c-359">В клиенте .NET эти параметры можно изменить параметры делегат для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="eac5c-359">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="eac5c-360">В клиенте JavaScript, эти параметры могут быть предоставлены в JavaScript объект, предоставляемый для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="eac5c-360">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="eac5c-361">В клиенте Java, эти параметры можно настроить на с методами `HttpHubConnectionBuilder` возвращаемые `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="eac5c-361">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="eac5c-362">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="eac5c-362">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
