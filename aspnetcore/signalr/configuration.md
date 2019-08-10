---
title: Конфигурация SignalR ASP.NET Core
author: bradygaster
description: Узнайте, как настроить приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 475d9664c588c06bfcd816959be8a425ee01c023
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68915085"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="d3867-103">Конфигурация SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d3867-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="d3867-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="d3867-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="d3867-105">ASP.NET Core SignalR поддерживает два протокола кодирования сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="d3867-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="d3867-106">Для каждого протокола предусмотрены параметры конфигурации сериализации.</span><span class="sxs-lookup"><span data-stu-id="d3867-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="d3867-107">Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , который можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices` методе.</span><span class="sxs-lookup"><span data-stu-id="d3867-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d3867-108">Метод принимает делегат, который `options` получает объект. `AddJsonProtocol`</span><span class="sxs-lookup"><span data-stu-id="d3867-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="d3867-109">Свойство [пайлоадсериализерсеттингс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) этого объекта является объектом JSON.NET `JsonSerializerSettings` , который можно использовать для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="d3867-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="d3867-110">Дополнительные сведения см. в [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) .</span><span class="sxs-lookup"><span data-stu-id="d3867-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="d3867-111">Например, чтобы настроить сериализатор для использования имен свойств "PascalCase" вместо имен по умолчанию "camelCase", используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="d3867-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="d3867-112">В клиенте .NET такой же `AddJsonProtocol` метод расширения существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="d3867-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="d3867-113">Чтобы `Microsoft.Extensions.DependencyInjection` разрешить метод расширения, необходимо импортировать пространство имен:</span><span class="sxs-lookup"><span data-stu-id="d3867-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="d3867-114">В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3867-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="d3867-115">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="d3867-115">MessagePack serialization options</span></span>

