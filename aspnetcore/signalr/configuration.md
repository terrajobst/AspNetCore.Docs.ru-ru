---
title: Конфигурация SignalR ASP.NET Core
author: bradygaster
description: Узнайте, как настроить приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 08/05/2019
uid: signalr/configuration
ms.openlocfilehash: 66f274fcda27392091de6b4be8c7221bc87b7585
ms.sourcegitcommit: c452e6af92e130413106c4863193f377cde4cd9c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/10/2019
ms.locfileid: "72246496"
---
# <a name="aspnet-core-signalr-configuration"></a><span data-ttu-id="d4b9f-103">Конфигурация SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4b9f-103">ASP.NET Core SignalR configuration</span></span>

## <a name="jsonmessagepack-serialization-options"></a><span data-ttu-id="d4b9f-104">Параметры сериализации JSON/MessagePack</span><span class="sxs-lookup"><span data-stu-id="d4b9f-104">JSON/MessagePack serialization options</span></span>

<span data-ttu-id="d4b9f-105">ASP.NET Core SignalR поддерживает два протокола кодирования сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-105">ASP.NET Core SignalR supports two protocols for encoding messages: [JSON](https://www.json.org/) and [MessagePack](https://msgpack.org/index.html).</span></span> <span data-ttu-id="d4b9f-106">Для каждого протокола предусмотрены параметры конфигурации сериализации.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-106">Each protocol has serialization configuration options.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d4b9f-107">Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) .</span><span class="sxs-lookup"><span data-stu-id="d4b9f-107">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method.</span></span> <span data-ttu-id="d4b9f-108">`AddJsonProtocol` можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-108">`AddJsonProtocol` can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="d4b9f-109">Метод `AddJsonProtocol` принимает делегат, который получает объект `options`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-109">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="d4b9f-110">Свойство [пайлоадсериализероптионс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) этого объекта представляет собой объект `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>, который можно использовать для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-110">The [PayloadSerializerOptions](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) property on that object is a `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions> object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="d4b9f-111">Дополнительные сведения см. в [документации System. Text. JSON](/dotnet/api/system.text.json).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-111">For more information, see the [System.Text.Json documentation](/dotnet/api/system.text.json).</span></span>

<span data-ttu-id="d4b9f-112">Например, чтобы настроить сериализатор так, чтобы он не менял регистр имен свойств, вместо имен по умолчанию "camelCase" используйте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-112">As an example, to configure the serializer to not change the casing of property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

<span data-ttu-id="d4b9f-113">В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-113">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="d4b9f-114">Чтобы разрешить метод расширения, необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-114">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

### <a name="switch-to-newtonsoftjson"></a><span data-ttu-id="d4b9f-115">Переключиться на Newtonsoft. JSON</span><span class="sxs-lookup"><span data-stu-id="d4b9f-115">Switch to Newtonsoft.Json</span></span>

<span data-ttu-id="d4b9f-116">Если вам нужны функции `Newtonsoft.Json`, которые не поддерживаются в `System.Text.Json`, см. раздел [Переключение на Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-116">If you need features of `Newtonsoft.Json` that aren't supported in `System.Text.Json`, See [Switch to Newtonsoft.Json](xref:migration/22-to-30#switch-to-newtonsoftjson).</span></span>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d4b9f-117">Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , который можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в методе `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-117">JSON serialization can be configured on the server using the [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) extension method, which can be added after [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) in your `Startup.ConfigureServices` method.</span></span> <span data-ttu-id="d4b9f-118">Метод `AddJsonProtocol` принимает делегат, который получает объект `options`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-118">The `AddJsonProtocol` method takes a delegate that receives an `options` object.</span></span> <span data-ttu-id="d4b9f-119">Свойство [пайлоадсериализерсеттингс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) этого объекта является объектом JSON.NET `JsonSerializerSettings`, который можно использовать для настройки сериализации аргументов и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-119">The [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) property on that object is a JSON.NET `JsonSerializerSettings` object that can be used to configure serialization of arguments and return values.</span></span> <span data-ttu-id="d4b9f-120">Дополнительные сведения см. в [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-120">For more information, see the [JSON.NET documentation](https://www.newtonsoft.com/json/help/html/Introduction.htm).</span></span>
 
<span data-ttu-id="d4b9f-121">Например, чтобы настроить сериализатор для использования имен свойств "PascalCase" вместо имен по умолчанию "camelCase", используйте следующий код в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-121">As an example, to configure the serializer to use "PascalCase" property names, instead of the default "camelCase" names, use the following code in `Startup.ConfigureServices`:</span></span>
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

<span data-ttu-id="d4b9f-122">В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-122">In the .NET client, the same `AddJsonProtocol` extension method exists on [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder).</span></span> <span data-ttu-id="d4b9f-123">Чтобы разрешить метод расширения, необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-123">The `Microsoft.Extensions.DependencyInjection` namespace must be imported to resolve the extension method:</span></span>

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

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d4b9f-124">В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-124">It's not possible to configure JSON serialization in the JavaScript client at this time.</span></span>

### <a name="messagepack-serialization-options"></a><span data-ttu-id="d4b9f-125">Параметры сериализации MessagePack</span><span class="sxs-lookup"><span data-stu-id="d4b9f-125">MessagePack serialization options</span></span>

<span data-ttu-id="d4b9f-126">MessagePack сериализацию можно настроить, предоставив делегат вызову [аддмессажепаккпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) .</span><span class="sxs-lookup"><span data-stu-id="d4b9f-126">MessagePack serialization can be configured by providing a delegate to the [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) call.</span></span> <span data-ttu-id="d4b9f-127">Дополнительные сведения см. [в разделе MessagePack в SignalR](xref:signalr/messagepackhubprotocol) .</span><span class="sxs-lookup"><span data-stu-id="d4b9f-127">See [MessagePack in SignalR](xref:signalr/messagepackhubprotocol) for more details.</span></span>

> [!NOTE]
> <span data-ttu-id="d4b9f-128">В настоящее время невозможно настроить сериализацию MessagePack в клиенте JavaScript.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-128">It's not possible to configure MessagePack serialization in the JavaScript client at this time.</span></span>

## <a name="configure-server-options"></a><span data-ttu-id="d4b9f-129">Настройка параметров сервера</span><span class="sxs-lookup"><span data-stu-id="d4b9f-129">Configure server options</span></span>

<span data-ttu-id="d4b9f-130">В следующей таблице описаны параметры для настройки концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-130">The following table describes options for configuring SignalR hubs:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="d4b9f-131">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-131">Option</span></span> | <span data-ttu-id="d4b9f-132">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-132">Default Value</span></span> | <span data-ttu-id="d4b9f-133">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-133">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="d4b9f-134">30 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-134">30 seconds</span></span> | <span data-ttu-id="d4b9f-135">Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности").</span><span class="sxs-lookup"><span data-stu-id="d4b9f-135">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="d4b9f-136">Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-136">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="d4b9f-137">Рекомендуемое значение равно удвоенному значению `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-137">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="d4b9f-138">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-138">15 seconds</span></span> | <span data-ttu-id="d4b9f-139">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-139">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="d4b9f-140">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-140">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-141">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-141">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d4b9f-142">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-142">15 seconds</span></span> | <span data-ttu-id="d4b9f-143">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-143">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="d4b9f-144">При изменении `KeepAliveInterval` измените параметр `ServerTimeout` @ no__t-2 @ no__t-3 на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-144">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="d4b9f-145">Рекомендуемое значение `ServerTimeout` @ no__t-1 @ no__t-2 равно удвоенному значению `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-145">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="d4b9f-146">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="d4b9f-146">All installed protocols</span></span> | <span data-ttu-id="d4b9f-147">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-147">Protocols supported by this hub.</span></span> <span data-ttu-id="d4b9f-148">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-148">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="d4b9f-149">Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-149">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d4b9f-150">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-150">The default is `false`, as these exception messages can contain sensitive information.</span></span> |
| `StreamBufferCapacity` | `10` | <span data-ttu-id="d4b9f-151">Максимальное число элементов, которые могут быть помещены в буфер для потоков загрузки клиента.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-151">The maximum number of items that can be buffered for client upload streams.</span></span> <span data-ttu-id="d4b9f-152">При достижении этого предела обработка вызовов блокируется до тех пор, пока сервер не обработает потоковые элементы.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-152">If this limit is reached, the processing of invocations is blocked until the server processes stream items.</span></span>|
| `MaximumReceiveMessageSize` | <span data-ttu-id="d4b9f-153">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d4b9f-153">32 KB</span></span> | <span data-ttu-id="d4b9f-154">Максимальный размер одного входящего сообщения концентратора.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-154">Maximum size of a single incoming hub message.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| <span data-ttu-id="d4b9f-155">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-155">Option</span></span> | <span data-ttu-id="d4b9f-156">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-156">Default Value</span></span> | <span data-ttu-id="d4b9f-157">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-157">Description</span></span> |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | <span data-ttu-id="d4b9f-158">30 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-158">30 seconds</span></span> | <span data-ttu-id="d4b9f-159">Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности").</span><span class="sxs-lookup"><span data-stu-id="d4b9f-159">The server will consider the client disconnected if it hasn't received a message (including keep-alive) in this interval.</span></span> <span data-ttu-id="d4b9f-160">Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-160">It could take longer than this timeout interval for the client to actually be marked disconnected, due to how this is implemented.</span></span> <span data-ttu-id="d4b9f-161">Рекомендуемое значение равно удвоенному значению `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-161">The recommended value is double the `KeepAliveInterval` value.</span></span>|
| `HandshakeTimeout` | <span data-ttu-id="d4b9f-162">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-162">15 seconds</span></span> | <span data-ttu-id="d4b9f-163">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-163">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="d4b9f-164">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-164">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-165">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-165">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d4b9f-166">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-166">15 seconds</span></span> | <span data-ttu-id="d4b9f-167">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-167">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="d4b9f-168">При изменении `KeepAliveInterval` измените параметр `ServerTimeout` @ no__t-2 @ no__t-3 на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-168">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="d4b9f-169">Рекомендуемое значение `ServerTimeout` @ no__t-1 @ no__t-2 равно удвоенному значению `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-169">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="d4b9f-170">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="d4b9f-170">All installed protocols</span></span> | <span data-ttu-id="d4b9f-171">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-171">Protocols supported by this hub.</span></span> <span data-ttu-id="d4b9f-172">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-172">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="d4b9f-173">Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-173">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d4b9f-174">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-174">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| <span data-ttu-id="d4b9f-175">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-175">Option</span></span> | <span data-ttu-id="d4b9f-176">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-176">Default Value</span></span> | <span data-ttu-id="d4b9f-177">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-177">Description</span></span> |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | <span data-ttu-id="d4b9f-178">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-178">15 seconds</span></span> | <span data-ttu-id="d4b9f-179">Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-179">If the client doesn't send an initial handshake message within this time interval, the connection is closed.</span></span> <span data-ttu-id="d4b9f-180">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-180">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-181">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-181">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d4b9f-182">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-182">15 seconds</span></span> | <span data-ttu-id="d4b9f-183">Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-183">If the server hasn't sent a message within this interval, a ping message is sent automatically to keep the connection open.</span></span> <span data-ttu-id="d4b9f-184">При изменении `KeepAliveInterval` измените параметр `ServerTimeout` @ no__t-2 @ no__t-3 на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-184">When changing `KeepAliveInterval`, change the `ServerTimeout`/`serverTimeoutInMilliseconds` setting on the client.</span></span> <span data-ttu-id="d4b9f-185">Рекомендуемое значение `ServerTimeout` @ no__t-1 @ no__t-2 равно удвоенному значению `KeepAliveInterval`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-185">The recommended `ServerTimeout`/`serverTimeoutInMilliseconds` value is double the `KeepAliveInterval` value.</span></span>  |
| `SupportedProtocols` | <span data-ttu-id="d4b9f-186">Все установленные протоколы</span><span class="sxs-lookup"><span data-stu-id="d4b9f-186">All installed protocols</span></span> | <span data-ttu-id="d4b9f-187">Протоколы, поддерживаемые этим концентратором.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-187">Protocols supported by this hub.</span></span> <span data-ttu-id="d4b9f-188">По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-188">By default, all protocols registered on the server are allowed, but protocols can be removed from this list to disable specific protocols for individual hubs.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="d4b9f-189">Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-189">If `true`, detailed exception messages are returned to clients when an exception is thrown in a Hub method.</span></span> <span data-ttu-id="d4b9f-190">Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-190">The default is `false`, as these exception messages can contain sensitive information.</span></span> |

