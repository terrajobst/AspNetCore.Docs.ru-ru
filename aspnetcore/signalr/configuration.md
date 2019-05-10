---
title: Конфигурация ASP.NET Core SignalR
author: bradygaster
description: Сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/15/2019
uid: signalr/configuration
ms.openlocfilehash: 703357fd52805e01515e5bac3b1a364ce7fe00f0
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087654"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="05e85-103">Конфигурация ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="05e85-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="05e85-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="05e85-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="05e85-105">ASP.NET Core SignalR поддерживает два протокола для кодировки сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="05e85-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="05e85-106">Каждый протокол имеет параметры конфигурации для сериализации.</span><span class="sxs-lookup"><span data-stu-id="05e85-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="05e85-107">Сериализация JSON можно настроить на сервере, используя [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) метод расширения, которую можно добавить после [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в вашей `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="05e85-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="05e85-108">`AddJsonProtocol` Метод принимает делегат, который получает `options` объекта.</span><span class="sxs-lookup"><span data-stu-id="05e85-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="05e85-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) свойство на этот объект является JSON.NET `JsonSerializerSettings` объект, который может использоваться для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="05e85-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="05e85-110">См. в разделе [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="05e85-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="05e85-111">Например чтобы настроить сериализатор, используемый имена свойств «PascalCase», а не имена «camelCase» по умолчанию, используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="05e85-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

<span data-ttu-id="05e85-112">В клиента .NET, так же `AddJsonProtocol` метод расширения существует на [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="05e85-112">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="05e85-113">`Microsoft.Extensions.DependencyInjection` Пространства имен должны импортироваться разрешить метод расширения:</span><span class="sxs-lookup"><span data-stu-id="05e85-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="05e85-114">Не поддерживается для настройки сериализации JSON в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="05e85-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="05e85-115">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="05e85-115">MessagePack serialization options</span></span>

<span data-ttu-id="05e85-116">MessagePack сериализации можно настроить, предоставьте делегат для [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) вызова.</span><span class="sxs-lookup"><span data-stu-id="05e85-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="05e85-117">См. в разделе [MessagePack в SignalR](xref:signalr/messagepackhubprotocol) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="05e85-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="05e85-118">Не поддерживается для настройки сериализации MessagePack в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="05e85-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="05e85-119">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="05e85-119">Configure server options</span></span>

<span data-ttu-id="05e85-120">В следующей таблице описаны параметры для настройки концентраторов SignalR:</span><span class="sxs-lookup"><span data-stu-id="05e85-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="05e85-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-121">Option</span></span> | <span data-ttu-id="05e85-122">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-122">Default Value</span></span> | <span data-ttu-id="05e85-123">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="05e85-124">30 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-124">30 seconds</span></span> | <span data-ttu-id="05e85-125">Сервер будет рассматривать клиента отключен, если он еще не получил сообщение (включая keep-alive) в этом интервале.</span><span class="sxs-lookup"><span data-stu-id="05e85-125">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="05e85-126">Может занять больше времени, чем этот интервал времени ожидания для клиента, фактически помечается как отключенный, из-за, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="05e85-126">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="05e85-127">Рекомендуемое значение — double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="05e85-127">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="05e85-128">15 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-128">15 seconds</span></span> | <span data-ttu-id="05e85-129">Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="05e85-129">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="05e85-130">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="05e85-130">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="05e85-131">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="05e85-131">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="05e85-132">15 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-132">15 seconds</span></span> | <span data-ttu-id="05e85-133">Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение.</span><span class="sxs-lookup"><span data-stu-id="05e85-133">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="05e85-134">При изменении `KeepAliveInterval`, изменить `ServerTimeout` / `serverTimeoutInMilliseconds` параметр на клиенте.</span><span class="sxs-lookup"><span data-stu-id="05e85-134">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="05e85-135">Минимальная рекомендуемая `ServerTimeout` / `serverTimeoutInMilliseconds` значение double `KeepAliveInterval` значение.</span><span class="sxs-lookup"><span data-stu-id="05e85-135">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="05e85-136">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="05e85-136">All installed protocols</span></span> | <span data-ttu-id="05e85-137">Протоколы, поддерживаемые этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="05e85-137">Protocols supported by this hub.</span></span> <span data-ttu-id="05e85-138">По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="05e85-138">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="05e85-139">Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="05e85-139">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="05e85-140">По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="05e85-140">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="05e85-141">Можно настроить параметры для всех концентраторов, предоставляя делегат параметры для `AddSignalR` вызов в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="05e85-141">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="05e85-142">Параметры для одного центра переопределить глобальные параметры, указанные на `AddSignalR` и могут настраиваться с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="05e85-142">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="05e85-143">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="05e85-143">Advanced HTTP configuration options</span></span>

<span data-ttu-id="05e85-144">Используйте `HttpConnectionDispatcherOptions` для расширенной настройки, относящиеся к транспортов и управление буфером памяти.</span><span class="sxs-lookup"><span data-stu-id="05e85-144">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="05e85-145">Эти параметры настраиваются путем передачи делегата для [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="05e85-145">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="05e85-146">В следующей таблице описаны параметры для настройки дополнительных параметров HTTP ASP.NET Core SignalR:</span><span class="sxs-lookup"><span data-stu-id="05e85-146">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="05e85-147">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-147">Option</span></span> | <span data-ttu-id="05e85-148">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-148">Default Value</span></span> | <span data-ttu-id="05e85-149">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-149">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="05e85-150">32 КБ</span><span class="sxs-lookup"><span data-stu-id="05e85-150">32 KB</span></span> | <span data-ttu-id="05e85-151">Максимальное число байтов, полученных от клиента, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="05e85-151">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="05e85-152">Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="05e85-152">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="05e85-153">Данные, собранные из автоматически `Authorize` атрибутов, примененных к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="05e85-153">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="05e85-154">Список [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="05e85-154">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="05e85-155">32 КБ</span><span class="sxs-lookup"><span data-stu-id="05e85-155">32 KB</span></span> | <span data-ttu-id="05e85-156">Максимальное число байтов, отправленных в приложение, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="05e85-156">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="05e85-157">Увеличение этого значения позволяет серверу для отправки сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="05e85-157">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="05e85-158">Включены все транспорты.</span><span class="sxs-lookup"><span data-stu-id="05e85-158">All Transports are enabled.</span></span> | <span data-ttu-id="05e85-159">Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="05e85-159">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="05e85-160">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="05e85-160">See below.</span></span> | <span data-ttu-id="05e85-161">Дополнительные параметры, характерные для транспорта длинный опрос.</span><span class="sxs-lookup"><span data-stu-id="05e85-161">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="05e85-162">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="05e85-162">See below.</span></span> | <span data-ttu-id="05e85-163">Дополнительные параметры, характерные для транспорта WebSockets.</span><span class="sxs-lookup"><span data-stu-id="05e85-163">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="05e85-164">Транспорт длинный опрос имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:</span><span class="sxs-lookup"><span data-stu-id="05e85-164">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="05e85-165">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-165">Option</span></span> | <span data-ttu-id="05e85-166">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-166">Default Value</span></span> | <span data-ttu-id="05e85-167">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-167">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="05e85-168">90 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-168">90 seconds</span></span> | <span data-ttu-id="05e85-169">Максимальное время ожидания сервером ответа сообщение, отправляемое клиенту перед завершением работы запрос на единый опроса.</span><span class="sxs-lookup"><span data-stu-id="05e85-169">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="05e85-170">Уменьшение этого значения приводит к клиентам выполнять чаще, новые запросы опроса.</span><span class="sxs-lookup"><span data-stu-id="05e85-170">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="05e85-171">Транспорт WebSocket имеет дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:</span><span class="sxs-lookup"><span data-stu-id="05e85-171">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="05e85-172">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-172">Option</span></span> | <span data-ttu-id="05e85-173">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-173">Default Value</span></span> | <span data-ttu-id="05e85-174">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="05e85-175">5 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-175">5 seconds</span></span> | <span data-ttu-id="05e85-176">После завершения работы сервера, если клиенту не удается закрыть в течение этого времени интервала, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="05e85-176">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="05e85-177">Делегат, который может использоваться для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение.</span><span class="sxs-lookup"><span data-stu-id="05e85-177">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="05e85-178">Делегат получает значения, запрошенной клиентом в качестве входных данных и должен возвращать необходимое значение.</span><span class="sxs-lookup"><span data-stu-id="05e85-178">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="05e85-179">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="05e85-179">Configure client options</span></span>

<span data-ttu-id="05e85-180">Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступна в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="05e85-180">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="05e85-181">Она также доступна в клиенте Java, но `HttpHubConnectionBuilder` производным классом является то, что содержит построитель параметры конфигурации, а также на `HubConnection` сам.</span><span class="sxs-lookup"><span data-stu-id="05e85-181">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="05e85-182">Настройка ведения журнала</span><span class="sxs-lookup"><span data-stu-id="05e85-182">Configure logging</span></span>

<span data-ttu-id="05e85-183">Настроить ведение журнала с помощью клиента .NET `ConfigureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="05e85-183">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="05e85-184">Ведение журнала, поставщики и фильтры могут быть зарегистрированы в так же как при работе на сервере.</span><span class="sxs-lookup"><span data-stu-id="05e85-184">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="05e85-185">См. в разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="05e85-185">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="05e85-186">Чтобы зарегистрировать поставщики ведения журналов, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="05e85-186">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="05e85-187">См. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) разделе документы с полным списком.</span><span class="sxs-lookup"><span data-stu-id="05e85-187">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="05e85-188">Например, чтобы включить ведение журнала консоли, установить `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="05e85-188">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="05e85-189">Вызовите `AddConsole` метод расширения:</span><span class="sxs-lookup"><span data-stu-id="05e85-189">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="05e85-190">В клиенте JavaScript, аналогичное `configureLogging` метод существует.</span><span class="sxs-lookup"><span data-stu-id="05e85-190">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="05e85-191">Укажите `LogLevel` значение, указывающее минимальный уровень журнала сообщений для получения.</span><span class="sxs-lookup"><span data-stu-id="05e85-191">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="05e85-192">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="05e85-192">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05e85-193">Вместо `LogLevel` значение, вы также можете предоставить `string` значение, представляющее имя уровня журнала.</span><span class="sxs-lookup"><span data-stu-id="05e85-193">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="05e85-194">Это полезно при настройке SignalR, ведение журнала в средах, где у вас нет доступа к `LogLevel` константы.</span><span class="sxs-lookup"><span data-stu-id="05e85-194">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="05e85-195">Ниже перечислены уровни ведения журнала доступны.</span><span class="sxs-lookup"><span data-stu-id="05e85-195">The following table lists the available log levels.</span></span> <span data-ttu-id="05e85-196">Значение, вы предоставляете `configureLogging` задает **минимальное** уровень, который будет записываться ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="05e85-196">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="05e85-197">Сообщения, регистрируемые на этом уровне **или уровни, перечисленных в таблице после него**, будут регистрироваться.</span><span class="sxs-lookup"><span data-stu-id="05e85-197">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="05e85-198">string</span><span class="sxs-lookup"><span data-stu-id="05e85-198">string</span></span> | <span data-ttu-id="05e85-199">LogLevel</span><span class="sxs-lookup"><span data-stu-id="05e85-199">LogLevel</span></span> |
| - | - |
| `"trace"` | `LogLevel.Trace` |
| `"debug"` | `LogLevel.Debug` |
| <span data-ttu-id="05e85-200">`"info"` **Или** `"information"`</span><span class="sxs-lookup"><span data-stu-id="05e85-200">`"info"` **or** `"information"`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="05e85-201">`"warn"` **Или** `"warning"`</span><span class="sxs-lookup"><span data-stu-id="05e85-201">`"warn"` **or** `"warning"`</span></span> | `LogLevel.Warning` |
| `"error"` | `LogLevel.Error` |
| `"critical"` | `LogLevel.Critical` |
| `"none"` | `LogLevel.None` |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="05e85-202">Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="05e85-202">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="05e85-203">Дополнительные сведения о ведении журнала см. в разделе [документации диагностики SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="05e85-203">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="05e85-204">Клиент SignalR Java использует [SLF4J](https://www.slf4j.org/) библиотеки для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="05e85-204">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="05e85-205">Это API высокого уровня ведения журнала, который позволяет пользователям библиотеки выбрал свою собственную реализацию определенного ведения журнала, Собрав воедино в зависимость от конкретных ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="05e85-205">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="05e85-206">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="05e85-206">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="05e85-207">Если не настроить ведение журнала в зависимости, SLF4J загружает средство ведения журнала по умолчанию без операции с следующее предупреждающее сообщение:</span><span class="sxs-lookup"><span data-stu-id="05e85-207">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="05e85-208">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="05e85-208">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="05e85-209">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="05e85-209">Configure allowed transports</span></span>

<span data-ttu-id="05e85-210">Можно настроить транспорт, используемый с SignalR в `WithUrl` вызова (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="05e85-210">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="05e85-211">Побитовое или-значения `HttpTransportType` может использоваться для ограничения клиента на использование только указанного транспорта.</span><span class="sxs-lookup"><span data-stu-id="05e85-211">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="05e85-212">Все транспорты включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="05e85-212">All transports are enabled by default.</span></span>

<span data-ttu-id="05e85-213">Например чтобы отключить события Server-Sent транспорта, но подключений WebSockets и длинный опрос:</span><span class="sxs-lookup"><span data-stu-id="05e85-213">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="05e85-214">В клиенте JavaScript транспортов настраиваются, задав `transport` на объект параметры, передаваемые `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="05e85-214">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="05e85-215">В этой версии Java клиента websockets-это доступно только транспорт.</span><span class="sxs-lookup"><span data-stu-id="05e85-215">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="05e85-216">В клиенте Java, следует выбрать транспорт `withTransport` метод `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="05e85-216">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="05e85-217">В клиенте Java по умолчанию с помощью транспорта WebSockets.</span><span class="sxs-lookup"><span data-stu-id="05e85-217">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="05e85-218">Клиент SignalR Java не поддерживает резервный транспорт.</span><span class="sxs-lookup"><span data-stu-id="05e85-218">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="05e85-219">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="05e85-219">Configure bearer authentication</span></span>

<span data-ttu-id="05e85-220">Чтобы указать данные проверки подлинности, а также SignalR запросов, используйте `AccessTokenProvider` параметр (`accessTokenFactory` в JavaScript) для указания функция, которая возвращает маркер необходимый тип доступа.</span><span class="sxs-lookup"><span data-stu-id="05e85-220">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="05e85-221">В клиенте .NET, этот маркер передается как HTTP «Проверки подлинности носителя» токена (с помощью `Authorization` заголовка с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="05e85-221">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="05e85-222">В клиенте JavaScript используется маркер доступа в качестве токена носителя, **за исключением** в некоторых случаях, где обозреватель API-интерфейсы ограничить возможность применения заголовки (в частности, в запросы событий Server-Sent и WebSockets).</span><span class="sxs-lookup"><span data-stu-id="05e85-222">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="05e85-223">В этих случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="05e85-223">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="05e85-224">В клиенте .NET `AccessTokenProvider` параметр может быть указан с помощью делегата параметры в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="05e85-224">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="05e85-225">В клиенте JavaScript настроен маркер доступа, задав `accessTokenFactory` на объект параметров в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="05e85-225">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="05e85-226">В клиенте SignalR Java, можно настроить токен носителя для проверки подлинности, предоставляя фабрику токена доступа для [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="05e85-226">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="05e85-227">Используйте [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [RxJava](https://github.com/ReactiveX/RxJava) [единый\<строка >](http://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="05e85-227">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](http://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="05e85-228">С помощью вызова [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), можно написать логику для получения маркеров доступа для вашего клиента.</span><span class="sxs-lookup"><span data-stu-id="05e85-228">With a call to [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="05e85-229">Настройка времени ожидания и параметры проверки активности</span><span class="sxs-lookup"><span data-stu-id="05e85-229">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="05e85-230">Доступны дополнительные параметры для настройки времени ожидания и поведение активности на `HubConnection` сам объект:</span><span class="sxs-lookup"><span data-stu-id="05e85-230">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="05e85-231">.NET</span><span class="sxs-lookup"><span data-stu-id="05e85-231">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="05e85-232">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-232">Option</span></span> | <span data-ttu-id="05e85-233">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-233">Default value</span></span> | <span data-ttu-id="05e85-234">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-234">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="05e85-235">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="05e85-235">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="05e85-236">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="05e85-236">Timeout for server activity.</span></span> <span data-ttu-id="05e85-237">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="05e85-237">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="05e85-238">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="05e85-238">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="05e85-239">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="05e85-239">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="05e85-240">15 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-240">15 seconds</span></span> | <span data-ttu-id="05e85-241">Время ожидания подтверждения исходным сервером.</span><span class="sxs-lookup"><span data-stu-id="05e85-241">Timeout for initial server handshake.</span></span> <span data-ttu-id="05e85-242">Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="05e85-242">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="05e85-243">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="05e85-243">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="05e85-244">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="05e85-244">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="05e85-245">В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="05e85-245">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="05e85-246">JavaScript</span><span class="sxs-lookup"><span data-stu-id="05e85-246">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="05e85-247">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-247">Option</span></span> | <span data-ttu-id="05e85-248">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-248">Default value</span></span> | <span data-ttu-id="05e85-249">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-249">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="05e85-250">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="05e85-250">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="05e85-251">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="05e85-251">Timeout for server activity.</span></span> <span data-ttu-id="05e85-252">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `onclose` событий.</span><span class="sxs-lookup"><span data-stu-id="05e85-252">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="05e85-253">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="05e85-253">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="05e85-254">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="05e85-254">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="05e85-255">Java</span><span class="sxs-lookup"><span data-stu-id="05e85-255">Java</span></span>](#tab/java)

| <span data-ttu-id="05e85-256">Параметр</span><span class="sxs-lookup"><span data-stu-id="05e85-256">Option</span></span> | <span data-ttu-id="05e85-257">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-257">Default value</span></span> | <span data-ttu-id="05e85-258">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-258">Description</span></span> |
| ----------- | ------------- | ----------- |
|<span data-ttu-id="05e85-259">`getServerTimeout` `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="05e85-259">`getServerTimeout` `setServerTimeout`</span></span> | <span data-ttu-id="05e85-260">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="05e85-260">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="05e85-261">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="05e85-261">Timeout for server activity.</span></span> <span data-ttu-id="05e85-262">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `onClose` событий.</span><span class="sxs-lookup"><span data-stu-id="05e85-262">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="05e85-263">Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="05e85-263">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="05e85-264">Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения.</span><span class="sxs-lookup"><span data-stu-id="05e85-264">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="05e85-265">15 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-265">15 seconds</span></span> | <span data-ttu-id="05e85-266">Время ожидания подтверждения исходным сервером.</span><span class="sxs-lookup"><span data-stu-id="05e85-266">Timeout for initial server handshake.</span></span> <span data-ttu-id="05e85-267">Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `onClose` событий.</span><span class="sxs-lookup"><span data-stu-id="05e85-267">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="05e85-268">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="05e85-268">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="05e85-269">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="05e85-269">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="05e85-270">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="05e85-270">Configure additional options</span></span>

