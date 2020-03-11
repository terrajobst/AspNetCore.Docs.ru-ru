---
title: Конфигурация SignalR ASP.NET Core
author: bradygaster
description: Узнайте, как настроить ASP.NET Core SignalR приложений.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 12/10/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: c225ff88110dc17185a430ac1c422d2433306115
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78651688"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="60a05-103">Конфигурация SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="60a05-103">ASP.NET Core SignalR configuration</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="60a05-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="60a05-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="60a05-105">ASP.NET Core SignalR поддерживает два протокола кодирования сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="60a05-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="60a05-106">Для каждого протокола предусмотрены параметры конфигурации сериализации.</span><span class="sxs-lookup"><span data-stu-id="60a05-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="60a05-107">Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="60a05-108">`AddJsonProtocol` можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="60a05-109">Метод `AddJsonProtocol` принимает делегат, который получает объект `options`.</span><span class="sxs-lookup"><span data-stu-id="60a05-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="60a05-110">Свойство [пайлоадсериализероптионс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) этого объекта является объектом `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>, который можно использовать для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="60a05-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="60a05-111">Дополнительные сведения см. в [документации System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="60a05-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="60a05-112">Например, чтобы настроить в сериализаторе не изменять регистр имен свойств, вместо имен по умолчанию используйте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="60a05-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="60a05-113">В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="60a05-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="60a05-114">Для разрешения метода расширения необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.</span><span class="sxs-lookup"><span data-stu-id="60a05-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> <span data-ttu-id="60a05-115">В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60a05-115">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="60a05-116">Переключиться на Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="60a05-116">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="60a05-117">Если вам нужны функции `Newtonsoft.Json`, которые не поддерживаются в `System.Text.Json`, см. раздел [Переключение на Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="60a05-117">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="60a05-118">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="60a05-118">MessagePack serialization options</span></span>

<span data-ttu-id="60a05-119">MessagePack сериализацию можно настроить, предоставив делегат вызову [аддмессажепаккпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-119">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="60a05-120">Дополнительные сведения см. [в разделе MessagePack в SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-120">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="60a05-121">В настоящее время невозможно настроить сериализацию MessagePack в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60a05-121">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="60a05-122">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="60a05-122">Configure server options</span></span>

<span data-ttu-id="60a05-123">В следующей таблице описаны параметры для настройки концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-123">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="60a05-124">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-124">Option</span></span> | <span data-ttu-id="60a05-125">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-125">Default Value</span></span> | <span data-ttu-id="60a05-126">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-126">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="60a05-127">30 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-127">30 seconds</span></span> | <span data-ttu-id="60a05-128">Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности").</span><span class="sxs-lookup"><span data-stu-id="60a05-128">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="60a05-129">Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="60a05-129">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="60a05-130">Рекомендуемое значение — это значение типа Double, равное `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="60a05-130">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="60a05-131">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-131">15 seconds</span></span> | <span data-ttu-id="60a05-132">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="60a05-132">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="60a05-133">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-133">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-134">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-134">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="60a05-135">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-135">15 seconds</span></span> | <span data-ttu-id="60a05-136">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="60a05-136">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="60a05-137">При изменении `KeepAliveInterval`измените параметр `ServerTimeout`/`serverTimeoutInMilliseconds` на клиенте.</span><span class="sxs-lookup"><span data-stu-id="60a05-137">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="60a05-138">Рекомендуемое `ServerTimeout`/`serverTimeoutInMilliseconds` является двойным значением `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="60a05-138">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="60a05-139">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="60a05-139">All installed protocols</span></span> | <span data-ttu-id="60a05-140">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="60a05-140">Protocols supported by this hub.</span></span> <span data-ttu-id="60a05-141">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="60a05-141">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="60a05-142">Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="60a05-142">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="60a05-143">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="60a05-143">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="60a05-144">Максимальное число элементов, которые могут быть помещены в буфер для потоков загрузки клиента.</span><span class="sxs-lookup"><span data-stu-id="60a05-144">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="60a05-145">При достижении этого предела обработка вызовов блокируется до тех пор, пока сервер не обработает потоковые элементы.</span><span class="sxs-lookup"><span data-stu-id="60a05-145">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="60a05-146">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-146">32 KB</span></span> | <span data-ttu-id="60a05-147">Максимальный размер одного входящего сообщения концентратора.</span><span class="sxs-lookup"><span data-stu-id="60a05-147">Maximum size of a single incoming hub message.</span></span> |

<span data-ttu-id="60a05-148">Параметры можно настроить для всех концентраторов, предоставив параметры делегата для вызова `AddSignalR` в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-148">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="60a05-149">Параметры одного концентратора переопределяют глобальные параметры, предоставленные в `AddSignalR` и могут быть настроены с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="60a05-149">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="60a05-150">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="60a05-150">Advanced HTTP configuration options</span></span>

<span data-ttu-id="60a05-151">Используйте `HttpConnectionDispatcherOptions` для настройки дополнительных параметров, относящихся к транспортам и управлению буферами памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-151">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="60a05-152">Эти параметры настраиваются путем передачи делегата в [мафуб\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="60a05-152">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```

<span data-ttu-id="60a05-153">В следующей таблице описаны параметры для настройки расширенных HTTP-параметров SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60a05-153">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="60a05-154">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-154">Option</span></span> | <span data-ttu-id="60a05-155">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-155">Default Value</span></span> | <span data-ttu-id="60a05-156">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-156">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="60a05-157">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-157">32 KB</span></span> | <span data-ttu-id="60a05-158">Максимальное число байтов, полученных от клиента, который буфер сервера перед применением обратной нагрузки.</span><span class="sxs-lookup"><span data-stu-id="60a05-158">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="60a05-159">Увеличение этого значения позволяет серверу быстрее принимать сообщения большего размера без применения недостаточной нагрузки, но может увеличить потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-159">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="60a05-160">Данные, автоматически собираемые из `Authorize` атрибутов, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="60a05-160">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="60a05-161">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="60a05-161">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="60a05-162">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-162">32 KB</span></span> | <span data-ttu-id="60a05-163">Максимальное число байтов, отправленных приложением, которые были буфером сервера, прежде чем наблюдать за нехваткой.</span><span class="sxs-lookup"><span data-stu-id="60a05-163">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="60a05-164">Увеличение этого значения позволяет серверу быстрее занимать больше сообщений, не ожидая перехода на резервную нагрузку, но может увеличить потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-164">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="60a05-165">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="60a05-165">All Transports are enabled.</span></span> | <span data-ttu-id="60a05-166">Битовая перечисление битовых флагов значений `HttpTransportType`, которые могут ограничивать возможности транспорта, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="60a05-166">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="60a05-167">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="60a05-167">See below.</span></span> | <span data-ttu-id="60a05-168">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="60a05-168">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="60a05-169">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="60a05-169">See below.</span></span> | <span data-ttu-id="60a05-170">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-170">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="60a05-171">У транспорта с длинным опросом есть дополнительные параметры, которые можно настроить с помощью свойства `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="60a05-171">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="60a05-172">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-172">Option</span></span> | <span data-ttu-id="60a05-173">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-173">Default Value</span></span> | <span data-ttu-id="60a05-174">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-174">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="60a05-175">90 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-175">90 seconds</span></span> | <span data-ttu-id="60a05-176">Максимальное количество времени, в течение которого сервер ожидает отправки сообщения клиенту перед завершением одного запроса опроса.</span><span class="sxs-lookup"><span data-stu-id="60a05-176">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="60a05-177">Уменьшение этого значения приводит к тому, что клиент будет чаще выдавать новые запросы на опрос.</span><span class="sxs-lookup"><span data-stu-id="60a05-177">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="60a05-178">Транспорт WebSocket имеет дополнительные параметры, которые можно настроить с помощью свойства `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="60a05-178">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="60a05-179">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-179">Option</span></span> | <span data-ttu-id="60a05-180">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-180">Default Value</span></span> | <span data-ttu-id="60a05-181">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-181">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="60a05-182">5 с</span><span class="sxs-lookup"><span data-stu-id="60a05-182">5 seconds</span></span> | <span data-ttu-id="60a05-183">Когда сервер закрывается, если не удается закрыть клиент в течение этого интервала времени, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="60a05-183">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="60a05-184">Делегат, который можно использовать для задания заголовка `Sec-WebSocket-Protocol` в пользовательском значении.</span><span class="sxs-lookup"><span data-stu-id="60a05-184">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="60a05-185">Делегат получает значения, запрошенные клиентом в качестве входных данных, и ожидается, что он возвращает нужное значение.</span><span class="sxs-lookup"><span data-stu-id="60a05-185">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="60a05-186">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="60a05-186">Configure client options</span></span>

