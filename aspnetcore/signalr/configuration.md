---
title: Конфигурация ASP.NET Core SignalR
author: tdykstra
description: Сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 07/31/2018
uid: signalr/configuration
ms.openlocfilehash: 32c0ad94fba09fa099c2ab4a6b1d6d79a5542d7f
ms.sourcegitcommit: a25b572eaed21791230c85416f449f66a405ec19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/01/2018
ms.locfileid: "39396066"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="4624b-103">Конфигурация ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4624b-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="4624b-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="4624b-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="4624b-105">ASP.NET Core SignalR поддерживает два протокола для кодировки сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="4624b-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="4624b-106">Каждый протокол имеет параметры конфигурации для сериализации.</span><span class="sxs-lookup"><span data-stu-id="4624b-106">Each protocol has serialization configuration options.</span></span>

<span data-ttu-id="4624b-107">Сериализация JSON можно настроить на сервере, используя [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) метод расширения, которую можно добавить после [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в вашей `Startup.ConfigureServices` метод.</span><span class="sxs-lookup"><span data-stu-id="4624b-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="4624b-108">`AddJsonProtocol` Метод принимает делегат, который получает `options` объекта.</span><span class="sxs-lookup"><span data-stu-id="4624b-108">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="4624b-109">[PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) свойство на этот объект является JSON.NET `JsonSerializerSettings` объект, который может использоваться для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="4624b-109">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="4624b-110">См. в разделе [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="4624b-110">See the [JSON.NET Documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm) for more details.</span></span>

<span data-ttu-id="4624b-111">Например чтобы настроить сериализатор, используемый имена свойств «PascalCase», а не имена «camelCase» по умолчанию, используйте следующий код:</span><span class="sxs-lookup"><span data-stu-id="4624b-111">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code:</span></span>

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

<span data-ttu-id="4624b-112">В клиента .NET, так же `AddJsonHubProtocol` метод расширения существует на [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="4624b-112">In the .NET client, the same `AddJsonHubProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="4624b-113">`Microsoft.Extensions.DependencyInjection` Пространства имен должны импортироваться разрешить метод расширения:</span><span class="sxs-lookup"><span data-stu-id="4624b-113">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> <span data-ttu-id="4624b-114">Не поддерживается для настройки сериализации JSON в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="4624b-114">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="4624b-115">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="4624b-115">MessagePack serialization options</span></span>

<span data-ttu-id="4624b-116">MessagePack сериализации можно настроить, предоставьте делегат для [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) вызова.</span><span class="sxs-lookup"><span data-stu-id="4624b-116">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="4624b-117">См. в разделе [MessagePack в SignalR](xref:signalr/messagepackhubprotocol) для получения дополнительных сведений.</span><span class="sxs-lookup"><span data-stu-id="4624b-117">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="4624b-118">Не поддерживается для настройки сериализации MessagePack в клиенте JavaScript в данный момент.</span><span class="sxs-lookup"><span data-stu-id="4624b-118">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="4624b-119">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="4624b-119">Configure server options</span></span>

<span data-ttu-id="4624b-120">В следующей таблице описаны параметры для настройки концентраторов SignalR:</span><span class="sxs-lookup"><span data-stu-id="4624b-120">The following table describes options for configuring SignalR hubs:</span></span>

| <span data-ttu-id="4624b-121">Параметр</span><span class="sxs-lookup"><span data-stu-id="4624b-121">Option</span></span> | <span data-ttu-id="4624b-122">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4624b-122">Default Value</span></span> | <span data-ttu-id="4624b-123">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-123">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="4624b-124">15 секунд</span><span class="sxs-lookup"><span data-stu-id="4624b-124">15 seconds</span></span> | <span data-ttu-id="4624b-125">Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="4624b-125">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="4624b-126">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="4624b-126">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="4624b-127">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="4624b-127">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="4624b-128">15 секунд</span><span class="sxs-lookup"><span data-stu-id="4624b-128">15 seconds</span></span> | <span data-ttu-id="4624b-129">Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение.</span><span class="sxs-lookup"><span data-stu-id="4624b-129">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> |
| `SupportedProtocols` | <span data-ttu-id="4624b-130">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="4624b-130">All installed protocols</span></span> | <span data-ttu-id="4624b-131">Протоколы, поддерживаемые этого концентратора.</span><span class="sxs-lookup"><span data-stu-id="4624b-131">Protocols supported by this hub.</span></span> <span data-ttu-id="4624b-132">По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="4624b-132">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="4624b-133">Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="4624b-133">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="4624b-134">По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="4624b-134">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

<span data-ttu-id="4624b-135">Можно настроить параметры для всех концентраторов, предоставляя делегат параметры для `AddSignalR` вызов в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="4624b-135">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

<span data-ttu-id="4624b-136">Параметры для одного центра переопределить глобальные параметры, указанные на `AddSignalR` и могут настраиваться с помощью [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span><span class="sxs-lookup"><span data-stu-id="4624b-136">Options for a single hub override the global options provided in `AddSignalR` and can be configured using [AddHubOptions\<T>](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

<span data-ttu-id="4624b-137">Используйте `HttpConnectionDispatcherOptions` для расширенной настройки, относящиеся к транспортов и управление буфером памяти.</span><span class="sxs-lookup"><span data-stu-id="4624b-137">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="4624b-138">Эти параметры настраиваются путем передачи делегата для [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span><span class="sxs-lookup"><span data-stu-id="4624b-138">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).</span></span>

| <span data-ttu-id="4624b-139">Параметр</span><span class="sxs-lookup"><span data-stu-id="4624b-139">Option</span></span> | <span data-ttu-id="4624b-140">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4624b-140">Default Value</span></span> | <span data-ttu-id="4624b-141">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-141">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="4624b-142">32 КБ</span><span class="sxs-lookup"><span data-stu-id="4624b-142">32 KB</span></span> | <span data-ttu-id="4624b-143">Максимальное число байтов, полученных от клиента, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="4624b-143">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="4624b-144">Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="4624b-144">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="4624b-145">Данные, собранные из автоматически `Authorize` атрибутов, примененных к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="4624b-145">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="4624b-146">Список [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="4624b-146">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="4624b-147">32 КБ</span><span class="sxs-lookup"><span data-stu-id="4624b-147">32 KB</span></span> | <span data-ttu-id="4624b-148">Максимальное число байтов, отправленных в приложение, буферы сервера.</span><span class="sxs-lookup"><span data-stu-id="4624b-148">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="4624b-149">Увеличение этого значения позволяет серверу для отправки сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="4624b-149">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="4624b-150">Включены все транспорты.</span><span class="sxs-lookup"><span data-stu-id="4624b-150">All Transports are enabled.</span></span> | <span data-ttu-id="4624b-151">Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="4624b-151">A bitmask of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="4624b-152">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="4624b-152">See below.</span></span> | <span data-ttu-id="4624b-153">Дополнительные параметры, характерные для транспорта длинный опрос.</span><span class="sxs-lookup"><span data-stu-id="4624b-153">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="4624b-154">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="4624b-154">See below.</span></span> | <span data-ttu-id="4624b-155">Дополнительные параметры, характерные для транспорта WebSockets.</span><span class="sxs-lookup"><span data-stu-id="4624b-155">Additional options specific to the WebSockets transport.</span></span> |

<span data-ttu-id="4624b-156">Транспорт длинный опрос имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:</span><span class="sxs-lookup"><span data-stu-id="4624b-156">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="4624b-157">Параметр</span><span class="sxs-lookup"><span data-stu-id="4624b-157">Option</span></span> | <span data-ttu-id="4624b-158">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4624b-158">Default Value</span></span> | <span data-ttu-id="4624b-159">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-159">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="4624b-160">90 секунд</span><span class="sxs-lookup"><span data-stu-id="4624b-160">90 seconds</span></span> | <span data-ttu-id="4624b-161">Максимальное время ожидания сервером ответа сообщение, отправляемое клиенту перед завершением работы запрос на единый опроса.</span><span class="sxs-lookup"><span data-stu-id="4624b-161">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="4624b-162">Уменьшение этого значения приводит к клиентам выполнять чаще, новые запросы опроса.</span><span class="sxs-lookup"><span data-stu-id="4624b-162">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="4624b-163">Транспорт WebSocket имеет дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:</span><span class="sxs-lookup"><span data-stu-id="4624b-163">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="4624b-164">Параметр</span><span class="sxs-lookup"><span data-stu-id="4624b-164">Option</span></span> | <span data-ttu-id="4624b-165">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4624b-165">Default Value</span></span> | <span data-ttu-id="4624b-166">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-166">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="4624b-167">5 секунд</span><span class="sxs-lookup"><span data-stu-id="4624b-167">5 seconds</span></span> | <span data-ttu-id="4624b-168">После завершения работы сервера, если клиенту не удается закрыть в течение этого времени интервала, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="4624b-168">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="4624b-169">Делегат, который может использоваться для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение.</span><span class="sxs-lookup"><span data-stu-id="4624b-169">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="4624b-170">Делегат получает значения, запрошенной клиентом в качестве входных данных и должен возвращать необходимое значение.</span><span class="sxs-lookup"><span data-stu-id="4624b-170">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="4624b-171">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="4624b-171">Configure client options</span></span>

<span data-ttu-id="4624b-172">Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступна в клиентах .NET и JavaScript), а также в `HubConnection` сам.</span><span class="sxs-lookup"><span data-stu-id="4624b-172">Client options can be configured on the `HubConnectionBuilder` type (available in both .NET and JavaScript clients), as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="4624b-173">Настройка ведения журнала</span><span class="sxs-lookup"><span data-stu-id="4624b-173">Configure logging</span></span>

<span data-ttu-id="4624b-174">Настроить ведение журнала с помощью клиента .NET `ConfigureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="4624b-174">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="4624b-175">Ведение журнала, поставщики и фильтры могут быть зарегистрированы в так же как при работе на сервере.</span><span class="sxs-lookup"><span data-stu-id="4624b-175">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="4624b-176">См. в разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Дополнительные сведения см.</span><span class="sxs-lookup"><span data-stu-id="4624b-176">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="4624b-177">Чтобы зарегистрировать поставщики ведения журналов, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="4624b-177">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="4624b-178">См. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) разделе документы с полным списком.</span><span class="sxs-lookup"><span data-stu-id="4624b-178">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="4624b-179">Например, чтобы включить ведение журнала консоли, установить `Microsoft.Extensions.Logging.Console` пакет NuGet.</span><span class="sxs-lookup"><span data-stu-id="4624b-179">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="4624b-180">Вызовите `AddConsole` метод расширения:</span><span class="sxs-lookup"><span data-stu-id="4624b-180">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="4624b-181">В клиенте JavaScript, аналогичное `configureLogging` метод существует.</span><span class="sxs-lookup"><span data-stu-id="4624b-181">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="4624b-182">Укажите `LogLevel` значение, указывающее минимальный уровень журнала сообщений для получения.</span><span class="sxs-lookup"><span data-stu-id="4624b-182">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="4624b-183">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="4624b-183">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> <span data-ttu-id="4624b-184">Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.</span><span class="sxs-lookup"><span data-stu-id="4624b-184">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="4624b-185">Ниже перечислены уровни ведения журнала, доступные для клиента JavaScript.</span><span class="sxs-lookup"><span data-stu-id="4624b-185">Log levels available to the JavaScript client are listed below.</span></span> <span data-ttu-id="4624b-186">Установка уровня журнала в одно из следующих значений включает ведение журнала сообщений в **или более поздней версии** этого уровня.</span><span class="sxs-lookup"><span data-stu-id="4624b-186">Setting the log level to one of these values enables logging of messages at **or above** that level.</span></span>

| <span data-ttu-id="4624b-187">Уровень</span><span class="sxs-lookup"><span data-stu-id="4624b-187">Level</span></span> | <span data-ttu-id="4624b-188">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-188">Description</span></span> |
| ----- | ----------- |
| `None` | <span data-ttu-id="4624b-189">Сообщения не регистрируются.</span><span class="sxs-lookup"><span data-stu-id="4624b-189">No messages are logged.</span></span> |
| `Critical` | <span data-ttu-id="4624b-190">Сообщения, которые означают сбой всего приложения.</span><span class="sxs-lookup"><span data-stu-id="4624b-190">Messages that indicate a failure in the entire app.</span></span> |
| `Error` | <span data-ttu-id="4624b-191">Сообщения, которые указывают на сбой в текущей операции.</span><span class="sxs-lookup"><span data-stu-id="4624b-191">Messages that indicate a failure in the current operation.</span></span> |
| `Warning` | <span data-ttu-id="4624b-192">Сообщения, которые указывают на проблему, не являющиеся неустранимыми.</span><span class="sxs-lookup"><span data-stu-id="4624b-192">Messages that indicate a non-fatal problem.</span></span> |
| `Information` | <span data-ttu-id="4624b-193">Информационные сообщения.</span><span class="sxs-lookup"><span data-stu-id="4624b-193">Informational messages.</span></span> |
| `Debug` | <span data-ttu-id="4624b-194">Диагностические сообщения, полезны для отладки.</span><span class="sxs-lookup"><span data-stu-id="4624b-194">Diagnostic messages useful for debugging.</span></span> |
| `Trace` | <span data-ttu-id="4624b-195">Очень подробные диагностические сообщения, предназначены для диагностики конкретных проблем.</span><span class="sxs-lookup"><span data-stu-id="4624b-195">Very detailed diagnostic messages designed for diagnosing specific issues.</span></span> |

### <a name="configure-allowed-transports"></a><span data-ttu-id="4624b-196">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="4624b-196">Configure allowed transports</span></span>

<span data-ttu-id="4624b-197">Можно настроить транспорт, используемый с SignalR в `WithUrl` вызова (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4624b-197">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="4624b-198">Побитовое или-значения `HttpTransportType` может использоваться для ограничения клиента на использование только указанного транспорта.</span><span class="sxs-lookup"><span data-stu-id="4624b-198">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="4624b-199">Все транспорты включены по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="4624b-199">All transports are enabled by default.</span></span>

<span data-ttu-id="4624b-200">Например чтобы отключить события Server-Sent транспорта, но подключений WebSockets и длинный опрос:</span><span class="sxs-lookup"><span data-stu-id="4624b-200">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="4624b-201">В клиенте JavaScript транспортов настраиваются, задав `transport` на объект параметры, передаваемые `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4624b-201">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a><span data-ttu-id="4624b-202">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="4624b-202">Configure bearer authentication</span></span>

<span data-ttu-id="4624b-203">Чтобы указать данные проверки подлинности, а также SignalR запросов, используйте `AccessTokenProvider` параметр (`accessTokenFactory` в JavaScript) для указания функция, которая возвращает маркер необходимый тип доступа.</span><span class="sxs-lookup"><span data-stu-id="4624b-203">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="4624b-204">В клиенте .NET, этот маркер передается как HTTP «Проверки подлинности носителя» токена (с помощью `Authorization` заголовка с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="4624b-204">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="4624b-205">В клиенте JavaScript используется маркер доступа в качестве токена носителя, **за исключением** в некоторых случаях, где обозреватель API-интерфейсы ограничить возможность применения заголовки (в частности, в запросы событий Server-Sent и WebSockets).</span><span class="sxs-lookup"><span data-stu-id="4624b-205">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="4624b-206">В этих случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="4624b-206">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="4624b-207">В клиенте .NET `AccessTokenProvider` параметр может быть указан с помощью делегата параметры в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="4624b-207">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

<span data-ttu-id="4624b-208">В клиенте JavaScript настроен маркер доступа, задав `accessTokenFactory` на объект параметров в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4624b-208">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="4624b-209">Настройка времени ожидания и параметры проверки активности</span><span class="sxs-lookup"><span data-stu-id="4624b-209">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="4624b-210">Доступны дополнительные параметры для настройки времени ожидания и поведение активности на `HubConnection` сам объект:</span><span class="sxs-lookup"><span data-stu-id="4624b-210">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>

| <span data-ttu-id="4624b-211">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="4624b-211">.NET Option</span></span> | <span data-ttu-id="4624b-212">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="4624b-212">JavaScript Option</span></span> | <span data-ttu-id="4624b-213">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4624b-213">Default Value</span></span> | <span data-ttu-id="4624b-214">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-214">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | <span data-ttu-id="4624b-215">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="4624b-215">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="4624b-216">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="4624b-216">Timeout for server activity.</span></span> <span data-ttu-id="4624b-217">Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4624b-217">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="4624b-218">Недоступно для настройки</span><span class="sxs-lookup"><span data-stu-id="4624b-218">Not configurable</span></span> | <span data-ttu-id="4624b-219">15 секунд</span><span class="sxs-lookup"><span data-stu-id="4624b-219">15 seconds</span></span> | <span data-ttu-id="4624b-220">Время ожидания подтверждения исходным сервером.</span><span class="sxs-lookup"><span data-stu-id="4624b-220">Timeout for initial server handshake.</span></span> <span data-ttu-id="4624b-221">Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `Closed` событий (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="4624b-221">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="4624b-222">Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения.</span><span class="sxs-lookup"><span data-stu-id="4624b-222">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="4624b-223">Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="4624b-223">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="4624b-224">В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения.</span><span class="sxs-lookup"><span data-stu-id="4624b-224">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span> <span data-ttu-id="4624b-225">В клиенте JavaScript значения времени ожидания задаются как число, указывающее длительность в миллисекундах.</span><span class="sxs-lookup"><span data-stu-id="4624b-225">In the JavaScript client, timeout values are specified as a number indicating the duration in milliseconds.</span></span>

### <a name="configure-additional-options"></a><span data-ttu-id="4624b-226">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="4624b-226">Configure additional options</span></span>

<span data-ttu-id="4624b-227">Дополнительные параметры можно настроить в `WithUrl` (`withUrl` в JavaScript) метод `HubConnectionBuilder`:</span><span class="sxs-lookup"><span data-stu-id="4624b-227">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder`:</span></span>

| <span data-ttu-id="4624b-228">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="4624b-228">.NET Option</span></span> | <span data-ttu-id="4624b-229">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="4624b-229">JavaScript Option</span></span> | <span data-ttu-id="4624b-230">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="4624b-230">Default Value</span></span> | <span data-ttu-id="4624b-231">Описание:</span><span class="sxs-lookup"><span data-stu-id="4624b-231">Description</span></span> |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | <span data-ttu-id="4624b-232">Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="4624b-232">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `skipNegotiation` | `false` | <span data-ttu-id="4624b-233">Задайте значение `true` пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="4624b-233">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="4624b-234">**Поддерживается только при передаче WebSockets только транспорта включена**.</span><span class="sxs-lookup"><span data-stu-id="4624b-234">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="4624b-235">Этот параметр не может быть включен, при использовании служба Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="4624b-235">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="4624b-236">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-236">Not configurable \*</span></span> | <span data-ttu-id="4624b-237">Empty</span><span class="sxs-lookup"><span data-stu-id="4624b-237">Empty</span></span> | <span data-ttu-id="4624b-238">Коллекция TLS-сертификатов, отправляемых для проверки подлинности запросов.</span><span class="sxs-lookup"><span data-stu-id="4624b-238">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="4624b-239">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-239">Not configurable \*</span></span> | <span data-ttu-id="4624b-240">Empty</span><span class="sxs-lookup"><span data-stu-id="4624b-240">Empty</span></span> | <span data-ttu-id="4624b-241">Коллекция файлов cookie HTTP для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="4624b-241">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="4624b-242">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-242">Not configurable \*</span></span> | <span data-ttu-id="4624b-243">Empty</span><span class="sxs-lookup"><span data-stu-id="4624b-243">Empty</span></span> | <span data-ttu-id="4624b-244">Учетные данные для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="4624b-244">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="4624b-245">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-245">Not configurable \*</span></span> | <span data-ttu-id="4624b-246">5 секунд</span><span class="sxs-lookup"><span data-stu-id="4624b-246">5 seconds</span></span> | <span data-ttu-id="4624b-247">WebSockets.</span><span class="sxs-lookup"><span data-stu-id="4624b-247">WebSockets only.</span></span> <span data-ttu-id="4624b-248">Максимальное количество времени, клиент ожидает после закрывающего тега для сервера подтвердить запрос на закрытие.</span><span class="sxs-lookup"><span data-stu-id="4624b-248">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="4624b-249">Если сервер не подтвердил закрытия в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="4624b-249">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="4624b-250">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-250">Not configurable \*</span></span> | <span data-ttu-id="4624b-251">Empty</span><span class="sxs-lookup"><span data-stu-id="4624b-251">Empty</span></span> | <span data-ttu-id="4624b-252">Словарь дополнительных HTTP-заголовков для отправки с каждым запросом HTTP.</span><span class="sxs-lookup"><span data-stu-id="4624b-252">A dictionary of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | <span data-ttu-id="4624b-253">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-253">Not configurable \*</span></span> | `null` | <span data-ttu-id="4624b-254">Делегат, который может использоваться для настройки или замените `HttpMessageHandler` используется для отправки запросов HTTP.</span><span class="sxs-lookup"><span data-stu-id="4624b-254">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="4624b-255">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4624b-255">Not used for WebSocket connections.</span></span> <span data-ttu-id="4624b-256">Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="4624b-256">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="4624b-257">Измените параметры на это значение по умолчанию и вернуть его или возвращают новую полностью `HttpMessageHandler` экземпляра.</span><span class="sxs-lookup"><span data-stu-id="4624b-257">Either modify settings on that default value and return it, or return a completely new `HttpMessageHandler` instance.</span></span> |
| `Proxy` | <span data-ttu-id="4624b-258">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-258">Not configurable \*</span></span> | `null` | <span data-ttu-id="4624b-259">HTTP-прокси для использования при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="4624b-259">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | <span data-ttu-id="4624b-260">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-260">Not configurable \*</span></span> | `false` | <span data-ttu-id="4624b-261">Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="4624b-261">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="4624b-262">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="4624b-262">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | <span data-ttu-id="4624b-263">Недоступно для настройки \*</span><span class="sxs-lookup"><span data-stu-id="4624b-263">Not configurable \*</span></span> | `null` | <span data-ttu-id="4624b-264">Делегат, который может использоваться для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="4624b-264">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="4624b-265">Получает экземпляр класса [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="4624b-265">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

<span data-ttu-id="4624b-266">Параметры, отмеченные звездочкой (\*) не может быть настроено в клиенте JavaScript из-за ограничений в браузере API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="4624b-266">Options marked with an asterisk (\*) aren't configurable in the JavaScript client, due to limitations in browser APIs.</span></span>

<span data-ttu-id="4624b-267">В клиенте .NET эти параметры можно изменить параметры делегат для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="4624b-267">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="4624b-268">В клиенте JavaScript, эти параметры могут быть предоставлены в JavaScript объект, предоставляемый для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="4624b-268">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    });
    .build();
```

## <a name="additional-resources"></a><span data-ttu-id="4624b-269">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4624b-269">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
