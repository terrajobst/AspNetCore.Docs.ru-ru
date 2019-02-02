---
title: Конфигурация ASP.NET Core SignalR
author: bradygaster
description: Сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/29/2019
uid: signalr/configuration
ms.openlocfilehash: ce970199984cdb8333ed1fd51f744dcda2df9c61
ms.sourcegitcommit: ed76cc752966c604a795fbc56d5a71d16ded0b58
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/02/2019
ms.locfileid: "55667613"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="bc99e-103">Конфигурация ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="bc99e-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="bc99e-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="bc99e-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="bc99e-105">ASP.NET Core SignalR поддерживает два протокола для кодировки сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="bc99e-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="bc99e-106">Каждый протокол имеет параметры конфигурации для сериализации.</span><span class="sxs-lookup"><span data-stu-id="bc99e-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="bc99e-107">Сериализация JSON можно настроить на сервере, используя [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) метод расширения, которую можно добавить после [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в вашей `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="bc99e-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="bc99e-108">`AddJsonProtocol` Метод принимает делегат, который получает `options` объекта.</span><span class="sxs-lookup"><span data-stu-id="bc99e-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="bc99e-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) свойство на этот объект является JSON.NET `JsonSerializerSettings` объект, который может использоваться для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="bc99e-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="bc99e-110">См. в разделе [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="bc99e-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="bc99e-111">Например чтобы настроить сериализатор, используемый имена свойств «PascalCase», а не имена «camelCase» по умолчанию, используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="bc99e-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="bc99e-112">В клиента .NET, так же `AddJsonProtocol` метод расширения существует на [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="bc99e-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="bc99e-113">`Microsoft.Extensions.DependencyInjection` Пространства имен должны импортироваться разрешить метод расширения:</span><span class="sxs-lookup"><span data-stu-id="bc99e-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="bc99e-114">Не поддерживается для настройки сериализации JSON в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="bc99e-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="bc99e-115">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="bc99e-115">MessagePack serialization options</span></span>

<span data-ttu-id="bc99e-116">MessagePack сериализации можно настроить, предоставьте делегат для [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) вызова.</span><span class="sxs-lookup"><span data-stu-id="bc99e-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="bc99e-117">См. в разделе [MessagePack в SignalR](xref:signalr/messagepackhubprotocol) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="bc99e-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="bc99e-118">Не поддерживается для настройки сериализации MessagePack в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="bc99e-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="bc99e-119">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="bc99e-119">Configure server options</span></span>