<span data-ttu-id="05e85-271">Дополнительные параметры можно настроить в `WithUrl` (`withUrl` в JavaScript) метод `HubConnectionBuilder` или на различных интерфейсов API настройки на `HttpHubConnectionBuilder` в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="05e85-271">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="05e85-272">.NET</span><span class="sxs-lookup"><span data-stu-id="05e85-272">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="05e85-273">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="05e85-273">.NET Option</span></span> |  <span data-ttu-id="05e85-274">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-274">Default value</span></span> | <span data-ttu-id="05e85-275">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-275">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="05e85-276">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="05e85-276">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="05e85-277">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="05e85-277">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="05e85-278">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="05e85-278">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="05e85-279">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="05e85-279">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="05e85-280">Empty</span><span class="sxs-lookup"><span data-stu-id="05e85-280">Empty</span></span> | <span data-ttu-id="05e85-281">Коллекция TLS-сертификатов, отправляемых для проверки подлинности запросов.</span><span class="sxs-lookup"><span data-stu-id="05e85-281">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="05e85-282">Empty</span><span class="sxs-lookup"><span data-stu-id="05e85-282">Empty</span></span> | <span data-ttu-id="05e85-283">Коллекция файлов cookie HTTP для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="05e85-283">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="05e85-284">Empty</span><span class="sxs-lookup"><span data-stu-id="05e85-284">Empty</span></span> | <span data-ttu-id="05e85-285">Учетные данные для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="05e85-285">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="05e85-286">5 секунд</span><span class="sxs-lookup"><span data-stu-id="05e85-286">5 seconds</span></span> | <span data-ttu-id="05e85-287">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="05e85-287">WebSockets only.</span></span> <span data-ttu-id="05e85-288">Максимальное количество времени, клиент ожидает после закрывающего тега для сервера подтвердить запрос на закрытие.</span><span class="sxs-lookup"><span data-stu-id="05e85-288">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="05e85-289">Если сервер не подтвердил закрытия в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="05e85-289">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="05e85-290">Empty</span><span class="sxs-lookup"><span data-stu-id="05e85-290">Empty</span></span> | <span data-ttu-id="05e85-291">Сопоставление дополнительных HTTP-заголовков для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="05e85-291">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="05e85-292">Делегат, который может использоваться для настройки или замените `HttpMessageHandler` используется для отправки запросов HTTP.</span><span class="sxs-lookup"><span data-stu-id="05e85-292">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="05e85-293">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="05e85-293">Not used for WebSocket connections.</span></span> <span data-ttu-id="05e85-294">Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="05e85-294">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="05e85-295">Измените параметры на это значение по умолчанию и вернуть его или возвращают новый `HttpMessageHandler` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="05e85-295">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="05e85-296">**Если заменить обработчик обязательно скопируйте параметры, которые вы хотите сохранить из обработчика, в противном случае настроенные параметры (например, файлы cookie и заголовки) нельзя применить новый обработчик.**</span><span class="sxs-lookup"><span data-stu-id="05e85-296">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="05e85-297">HTTP-прокси для использования при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="05e85-297">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="05e85-298">Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="05e85-298">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="05e85-299">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="05e85-299">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="05e85-300">Делегат, который может использоваться для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="05e85-300">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="05e85-301">Получает экземпляр класса [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="05e85-301">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="05e85-302">JavaScript</span><span class="sxs-lookup"><span data-stu-id="05e85-302">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="05e85-303">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="05e85-303">JavaScript Option</span></span> | <span data-ttu-id="05e85-304">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-304">Default Value</span></span> | <span data-ttu-id="05e85-305">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-305">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="05e85-306">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="05e85-306">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="05e85-307">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="05e85-307">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="05e85-308">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="05e85-308">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="05e85-309">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="05e85-309">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="05e85-310">Java</span><span class="sxs-lookup"><span data-stu-id="05e85-310">Java</span></span>](#tab/java)

| <span data-ttu-id="05e85-311">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="05e85-311">Java Option</span></span> | <span data-ttu-id="05e85-312">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="05e85-312">Default Value</span></span> | <span data-ttu-id="05e85-313">Описание</span><span class="sxs-lookup"><span data-stu-id="05e85-313">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="05e85-314">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="05e85-314">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="05e85-315">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="05e85-315">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="05e85-316">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="05e85-316">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="05e85-317">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="05e85-317">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="05e85-318">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="05e85-318">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="05e85-319">Empty</span><span class="sxs-lookup"><span data-stu-id="05e85-319">Empty</span></span> | <span data-ttu-id="05e85-320">Сопоставление дополнительных HTTP-заголовков для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="05e85-320">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="05e85-321">В клиенте .NET эти параметры можно изменить параметры делегат для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="05e85-321">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="05e85-322">В клиенте JavaScript, эти параметры могут быть предоставлены в JavaScript объект, предоставляемый для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="05e85-322">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="05e85-323">В клиенте Java, эти параметры можно настроить на с методами `HttpHubConnectionBuilder` возвращаемые `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="05e85-323">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="05e85-324">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="05e85-324">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