<span data-ttu-id="d3867-116">MessagePack сериализацию можно настроить, предоставив делегат вызову [аддмессажепаккпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="d3867-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="d3867-117">Дополнительные сведения см. [в разделе MessagePack в SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="d3867-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="d3867-118">В настоящее время невозможно настроить сериализацию MessagePack в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d3867-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="d3867-119">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="d3867-119">Configure server options</span></span>

<span data-ttu-id="d3867-120">В следующей таблице описаны параметры для настройки концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3867-120">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="d3867-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-121">Option</span></span> | <span data-ttu-id="d3867-122">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-122">Default Value</span></span> | <span data-ttu-id="d3867-123">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="d3867-124">30 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-124">30 seconds</span></span> | <span data-ttu-id="d3867-125">Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности").</span><span class="sxs-lookup"><span data-stu-id="d3867-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="d3867-126">Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="d3867-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="d3867-127">Рекомендуемое значение — двойное `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="d3867-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="d3867-128">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-128">15 seconds</span></span> | <span data-ttu-id="d3867-129">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="d3867-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="d3867-130">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-131">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d3867-132">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-132">15 seconds</span></span> | <span data-ttu-id="d3867-133">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="d3867-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="d3867-134">При изменении `KeepAliveInterval` параметранаклиенте`serverTimeoutInMilliseconds` `ServerTimeout` / измените параметр.</span><span class="sxs-lookup"><span data-stu-id="d3867-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="d3867-135">Рекомендуемое `ServerTimeout` / значение—`KeepAliveInterval` двойное значение. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="d3867-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="d3867-136">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="d3867-136">All installed protocols</span></span> | <span data-ttu-id="d3867-137">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="d3867-137">Protocols supported by this hub.</span></span> <span data-ttu-id="d3867-138">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="d3867-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="d3867-139">Если `true`значение равно, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="d3867-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d3867-140">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="d3867-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="d3867-141">Максимальное число элементов, которые могут быть помещены в буфер для потоков загрузки клиента.</span><span class="sxs-lookup"><span data-stu-id="d3867-141">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="d3867-142">При достижении этого предела обработка вызовов блокируется до тех пор, пока сервер не обработает потоковые элементы.</span><span class="sxs-lookup"><span data-stu-id="d3867-142">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="d3867-143">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d3867-143">32 KB</span></span> | <span data-ttu-id="d3867-144">Максимальный размер одного входящего сообщения концентратора.</span><span class="sxs-lookup"><span data-stu-id="d3867-144">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="d3867-145">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-145">Option</span></span> | <span data-ttu-id="d3867-146">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-146">Default Value</span></span> | <span data-ttu-id="d3867-147">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="d3867-148">30 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-148">30 seconds</span></span> | <span data-ttu-id="d3867-149">Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности").</span><span class="sxs-lookup"><span data-stu-id="d3867-149">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="d3867-150">Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="d3867-150">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="d3867-151">Рекомендуемое значение — двойное `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="d3867-151">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="d3867-152">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-152">15 seconds</span></span> | <span data-ttu-id="d3867-153">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="d3867-153">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="d3867-154">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-154">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-155">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-155">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d3867-156">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-156">15 seconds</span></span> | <span data-ttu-id="d3867-157">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="d3867-157">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="d3867-158">При изменении `KeepAliveInterval` параметранаклиенте`serverTimeoutInMilliseconds` `ServerTimeout` / измените параметр.</span><span class="sxs-lookup"><span data-stu-id="d3867-158">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="d3867-159">Рекомендуемое `ServerTimeout` / значение—`KeepAliveInterval` двойное значение. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="d3867-159">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="d3867-160">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="d3867-160">All installed protocols</span></span> | <span data-ttu-id="d3867-161">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="d3867-161">Protocols supported by this hub.</span></span> <span data-ttu-id="d3867-162">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="d3867-162">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="d3867-163">Если `true`значение равно, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="d3867-163">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d3867-164">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="d3867-164">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="d3867-165">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-165">Option</span></span> | <span data-ttu-id="d3867-166">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-166">Default Value</span></span> | <span data-ttu-id="d3867-167">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="d3867-168">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-168">15 seconds</span></span> | <span data-ttu-id="d3867-169">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="d3867-169">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="d3867-170">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-170">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-171">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-171">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d3867-172">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-172">15 seconds</span></span> | <span data-ttu-id="d3867-173">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="d3867-173">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="d3867-174">При изменении `KeepAliveInterval` параметранаклиенте`serverTimeoutInMilliseconds` `ServerTimeout` / измените параметр.</span><span class="sxs-lookup"><span data-stu-id="d3867-174">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="d3867-175">Рекомендуемое `ServerTimeout` / значение—`KeepAliveInterval` двойное значение. `serverTimeoutInMilliseconds`</span><span class="sxs-lookup"><span data-stu-id="d3867-175">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="d3867-176">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="d3867-176">All installed protocols</span></span> | <span data-ttu-id="d3867-177">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="d3867-177">Protocols supported by this hub.</span></span> <span data-ttu-id="d3867-178">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="d3867-178">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="d3867-179">Если `true`значение равно, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="d3867-179">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d3867-180">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="d3867-180">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="d3867-181">Параметры можно настроить для всех концентраторов, предоставив делегаты параметров для `AddSignalR` вызова в. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="d3867-181">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="d3867-182">Параметры одного концентратора переопределяют глобальные параметры, предоставленные `AddSignalR` в, и могут быть <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>настроены с помощью:</span><span class="sxs-lookup"><span data-stu-id="d3867-182">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="d3867-183">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="d3867-183">Advanced HTTP configuration options</span></span>