::: moniker-end

<span data-ttu-id="d4b9f-191">Параметры можно настроить для всех концентраторов, предоставив параметры делегата для вызова `AddSignalR` в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-191">Options can be configured for all hubs by providing an options delegate to the `AddSignalR` call in `Startup.ConfigureServices`.</span></span>

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

<span data-ttu-id="d4b9f-192">Параметры одного концентратора переопределяют глобальные параметры, предоставленные в `AddSignalR`, и могут быть настроены с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-192">Options for a single hub override the global options provided in `AddSignalR` and can be configured using <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:</span></span>

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a><span data-ttu-id="d4b9f-193">Дополнительные параметры конфигурации HTTP</span><span class="sxs-lookup"><span data-stu-id="d4b9f-193">Advanced HTTP configuration options</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d4b9f-194">Используйте `HttpConnectionDispatcherOptions`, чтобы настроить дополнительные параметры, связанные с транспортами и управлением буферами памяти.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-194">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="d4b9f-195">Эти параметры настраиваются путем передачи делегата в [мафуб @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-195">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) in `Startup.Configure`.</span></span>

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
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="d4b9f-196">Используйте `HttpConnectionDispatcherOptions`, чтобы настроить дополнительные параметры, связанные с транспортами и управлением буферами памяти.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-196">Use `HttpConnectionDispatcherOptions` to configure advanced settings related to transports and memory buffer management.</span></span> <span data-ttu-id="d4b9f-197">Эти параметры настраиваются путем передачи делегата в [мафуб @ no__t-1T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-197">These options are configured by passing a delegate to [MapHub\<T>](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) in `Startup.Configure`.</span></span>

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

::: moniker-end

<span data-ttu-id="d4b9f-198">В следующей таблице описаны параметры для настройки расширенных HTTP-параметров SignalR ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-198">The following table describes options for configuring ASP.NET Core SignalR's advanced HTTP options:</span></span>

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="d4b9f-199">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-199">Option</span></span> | <span data-ttu-id="d4b9f-200">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-200">Default Value</span></span> | <span data-ttu-id="d4b9f-201">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-201">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="d4b9f-202">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d4b9f-202">32 KB</span></span> | <span data-ttu-id="d4b9f-203">Максимальное число байтов, полученных от клиента, который буфер сервера перед применением обратной нагрузки.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-203">The maximum number of bytes received from the client that the server buffers before applying backpressure.</span></span> <span data-ttu-id="d4b9f-204">Увеличение этого значения позволяет серверу быстрее принимать сообщения большего размера без применения недостаточной нагрузки, но может увеличить потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-204">Increasing this value allows the server to receive larger messages more quickly without applying backpressure, but can increase memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="d4b9f-205">Данные автоматически собираются из атрибутов `Authorize`, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-205">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="d4b9f-206">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-206">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="d4b9f-207">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d4b9f-207">32 KB</span></span> | <span data-ttu-id="d4b9f-208">Максимальное число байтов, отправленных приложением, которые были буфером сервера, прежде чем наблюдать за нехваткой.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-208">The maximum number of bytes sent by the app that the server buffers before observing backpressure.</span></span> <span data-ttu-id="d4b9f-209">Увеличение этого значения позволяет серверу быстрее занимать больше сообщений, не ожидая перехода на резервную нагрузку, но может увеличить потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-209">Increasing this value allows the server to buffer larger messages more quickly without awaiting backpressure, but can increase memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="d4b9f-210">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-210">All Transports are enabled.</span></span> | <span data-ttu-id="d4b9f-211">Побитовое перечисление значений `HttpTransportType`, которые могут ограничивать использование транспортов, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-211">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="d4b9f-212">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-212">See below.</span></span> | <span data-ttu-id="d4b9f-213">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-213">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="d4b9f-214">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-214">See below.</span></span> | <span data-ttu-id="d4b9f-215">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-215">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="d4b9f-216">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-216">Option</span></span> | <span data-ttu-id="d4b9f-217">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-217">Default Value</span></span> | <span data-ttu-id="d4b9f-218">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-218">Description</span></span> |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | <span data-ttu-id="d4b9f-219">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d4b9f-219">32 KB</span></span> | <span data-ttu-id="d4b9f-220">Максимальное число байтов, полученных от клиента, который является буфером сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-220">The maximum number of bytes received from the client that the server buffers.</span></span> <span data-ttu-id="d4b9f-221">Увеличение этого значения позволяет серверу принимать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-221">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `AuthorizationData` | <span data-ttu-id="d4b9f-222">Данные автоматически собираются из атрибутов `Authorize`, применяемых к классу Hub.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-222">Data automatically gathered from the `Authorize` attributes applied to the Hub class.</span></span> | <span data-ttu-id="d4b9f-223">Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-223">A list of [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) objects used to determine if a client is authorized to connect to the hub.</span></span> |
| `TransportMaxBufferSize` | <span data-ttu-id="d4b9f-224">32 КБ</span><span class="sxs-lookup"><span data-stu-id="d4b9f-224">32 KB</span></span> | <span data-ttu-id="d4b9f-225">Максимальное число байтов, отправленных приложением, которые используются в буфере сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-225">The maximum number of bytes sent by the app that the server buffers.</span></span> <span data-ttu-id="d4b9f-226">Увеличение этого значения позволяет серверу передавать большие сообщения, но может негативно сказаться на потреблении памяти.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-226">Increasing this value allows the server to send larger messages, but can negatively impact memory consumption.</span></span> |
| `Transports` | <span data-ttu-id="d4b9f-227">Все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-227">All Transports are enabled.</span></span> | <span data-ttu-id="d4b9f-228">Побитовое перечисление значений `HttpTransportType`, которые могут ограничивать использование транспортов, которые клиент может использовать для подключения.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-228">A bit flags enum of `HttpTransportType` values that can restrict the transports a client can use to connect.</span></span> |
| `LongPolling` | <span data-ttu-id="d4b9f-229">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-229">See below.</span></span> | <span data-ttu-id="d4b9f-230">Дополнительные параметры, относящиеся к длительному транспортному потоку.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-230">Additional options specific to the Long Polling transport.</span></span> |
| `WebSockets` | <span data-ttu-id="d4b9f-231">См. ниже.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-231">See below.</span></span> | <span data-ttu-id="d4b9f-232">Дополнительные параметры, относящиеся к транспортному протоколу WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-232">Additional options specific to the WebSockets transport.</span></span> |

::: moniker-end

<span data-ttu-id="d4b9f-233">Долгосрочный транспорт с длительным опросом имеет дополнительные параметры, которые можно настроить с помощью свойства `LongPolling`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-233">The Long Polling transport has additional options that can be configured using the `LongPolling` property:</span></span>

| <span data-ttu-id="d4b9f-234">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-234">Option</span></span> | <span data-ttu-id="d4b9f-235">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-235">Default Value</span></span> | <span data-ttu-id="d4b9f-236">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-236">Description</span></span> |
| ------ | ------------- | ----------- |
| `PollTimeout` | <span data-ttu-id="d4b9f-237">90 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-237">90 seconds</span></span> | <span data-ttu-id="d4b9f-238">Максимальное количество времени, в течение которого сервер ожидает отправки сообщения клиенту перед завершением одного запроса опроса.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-238">The maximum amount of time the server waits for a message to send to the client before terminating a single poll request.</span></span> <span data-ttu-id="d4b9f-239">Уменьшение этого значения приводит к тому, что клиент будет чаще выдавать новые запросы на опрос.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-239">Decreasing this value causes the client to issue new poll requests more frequently.</span></span> |

<span data-ttu-id="d4b9f-240">Транспорт WebSocket имеет дополнительные параметры, которые можно настроить с помощью свойства `WebSockets`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-240">The WebSocket transport has additional options that can be configured using the `WebSockets` property:</span></span>

| <span data-ttu-id="d4b9f-241">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-241">Option</span></span> | <span data-ttu-id="d4b9f-242">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-242">Default Value</span></span> | <span data-ttu-id="d4b9f-243">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-243">Description</span></span> |
| ------ | ------------- | ----------- |
| `CloseTimeout` | <span data-ttu-id="d4b9f-244">5 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-244">5 seconds</span></span> | <span data-ttu-id="d4b9f-245">Когда сервер закрывается, если не удается закрыть клиент в течение этого интервала времени, соединение будет разорвано.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-245">After the server closes, if the client fails to close within this time interval, the connection is terminated.</span></span> |
| `SubProtocolSelector` | `null` | <span data-ttu-id="d4b9f-246">Делегат, который можно использовать для задания заголовка `Sec-WebSocket-Protocol` в пользовательском значении.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-246">A delegate that can be used to set the `Sec-WebSocket-Protocol` header to a custom value.</span></span> <span data-ttu-id="d4b9f-247">Делегат получает значения, запрошенные клиентом в качестве входных данных, и ожидается, что он возвращает нужное значение.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-247">The delegate receives the values requested by the client as input and is expected to return the desired value.</span></span> |

## <a name="configure-client-options"></a><span data-ttu-id="d4b9f-248">Настройка параметров клиента</span><span class="sxs-lookup"><span data-stu-id="d4b9f-248">Configure client options</span></span>

<span data-ttu-id="d4b9f-249">Параметры клиента можно настроить в типе `HubConnectionBuilder` (доступен в клиентах .NET и JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-249">Client options can be configured on the `HubConnectionBuilder` type (available in the .NET and JavaScript clients).</span></span> <span data-ttu-id="d4b9f-250">Он также доступен в клиенте Java, однако подкласс `HttpHubConnectionBuilder` содержит параметры конфигурации построителя, а также сам `HubConnection`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-250">It's also available in the Java client, but the `HttpHubConnectionBuilder` subclass is what contains the builder configuration options, as well as on the `HubConnection` itself.</span></span>

### <a name="configure-logging"></a><span data-ttu-id="d4b9f-251">Настройка ведения журнала</span><span class="sxs-lookup"><span data-stu-id="d4b9f-251">Configure logging</span></span>

<span data-ttu-id="d4b9f-252">Ведение журнала настраивается в клиенте .NET с помощью метода `ConfigureLogging`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-252">Logging is configured in the .NET Client using the `ConfigureLogging` method.</span></span> <span data-ttu-id="d4b9f-253">Регистраторы и фильтры могут быть зарегистрированы так же, как и на сервере.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-253">Logging providers and filters can be registered in the same way as they are on the server.</span></span> <span data-ttu-id="d4b9f-254">Дополнительные сведения см. в документации по [ведению журнала ASP.NET Core](xref:fundamentals/logging/index) .</span><span class="sxs-lookup"><span data-stu-id="d4b9f-254">See the [Logging in ASP.NET Core](xref:fundamentals/logging/index) documentation for more information.</span></span>

> [!NOTE]
> <span data-ttu-id="d4b9f-255">Чтобы зарегистрировать регистраторы, необходимо установить необходимые пакеты.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-255">In order to register Logging providers, you must install the necessary packages.</span></span> <span data-ttu-id="d4b9f-256">Полный список см. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) документации.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-256">See the [Built-in logging providers](xref:fundamentals/logging/index#built-in-logging-providers) section of the docs for a full list.</span></span>

<span data-ttu-id="d4b9f-257">Например, чтобы включить ведение журнала консоли, установите пакет NuGet `Microsoft.Extensions.Logging.Console`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-257">For example, to enable Console logging, install the `Microsoft.Extensions.Logging.Console` NuGet package.</span></span> <span data-ttu-id="d4b9f-258">Вызовите метод расширения `AddConsole`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-258">Call the `AddConsole` extension method:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

<span data-ttu-id="d4b9f-259">В клиенте JavaScript существует аналогичный метод `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-259">In the JavaScript client, a similar `configureLogging` method exists.</span></span> <span data-ttu-id="d4b9f-260">Укажите значение `LogLevel`, указывающее минимальный уровень создаваемых сообщений журнала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-260">Provide a `LogLevel` value indicating the minimum level of log messages to produce.</span></span> <span data-ttu-id="d4b9f-261">Журналы записываются в окно консоли браузера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-261">Logs are written to the browser console window.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d4b9f-262">Вместо значения `LogLevel` можно также указать значение `string`, представляющее имя уровня журнала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-262">Instead of a `LogLevel` value, you can also provide a `string` value representing a log level name.</span></span> <span data-ttu-id="d4b9f-263">Это полезно при настройке ведения журнала SignalR в средах, где у вас нет доступа к константам `LogLevel`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-263">This is useful when configuring SignalR logging in environments where you don't have access to the `LogLevel` constants.</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

<span data-ttu-id="d4b9f-264">В следующей таблице перечислены доступные уровни ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-264">The following table lists the available log levels.</span></span> <span data-ttu-id="d4b9f-265">Значение `configureLogging` задает **Минимальный** уровень ведения журнала, который будет записан в журнал.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-265">The value you provide to `configureLogging` sets the **minimum** log level that will be logged.</span></span> <span data-ttu-id="d4b9f-266">Сообщения, зарегистрированные на этом уровне **или перечисленные в таблице уровни**, будут занесены в журнал.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-266">Messages logged at this level, **or the levels listed after it in the table**, will be logged.</span></span>

| <span data-ttu-id="d4b9f-267">Строка</span><span class="sxs-lookup"><span data-stu-id="d4b9f-267">String</span></span>                      | <span data-ttu-id="d4b9f-268">LogLevel</span><span class="sxs-lookup"><span data-stu-id="d4b9f-268">LogLevel</span></span>               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| <span data-ttu-id="d4b9f-269">`info` **или** `information`</span><span class="sxs-lookup"><span data-stu-id="d4b9f-269">`info` **or** `information`</span></span> | `LogLevel.Information` |
| <span data-ttu-id="d4b9f-270">`warn` **или** `warning`</span><span class="sxs-lookup"><span data-stu-id="d4b9f-270">`warn` **or** `warning`</span></span>     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> <span data-ttu-id="d4b9f-271">Чтобы полностью отключить ведение журнала, укажите `signalR.LogLevel.None` в методе `configureLogging`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-271">To disable logging entirely, specify `signalR.LogLevel.None` in the `configureLogging` method.</span></span>

<span data-ttu-id="d4b9f-272">Дополнительные сведения о ведении журналов см. в [документации по диагностике SignalR](xref:signalr/diagnostics).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-272">For more information on logging, see the [SignalR Diagnostics documentation](xref:signalr/diagnostics).</span></span>

<span data-ttu-id="d4b9f-273">Клиент, использующий SignalR Java, использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-273">The SignalR Java client uses the [SLF4J](https://www.slf4j.org/) library for logging.</span></span> <span data-ttu-id="d4b9f-274">Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-274">It's a high-level logging API that allows users of the library to chose their own specific logging implementation by bringing in a specific logging dependency.</span></span> <span data-ttu-id="d4b9f-275">В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-275">The following code snippet shows how to use `java.util.logging` with the SignalR Java client.</span></span>

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

<span data-ttu-id="d4b9f-276">Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-276">If you don't configure logging in your dependencies, SLF4J loads a default no-operation logger with the following warning message:</span></span>

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

<span data-ttu-id="d4b9f-277">Это можно игнорировать.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-277">This can safely be ignored.</span></span>

### <a name="configure-allowed-transports"></a><span data-ttu-id="d4b9f-278">Настройка разрешенных транспортов</span><span class="sxs-lookup"><span data-stu-id="d4b9f-278">Configure allowed transports</span></span>

<span data-ttu-id="d4b9f-279">Транспорты, используемые SignalR, можно настроить в вызове `WithUrl` (`withUrl` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-279">The transports used by SignalR can be configured in the `WithUrl` call (`withUrl` in JavaScript).</span></span> <span data-ttu-id="d4b9f-280">Побитовое или для значений `HttpTransportType` можно использовать для ограничения использования клиентом только указанных транспортов.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-280">A bitwise-OR of the values of `HttpTransportType` can be used to restrict the client to only use the specified transports.</span></span> <span data-ttu-id="d4b9f-281">По умолчанию все транспорты включены.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-281">All transports are enabled by default.</span></span>

<span data-ttu-id="d4b9f-282">Например, чтобы отключить транспорт событий, отправленных сервером, но разрешить соединения WebSockets и long опрашиваете:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-282">For example, to disable the Server-Sent Events transport, but allow WebSockets and Long Polling connections:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

<span data-ttu-id="d4b9f-283">В клиенте JavaScript транспорты настраиваются путем установки поля `transport` в объекте Options, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-283">In the JavaScript client, transports are configured by setting the `transport` field on the options object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="d4b9f-284">В этой версии клиентские WebSocket-сокеты Java являются единственным доступным транспортом.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-284">In this version of the Java client websockets is the only available transport.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="d4b9f-285">В клиенте Java транспорт выбирается с помощью метода `withTransport` в `HttpHubConnectionBuilder`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-285">In the Java client, the transport is selected with the `withTransport` method on the `HttpHubConnectionBuilder`.</span></span> <span data-ttu-id="d4b9f-286">Клиент Java по умолчанию использует транспорт WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-286">The Java client defaults to using the WebSockets transport.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> <span data-ttu-id="d4b9f-287">Клиент Java SignalR пока не поддерживает откат транспорта.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-287">The SignalR Java client doesn't support transport fallback yet.</span></span>

::: moniker-end

### <a name="configure-bearer-authentication"></a><span data-ttu-id="d4b9f-288">Настройка проверки подлинности носителя</span><span class="sxs-lookup"><span data-stu-id="d4b9f-288">Configure bearer authentication</span></span>

<span data-ttu-id="d4b9f-289">Чтобы предоставить данные проверки подлинности вместе с запросами SignalR, используйте параметр `AccessTokenProvider` (`accessTokenFactory` в JavaScript), чтобы указать функцию, которая возвращает нужный маркер доступа.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-289">To provide authentication data along with SignalR requests, use the `AccessTokenProvider` option (`accessTokenFactory` in JavaScript) to specify a function that returns the desired access token.</span></span> <span data-ttu-id="d4b9f-290">В клиенте .NET этот маркер доступа передается в качестве токена "Проверка подлинности носителя" HTTP (с помощью заголовка `Authorization` с типом `Bearer`).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-290">In the .NET Client, this access token is passed in as an HTTP "Bearer Authentication" token (Using the `Authorization` header with a type of `Bearer`).</span></span> <span data-ttu-id="d4b9f-291">В клиенте JavaScript маркер доступа используется в качестве токена носителя, **за исключением** случаев, когда интерфейсы API браузера ограничивают возможность применения заголовков (в частности, при отправке сервером событий и запросах WebSocket).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-291">In the JavaScript client, the access token is used as a Bearer token, **except** in a few cases where browser APIs restrict the ability to apply headers (specifically, in Server-Sent Events and WebSockets requests).</span></span> <span data-ttu-id="d4b9f-292">В этих случаях маркер доступа предоставляется как значение строки запроса `access_token`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-292">In these cases, the access token is provided as a query string value `access_token`.</span></span>

<span data-ttu-id="d4b9f-293">В клиенте .NET параметр `AccessTokenProvider` можно указать с помощью делегата Options в `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-293">In the .NET client, the `AccessTokenProvider` option can be specified using the options delegate in `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

<span data-ttu-id="d4b9f-294">В клиенте JavaScript маркер доступа настраивается путем установки поля `accessTokenFactory` в объекте Options в `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-294">In the JavaScript client, the access token is configured by setting the `accessTokenFactory` field on the options object in `withUrl`:</span></span>

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

<span data-ttu-id="d4b9f-295">В клиенте SignalR Java можно настроить токен носителя, который будет использоваться для проверки подлинности, предоставив фабрику маркеров доступа для [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-295">In the SignalR Java client, you can configure a bearer token to use for authentication by providing an access token factory to the [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java).</span></span> <span data-ttu-id="d4b9f-296">Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [рксжава](https://github.com/ReactiveX/RxJava) [Single @ no__t-3String >](https://reactivex.io/documentation/single.html).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-296">Use [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) to provide an [RxJava](https://github.com/ReactiveX/RxJava) [Single\<String>](https://reactivex.io/documentation/single.html).</span></span> <span data-ttu-id="d4b9f-297">При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-297">With a call to [Single.defer](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), you can write logic to produce access tokens for your client.</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a><span data-ttu-id="d4b9f-298">Настройка времени ожидания и параметров поддержания активности</span><span class="sxs-lookup"><span data-stu-id="d4b9f-298">Configure timeout and keep-alive options</span></span>

<span data-ttu-id="d4b9f-299">Дополнительные параметры для настройки времени ожидания и проверки активности доступны для самого объекта `HubConnection`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-299">Additional options for configuring timeout and keep-alive behavior are available on the `HubConnection` object itself:</span></span>


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="d4b9f-300">.NET</span><span class="sxs-lookup"><span data-stu-id="d4b9f-300">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="d4b9f-301">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-301">Option</span></span> | <span data-ttu-id="d4b9f-302">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-302">Default value</span></span> | <span data-ttu-id="d4b9f-303">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-303">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="d4b9f-304">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-304">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-305">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-305">Timeout for server activity.</span></span> <span data-ttu-id="d4b9f-306">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-306">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d4b9f-307">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-307">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d4b9f-308">Рекомендуемое значение — это, по крайней мере, двойное значение сервера `KeepAliveInterval`, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-308">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="d4b9f-309">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-309">15 seconds</span></span> | <span data-ttu-id="d4b9f-310">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-310">Timeout for initial server handshake.</span></span> <span data-ttu-id="d4b9f-311">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-311">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d4b9f-312">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-312">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-313">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-313">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| `KeepAliveInterval` | <span data-ttu-id="d4b9f-314">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-314">15 seconds</span></span> | <span data-ttu-id="d4b9f-315">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-315">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="d4b9f-316">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-316">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="d4b9f-317">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-317">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

<span data-ttu-id="d4b9f-318">В клиенте .NET значения времени ожидания указываются как значения `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-318">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="d4b9f-319">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d4b9f-319">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="d4b9f-320">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-320">Option</span></span> | <span data-ttu-id="d4b9f-321">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-321">Default value</span></span> | <span data-ttu-id="d4b9f-322">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-322">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="d4b9f-323">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-323">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-324">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-324">Timeout for server activity.</span></span> <span data-ttu-id="d4b9f-325">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-325">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="d4b9f-326">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-326">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d4b9f-327">Рекомендуемое значение — это, по крайней мере, двойное значение сервера `KeepAliveInterval`, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-327">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `keepAliveIntervalInMilliseconds` | <span data-ttu-id="d4b9f-328">15 секунд (15 000 миллисекундах)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-328">15 seconds (15,000 miliseconds)</span></span> | <span data-ttu-id="d4b9f-329">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-329">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="d4b9f-330">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-330">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="d4b9f-331">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-331">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="d4b9f-332">Java</span><span class="sxs-lookup"><span data-stu-id="d4b9f-332">Java</span></span>](#tab/java)

| <span data-ttu-id="d4b9f-333">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-333">Option</span></span> | <span data-ttu-id="d4b9f-334">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-334">Default value</span></span> | <span data-ttu-id="d4b9f-335">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-335">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="d4b9f-336">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="d4b9f-336">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="d4b9f-337">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-337">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-338">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-338">Timeout for server activity.</span></span> <span data-ttu-id="d4b9f-339">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-339">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="d4b9f-340">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-340">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d4b9f-341">Рекомендуемое значение — это, по крайней мере, двойное значение сервера `KeepAliveInterval`, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-341">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="d4b9f-342">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-342">15 seconds</span></span> | <span data-ttu-id="d4b9f-343">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-343">Timeout for initial server handshake.</span></span> <span data-ttu-id="d4b9f-344">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-344">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="d4b9f-345">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-345">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-346">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-346">For more detail on the Handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |
| <span data-ttu-id="d4b9f-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span><span class="sxs-lookup"><span data-stu-id="d4b9f-347">`getKeepAliveInterval` / `setKeepAliveInterval`</span></span> | <span data-ttu-id="d4b9f-348">15 секунд (15 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-348">15 seconds (15,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-349">Определяет интервал, с которым клиент отправляет сообщения проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-349">Determines the interval at which the client sends ping messages.</span></span> <span data-ttu-id="d4b9f-350">Отправка любого сообщения от клиента сбрасывает таймер до начала интервала.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-350">Sending any message from the client resets the timer to the start of the interval.</span></span> <span data-ttu-id="d4b9f-351">Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-351">If the client hasn't sent a message in the `ClientTimeoutInterval` set on the server, the server considers the client disconnected.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[<span data-ttu-id="d4b9f-352">.NET</span><span class="sxs-lookup"><span data-stu-id="d4b9f-352">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="d4b9f-353">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-353">Option</span></span> | <span data-ttu-id="d4b9f-354">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-354">Default value</span></span> | <span data-ttu-id="d4b9f-355">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-355">Description</span></span> |
| ------ | ------------- | ----------- |
| `ServerTimeout` | <span data-ttu-id="d4b9f-356">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-356">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-357">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-357">Timeout for server activity.</span></span> <span data-ttu-id="d4b9f-358">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-358">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d4b9f-359">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-359">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d4b9f-360">Рекомендуемое значение — это, по крайней мере, двойное значение сервера `KeepAliveInterval`, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-360">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |
| `HandshakeTimeout` | <span data-ttu-id="d4b9f-361">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-361">15 seconds</span></span> | <span data-ttu-id="d4b9f-362">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-362">Timeout for initial server handshake.</span></span> <span data-ttu-id="d4b9f-363">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-363">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `Closed` event (`onclose` in JavaScript).</span></span> <span data-ttu-id="d4b9f-364">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-364">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-365">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-365">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

<span data-ttu-id="d4b9f-366">В клиенте .NET значения времени ожидания указываются как значения `TimeSpan`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-366">In the .NET Client, timeout values are specified as `TimeSpan` values.</span></span>

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="d4b9f-367">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d4b9f-367">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="d4b9f-368">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-368">Option</span></span> | <span data-ttu-id="d4b9f-369">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-369">Default value</span></span> | <span data-ttu-id="d4b9f-370">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-370">Description</span></span> |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | <span data-ttu-id="d4b9f-371">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-371">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-372">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-372">Timeout for server activity.</span></span> <span data-ttu-id="d4b9f-373">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-373">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onclose` event.</span></span> <span data-ttu-id="d4b9f-374">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-374">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d4b9f-375">Рекомендуемое значение — это, по крайней мере, двойное значение сервера `KeepAliveInterval`, чтобы разрешить поступление проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-375">The recommended value is a number at least double the server's `KeepAliveInterval` value to allow time for pings to arrive.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="d4b9f-376">Java</span><span class="sxs-lookup"><span data-stu-id="d4b9f-376">Java</span></span>](#tab/java)

| <span data-ttu-id="d4b9f-377">Параметр</span><span class="sxs-lookup"><span data-stu-id="d4b9f-377">Option</span></span> | <span data-ttu-id="d4b9f-378">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-378">Default value</span></span> | <span data-ttu-id="d4b9f-379">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-379">Description</span></span> |
| ------ | ------------- | ----------- |
| <span data-ttu-id="d4b9f-380">`getServerTimeout` / `setServerTimeout`</span><span class="sxs-lookup"><span data-stu-id="d4b9f-380">`getServerTimeout` / `setServerTimeout`</span></span> | <span data-ttu-id="d4b9f-381">30 секунд (30 000 миллисекунд)</span><span class="sxs-lookup"><span data-stu-id="d4b9f-381">30 seconds (30,000 milliseconds)</span></span> | <span data-ttu-id="d4b9f-382">Время ожидания активности сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-382">Timeout for server activity.</span></span> <span data-ttu-id="d4b9f-383">Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-383">If the server hasn't sent a message in this interval, the client considers the server disconnected and triggers the `onClose` event.</span></span> <span data-ttu-id="d4b9f-384">Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-384">This value must be large enough for a ping message to be sent from the server **and** received by the client within the timeout interval.</span></span> <span data-ttu-id="d4b9f-385">Рекомендуемое значение — это число по крайней мере удвоенного значения `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-385">The recommended value is a number at least double the server's `KeepAliveInterval` value, to allow time for pings to arrive.</span></span> |
| `withHandshakeResponseTimeout` | <span data-ttu-id="d4b9f-386">15 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-386">15 seconds</span></span> | <span data-ttu-id="d4b9f-387">Время ожидания для первоначального подтверждения сервера.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-387">Timeout for initial server handshake.</span></span> <span data-ttu-id="d4b9f-388">Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-388">If the server doesn't send a handshake response in this interval, the client cancels the handshake and triggers the `onClose` event.</span></span> <span data-ttu-id="d4b9f-389">Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-389">This is an advanced setting that should only be modified if handshake timeout errors are occurring due to severe network latency.</span></span> <span data-ttu-id="d4b9f-390">Дополнительные сведения о процессе подтверждения связи см. в статье [Спецификация протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span><span class="sxs-lookup"><span data-stu-id="d4b9f-390">For more detail on the handshake process, see the [SignalR Hub Protocol Specification](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md).</span></span> |

::: moniker-end

---

### <a name="configure-additional-options"></a><span data-ttu-id="d4b9f-391">Настройка дополнительных параметров</span><span class="sxs-lookup"><span data-stu-id="d4b9f-391">Configure additional options</span></span>

<span data-ttu-id="d4b9f-392">Дополнительные параметры можно настроить в методе `WithUrl` (`withUrl` в JavaScript) в `HubConnectionBuilder` или в различных API конфигурации в `HttpHubConnectionBuilder` в клиенте Java:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-392">Additional options can be configured in the `WithUrl` (`withUrl` in JavaScript) method on `HubConnectionBuilder` or on the various configuration APIs on the `HttpHubConnectionBuilder` in the Java client:</span></span>

# <a name="nettabdotnet"></a>[<span data-ttu-id="d4b9f-393">.NET</span><span class="sxs-lookup"><span data-stu-id="d4b9f-393">.NET</span></span>](#tab/dotnet)

| <span data-ttu-id="d4b9f-394">Параметр .NET</span><span class="sxs-lookup"><span data-stu-id="d4b9f-394">.NET Option</span></span> |  <span data-ttu-id="d4b9f-395">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="d4b9f-395">Default value</span></span> | <span data-ttu-id="d4b9f-396">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-396">Description</span></span> |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | <span data-ttu-id="d4b9f-397">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-397">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `SkipNegotiation` | `false` | <span data-ttu-id="d4b9f-398">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-398">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d4b9f-399">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-399">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d4b9f-400">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-400">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| `ClientCertificates` | <span data-ttu-id="d4b9f-401">Empty</span><span class="sxs-lookup"><span data-stu-id="d4b9f-401">Empty</span></span> | <span data-ttu-id="d4b9f-402">Коллекция сертификатов TLS для отправки в запросы проверки подлинности.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-402">A collection of TLS certificates to send to authenticate requests.</span></span> |
| `Cookies` | <span data-ttu-id="d4b9f-403">Empty</span><span class="sxs-lookup"><span data-stu-id="d4b9f-403">Empty</span></span> | <span data-ttu-id="d4b9f-404">Коллекция файлов cookie HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-404">A collection of HTTP cookies to send with every HTTP request.</span></span> |
| `Credentials` | <span data-ttu-id="d4b9f-405">Empty</span><span class="sxs-lookup"><span data-stu-id="d4b9f-405">Empty</span></span> | <span data-ttu-id="d4b9f-406">Учетные данные для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-406">Credentials to send with every HTTP request.</span></span> |
| `CloseTimeout` | <span data-ttu-id="d4b9f-407">5 секунд</span><span class="sxs-lookup"><span data-stu-id="d4b9f-407">5 seconds</span></span> | <span data-ttu-id="d4b9f-408">Только WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-408">WebSockets only.</span></span> <span data-ttu-id="d4b9f-409">Максимальное время ожидания клиента после закрытия сервера для подтверждения запроса на закрытие.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-409">The maximum amount of time the client waits after closing for the server to acknowledge the close request.</span></span> <span data-ttu-id="d4b9f-410">Если сервер не подтверждает закрытие в течение этого времени, клиент отключается.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-410">If the server doesn't acknowledge the close within this time, the client disconnects.</span></span> |
| `Headers` | <span data-ttu-id="d4b9f-411">Empty</span><span class="sxs-lookup"><span data-stu-id="d4b9f-411">Empty</span></span> | <span data-ttu-id="d4b9f-412">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-412">A Map of additional HTTP headers to send with every HTTP request.</span></span> |
| `HttpMessageHandlerFactory` | `null` | <span data-ttu-id="d4b9f-413">Делегат, который можно использовать для настройки или замены `HttpMessageHandler`, используемой для отправки запросов HTTP.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-413">A delegate that can be used to configure or replace the `HttpMessageHandler` used to send HTTP requests.</span></span> <span data-ttu-id="d4b9f-414">Не используется для соединений WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-414">Not used for WebSocket connections.</span></span> <span data-ttu-id="d4b9f-415">Этот делегат должен возвращать значение, отличное от NULL, и получает значение по умолчанию в качестве параметра.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-415">This delegate must return a non-null value, and it receives the default value as a parameter.</span></span> <span data-ttu-id="d4b9f-416">Либо измените параметры для этого значения по умолчанию и верните его, либо возвратите новый экземпляр `HttpMessageHandler`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-416">Either modify settings on that default value and return it, or return a new `HttpMessageHandler` instance.</span></span> <span data-ttu-id="d4b9f-417">**При замене обработчика обязательно скопируйте параметры, которые необходимо сохранить из предоставленного обработчика. в противном случае настроенные параметры (такие как файлы cookie и заголовки) не будут применяться к новому обработчику.**</span><span class="sxs-lookup"><span data-stu-id="d4b9f-417">**When replacing the handler make sure to copy the settings you want to keep from the provided handler, otherwise, the configured options (such as Cookies and Headers) won't apply to the new handler.**</span></span> |
| `Proxy` | `null` | <span data-ttu-id="d4b9f-418">HTTP-прокси, используемый при отправке HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-418">An HTTP proxy to use when sending HTTP requests.</span></span> |
| `UseDefaultCredentials` | `false` | <span data-ttu-id="d4b9f-419">Установите это логическое значение, чтобы отправить учетные данные по умолчанию для запросов HTTP и WebSockets.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-419">Set this boolean to send the default credentials for HTTP and WebSockets requests.</span></span> <span data-ttu-id="d4b9f-420">Это позволяет использовать проверку подлинности Windows.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-420">This enables the use of Windows authentication.</span></span> |
| `WebSocketConfiguration` | `null` | <span data-ttu-id="d4b9f-421">Делегат, который можно использовать для настройки дополнительных параметров WebSocket.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-421">A delegate that can be used to configure additional WebSocket options.</span></span> <span data-ttu-id="d4b9f-422">Получает экземпляр [клиентвебсоккетоптионс](/dotnet/api/system.net.websockets.clientwebsocketoptions) , который можно использовать для настройки параметров.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-422">Receives an instance of [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) that can be used to configure the options.</span></span> |

# <a name="javascripttabjavascript"></a>[<span data-ttu-id="d4b9f-423">JavaScript</span><span class="sxs-lookup"><span data-stu-id="d4b9f-423">JavaScript</span></span>](#tab/javascript)

| <span data-ttu-id="d4b9f-424">Параметр JavaScript</span><span class="sxs-lookup"><span data-stu-id="d4b9f-424">JavaScript Option</span></span> | <span data-ttu-id="d4b9f-425">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-425">Default Value</span></span> | <span data-ttu-id="d4b9f-426">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-426">Description</span></span> |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | <span data-ttu-id="d4b9f-427">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-427">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `skipNegotiation` | `false` | <span data-ttu-id="d4b9f-428">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-428">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d4b9f-429">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-429">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d4b9f-430">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-430">This setting can't be enabled when using the Azure SignalR Service.</span></span> |

# <a name="javatabjava"></a>[<span data-ttu-id="d4b9f-431">Java</span><span class="sxs-lookup"><span data-stu-id="d4b9f-431">Java</span></span>](#tab/java)

| <span data-ttu-id="d4b9f-432">Параметр Java</span><span class="sxs-lookup"><span data-stu-id="d4b9f-432">Java Option</span></span> | <span data-ttu-id="d4b9f-433">Default Value</span><span class="sxs-lookup"><span data-stu-id="d4b9f-433">Default Value</span></span> | <span data-ttu-id="d4b9f-434">Описание</span><span class="sxs-lookup"><span data-stu-id="d4b9f-434">Description</span></span> |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | <span data-ttu-id="d4b9f-435">Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-435">A function returning a string that is provided as a Bearer authentication token in HTTP requests.</span></span> |
| `shouldSkipNegotiate` | `false` | <span data-ttu-id="d4b9f-436">Задайте значение `true`, чтобы пропустить шаг согласования.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-436">Set this to `true` to skip the negotiation step.</span></span> <span data-ttu-id="d4b9f-437">**Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-437">**Only supported when the WebSockets transport is the only enabled transport**.</span></span> <span data-ttu-id="d4b9f-438">Этот параметр нельзя включить при использовании службы Azure SignalR.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-438">This setting can't be enabled when using the Azure SignalR Service.</span></span> |
| <span data-ttu-id="d4b9f-439">`withHeader` `withHeaders`</span><span class="sxs-lookup"><span data-stu-id="d4b9f-439">`withHeader` `withHeaders`</span></span> | <span data-ttu-id="d4b9f-440">Empty</span><span class="sxs-lookup"><span data-stu-id="d4b9f-440">Empty</span></span> | <span data-ttu-id="d4b9f-441">Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-441">A Map of additional HTTP headers to send with every HTTP request.</span></span> |

---

<span data-ttu-id="d4b9f-442">В клиенте .NET эти параметры можно изменить с помощью делегата Options, предоставленного для `WithUrl`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-442">In the .NET Client, these options can be modified by the options delegate provided to `WithUrl`:</span></span>

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

<span data-ttu-id="d4b9f-443">В клиенте JavaScript эти параметры можно указать в объекте JavaScript, предоставленном для `withUrl`:</span><span class="sxs-lookup"><span data-stu-id="d4b9f-443">In the JavaScript Client, these options can be provided in a JavaScript object provided to `withUrl`:</span></span>

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

<span data-ttu-id="d4b9f-444">В клиенте Java эти параметры можно настроить с помощью методов в `HttpHubConnectionBuilder`, возвращаемых из `HubConnectionBuilder.create("HUB URL")`.</span><span class="sxs-lookup"><span data-stu-id="d4b9f-444">In the Java client, these options can be configured with the methods on the `HttpHubConnectionBuilder` returned from the `HubConnectionBuilder.create("HUB URL")`</span></span>

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a><span data-ttu-id="d4b9f-445">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d4b9f-445">Additional resources</span></span>

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