<span data-ttu-id="60a05-187">Параметры клиента можно настроить для типа `HubConnectionBuilder` (доступного в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-187">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="60a05-188">Он также доступен в клиенте Java, но `HttpHubConnectionBuilder` подкласс содержит параметры конфигурации построителя, а также сам `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="60a05-188">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="60a05-189">Настройка журнала</span><span class="sxs-lookup"><span data-stu-id="60a05-189">Configure logging</span></span>

<span data-ttu-id="60a05-190">Ведение журнала настраивается в клиенте .NET с помощью метода `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="60a05-190">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="60a05-191">Регистраторы и фильтры могут быть зарегистрированы так же, как и на сервере.</span><span class="sxs-lookup"><span data-stu-id="60a05-191">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="60a05-192">Дополнительные сведения см. в документации по [ведению журнала ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="60a05-192">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="60a05-193">Чтобы зарегистрировать регистраторы, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="60a05-193">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="60a05-194">Полный список см. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) документации.</span><span class="sxs-lookup"><span data-stu-id="60a05-194">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="60a05-195">Например, чтобы включить ведение журнала консоли, установите `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="60a05-195">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="60a05-196">Вызовите метод расширения `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="60a05-196">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="60a05-197">В клиенте JavaScript аналогичный метод `configureLogging` существует.</span><span class="sxs-lookup"><span data-stu-id="60a05-197">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="60a05-198">Укажите `LogLevel` значение, указывающее минимальный уровень создаваемых сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-198">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="60a05-199">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="60a05-199">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

<span data-ttu-id="60a05-200">Вместо `LogLevel` значения можно также указать `string` значение, представляющее имя уровня журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-200">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="60a05-201">Это полезно при настройке ведения журнала SignalR в средах, где у вас нет доступа к `LogLevel`ным константам.</span><span class="sxs-lookup"><span data-stu-id="60a05-201">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="60a05-202">В следующей таблице перечислены доступные уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-202">The following table lists the available log levels.</span></span> <span data-ttu-id="60a05-203">Значение, которое вы задаете для `configureLogging`, задает **Минимальный** уровень ведения журнала, который будет регистрироваться в журнале.</span><span class="sxs-lookup"><span data-stu-id="60a05-203">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="60a05-204">Сообщения, зарегистрированные на этом уровне **или перечисленные в таблице уровни**, будут занесены в журнал.</span><span class="sxs-lookup"><span data-stu-id="60a05-204">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="60a05-205">String</span><span class="sxs-lookup"><span data-stu-id="60a05-205">String</span></span>                      | <span data-ttu-id="60a05-206">LogLevel</span><span class="sxs-lookup"><span data-stu-id="60a05-206">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="60a05-207">`info` **или** `information`</span><span class="sxs-lookup"><span data-stu-id="60a05-207">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="60a05-208">`warn` **или** `warning`</span><span class="sxs-lookup"><span data-stu-id="60a05-208">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

> [!NOTE]
> <span data-ttu-id="60a05-209">Чтобы полностью отключить ведение журнала, укажите `signalR.LogLevel.None` в методе `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="60a05-209">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="60a05-210">Дополнительные сведения о ведении журналов см. в [документации по диагностике SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="60a05-210">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="60a05-211">Клиент, использующий SignalR Java, использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-211">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="60a05-212">Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-212">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="60a05-213">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="60a05-213">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="60a05-214">Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:</span><span class="sxs-lookup"><span data-stu-id="60a05-214">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="60a05-215">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="60a05-215">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="60a05-216">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="60a05-216">Configure allowed transports</span></span>

<span data-ttu-id="60a05-217">Транспорты, используемые SignalR, можно настроить в вызове `WithUrl` (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-217">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="60a05-218">Побитовое или для значений `HttpTransportType` можно использовать, чтобы ограничить клиент использованием только указанных транспортов.</span><span class="sxs-lookup"><span data-stu-id="60a05-218">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="60a05-219">По умолчанию все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="60a05-219">All transports are enabled by default.</span></span>

<span data-ttu-id="60a05-220">Например, чтобы отключить транспорт событий, отправленных сервером, но разрешить соединения WebSockets и long опрашиваете:</span><span class="sxs-lookup"><span data-stu-id="60a05-220">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="60a05-221">В клиенте JavaScript транспорты настраиваются путем установки поля `transport` в объекте Options, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-221">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="60a05-222">В этой версии клиентские WebSocket-сокеты Java являются единственным доступным транспортом.</span><span class="sxs-lookup"><span data-stu-id="60a05-222">In this version of the Java client websockets is the only available transport.</span></span>

<span data-ttu-id="60a05-223">В клиенте Java транспорт выбирается с помощью метода `withTransport` на `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="60a05-223">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="60a05-224">Клиент Java по умолчанию использует транспорт WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-224">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="60a05-225">Клиент Java SignalR пока не поддерживает откат транспорта.</span><span class="sxs-lookup"><span data-stu-id="60a05-225">The SignalR Java client doesn't support transport fallback yet.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="60a05-226">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="60a05-226">Configure bearer authentication</span></span>

<span data-ttu-id="60a05-227">Чтобы предоставить данные проверки подлинности вместе с запросами SignalR, используйте параметр `AccessTokenProvider` (`accessTokenFactory` в JavaScript), чтобы указать функцию, которая возвращает нужный маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="60a05-227">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="60a05-228">В клиенте .NET этот маркер доступа передается в качестве токена "Проверка подлинности носителя" HTTP (с использованием заголовка `Authorization` с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="60a05-228">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="60a05-229">В клиенте JavaScript маркер доступа используется в качестве токена носителя, **за исключением** случаев, когда интерфейсы API браузера ограничивают возможность применения заголовков (в частности, при отправке сервером событий и запросах WebSocket).</span><span class="sxs-lookup"><span data-stu-id="60a05-229">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="60a05-230">В таких случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="60a05-230">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="60a05-231">В клиенте .NET параметр `AccessTokenProvider` можно указать с помощью делегата Options в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-231">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="60a05-232">В клиенте JavaScript маркер доступа настраивается путем установки поля `accessTokenFactory` в объекте Options в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-232">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="60a05-233">В клиенте SignalR Java можно настроить токен носителя, который будет использоваться для проверки подлинности, предоставив фабрику маркеров доступа для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="60a05-233">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="60a05-234">Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [Рксжава](https://github.com/ReactiveX/RxJava) [одно\<String >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="60a05-234">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="60a05-235">При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.</span><span class="sxs-lookup"><span data-stu-id="60a05-235">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="60a05-236">Настройка времени ожидания и параметров поддержания активности</span><span class="sxs-lookup"><span data-stu-id="60a05-236">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="60a05-237">Дополнительные параметры для настройки времени ожидания и проверки активности доступны для самого `HubConnection` объекта:</span><span class="sxs-lookup"><span data-stu-id="60a05-237">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="60a05-238">.NET</span><span class="sxs-lookup"><span data-stu-id="60a05-238">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="60a05-239">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-239">Option</span></span> | <span data-ttu-id="60a05-240">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-240">Default value</span></span> | <span data-ttu-id="60a05-241">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-241">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="60a05-242">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-242">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-243">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-243">Timeout for server activity.</span></span> <span data-ttu-id="60a05-244">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-244">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="60a05-245">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-245">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-246">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-246">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="60a05-247">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-247">15 seconds</span></span> | <span data-ttu-id="60a05-248">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-248">Timeout for initial server handshake.</span></span> <span data-ttu-id="60a05-249">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-249">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="60a05-250">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-250">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-251">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-251">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="60a05-252">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-252">15 seconds</span></span> | <span data-ttu-id="60a05-253">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-253">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="60a05-254">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="60a05-254">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="60a05-255">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="60a05-255">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="60a05-256">В клиенте .NET значения времени ожидания указываются как `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="60a05-256">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="60a05-257">JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-257">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="60a05-258">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-258">Option</span></span> | <span data-ttu-id="60a05-259">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-259">Default value</span></span> | <span data-ttu-id="60a05-260">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-260">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="60a05-261">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-261">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-262">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-262">Timeout for server activity.</span></span> <span data-ttu-id="60a05-263">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-263">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="60a05-264">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-264">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-265">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-265">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="60a05-266">15 секунд (15 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-266">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="60a05-267">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-267">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="60a05-268">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="60a05-268">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="60a05-269">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="60a05-269">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="60a05-270">Java</span><span class="sxs-lookup"><span data-stu-id="60a05-270">Java</span></span>](#tab/java)

| <span data-ttu-id="60a05-271">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-271">Option</span></span> | <span data-ttu-id="60a05-272">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-272">Default value</span></span> | <span data-ttu-id="60a05-273">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-273">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="60a05-274">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="60a05-274">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="60a05-275">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-275">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-276">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-276">Timeout for server activity.</span></span> <span data-ttu-id="60a05-277">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-277">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="60a05-278">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-278">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-279">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-279">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="60a05-280">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-280">15 seconds</span></span> | <span data-ttu-id="60a05-281">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-281">Timeout for initial server handshake.</span></span> <span data-ttu-id="60a05-282">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-282">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="60a05-283">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-283">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-284">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-284">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="60a05-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="60a05-285">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="60a05-286">15 секунд (15 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-286">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="60a05-287">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-287">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="60a05-288">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="60a05-288">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="60a05-289">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="60a05-289">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="60a05-290">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="60a05-290">Configure additional options</span></span>

<span data-ttu-id="60a05-291">Дополнительные параметры можно настроить в методе `WithUrl` (`withUrl` в JavaScript) на `HubConnectionBuilder` или в различных API-интерфейсах конфигурации `HttpHubConnectionBuilder` в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="60a05-291">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="60a05-292">.NET</span><span class="sxs-lookup"><span data-stu-id="60a05-292">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="60a05-293">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="60a05-293">.NET Option</span></span> |  <span data-ttu-id="60a05-294">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-294">Default value</span></span> | <span data-ttu-id="60a05-295">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-295">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="60a05-296">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-296">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="60a05-297">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-297">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-298">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-298">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-299">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-299">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="60a05-300">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-300">Empty</span></span> | <span data-ttu-id="60a05-301">Коллекция сертификатов TLS для отправки в запросы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="60a05-301">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="60a05-302">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-302">Empty</span></span> | <span data-ttu-id="60a05-303">Коллекция файлов cookie HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-303">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="60a05-304">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-304">Empty</span></span> | <span data-ttu-id="60a05-305">Учетные данные для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-305">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="60a05-306">5 с</span><span class="sxs-lookup"><span data-stu-id="60a05-306">5 seconds</span></span> | <span data-ttu-id="60a05-307">Только WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-307">WebSockets only.</span></span> <span data-ttu-id="60a05-308">Максимальное время ожидания клиента после закрытия сервера для подтверждения запроса на закрытие.</span><span class="sxs-lookup"><span data-stu-id="60a05-308">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="60a05-309">Если сервер не подтверждает закрытие в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="60a05-309">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="60a05-310">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-310">Empty</span></span> | <span data-ttu-id="60a05-311">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-311">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="60a05-312">Делегат, который можно использовать для настройки или замены `HttpMessageHandler`, используемой для отправки HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="60a05-312">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="60a05-313">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-313">Not used for WebSocket connections.</span></span> <span data-ttu-id="60a05-314">Этот делегат должен возвращать значение, отличное от NULL, и получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="60a05-314">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="60a05-315">Либо измените параметры для этого значения по умолчанию и верните его, либо возвратите новый экземпляр `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="60a05-315">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="60a05-316">**При замене обработчика обязательно скопируйте параметры, которые необходимо сохранить из предоставленного обработчика. в противном случае настроенные параметры (такие как файлы cookie и заголовки) не будут применяться к новому обработчику.**</span><span class="sxs-lookup"><span data-stu-id="60a05-316">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="60a05-317">HTTP-прокси, используемый при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="60a05-317">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="60a05-318">Установите это логическое значение, чтобы отправить учетные данные по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-318">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="60a05-319">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="60a05-319">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="60a05-320">Делегат, который можно использовать для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-320">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="60a05-321">Получает экземпляр [клиентвебсоккетоптионс](/dotnet/api/system.net.websockets.clientwebsocketoptions) , который можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="60a05-321">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="60a05-322">JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-322">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="60a05-323">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-323">JavaScript Option</span></span> | <span data-ttu-id="60a05-324">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-324">Default Value</span></span> | <span data-ttu-id="60a05-325">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-325">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="60a05-326">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-326">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="60a05-327">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-327">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-328">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-328">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-329">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-329">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="60a05-330">Java</span><span class="sxs-lookup"><span data-stu-id="60a05-330">Java</span></span>](#tab/java)

| <span data-ttu-id="60a05-331">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="60a05-331">Java Option</span></span> | <span data-ttu-id="60a05-332">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-332">Default Value</span></span> | <span data-ttu-id="60a05-333">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-333">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="60a05-334">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-334">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="60a05-335">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-335">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-336">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-336">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-337">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-337">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="60a05-338">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="60a05-338">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="60a05-339">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-339">Empty</span></span> | <span data-ttu-id="60a05-340">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-340">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="60a05-341">В клиенте .NET эти параметры можно изменить с помощью делегата Options, предоставленного для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-341">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="60a05-342">В клиенте JavaScript эти параметры можно указать в объекте JavaScript, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-342">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="60a05-343">В клиенте Java эти параметры можно настроить с помощью методов в `HttpHubConnectionBuilder`, возвращаемых из `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="60a05-343">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="60a05-344">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60a05-344">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="= aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="60a05-345">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="60a05-345">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="60a05-346">ASP.NET Core SignalR поддерживает два протокола кодирования сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="60a05-346">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="60a05-347">Для каждого протокола предусмотрены параметры конфигурации сериализации.</span><span class="sxs-lookup"><span data-stu-id="60a05-347">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="60a05-348">Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , который можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-348">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="60a05-349">Метод `AddJsonProtocol` принимает делегат, который получает объект `options`.</span><span class="sxs-lookup"><span data-stu-id="60a05-349">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="60a05-350">Свойство [пайлоадсериализерсеттингс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) этого объекта является объектом JSON.NET `JsonSerializerSettings`, который можно использовать для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="60a05-350">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="60a05-351">Дополнительные сведения см. в [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="60a05-351">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="60a05-352">Например, чтобы настроить сериализатор для использования имен свойств "PascalCase" вместо имен по умолчанию "camelCase", используйте следующий код в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-352">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="60a05-353">В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="60a05-353">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="60a05-354">Для разрешения метода расширения необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.</span><span class="sxs-lookup"><span data-stu-id="60a05-354">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="60a05-355">В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60a05-355">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="60a05-356">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="60a05-356">MessagePack serialization options</span></span>

<span data-ttu-id="60a05-357">MessagePack сериализацию можно настроить, предоставив делегат вызову [аддмессажепаккпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-357">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="60a05-358">Дополнительные сведения см. [в разделе MessagePack в SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-358">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="60a05-359">В настоящее время невозможно настроить сериализацию MessagePack в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60a05-359">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="60a05-360">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="60a05-360">Configure server options</span></span>

<span data-ttu-id="60a05-361">В следующей таблице описаны параметры для настройки концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-361">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="60a05-362">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-362">Option</span></span> | <span data-ttu-id="60a05-363">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-363">Default Value</span></span> | <span data-ttu-id="60a05-364">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-364">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="60a05-365">30 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-365">30 seconds</span></span> | <span data-ttu-id="60a05-366">Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности").</span><span class="sxs-lookup"><span data-stu-id="60a05-366">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="60a05-367">Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="60a05-367">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="60a05-368">Рекомендуемое значение — это значение типа Double, равное `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="60a05-368">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="60a05-369">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-369">15 seconds</span></span> | <span data-ttu-id="60a05-370">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="60a05-370">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="60a05-371">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-371">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-372">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-372">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="60a05-373">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-373">15 seconds</span></span> | <span data-ttu-id="60a05-374">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="60a05-374">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="60a05-375">При изменении `KeepAliveInterval`измените параметр `ServerTimeout`/`serverTimeoutInMilliseconds` на клиенте.</span><span class="sxs-lookup"><span data-stu-id="60a05-375">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="60a05-376">Рекомендуемое `ServerTimeout`/`serverTimeoutInMilliseconds` является двойным значением `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="60a05-376">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="60a05-377">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="60a05-377">All installed protocols</span></span> | <span data-ttu-id="60a05-378">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="60a05-378">Protocols supported by this hub.</span></span> <span data-ttu-id="60a05-379">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="60a05-379">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="60a05-380">Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="60a05-380">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="60a05-381">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="60a05-381">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="60a05-382">Параметры можно настроить для всех концентраторов, предоставив параметры делегата для вызова `AddSignalR` в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-382">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="60a05-383">Параметры одного концентратора переопределяют глобальные параметры, предоставленные в `AddSignalR` и могут быть настроены с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="60a05-383">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="60a05-384">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="60a05-384">Advanced HTTP configuration options</span></span>

<span data-ttu-id="60a05-385">Используйте `HttpConnectionDispatcherOptions` для настройки дополнительных параметров, относящихся к транспортам и управлению буферами памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-385">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="60a05-386">Эти параметры настраиваются путем передачи делегата в [мафуб\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="60a05-386">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="60a05-387">В следующей таблице описаны параметры для настройки расширенных HTTP-параметров SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60a05-387">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="60a05-388">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-388">Option</span></span> | <span data-ttu-id="60a05-389">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-389">Default Value</span></span> | <span data-ttu-id="60a05-390">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-390">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="60a05-391">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-391">32 KB</span></span> | <span data-ttu-id="60a05-392">Максимальное число байтов, полученных от клиента, который является буфером сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-392">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="60a05-393">Увеличение этого значения позволяет серверу принимать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-393">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="60a05-394">Данные, автоматически собираемые из `Authorize` атрибутов, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="60a05-394">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="60a05-395">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="60a05-395">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="60a05-396">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-396">32 KB</span></span> | <span data-ttu-id="60a05-397">Максимальное число байтов, отправленных приложением, которые используются в буфере сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-397">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="60a05-398">Увеличение этого значения позволяет серверу передавать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-398">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="60a05-399">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="60a05-399">All Transports are enabled.</span></span> | <span data-ttu-id="60a05-400">Битовая перечисление битовых флагов значений `HttpTransportType`, которые могут ограничивать возможности транспорта, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="60a05-400">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="60a05-401">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="60a05-401">See below.</span></span> | <span data-ttu-id="60a05-402">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="60a05-402">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="60a05-403">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="60a05-403">See below.</span></span> | <span data-ttu-id="60a05-404">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-404">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="60a05-405">У транспорта с длинным опросом есть дополнительные параметры, которые можно настроить с помощью свойства `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="60a05-405">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="60a05-406">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-406">Option</span></span> | <span data-ttu-id="60a05-407">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-407">Default Value</span></span> | <span data-ttu-id="60a05-408">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-408">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="60a05-409">90 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-409">90 seconds</span></span> | <span data-ttu-id="60a05-410">Максимальное количество времени, в течение которого сервер ожидает отправки сообщения клиенту перед завершением одного запроса опроса.</span><span class="sxs-lookup"><span data-stu-id="60a05-410">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="60a05-411">Уменьшение этого значения приводит к тому, что клиент будет чаще выдавать новые запросы на опрос.</span><span class="sxs-lookup"><span data-stu-id="60a05-411">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="60a05-412">Транспорт WebSocket имеет дополнительные параметры, которые можно настроить с помощью свойства `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="60a05-412">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="60a05-413">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-413">Option</span></span> | <span data-ttu-id="60a05-414">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-414">Default Value</span></span> | <span data-ttu-id="60a05-415">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-415">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="60a05-416">5 с</span><span class="sxs-lookup"><span data-stu-id="60a05-416">5 seconds</span></span> | <span data-ttu-id="60a05-417">Когда сервер закрывается, если не удается закрыть клиент в течение этого интервала времени, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="60a05-417">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="60a05-418">Делегат, который можно использовать для задания заголовка `Sec-WebSocket-Protocol` в пользовательском значении.</span><span class="sxs-lookup"><span data-stu-id="60a05-418">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="60a05-419">Делегат получает значения, запрошенные клиентом в качестве входных данных, и ожидается, что он возвращает нужное значение.</span><span class="sxs-lookup"><span data-stu-id="60a05-419">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="60a05-420">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="60a05-420">Configure client options</span></span>

<span data-ttu-id="60a05-421">Параметры клиента можно настроить для типа `HubConnectionBuilder` (доступного в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-421">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="60a05-422">Он также доступен в клиенте Java, но `HttpHubConnectionBuilder` подкласс содержит параметры конфигурации построителя, а также сам `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="60a05-422">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="60a05-423">Настройка журнала</span><span class="sxs-lookup"><span data-stu-id="60a05-423">Configure logging</span></span>

<span data-ttu-id="60a05-424">Ведение журнала настраивается в клиенте .NET с помощью метода `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="60a05-424">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="60a05-425">Регистраторы и фильтры могут быть зарегистрированы так же, как и на сервере.</span><span class="sxs-lookup"><span data-stu-id="60a05-425">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="60a05-426">Дополнительные сведения см. в документации по [ведению журнала ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="60a05-426">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="60a05-427">Чтобы зарегистрировать регистраторы, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="60a05-427">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="60a05-428">Полный список см. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) документации.</span><span class="sxs-lookup"><span data-stu-id="60a05-428">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="60a05-429">Например, чтобы включить ведение журнала консоли, установите `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="60a05-429">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="60a05-430">Вызовите метод расширения `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="60a05-430">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="60a05-431">В клиенте JavaScript аналогичный метод `configureLogging` существует.</span><span class="sxs-lookup"><span data-stu-id="60a05-431">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="60a05-432">Укажите `LogLevel` значение, указывающее минимальный уровень создаваемых сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-432">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="60a05-433">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="60a05-433">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="60a05-434">Чтобы полностью отключить ведение журнала, укажите `signalR.LogLevel.None` в методе `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="60a05-434">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="60a05-435">Дополнительные сведения о ведении журналов см. в [документации по диагностике SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="60a05-435">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="60a05-436">Клиент, использующий SignalR Java, использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-436">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="60a05-437">Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-437">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="60a05-438">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="60a05-438">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="60a05-439">Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:</span><span class="sxs-lookup"><span data-stu-id="60a05-439">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="60a05-440">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="60a05-440">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="60a05-441">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="60a05-441">Configure allowed transports</span></span>

<span data-ttu-id="60a05-442">Транспорты, используемые SignalR, можно настроить в вызове `WithUrl` (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-442">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="60a05-443">Побитовое или для значений `HttpTransportType` можно использовать, чтобы ограничить клиент использованием только указанных транспортов.</span><span class="sxs-lookup"><span data-stu-id="60a05-443">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="60a05-444">По умолчанию все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="60a05-444">All transports are enabled by default.</span></span>

<span data-ttu-id="60a05-445">Например, чтобы отключить транспорт событий, отправленных сервером, но разрешить соединения WebSockets и long опрашиваете:</span><span class="sxs-lookup"><span data-stu-id="60a05-445">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="60a05-446">В клиенте JavaScript транспорты настраиваются путем установки поля `transport` в объекте Options, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-446">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

<span data-ttu-id="60a05-447">В этой версии клиентские WebSocket-сокеты Java являются единственным доступным транспортом.</span><span class="sxs-lookup"><span data-stu-id="60a05-447">In this version of the Java client websockets is the only available transport.</span></span>

### <a name="configure-bearer-authentication"></a><span data-ttu-id="60a05-448">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="60a05-448">Configure bearer authentication</span></span>

<span data-ttu-id="60a05-449">Чтобы предоставить данные проверки подлинности вместе с запросами SignalR, используйте параметр `AccessTokenProvider` (`accessTokenFactory` в JavaScript), чтобы указать функцию, которая возвращает нужный маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="60a05-449">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="60a05-450">В клиенте .NET этот маркер доступа передается в качестве токена "Проверка подлинности носителя" HTTP (с использованием заголовка `Authorization` с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="60a05-450">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="60a05-451">В клиенте JavaScript маркер доступа используется в качестве токена носителя, **за исключением** случаев, когда интерфейсы API браузера ограничивают возможность применения заголовков (в частности, при отправке сервером событий и запросах WebSocket).</span><span class="sxs-lookup"><span data-stu-id="60a05-451">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="60a05-452">В таких случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="60a05-452">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="60a05-453">В клиенте .NET параметр `AccessTokenProvider` можно указать с помощью делегата Options в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-453">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="60a05-454">В клиенте JavaScript маркер доступа настраивается путем установки поля `accessTokenFactory` в объекте Options в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-454">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="60a05-455">В клиенте SignalR Java можно настроить токен носителя, который будет использоваться для проверки подлинности, предоставив фабрику маркеров доступа для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="60a05-455">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="60a05-456">Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [Рксжава](https://github.com/ReactiveX/RxJava) [одно\<String >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="60a05-456">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="60a05-457">При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.</span><span class="sxs-lookup"><span data-stu-id="60a05-457">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="60a05-458">Настройка времени ожидания и параметров поддержания активности</span><span class="sxs-lookup"><span data-stu-id="60a05-458">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="60a05-459">Дополнительные параметры для настройки времени ожидания и проверки активности доступны для самого `HubConnection` объекта:</span><span class="sxs-lookup"><span data-stu-id="60a05-459">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="60a05-460">.NET</span><span class="sxs-lookup"><span data-stu-id="60a05-460">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="60a05-461">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-461">Option</span></span> | <span data-ttu-id="60a05-462">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-462">Default value</span></span> | <span data-ttu-id="60a05-463">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-463">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="60a05-464">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-464">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-465">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-465">Timeout for server activity.</span></span> <span data-ttu-id="60a05-466">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-466">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="60a05-467">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-467">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-468">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-468">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="60a05-469">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-469">15 seconds</span></span> | <span data-ttu-id="60a05-470">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-470">Timeout for initial server handshake.</span></span> <span data-ttu-id="60a05-471">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-471">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="60a05-472">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-472">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-473">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-473">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="60a05-474">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-474">15 seconds</span></span> | <span data-ttu-id="60a05-475">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-475">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="60a05-476">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="60a05-476">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="60a05-477">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="60a05-477">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="60a05-478">В клиенте .NET значения времени ожидания указываются как `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="60a05-478">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="60a05-479">JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-479">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="60a05-480">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-480">Option</span></span> | <span data-ttu-id="60a05-481">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-481">Default value</span></span> | <span data-ttu-id="60a05-482">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-482">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="60a05-483">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-483">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-484">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-484">Timeout for server activity.</span></span> <span data-ttu-id="60a05-485">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-485">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="60a05-486">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-486">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-487">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-487">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="60a05-488">15 секунд (15 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-488">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="60a05-489">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-489">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="60a05-490">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="60a05-490">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="60a05-491">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="60a05-491">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="java"></a>[<span data-ttu-id="60a05-492">Java</span><span class="sxs-lookup"><span data-stu-id="60a05-492">Java</span></span>](#tab/java)

| <span data-ttu-id="60a05-493">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-493">Option</span></span> | <span data-ttu-id="60a05-494">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-494">Default value</span></span> | <span data-ttu-id="60a05-495">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-495">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="60a05-496">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="60a05-496">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="60a05-497">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-497">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-498">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-498">Timeout for server activity.</span></span> <span data-ttu-id="60a05-499">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-499">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="60a05-500">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-500">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-501">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-501">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="60a05-502">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-502">15 seconds</span></span> | <span data-ttu-id="60a05-503">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-503">Timeout for initial server handshake.</span></span> <span data-ttu-id="60a05-504">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-504">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="60a05-505">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-505">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-506">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-506">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="60a05-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="60a05-507">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="60a05-508">15 секунд (15 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-508">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="60a05-509">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-509">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="60a05-510">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="60a05-510">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="60a05-511">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="60a05-511">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="60a05-512">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="60a05-512">Configure additional options</span></span>

<span data-ttu-id="60a05-513">Дополнительные параметры можно настроить в методе `WithUrl` (`withUrl` в JavaScript) на `HubConnectionBuilder` или в различных API-интерфейсах конфигурации `HttpHubConnectionBuilder` в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="60a05-513">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="60a05-514">.NET</span><span class="sxs-lookup"><span data-stu-id="60a05-514">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="60a05-515">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="60a05-515">.NET Option</span></span> |  <span data-ttu-id="60a05-516">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-516">Default value</span></span> | <span data-ttu-id="60a05-517">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-517">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="60a05-518">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-518">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="60a05-519">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-519">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-520">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-520">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-521">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-521">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="60a05-522">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-522">Empty</span></span> | <span data-ttu-id="60a05-523">Коллекция сертификатов TLS для отправки в запросы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="60a05-523">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="60a05-524">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-524">Empty</span></span> | <span data-ttu-id="60a05-525">Коллекция файлов cookie HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-525">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="60a05-526">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-526">Empty</span></span> | <span data-ttu-id="60a05-527">Учетные данные для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-527">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="60a05-528">5 с</span><span class="sxs-lookup"><span data-stu-id="60a05-528">5 seconds</span></span> | <span data-ttu-id="60a05-529">Только WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-529">WebSockets only.</span></span> <span data-ttu-id="60a05-530">Максимальное время ожидания клиента после закрытия сервера для подтверждения запроса на закрытие.</span><span class="sxs-lookup"><span data-stu-id="60a05-530">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="60a05-531">Если сервер не подтверждает закрытие в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="60a05-531">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="60a05-532">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-532">Empty</span></span> | <span data-ttu-id="60a05-533">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-533">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="60a05-534">Делегат, который можно использовать для настройки или замены `HttpMessageHandler`, используемой для отправки HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="60a05-534">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="60a05-535">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-535">Not used for WebSocket connections.</span></span> <span data-ttu-id="60a05-536">Этот делегат должен возвращать значение, отличное от NULL, и получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="60a05-536">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="60a05-537">Либо измените параметры для этого значения по умолчанию и верните его, либо возвратите новый экземпляр `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="60a05-537">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="60a05-538">**При замене обработчика обязательно скопируйте параметры, которые необходимо сохранить из предоставленного обработчика. в противном случае настроенные параметры (такие как файлы cookie и заголовки) не будут применяться к новому обработчику.**</span><span class="sxs-lookup"><span data-stu-id="60a05-538">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="60a05-539">HTTP-прокси, используемый при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="60a05-539">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="60a05-540">Установите это логическое значение, чтобы отправить учетные данные по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-540">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="60a05-541">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="60a05-541">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="60a05-542">Делегат, который можно использовать для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-542">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="60a05-543">Получает экземпляр [клиентвебсоккетоптионс](/dotnet/api/system.net.websockets.clientwebsocketoptions) , который можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="60a05-543">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="60a05-544">JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-544">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="60a05-545">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-545">JavaScript Option</span></span> | <span data-ttu-id="60a05-546">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-546">Default Value</span></span> | <span data-ttu-id="60a05-547">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-547">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="60a05-548">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-548">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="60a05-549">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-549">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-550">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-550">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-551">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-551">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="60a05-552">Java</span><span class="sxs-lookup"><span data-stu-id="60a05-552">Java</span></span>](#tab/java)

| <span data-ttu-id="60a05-553">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="60a05-553">Java Option</span></span> | <span data-ttu-id="60a05-554">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-554">Default Value</span></span> | <span data-ttu-id="60a05-555">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-555">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="60a05-556">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-556">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="60a05-557">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-557">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-558">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-558">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-559">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-559">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="60a05-560">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="60a05-560">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="60a05-561">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-561">Empty</span></span> | <span data-ttu-id="60a05-562">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-562">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="60a05-563">В клиенте .NET эти параметры можно изменить с помощью делегата Options, предоставленного для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-563">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="60a05-564">В клиенте JavaScript эти параметры можно указать в объекте JavaScript, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-564">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="60a05-565">В клиенте Java эти параметры можно настроить с помощью методов в `HttpHubConnectionBuilder`, возвращаемых из `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="60a05-565">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="60a05-566">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60a05-566">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end
::: moniker range="< aspnetcore-2.2"

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="60a05-567">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="60a05-567">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="60a05-568">ASP.NET Core SignalR поддерживает два протокола кодирования сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="60a05-568">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="60a05-569">Для каждого протокола предусмотрены параметры конфигурации сериализации.</span><span class="sxs-lookup"><span data-stu-id="60a05-569">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="60a05-570">Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , который можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-570">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="60a05-571">Метод `AddJsonProtocol` принимает делегат, который получает объект `options`.</span><span class="sxs-lookup"><span data-stu-id="60a05-571">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="60a05-572">Свойство [пайлоадсериализерсеттингс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) этого объекта является объектом JSON.NET `JsonSerializerSettings`, который можно использовать для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="60a05-572">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="60a05-573">Дополнительные сведения см. в [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="60a05-573">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="60a05-574">Например, чтобы настроить сериализатор для использования имен свойств "PascalCase" вместо имен по умолчанию "camelCase", используйте следующий код в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-574">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="60a05-575">В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="60a05-575">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="60a05-576">Для разрешения метода расширения необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.</span><span class="sxs-lookup"><span data-stu-id="60a05-576">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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
> <span data-ttu-id="60a05-577">В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60a05-577">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="60a05-578">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="60a05-578">MessagePack serialization options</span></span>

<span data-ttu-id="60a05-579">MessagePack сериализацию можно настроить, предоставив делегат вызову [аддмессажепаккпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-579">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="60a05-580">Дополнительные сведения см. [в разделе MessagePack в SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="60a05-580">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="60a05-581">В настоящее время невозможно настроить сериализацию MessagePack в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="60a05-581">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="60a05-582">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="60a05-582">Configure server options</span></span>

<span data-ttu-id="60a05-583">В следующей таблице описаны параметры для настройки концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-583">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="60a05-584">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-584">Option</span></span> | <span data-ttu-id="60a05-585">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-585">Default Value</span></span> | <span data-ttu-id="60a05-586">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-586">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="60a05-587">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-587">15 seconds</span></span> | <span data-ttu-id="60a05-588">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="60a05-588">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="60a05-589">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-589">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-590">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-590">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="60a05-591">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-591">15 seconds</span></span> | <span data-ttu-id="60a05-592">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="60a05-592">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="60a05-593">При изменении `KeepAliveInterval`измените параметр `ServerTimeout`/`serverTimeoutInMilliseconds` на клиенте.</span><span class="sxs-lookup"><span data-stu-id="60a05-593">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="60a05-594">Рекомендуемое `ServerTimeout`/`serverTimeoutInMilliseconds` является двойным значением `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="60a05-594">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="60a05-595">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="60a05-595">All installed protocols</span></span> | <span data-ttu-id="60a05-596">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="60a05-596">Protocols supported by this hub.</span></span> <span data-ttu-id="60a05-597">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="60a05-597">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="60a05-598">Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="60a05-598">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="60a05-599">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="60a05-599">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="60a05-600">Параметры можно настроить для всех концентраторов, предоставив параметры делегата для вызова `AddSignalR` в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="60a05-600">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="60a05-601">Параметры одного концентратора переопределяют глобальные параметры, предоставленные в `AddSignalR` и могут быть настроены с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="60a05-601">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="60a05-602">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="60a05-602">Advanced HTTP configuration options</span></span>

<span data-ttu-id="60a05-603">Используйте `HttpConnectionDispatcherOptions` для настройки дополнительных параметров, относящихся к транспортам и управлению буферами памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-603">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="60a05-604">Эти параметры настраиваются путем передачи делегата в [мафуб\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="60a05-604">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

<span data-ttu-id="60a05-605">В следующей таблице описаны параметры для настройки расширенных HTTP-параметров SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="60a05-605">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

| <span data-ttu-id="60a05-606">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-606">Option</span></span> | <span data-ttu-id="60a05-607">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-607">Default Value</span></span> | <span data-ttu-id="60a05-608">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-608">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="60a05-609">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-609">32 KB</span></span> | <span data-ttu-id="60a05-610">Максимальное число байтов, полученных от клиента, который является буфером сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-610">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="60a05-611">Увеличение этого значения позволяет серверу принимать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-611">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="60a05-612">Данные, автоматически собираемые из `Authorize` атрибутов, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="60a05-612">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="60a05-613">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="60a05-613">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="60a05-614">32 КБ</span><span class="sxs-lookup"><span data-stu-id="60a05-614">32 KB</span></span> | <span data-ttu-id="60a05-615">Максимальное число байтов, отправленных приложением, которые используются в буфере сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-615">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="60a05-616">Увеличение этого значения позволяет серверу передавать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="60a05-616">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="60a05-617">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="60a05-617">All Transports are enabled.</span></span> | <span data-ttu-id="60a05-618">Битовая перечисление битовых флагов значений `HttpTransportType`, которые могут ограничивать возможности транспорта, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="60a05-618">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="60a05-619">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="60a05-619">See below.</span></span> | <span data-ttu-id="60a05-620">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="60a05-620">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="60a05-621">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="60a05-621">See below.</span></span> | <span data-ttu-id="60a05-622">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-622">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="60a05-623">У транспорта с длинным опросом есть дополнительные параметры, которые можно настроить с помощью свойства `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="60a05-623">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="60a05-624">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-624">Option</span></span> | <span data-ttu-id="60a05-625">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-625">Default Value</span></span> | <span data-ttu-id="60a05-626">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-626">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="60a05-627">90 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-627">90 seconds</span></span> | <span data-ttu-id="60a05-628">Максимальное количество времени, в течение которого сервер ожидает отправки сообщения клиенту перед завершением одного запроса опроса.</span><span class="sxs-lookup"><span data-stu-id="60a05-628">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="60a05-629">Уменьшение этого значения приводит к тому, что клиент будет чаще выдавать новые запросы на опрос.</span><span class="sxs-lookup"><span data-stu-id="60a05-629">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="60a05-630">Транспорт WebSocket имеет дополнительные параметры, которые можно настроить с помощью свойства `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="60a05-630">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="60a05-631">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-631">Option</span></span> | <span data-ttu-id="60a05-632">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-632">Default Value</span></span> | <span data-ttu-id="60a05-633">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-633">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="60a05-634">5 с</span><span class="sxs-lookup"><span data-stu-id="60a05-634">5 seconds</span></span> | <span data-ttu-id="60a05-635">Когда сервер закрывается, если не удается закрыть клиент в течение этого интервала времени, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="60a05-635">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="60a05-636">Делегат, который можно использовать для задания заголовка `Sec-WebSocket-Protocol` в пользовательском значении.</span><span class="sxs-lookup"><span data-stu-id="60a05-636">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="60a05-637">Делегат получает значения, запрошенные клиентом в качестве входных данных, и ожидается, что он возвращает нужное значение.</span><span class="sxs-lookup"><span data-stu-id="60a05-637">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="60a05-638">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="60a05-638">Configure client options</span></span>

<span data-ttu-id="60a05-639">Параметры клиента можно настроить для типа `HubConnectionBuilder` (доступного в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-639">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="60a05-640">Он также доступен в клиенте Java, но `HttpHubConnectionBuilder` подкласс содержит параметры конфигурации построителя, а также сам `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="60a05-640">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="60a05-641">Настройка журнала</span><span class="sxs-lookup"><span data-stu-id="60a05-641">Configure logging</span></span>

<span data-ttu-id="60a05-642">Ведение журнала настраивается в клиенте .NET с помощью метода `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="60a05-642">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="60a05-643">Регистраторы и фильтры могут быть зарегистрированы так же, как и на сервере.</span><span class="sxs-lookup"><span data-stu-id="60a05-643">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="60a05-644">Дополнительные сведения см. в документации по [ведению журнала ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="60a05-644">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="60a05-645">Чтобы зарегистрировать регистраторы, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="60a05-645">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="60a05-646">Полный список см. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) документации.</span><span class="sxs-lookup"><span data-stu-id="60a05-646">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="60a05-647">Например, чтобы включить ведение журнала консоли, установите `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="60a05-647">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="60a05-648">Вызовите метод расширения `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="60a05-648">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="60a05-649">В клиенте JavaScript аналогичный метод `configureLogging` существует.</span><span class="sxs-lookup"><span data-stu-id="60a05-649">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="60a05-650">Укажите `LogLevel` значение, указывающее минимальный уровень создаваемых сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-650">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="60a05-651">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="60a05-651">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="60a05-652">Чтобы полностью отключить ведение журнала, укажите `signalR.LogLevel.None` в методе `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="60a05-652">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="60a05-653">Дополнительные сведения о ведении журналов см. в [документации по диагностике SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="60a05-653">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="60a05-654">Клиент, использующий SignalR Java, использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-654">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="60a05-655">Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="60a05-655">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="60a05-656">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="60a05-656">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="60a05-657">Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:</span><span class="sxs-lookup"><span data-stu-id="60a05-657">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="60a05-658">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="60a05-658">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="60a05-659">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="60a05-659">Configure allowed transports</span></span>

<span data-ttu-id="60a05-660">Транспорты, используемые SignalR, можно настроить в вызове `WithUrl` (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-660">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="60a05-661">Побитовое или для значений `HttpTransportType` можно использовать, чтобы ограничить клиент использованием только указанных транспортов.</span><span class="sxs-lookup"><span data-stu-id="60a05-661">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="60a05-662">По умолчанию все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="60a05-662">All transports are enabled by default.</span></span>

<span data-ttu-id="60a05-663">Например, чтобы отключить транспорт событий, отправленных сервером, но разрешить соединения WebSockets и long опрашиваете:</span><span class="sxs-lookup"><span data-stu-id="60a05-663">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="60a05-664">В клиенте JavaScript транспорты настраиваются путем установки поля `transport` в объекте Options, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-664">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="60a05-665">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="60a05-665">Configure bearer authentication</span></span>

<span data-ttu-id="60a05-666">Чтобы предоставить данные проверки подлинности вместе с запросами SignalR, используйте параметр `AccessTokenProvider` (`accessTokenFactory` в JavaScript), чтобы указать функцию, которая возвращает нужный маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="60a05-666">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="60a05-667">В клиенте .NET этот маркер доступа передается в качестве токена "Проверка подлинности носителя" HTTP (с использованием заголовка `Authorization` с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="60a05-667">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="60a05-668">В клиенте JavaScript маркер доступа используется в качестве токена носителя, **за исключением** случаев, когда интерфейсы API браузера ограничивают возможность применения заголовков (в частности, при отправке сервером событий и запросах WebSocket).</span><span class="sxs-lookup"><span data-stu-id="60a05-668">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="60a05-669">В таких случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="60a05-669">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="60a05-670">В клиенте .NET параметр `AccessTokenProvider` можно указать с помощью делегата Options в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-670">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="60a05-671">В клиенте JavaScript маркер доступа настраивается путем установки поля `accessTokenFactory` в объекте Options в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-671">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="60a05-672">В клиенте SignalR Java можно настроить токен носителя, который будет использоваться для проверки подлинности, предоставив фабрику маркеров доступа для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="60a05-672">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="60a05-673">Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [Рксжава](https://github.com/ReactiveX/RxJava) [одно\<String >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="60a05-673">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="60a05-674">При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.</span><span class="sxs-lookup"><span data-stu-id="60a05-674">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="60a05-675">Настройка времени ожидания и параметров поддержания активности</span><span class="sxs-lookup"><span data-stu-id="60a05-675">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="60a05-676">Дополнительные параметры для настройки времени ожидания и проверки активности доступны для самого `HubConnection` объекта:</span><span class="sxs-lookup"><span data-stu-id="60a05-676">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

# <a name="net"></a>[<span data-ttu-id="60a05-677">.NET</span><span class="sxs-lookup"><span data-stu-id="60a05-677">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="60a05-678">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-678">Option</span></span> | <span data-ttu-id="60a05-679">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-679">Default value</span></span> | <span data-ttu-id="60a05-680">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-680">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="60a05-681">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-681">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-682">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-682">Timeout for server activity.</span></span> <span data-ttu-id="60a05-683">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-683">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="60a05-684">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-684">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-685">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-685">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="60a05-686">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-686">15 seconds</span></span> | <span data-ttu-id="60a05-687">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-687">Timeout for initial server handshake.</span></span> <span data-ttu-id="60a05-688">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="60a05-688">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="60a05-689">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-689">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-690">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-690">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="60a05-691">В клиенте .NET значения времени ожидания указываются как `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="60a05-691">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascript"></a>[<span data-ttu-id="60a05-692">JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-692">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="60a05-693">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-693">Option</span></span> | <span data-ttu-id="60a05-694">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-694">Default value</span></span> | <span data-ttu-id="60a05-695">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-695">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="60a05-696">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-696">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-697">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-697">Timeout for server activity.</span></span> <span data-ttu-id="60a05-698">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-698">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="60a05-699">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-699">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-700">Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-700">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="java"></a>[<span data-ttu-id="60a05-701">Java</span><span class="sxs-lookup"><span data-stu-id="60a05-701">Java</span></span>](#tab/java)

| <span data-ttu-id="60a05-702">Параметр</span><span class="sxs-lookup"><span data-stu-id="60a05-702">Option</span></span> | <span data-ttu-id="60a05-703">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-703">Default value</span></span> | <span data-ttu-id="60a05-704">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-704">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="60a05-705">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="60a05-705">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="60a05-706">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="60a05-706">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="60a05-707">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-707">Timeout for server activity.</span></span> <span data-ttu-id="60a05-708">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-708">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="60a05-709">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="60a05-709">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="60a05-710">Рекомендуемое значение — это число по крайней мере удвоенного значения `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="60a05-710">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="60a05-711">15 секунд</span><span class="sxs-lookup"><span data-stu-id="60a05-711">15 seconds</span></span> | <span data-ttu-id="60a05-712">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="60a05-712">Timeout for initial server handshake.</span></span> <span data-ttu-id="60a05-713">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="60a05-713">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="60a05-714">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="60a05-714">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="60a05-715">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="60a05-715">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

---

### <a name="configure-additional-options"></a><span data-ttu-id="60a05-716">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="60a05-716">Configure additional options</span></span>

<span data-ttu-id="60a05-717">Дополнительные параметры можно настроить в методе `WithUrl` (`withUrl` в JavaScript) на `HubConnectionBuilder` или в различных API-интерфейсах конфигурации `HttpHubConnectionBuilder` в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="60a05-717">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="net"></a>[<span data-ttu-id="60a05-718">.NET</span><span class="sxs-lookup"><span data-stu-id="60a05-718">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="60a05-719">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="60a05-719">.NET Option</span></span> |  <span data-ttu-id="60a05-720">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-720">Default value</span></span> | <span data-ttu-id="60a05-721">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-721">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="60a05-722">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-722">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="60a05-723">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-723">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-724">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-724">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-725">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-725">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="60a05-726">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-726">Empty</span></span> | <span data-ttu-id="60a05-727">Коллекция сертификатов TLS для отправки в запросы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="60a05-727">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="60a05-728">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-728">Empty</span></span> | <span data-ttu-id="60a05-729">Коллекция файлов cookie HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-729">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="60a05-730">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-730">Empty</span></span> | <span data-ttu-id="60a05-731">Учетные данные для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-731">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="60a05-732">5 с</span><span class="sxs-lookup"><span data-stu-id="60a05-732">5 seconds</span></span> | <span data-ttu-id="60a05-733">Только WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-733">WebSockets only.</span></span> <span data-ttu-id="60a05-734">Максимальное время ожидания клиента после закрытия сервера для подтверждения запроса на закрытие.</span><span class="sxs-lookup"><span data-stu-id="60a05-734">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="60a05-735">Если сервер не подтверждает закрытие в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="60a05-735">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="60a05-736">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-736">Empty</span></span> | <span data-ttu-id="60a05-737">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-737">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="60a05-738">Делегат, который можно использовать для настройки или замены `HttpMessageHandler`, используемой для отправки HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="60a05-738">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="60a05-739">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-739">Not used for WebSocket connections.</span></span> <span data-ttu-id="60a05-740">Этот делегат должен возвращать значение, отличное от NULL, и получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="60a05-740">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="60a05-741">Либо измените параметры для этого значения по умолчанию и верните его, либо возвратите новый экземпляр `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="60a05-741">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="60a05-742">**При замене обработчика обязательно скопируйте параметры, которые необходимо сохранить из предоставленного обработчика. в противном случае настроенные параметры (такие как файлы cookie и заголовки) не будут применяться к новому обработчику.**</span><span class="sxs-lookup"><span data-stu-id="60a05-742">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="60a05-743">HTTP-прокси, используемый при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="60a05-743">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="60a05-744">Установите это логическое значение, чтобы отправить учетные данные по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="60a05-744">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="60a05-745">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="60a05-745">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="60a05-746">Делегат, который можно использовать для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="60a05-746">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="60a05-747">Получает экземпляр [клиентвебсоккетоптионс](/dotnet/api/system.net.websockets.clientwebsocketoptions) , который можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="60a05-747">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascript"></a>[<span data-ttu-id="60a05-748">JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-748">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="60a05-749">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="60a05-749">JavaScript Option</span></span> | <span data-ttu-id="60a05-750">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-750">Default Value</span></span> | <span data-ttu-id="60a05-751">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-751">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="60a05-752">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-752">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="60a05-753">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-753">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-754">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-754">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-755">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-755">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="java"></a>[<span data-ttu-id="60a05-756">Java</span><span class="sxs-lookup"><span data-stu-id="60a05-756">Java</span></span>](#tab/java)

| <span data-ttu-id="60a05-757">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="60a05-757">Java Option</span></span> | <span data-ttu-id="60a05-758">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="60a05-758">Default Value</span></span> | <span data-ttu-id="60a05-759">Description</span><span class="sxs-lookup"><span data-stu-id="60a05-759">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="60a05-760">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="60a05-760">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="60a05-761">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="60a05-761">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="60a05-762">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="60a05-762">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="60a05-763">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="60a05-763">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="60a05-764">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="60a05-764">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="60a05-765">Empty</span><span class="sxs-lookup"><span data-stu-id="60a05-765">Empty</span></span> | <span data-ttu-id="60a05-766">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="60a05-766">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="60a05-767">В клиенте .NET эти параметры можно изменить с помощью делегата Options, предоставленного для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-767">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="60a05-768">В клиенте JavaScript эти параметры можно указать в объекте JavaScript, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="60a05-768">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="60a05-769">В клиенте Java эти параметры можно настроить с помощью методов в `HttpHubConnectionBuilder`, возвращаемых из `HubConnectionBuilder.create("HUB URL")`</span><span class="sxs-lookup"><span data-stu-id="60a05-769">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="60a05-770">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="60a05-770">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>

::: moniker-end