<span data-ttu-id="d3867-184">Используется `HttpConnectionDispatcherOptions` для настройки дополнительных параметров, относящихся к транспортам и управлению буферами памяти.</span><span class="sxs-lookup"><span data-stu-id="d3867-184">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="d3867-185">Эти параметры настраиваются путем передачи делегата в [мафуб\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d3867-185">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="d3867-186">В следующей таблице описаны параметры для настройки расширенных HTTP-параметров SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d3867-186">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="d3867-187">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-187">Option</span></span> | <span data-ttu-id="d3867-188">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-188">Default Value</span></span> | <span data-ttu-id="d3867-189">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-189">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="d3867-190">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d3867-190">32 KB</span></span> | <span data-ttu-id="d3867-191">Максимальное число байтов, полученных от клиента, который буфер сервера перед применением обратной нагрузки.</span><span class="sxs-lookup"><span data-stu-id="d3867-191">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="d3867-192">Увеличение этого значения позволяет серверу быстрее принимать сообщения большего размера без применения недостаточной нагрузки, но может увеличить потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="d3867-192">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="d3867-193">Данные автоматически собираются из `Authorize` атрибутов, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="d3867-193">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="d3867-194">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="d3867-194">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="d3867-195">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d3867-195">32 KB</span></span> | <span data-ttu-id="d3867-196">Максимальное число байтов, отправленных приложением, которые были буфером сервера, прежде чем наблюдать за нехваткой.</span><span class="sxs-lookup"><span data-stu-id="d3867-196">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="d3867-197">Увеличение этого значения позволяет серверу быстрее занимать больше сообщений, не ожидая перехода на резервную нагрузку, но может увеличить потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="d3867-197">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="d3867-198">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="d3867-198">All Transports are enabled.</span></span> | <span data-ttu-id="d3867-199">Битовое перечисление `HttpTransportType` значений, которое может ограничивать возможности транспорта, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="d3867-199">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="d3867-200">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d3867-200">See below.</span></span> | <span data-ttu-id="d3867-201">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="d3867-201">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="d3867-202">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d3867-202">See below.</span></span> | <span data-ttu-id="d3867-203">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3867-203">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="d3867-204">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-204">Option</span></span> | <span data-ttu-id="d3867-205">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-205">Default Value</span></span> | <span data-ttu-id="d3867-206">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-206">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="d3867-207">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d3867-207">32 KB</span></span> | <span data-ttu-id="d3867-208">Максимальное число байтов, полученных от клиента, который является буфером сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-208">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="d3867-209">Увеличение этого значения позволяет серверу принимать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="d3867-209">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="d3867-210">Данные автоматически собираются из `Authorize` атрибутов, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="d3867-210">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="d3867-211">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="d3867-211">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="d3867-212">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d3867-212">32 KB</span></span> | <span data-ttu-id="d3867-213">Максимальное число байтов, отправленных приложением, которые используются в буфере сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-213">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="d3867-214">Увеличение этого значения позволяет серверу передавать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="d3867-214">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="d3867-215">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="d3867-215">All Transports are enabled.</span></span> | <span data-ttu-id="d3867-216">Битовое перечисление `HttpTransportType` значений, которое может ограничивать возможности транспорта, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="d3867-216">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="d3867-217">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d3867-217">See below.</span></span> | <span data-ttu-id="d3867-218">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="d3867-218">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="d3867-219">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d3867-219">See below.</span></span> | <span data-ttu-id="d3867-220">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3867-220">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="d3867-221">У транспорта с длинным опросом есть дополнительные параметры, которые можно настроить `LongPolling` с помощью свойства:</span><span class="sxs-lookup"><span data-stu-id="d3867-221">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="d3867-222">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-222">Option</span></span> | <span data-ttu-id="d3867-223">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-223">Default Value</span></span> | <span data-ttu-id="d3867-224">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-224">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="d3867-225">90 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-225">90 seconds</span></span> | <span data-ttu-id="d3867-226">Максимальное количество времени, в течение которого сервер ожидает отправки сообщения клиенту перед завершением одного запроса опроса.</span><span class="sxs-lookup"><span data-stu-id="d3867-226">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="d3867-227">Уменьшение этого значения приводит к тому, что клиент будет чаще выдавать новые запросы на опрос.</span><span class="sxs-lookup"><span data-stu-id="d3867-227">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="d3867-228">Транспорт WebSocket имеет дополнительные параметры, которые можно настроить с помощью `WebSockets` свойства:</span><span class="sxs-lookup"><span data-stu-id="d3867-228">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="d3867-229">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-229">Option</span></span> | <span data-ttu-id="d3867-230">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-230">Default Value</span></span> | <span data-ttu-id="d3867-231">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-231">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="d3867-232">5 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-232">5 seconds</span></span> | <span data-ttu-id="d3867-233">Когда сервер закрывается, если не удается закрыть клиент в течение этого интервала времени, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="d3867-233">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="d3867-234">Делегат, который можно использовать для задания `Sec-WebSocket-Protocol` заголовка в пользовательском значении.</span><span class="sxs-lookup"><span data-stu-id="d3867-234">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="d3867-235">Делегат получает значения, запрошенные клиентом в качестве входных данных, и ожидается, что он возвращает нужное значение.</span><span class="sxs-lookup"><span data-stu-id="d3867-235">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="d3867-236">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="d3867-236">Configure client options</span></span>

<span data-ttu-id="d3867-237">Параметры клиента можно настроить для `HubConnectionBuilder` типа (доступного в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d3867-237">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="d3867-238">Он также доступен в клиенте Java, но `HttpHubConnectionBuilder` он содержит параметры конфигурации построителя, а также `HubConnection` сам подкласс.</span><span class="sxs-lookup"><span data-stu-id="d3867-238">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="d3867-239">Настройка ведения журнала</span><span class="sxs-lookup"><span data-stu-id="d3867-239">Configure logging</span></span>

<span data-ttu-id="d3867-240">Ведение журнала настраивается в клиенте .NET `ConfigureLogging` с помощью метода.</span><span class="sxs-lookup"><span data-stu-id="d3867-240">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="d3867-241">Регистраторы и фильтры могут быть зарегистрированы так же, как и на сервере.</span><span class="sxs-lookup"><span data-stu-id="d3867-241">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="d3867-242">Дополнительные сведения см. в документации по [ведению журнала ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="d3867-242">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="d3867-243">Чтобы зарегистрировать регистраторы, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="d3867-243">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="d3867-244">Полный список см. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) документации.</span><span class="sxs-lookup"><span data-stu-id="d3867-244">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="d3867-245">Например, чтобы включить ведение журнала консоли, установите `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="d3867-245">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="d3867-246">Вызовите `AddConsole` метод расширения:</span><span class="sxs-lookup"><span data-stu-id="d3867-246">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="d3867-247">В клиенте JavaScript аналогичный `configureLogging` метод существует.</span><span class="sxs-lookup"><span data-stu-id="d3867-247">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="d3867-248">`LogLevel` Укажите значение, указывающее минимальный уровень создаваемых сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="d3867-248">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="d3867-249">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="d3867-249">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d3867-250">Вместо значения можно также указать значение, представляющее имя уровня журнала. `string` `LogLevel`</span><span class="sxs-lookup"><span data-stu-id="d3867-250">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="d3867-251">Это полезно при настройке ведения журнала SignalR в средах, где у вас нет доступа к `LogLevel` константам.</span><span class="sxs-lookup"><span data-stu-id="d3867-251">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="d3867-252">В следующей таблице перечислены доступные уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d3867-252">The following table lists the available log levels.</span></span> <span data-ttu-id="d3867-253">Значение, которое вы указываете для `configureLogging` установки **минимального** уровня ведения журнала, который будет записан в журнал.</span><span class="sxs-lookup"><span data-stu-id="d3867-253">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="d3867-254">Сообщения, зарегистрированные на этом уровне **или перечисленные в таблице уровни**, будут занесены в журнал.</span><span class="sxs-lookup"><span data-stu-id="d3867-254">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="d3867-255">String</span><span class="sxs-lookup"><span data-stu-id="d3867-255">String</span></span>                      | <span data-ttu-id="d3867-256">LogLevel</span><span class="sxs-lookup"><span data-stu-id="d3867-256">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="d3867-257">`info`**или**`information`</span><span class="sxs-lookup"><span data-stu-id="d3867-257">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="d3867-258">`warn`**или**`warning`</span><span class="sxs-lookup"><span data-stu-id="d3867-258">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d3867-259">Чтобы полностью отключить ведение журнала, `signalR.LogLevel.None` укажите `configureLogging` в методе.</span><span class="sxs-lookup"><span data-stu-id="d3867-259">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="d3867-260">Дополнительные сведения о ведении журналов см. в [документации по диагностике SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="d3867-260">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="d3867-261">Клиент, использующий SignalR Java, использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d3867-261">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="d3867-262">Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d3867-262">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="d3867-263">В следующем фрагменте кода показано, как `java.util.logging` использовать с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="d3867-263">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="d3867-264">Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:</span><span class="sxs-lookup"><span data-stu-id="d3867-264">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="d3867-265">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="d3867-265">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="d3867-266">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="d3867-266">Configure allowed transports</span></span>

<span data-ttu-id="d3867-267">Транспорты, используемые SignalR, можно настроить в `WithUrl` вызове (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d3867-267">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="d3867-268">Побитовое или для значений `HttpTransportType` можно использовать, чтобы ограничить клиент использованием только указанных транспортов.</span><span class="sxs-lookup"><span data-stu-id="d3867-268">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="d3867-269">По умолчанию все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="d3867-269">All transports are enabled by default.</span></span>

<span data-ttu-id="d3867-270">Например, чтобы отключить транспорт событий, отправленных сервером, но разрешить соединения WebSockets и long опрашиваете:</span><span class="sxs-lookup"><span data-stu-id="d3867-270">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="d3867-271">В клиенте JavaScript транспорты настраиваются путем установки `transport` поля для объекта Options, указанного в: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="d3867-271">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d3867-272">В этой версии клиентские WebSocket-сокеты Java являются единственным доступным транспортом.</span><span class="sxs-lookup"><span data-stu-id="d3867-272">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="d3867-273">В клиенте Java транспорт выбирается с помощью `withTransport` метода `HttpHubConnectionBuilder`в.</span><span class="sxs-lookup"><span data-stu-id="d3867-273">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="d3867-274">Клиент Java по умолчанию использует транспорт WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d3867-274">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="d3867-275">Клиент Java SignalR пока не поддерживает откат транспорта.</span><span class="sxs-lookup"><span data-stu-id="d3867-275">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="d3867-276">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="d3867-276">Configure bearer authentication</span></span>

<span data-ttu-id="d3867-277">Чтобы предоставить данные проверки подлинности вместе с запросами SignalR, `AccessTokenProvider` используйте параметр`accessTokenFactory` (в JavaScript), чтобы указать функцию, которая возвращает нужный маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="d3867-277">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="d3867-278">В клиенте .NET этот маркер доступа передается в качестве токена "Проверка подлинности носителя" HTTP ( `Authorization` с использованием заголовка с `Bearer`типом).</span><span class="sxs-lookup"><span data-stu-id="d3867-278">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="d3867-279">В клиенте JavaScript маркер доступа используется в качестве токена носителя, **за исключением** случаев, когда интерфейсы API браузера ограничивают возможность применения заголовков (в частности, при отправке сервером событий и запросах WebSocket).</span><span class="sxs-lookup"><span data-stu-id="d3867-279">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="d3867-280">В этих случаях маркер доступа предоставляется как значение `access_token`строки запроса.</span><span class="sxs-lookup"><span data-stu-id="d3867-280">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="d3867-281">В клиенте `AccessTokenProvider` .NET параметр можно указать с помощью делегата Options в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="d3867-281">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="d3867-282">В клиенте JavaScript маркер доступа настраивается путем установки `accessTokenFactory` поля для объекта Options в: `withUrl`</span><span class="sxs-lookup"><span data-stu-id="d3867-282">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="d3867-283">В клиенте SignalR Java можно настроить токен носителя, который будет использоваться для проверки подлинности, предоставив фабрику маркеров доступа для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="d3867-283">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="d3867-284">Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [\<однострочного >](https://reactivex.io/documentation/single.html) [рксжава](https://github.com/ReactiveX/RxJava) .</span><span class="sxs-lookup"><span data-stu-id="d3867-284">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="d3867-285">При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.</span><span class="sxs-lookup"><span data-stu-id="d3867-285">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="d3867-286">Настройка времени ожидания и параметров поддержания активности</span><span class="sxs-lookup"><span data-stu-id="d3867-286">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="d3867-287">Дополнительные параметры для настройки времени ожидания и проверки активности доступны для `HubConnection` самого объекта:</span><span class="sxs-lookup"><span data-stu-id="d3867-287">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="d3867-288">.NET</span><span class="sxs-lookup"><span data-stu-id="d3867-288">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="d3867-289">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-289">Option</span></span> | <span data-ttu-id="d3867-290">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-290">Default value</span></span> | <span data-ttu-id="d3867-291">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-291">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="d3867-292">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-292">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d3867-293">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-293">Timeout for server activity.</span></span> <span data-ttu-id="d3867-294">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует `Closed` событие (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d3867-294">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d3867-295">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d3867-295">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d3867-296">Рекомендуемое значение — это число, по крайней мере, `KeepAliveInterval` двойное значение сервера, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-296">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="d3867-297">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-297">15 seconds</span></span> | <span data-ttu-id="d3867-298">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-298">Timeout for initial server handshake.</span></span> <span data-ttu-id="d3867-299">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует `Closed` событие (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d3867-299">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d3867-300">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-300">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-301">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-301">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d3867-302">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-302">15 seconds</span></span> | <span data-ttu-id="d3867-303">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-303">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="d3867-304">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="d3867-304">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="d3867-305">Если клиент не отправил сообщение в `ClientTimeoutInterval` наборе на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="d3867-305">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="d3867-306">В клиенте .NET значения времени ожидания задаются `TimeSpan` в виде значений.</span><span class="sxs-lookup"><span data-stu-id="d3867-306">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="d3867-307">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d3867-307">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="d3867-308">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-308">Option</span></span> | <span data-ttu-id="d3867-309">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-309">Default value</span></span> | <span data-ttu-id="d3867-310">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-310">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="d3867-311">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-311">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d3867-312">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-312">Timeout for server activity.</span></span> <span data-ttu-id="d3867-313">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует `onclose` событие.</span><span class="sxs-lookup"><span data-stu-id="d3867-313">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="d3867-314">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d3867-314">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d3867-315">Рекомендуемое значение — это число, по крайней мере, `KeepAliveInterval` двойное значение сервера, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-315">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="d3867-316">15 секунд (15 000 миллисекундах)</span><span class="sxs-lookup"><span data-stu-id="d3867-316">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="d3867-317">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-317">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="d3867-318">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="d3867-318">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="d3867-319">Если клиент не отправил сообщение в `ClientTimeoutInterval` наборе на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="d3867-319">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="d3867-320">Java</span><span class="sxs-lookup"><span data-stu-id="d3867-320">Java</span></span>](#tab/java)

| <span data-ttu-id="d3867-321">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-321">Option</span></span> | <span data-ttu-id="d3867-322">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-322">Default value</span></span> | <span data-ttu-id="d3867-323">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-323">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="d3867-324">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="d3867-324">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="d3867-325">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-325">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d3867-326">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-326">Timeout for server activity.</span></span> <span data-ttu-id="d3867-327">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует `onClose` событие.</span><span class="sxs-lookup"><span data-stu-id="d3867-327">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="d3867-328">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d3867-328">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d3867-329">Рекомендуемое значение — это число, по крайней мере, `KeepAliveInterval` двойное значение сервера, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-329">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="d3867-330">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-330">15 seconds</span></span> | <span data-ttu-id="d3867-331">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-331">Timeout for initial server handshake.</span></span> <span data-ttu-id="d3867-332">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует `onClose` событие.</span><span class="sxs-lookup"><span data-stu-id="d3867-332">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="d3867-333">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-333">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-334">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-334">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="d3867-335">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="d3867-335">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="d3867-336">15 секунд (15 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-336">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="d3867-337">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-337">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="d3867-338">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="d3867-338">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="d3867-339">Если клиент не отправил сообщение в `ClientTimeoutInterval` наборе на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="d3867-339">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="d3867-340">.NET</span><span class="sxs-lookup"><span data-stu-id="d3867-340">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="d3867-341">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-341">Option</span></span> | <span data-ttu-id="d3867-342">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-342">Default value</span></span> | <span data-ttu-id="d3867-343">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-343">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="d3867-344">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-344">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d3867-345">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-345">Timeout for server activity.</span></span> <span data-ttu-id="d3867-346">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует `Closed` событие (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d3867-346">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d3867-347">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d3867-347">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d3867-348">Рекомендуемое значение — это число, по крайней мере, `KeepAliveInterval` двойное значение сервера, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-348">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="d3867-349">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-349">15 seconds</span></span> | <span data-ttu-id="d3867-350">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-350">Timeout for initial server handshake.</span></span> <span data-ttu-id="d3867-351">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует `Closed` событие (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d3867-351">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d3867-352">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-352">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-353">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-353">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="d3867-354">В клиенте .NET значения времени ожидания задаются `TimeSpan` в виде значений.</span><span class="sxs-lookup"><span data-stu-id="d3867-354">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="d3867-355">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d3867-355">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="d3867-356">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-356">Option</span></span> | <span data-ttu-id="d3867-357">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-357">Default value</span></span> | <span data-ttu-id="d3867-358">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-358">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="d3867-359">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-359">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d3867-360">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-360">Timeout for server activity.</span></span> <span data-ttu-id="d3867-361">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует `onclose` событие.</span><span class="sxs-lookup"><span data-stu-id="d3867-361">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="d3867-362">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d3867-362">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d3867-363">Рекомендуемое значение — это число, по крайней мере, `KeepAliveInterval` двойное значение сервера, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-363">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="d3867-364">Java</span><span class="sxs-lookup"><span data-stu-id="d3867-364">Java</span></span>](#tab/java)

| <span data-ttu-id="d3867-365">Параметр</span><span class="sxs-lookup"><span data-stu-id="d3867-365">Option</span></span> | <span data-ttu-id="d3867-366">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-366">Default value</span></span> | <span data-ttu-id="d3867-367">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-367">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="d3867-368">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="d3867-368">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="d3867-369">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d3867-369">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d3867-370">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-370">Timeout for server activity.</span></span> <span data-ttu-id="d3867-371">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует `onClose` событие.</span><span class="sxs-lookup"><span data-stu-id="d3867-371">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="d3867-372">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d3867-372">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d3867-373">Рекомендуемое значение — это число, по крайней мере, `KeepAliveInterval` двойное значение сервера, которое позволяет получить время для проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d3867-373">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="d3867-374">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-374">15 seconds</span></span> | <span data-ttu-id="d3867-375">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d3867-375">Timeout for initial server handshake.</span></span> <span data-ttu-id="d3867-376">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует `onClose` событие.</span><span class="sxs-lookup"><span data-stu-id="d3867-376">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="d3867-377">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d3867-377">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d3867-378">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d3867-378">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="d3867-379">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="d3867-379">Configure additional options</span></span>

<span data-ttu-id="d3867-380">Дополнительные `WithUrl` параметры можно настроить в методе (в JavaScript) для`withUrl` `HubConnectionBuilder` `HttpHubConnectionBuilder` или в различных API-интерфейсах конфигурации в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="d3867-380">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="d3867-381">.NET</span><span class="sxs-lookup"><span data-stu-id="d3867-381">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="d3867-382">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="d3867-382">.NET Option</span></span> |  <span data-ttu-id="d3867-383">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-383">Default value</span></span> | <span data-ttu-id="d3867-384">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-384">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="d3867-385">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="d3867-385">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="d3867-386">Установите этот параметр `true` в значение, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="d3867-386">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d3867-387">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным**транспортом.</span><span class="sxs-lookup"><span data-stu-id="d3867-387">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d3867-388">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3867-388">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="d3867-389">Empty</span><span class="sxs-lookup"><span data-stu-id="d3867-389">Empty</span></span> | <span data-ttu-id="d3867-390">Коллекция сертификатов TLS для отправки в запросы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d3867-390">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="d3867-391">Empty</span><span class="sxs-lookup"><span data-stu-id="d3867-391">Empty</span></span> | <span data-ttu-id="d3867-392">Коллекция файлов cookie HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d3867-392">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="d3867-393">Empty</span><span class="sxs-lookup"><span data-stu-id="d3867-393">Empty</span></span> | <span data-ttu-id="d3867-394">Учетные данные для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d3867-394">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="d3867-395">5 секунд</span><span class="sxs-lookup"><span data-stu-id="d3867-395">5 seconds</span></span> | <span data-ttu-id="d3867-396">Только WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d3867-396">WebSockets only.</span></span> <span data-ttu-id="d3867-397">Максимальное время ожидания клиента после закрытия сервера для подтверждения запроса на закрытие.</span><span class="sxs-lookup"><span data-stu-id="d3867-397">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="d3867-398">Если сервер не подтверждает закрытие в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="d3867-398">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="d3867-399">Empty</span><span class="sxs-lookup"><span data-stu-id="d3867-399">Empty</span></span> | <span data-ttu-id="d3867-400">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d3867-400">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="d3867-401">Делегат, который можно использовать для настройки или замены `HttpMessageHandler` используемого для отправки HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d3867-401">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="d3867-402">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3867-402">Not used for WebSocket connections.</span></span> <span data-ttu-id="d3867-403">Этот делегат должен возвращать значение, отличное от NULL, и получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="d3867-403">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="d3867-404">Либо измените параметры для этого значения по умолчанию и верните его, либо возвратите новый `HttpMessageHandler` экземпляр.</span><span class="sxs-lookup"><span data-stu-id="d3867-404">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="d3867-405">**При замене обработчика обязательно скопируйте параметры, которые необходимо сохранить из предоставленного обработчика. в противном случае настроенные параметры (такие как файлы cookie и заголовки) не будут применяться к новому обработчику.**</span><span class="sxs-lookup"><span data-stu-id="d3867-405">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="d3867-406">HTTP-прокси, используемый при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d3867-406">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="d3867-407">Установите это логическое значение, чтобы отправить учетные данные по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d3867-407">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="d3867-408">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="d3867-408">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="d3867-409">Делегат, который можно использовать для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d3867-409">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="d3867-410">Получает экземпляр [клиентвебсоккетоптионс](/dotnet/api/system.net.websockets.clientwebsocketoptions) , который можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="d3867-410">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="d3867-411">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d3867-411">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="d3867-412">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="d3867-412">JavaScript Option</span></span> | <span data-ttu-id="d3867-413">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-413">Default Value</span></span> | <span data-ttu-id="d3867-414">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-414">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="d3867-415">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="d3867-415">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="d3867-416">Установите этот параметр `true` в значение, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="d3867-416">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d3867-417">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным**транспортом.</span><span class="sxs-lookup"><span data-stu-id="d3867-417">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d3867-418">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3867-418">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="d3867-419">Java</span><span class="sxs-lookup"><span data-stu-id="d3867-419">Java</span></span>](#tab/java)

| <span data-ttu-id="d3867-420">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="d3867-420">Java Option</span></span> | <span data-ttu-id="d3867-421">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d3867-421">Default Value</span></span> | <span data-ttu-id="d3867-422">Описание</span><span class="sxs-lookup"><span data-stu-id="d3867-422">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="d3867-423">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="d3867-423">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="d3867-424">Установите этот параметр `true` в значение, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="d3867-424">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d3867-425">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным**транспортом.</span><span class="sxs-lookup"><span data-stu-id="d3867-425">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d3867-426">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d3867-426">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="d3867-427">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="d3867-427">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="d3867-428">Empty</span><span class="sxs-lookup"><span data-stu-id="d3867-428">Empty</span></span> | <span data-ttu-id="d3867-429">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d3867-429">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="d3867-430">В клиенте .NET эти параметры можно изменить с помощью делегата Options, предоставленного `WithUrl`для:</span><span class="sxs-lookup"><span data-stu-id="d3867-430">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="d3867-431">В клиенте JavaScript эти параметры можно предоставить в объекте JavaScript, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d3867-431">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="d3867-432">В клиенте Java эти параметры можно настроить с помощью методов, `HttpHubConnectionBuilder` возвращаемых из`HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="d3867-432">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="d3867-433">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d3867-433">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