<span data-ttu-id="bc99e-120">В следующей таблице описаны параметры для настройки концентраторов SignalR:</span><span class="sxs-lookup"><span data-stu-id="bc99e-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="bc99e-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="bc99e-121">Option</span></span> | <span data-ttu-id="bc99e-122">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bc99e-122">Default Value</span></span> | <span data-ttu-id="bc99e-123">Описание:</span><span class="sxs-lookup"><span data-stu-id="bc99e-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="bc99e-124">30 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-124">30 seconds</span></span> | <span data-ttu-id="bc99e-125">Сервер будет рассматривать клиента отключен, если он еще не получил сообщение (включая keep-alive) в этом интервале.</span><span class="sxs-lookup"><span data-stu-id="bc99e-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="bc99e-126">Может занять больше времени, чем этот интервал времени ожидания для клиента, фактически помечается как отключенный, из-за, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="bc99e-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="bc99e-127">Рекомендуемое значение — double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="bc99e-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="bc99e-128">15 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-128">15 seconds</span></span> | <span data-ttu-id="bc99e-129">Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="bc99e-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="bc99e-130">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="bc99e-131">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="bc99e-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="bc99e-132">15 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-132">15 seconds</span></span> | <span data-ttu-id="bc99e-133">Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение.</span><span class="sxs-lookup"><span data-stu-id="bc99e-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="bc99e-134">При изменении `KeepAliveInterval`, изменить `ServerTimeout` / `serverTimeoutInMilliseconds` параметр на клиенте.</span><span class="sxs-lookup"><span data-stu-id="bc99e-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="bc99e-135">Минимальная рекомендуемая `ServerTimeout` / `serverTimeoutInMilliseconds` значение double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="bc99e-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="bc99e-136">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="bc99e-136">All installed protocols</span></span> | <span data-ttu-id="bc99e-137">Протоколы, поддерживаемые этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="bc99e-137">Protocols supported by this hub.</span></span> <span data-ttu-id="bc99e-138">По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="bc99e-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="bc99e-139">Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="bc99e-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="bc99e-140">По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="bc99e-141">Можно настроить параметры для всех концентраторов, предоставляя делегат параметры для `AddSignalR` вызов в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="bc99e-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="bc99e-142">Параметры для одного центра переопределить глобальные параметры, указанные на `AddSignalR` и могут настраиваться с помощью [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="bc99e-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

<span data-ttu-id="bc99e-143">Используйте `HttpConnectionDispatcherOptions` для расширенной настройки, относящиеся к транспортов и управление буфером памяти.</span><span class="sxs-lookup"><span data-stu-id="bc99e-143">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="bc99e-144">Эти параметры настраиваются путем передачи делегата для [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="bc99e-144">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="bc99e-145">Параметр</span><span class="sxs-lookup"><span data-stu-id="bc99e-145">Option</span></span> | <span data-ttu-id="bc99e-146">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bc99e-146">Default Value</span></span> | <span data-ttu-id="bc99e-147">Описание:</span><span class="sxs-lookup"><span data-stu-id="bc99e-147">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="bc99e-148">32 КБ</span><span class="sxs-lookup"><span data-stu-id="bc99e-148">32 KB</span></span> | <span data-ttu-id="bc99e-149">Максимальное число байтов, полученных от клиента, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="bc99e-149">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="bc99e-150">Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="bc99e-150">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="bc99e-151">Данные, собранные из автоматически `Authorize` атрибутов, примененных к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="bc99e-151">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="bc99e-152">Список [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="bc99e-152">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="bc99e-153">32 КБ</span><span class="sxs-lookup"><span data-stu-id="bc99e-153">32 KB</span></span> | <span data-ttu-id="bc99e-154">Максимальное число байтов, отправленных в приложение, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="bc99e-154">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="bc99e-155">Увеличение этого значения позволяет серверу для отправки сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="bc99e-155">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="bc99e-156">Включены все транспорты.</span><span class="sxs-lookup"><span data-stu-id="bc99e-156">All Transports are enabled.</span></span> | <span data-ttu-id="bc99e-157">Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-157">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="bc99e-158">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="bc99e-158">See below.</span></span> | <span data-ttu-id="bc99e-159">Дополнительные параметры, характерные для транспорта длинный опрос.</span><span class="sxs-lookup"><span data-stu-id="bc99e-159">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="bc99e-160">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="bc99e-160">See below.</span></span> | <span data-ttu-id="bc99e-161">Дополнительные параметры, характерные для транспорта WebSockets.</span><span class="sxs-lookup"><span data-stu-id="bc99e-161">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="bc99e-162">Транспорт длинный опрос имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:</span><span class="sxs-lookup"><span data-stu-id="bc99e-162">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="bc99e-163">Параметр</span><span class="sxs-lookup"><span data-stu-id="bc99e-163">Option</span></span> | <span data-ttu-id="bc99e-164">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bc99e-164">Default Value</span></span> | <span data-ttu-id="bc99e-165">Описание:</span><span class="sxs-lookup"><span data-stu-id="bc99e-165">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="bc99e-166">90 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-166">90 seconds</span></span> | <span data-ttu-id="bc99e-167">Максимальное время ожидания сервером ответа сообщение, отправляемое клиенту перед завершением работы запрос на единый опроса.</span><span class="sxs-lookup"><span data-stu-id="bc99e-167">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="bc99e-168">Уменьшение этого значения приводит к клиентам выполнять чаще, новые запросы опроса.</span><span class="sxs-lookup"><span data-stu-id="bc99e-168">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="bc99e-169">Транспорт WebSocket имеет дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:</span><span class="sxs-lookup"><span data-stu-id="bc99e-169">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="bc99e-170">Параметр</span><span class="sxs-lookup"><span data-stu-id="bc99e-170">Option</span></span> | <span data-ttu-id="bc99e-171">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bc99e-171">Default Value</span></span> | <span data-ttu-id="bc99e-172">Описание:</span><span class="sxs-lookup"><span data-stu-id="bc99e-172">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="bc99e-173">5 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-173">5 seconds</span></span> | <span data-ttu-id="bc99e-174">После завершения работы сервера, если клиенту не удается закрыть в течение этого времени интервала, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="bc99e-174">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="bc99e-175">Делегат, который может использоваться для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение.</span><span class="sxs-lookup"><span data-stu-id="bc99e-175">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="bc99e-176">Делегат получает значения, запрошенной клиентом в качестве входных данных и должен возвращать необходимое значение.</span><span class="sxs-lookup"><span data-stu-id="bc99e-176">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="bc99e-177">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="bc99e-177">Configure client options</span></span>

<span data-ttu-id="bc99e-178">Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступна в клиентах .NET и JavaScript), а также в `HubConnection` сам.</span><span class="sxs-lookup"><span data-stu-id="bc99e-178">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="bc99e-179">Настройка ведения журнала</span><span class="sxs-lookup"><span data-stu-id="bc99e-179">Configure logging</span></span>

<span data-ttu-id="bc99e-180">Настроить ведение журнала с помощью клиента .NET `ConfigureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="bc99e-180">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="bc99e-181">Ведение журнала, поставщики и фильтры могут быть зарегистрированы в так же как при работе на сервере.</span><span class="sxs-lookup"><span data-stu-id="bc99e-181">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="bc99e-182">См. в разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="bc99e-182">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="bc99e-183">Чтобы зарегистрировать поставщики ведения журналов, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="bc99e-183">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="bc99e-184">См. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) разделе документы с полным списком.</span><span class="sxs-lookup"><span data-stu-id="bc99e-184">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="bc99e-185">Например, чтобы включить ведение журнала консоли, установить `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="bc99e-185">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="bc99e-186">Вызовите `AddConsole` метод расширения:</span><span class="sxs-lookup"><span data-stu-id="bc99e-186">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="bc99e-187">В клиенте JavaScript, аналогичное `configureLogging` метод существует.</span><span class="sxs-lookup"><span data-stu-id="bc99e-187">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="bc99e-188">Укажите `LogLevel` значение, указывающее минимальный уровень журнала сообщений для получения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-188">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="bc99e-189">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="bc99e-189">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="bc99e-190">Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="bc99e-190">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="bc99e-191">Ниже перечислены уровни ведения журнала, доступные для клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="bc99e-191">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="bc99e-192">Установка уровня журнала в одно из следующих значений включает ведение журнала сообщений в **или более поздней версии** этого уровня.</span><span class="sxs-lookup"><span data-stu-id="bc99e-192">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="bc99e-193">Уровень</span><span class="sxs-lookup"><span data-stu-id="bc99e-193">Level</span></span> | <span data-ttu-id="bc99e-194">Описание</span><span class="sxs-lookup"><span data-stu-id="bc99e-194">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="bc99e-195">Сообщения не регистрируются.</span><span class="sxs-lookup"><span data-stu-id="bc99e-195">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="bc99e-196">Сообщения, которые означают сбой всего приложения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-196">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="bc99e-197">Сообщения, которые указывают на сбой в текущей операции.</span><span class="sxs-lookup"><span data-stu-id="bc99e-197">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="bc99e-198">Сообщения, которые указывают на проблему, не являющиеся неустранимыми.</span><span class="sxs-lookup"><span data-stu-id="bc99e-198">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="bc99e-199">Информационные сообщения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-199">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="bc99e-200">Диагностические сообщения, полезны для отладки.</span><span class="sxs-lookup"><span data-stu-id="bc99e-200">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="bc99e-201">Очень подробные диагностические сообщения, предназначены для диагностики конкретных проблем.</span><span class="sxs-lookup"><span data-stu-id="bc99e-201">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="bc99e-202">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="bc99e-202">Configure allowed transports</span></span>

<span data-ttu-id="bc99e-203">Можно настроить транспорт, используемый с SignalR в `WithUrl` вызова (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="bc99e-203">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="bc99e-204">Побитовое или-значения `HttpTransportType` может использоваться для ограничения клиента на использование только указанного транспорта.</span><span class="sxs-lookup"><span data-stu-id="bc99e-204">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="bc99e-205">Все транспорты включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="bc99e-205">All transports are enabled by default.</span></span>

<span data-ttu-id="bc99e-206">Например чтобы отключить события Server-Sent транспорта, но подключений WebSockets и длинный опрос:</span><span class="sxs-lookup"><span data-stu-id="bc99e-206">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="bc99e-207">В клиенте JavaScript транспортов настраиваются, задав `transport` на объект параметры, передаваемые `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="bc99e-207">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="bc99e-208">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="bc99e-208">Configure bearer authentication</span></span>

<span data-ttu-id="bc99e-209">Чтобы указать данные проверки подлинности, а также SignalR запросов, используйте `AccessTokenProvider` параметр (`accessTokenFactory` в JavaScript) для указания функция, которая возвращает маркер необходимый тип доступа.</span><span class="sxs-lookup"><span data-stu-id="bc99e-209">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="bc99e-210">В клиенте .NET, этот маркер передается как HTTP «Проверки подлинности носителя» токена (с помощью `Authorization` заголовка с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="bc99e-210">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="bc99e-211">В клиенте JavaScript используется маркер доступа в качестве токена носителя, **за исключением** в некоторых случаях, где обозреватель API-интерфейсы ограничить возможность применения заголовки (в частности, в запросы событий Server-Sent и WebSockets).</span><span class="sxs-lookup"><span data-stu-id="bc99e-211">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="bc99e-212">В этих случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="bc99e-212">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="bc99e-213">В клиенте .NET `AccessTokenProvider` параметр может быть указан с помощью делегата параметры в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="bc99e-213">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="bc99e-214">В клиенте JavaScript настроен маркер доступа, задав `accessTokenFactory` на объект параметров в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="bc99e-214">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="bc99e-215">Настройка времени ожидания и параметры проверки активности</span><span class="sxs-lookup"><span data-stu-id="bc99e-215">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="bc99e-216">Доступны дополнительные параметры для настройки времени ожидания и поведение активности на `HubConnection` сам объект:</span><span class="sxs-lookup"><span data-stu-id="bc99e-216">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="bc99e-217">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="bc99e-217">.NET Option</span></span> | <span data-ttu-id="bc99e-218">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="bc99e-218">JavaScript Option</span></span> | <span data-ttu-id="bc99e-219">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bc99e-219">Default Value</span></span> | <span data-ttu-id="bc99e-220">Описание:</span><span class="sxs-lookup"><span data-stu-id="bc99e-220">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="bc99e-221">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="bc99e-221">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="bc99e-222">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="bc99e-222">Timeout for server activity.</span></span> <span data-ttu-id="bc99e-223">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="bc99e-223">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="bc99e-224">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="bc99e-224">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="bc99e-225">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-225">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="bc99e-226">Недоступно для настройки</span><span class="sxs-lookup"><span data-stu-id="bc99e-226">Not configurable</span></span> | <span data-ttu-id="bc99e-227">15 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-227">15 seconds</span></span> | <span data-ttu-id="bc99e-228">Время ожидания подтверждения исходным сервером.</span><span class="sxs-lookup"><span data-stu-id="bc99e-228">Timeout for initial server handshake.</span></span> <span data-ttu-id="bc99e-229">Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="bc99e-229">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="bc99e-230">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-230">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="bc99e-231">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="bc99e-231">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="bc99e-232">В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="bc99e-232">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="bc99e-233">В клиенте JavaScript значения времени ожидания задаются как число, указывающее длительность в миллисекундах.</span><span class="sxs-lookup"><span data-stu-id="bc99e-233">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="bc99e-234">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="bc99e-234">Configure additional options</span></span>

<span data-ttu-id="bc99e-235">Дополнительные параметры можно настроить в `WithUrl` (`withUrl` в JavaScript) метод `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="bc99e-235">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="bc99e-236">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="bc99e-236">.NET Option</span></span> | <span data-ttu-id="bc99e-237">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="bc99e-237">JavaScript Option</span></span> | <span data-ttu-id="bc99e-238">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="bc99e-238">Default Value</span></span> | <span data-ttu-id="bc99e-239">Описание:</span><span class="sxs-lookup"><span data-stu-id="bc99e-239">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="bc99e-240">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="bc99e-240">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="bc99e-241">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="bc99e-241">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="bc99e-242">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="bc99e-242">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="bc99e-243">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="bc99e-243">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="bc99e-244">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-244">Not configurable \*</span></span> | <span data-ttu-id="bc99e-245">Empty</span><span class="sxs-lookup"><span data-stu-id="bc99e-245">Empty</span></span> | <span data-ttu-id="bc99e-246">Коллекция TLS-сертификатов, отправляемых для проверки подлинности запросов.</span><span class="sxs-lookup"><span data-stu-id="bc99e-246">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="bc99e-247">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-247">Not configurable \*</span></span> | <span data-ttu-id="bc99e-248">Empty</span><span class="sxs-lookup"><span data-stu-id="bc99e-248">Empty</span></span> | <span data-ttu-id="bc99e-249">Коллекция файлов cookie HTTP для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc99e-249">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="bc99e-250">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-250">Not configurable \*</span></span> | <span data-ttu-id="bc99e-251">Empty</span><span class="sxs-lookup"><span data-stu-id="bc99e-251">Empty</span></span> | <span data-ttu-id="bc99e-252">Учетные данные для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc99e-252">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="bc99e-253">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-253">Not configurable \*</span></span> | <span data-ttu-id="bc99e-254">5 секунд</span><span class="sxs-lookup"><span data-stu-id="bc99e-254">5 seconds</span></span> | <span data-ttu-id="bc99e-255">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="bc99e-255">WebSockets only.</span></span> <span data-ttu-id="bc99e-256">Максимальное количество времени, клиент ожидает после закрывающего тега для сервера подтвердить запрос на закрытие.</span><span class="sxs-lookup"><span data-stu-id="bc99e-256">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="bc99e-257">Если сервер не подтвердил закрытия в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="bc99e-257">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="bc99e-258">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-258">Not configurable \*</span></span> | <span data-ttu-id="bc99e-259">Empty</span><span class="sxs-lookup"><span data-stu-id="bc99e-259">Empty</span></span> | <span data-ttu-id="bc99e-260">Словарь дополнительных HTTP-заголовков для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc99e-260">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="bc99e-261">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-261">Not configurable \*</span></span> | `null` | <span data-ttu-id="bc99e-262">Делегат, который может использоваться для настройки или замените `HttpMessageHandler` используется для отправки запросов HTTP.</span><span class="sxs-lookup"><span data-stu-id="bc99e-262">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="bc99e-263">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bc99e-263">Not used for WebSocket connections.</span></span> <span data-ttu-id="bc99e-264">Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="bc99e-264">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="bc99e-265">Измените параметры на это значение по умолчанию и вернуть его или возвращают новый `HttpMessageHandler` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="bc99e-265">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="bc99e-266">**Если заменить обработчик обязательно скопируйте параметры, которые вы хотите сохранить из обработчика, в противном случае настроенные параметры (например, файлы cookie и заголовки) нельзя применить новый обработчик.**</span><span class="sxs-lookup"><span data-stu-id="bc99e-266">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | <span data-ttu-id="bc99e-267">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-267">Not configurable \*</span></span> | `null` | <span data-ttu-id="bc99e-268">HTTP-прокси для использования при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="bc99e-268">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="bc99e-269">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-269">Not configurable \*</span></span> | `false` | <span data-ttu-id="bc99e-270">Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="bc99e-270">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="bc99e-271">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="bc99e-271">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="bc99e-272">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="bc99e-272">Not configurable \*</span></span> | `null` | <span data-ttu-id="bc99e-273">Делегат, который может использоваться для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="bc99e-273">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="bc99e-274">Получает экземпляр класса [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="bc99e-274">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="bc99e-275">Параметры, отмеченные звездочкой (\*) не может быть настроено в клиенте JavaScript из-за ограничений в браузере API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="bc99e-275">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="bc99e-276">В клиенте .NET эти параметры можно изменить параметры делегат для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="bc99e-276">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="bc99e-277">В клиенте JavaScript, эти параметры могут быть предоставлены в JavaScript объект, предоставляемый для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="bc99e-277">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="bc99e-278">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="bc99e-278">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
