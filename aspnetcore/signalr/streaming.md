---
title: Использование потоковой передачи в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как выполнять потоковую передачу данных между клиентом и сервером.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/streaming
ms.openlocfilehash: 21dd8180fe168f81ed68b01f02b81a6264d6e5a6
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78654976"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a><span data-ttu-id="3b671-103">Использование потоковой передачи в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="3b671-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="3b671-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="3b671-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3b671-105">ASP.NET Core SignalR поддерживает потоковую передачу от клиента к серверу и от сервера к клиенту.</span><span class="sxs-lookup"><span data-stu-id="3b671-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="3b671-106">Это полезно для сценариев, в которых фрагменты данных поступают с течением времени.</span><span class="sxs-lookup"><span data-stu-id="3b671-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="3b671-107">При потоковой передаче каждый фрагмент отправляется клиенту или серверу сразу после того, как он становится доступным, а не ожидает, пока все данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="3b671-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3b671-108">ASP.NET Core SignalR поддерживает потоковые возвращаемые значения методов сервера.</span><span class="sxs-lookup"><span data-stu-id="3b671-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="3b671-109">Это полезно для сценариев, в которых фрагменты данных поступают с течением времени.</span><span class="sxs-lookup"><span data-stu-id="3b671-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="3b671-110">Когда возвращаемое значение передается клиенту в потоке, каждый фрагмент отправляется клиенту, как только он становится доступным, а не ожидает, пока все данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="3b671-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="3b671-111">[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="3b671-111">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="3b671-112">Настройка концентратора для потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="3b671-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3b671-113">Метод концентратора автоматически превращается в метод концентратора потоковой передачи, когда он возвращает <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="3b671-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3b671-114">При возврате <xref:System.Threading.Channels.ChannelReader%601> или `Task<ChannelReader<T>>`метод концентратора автоматически превращается в метод концентратора потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="3b671-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="3b671-115">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="3b671-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3b671-116">Методы концентратора потоковой передачи могут возвращать `IAsyncEnumerable<T>` в дополнение к `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="3b671-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="3b671-117">Самый простой способ вернуть `IAsyncEnumerable<T>` — сделать метод концентратора асинхронным методом итератора, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="3b671-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="3b671-118">Методы асинхронного итератора концентратора могут принимать параметр `CancellationToken`, который активируется, когда клиент отменяет подписывание из потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="3b671-119">Методы асинхронных итераторов позволяют избежать проблем, распространенных с помощью каналов, например, не возвращая `ChannelReader` ранних или завершающих работу метода без завершения <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="3b671-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="3b671-120">В следующем примере показаны основные сведения о потоковой передаче данных клиенту с помощью каналов.</span><span class="sxs-lookup"><span data-stu-id="3b671-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="3b671-121">Каждый раз, когда объект записывается в <xref:System.Threading.Channels.ChannelWriter%601>, объект немедленно отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="3b671-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="3b671-122">В конце `ChannelWriter` завершается, чтобы сообщить клиенту, что поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="3b671-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="3b671-123">Запись в `ChannelWriter<T>` в фоновом потоке и возврат `ChannelReader` как можно скорее.</span><span class="sxs-lookup"><span data-stu-id="3b671-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="3b671-124">Другие вызовы концентратора блокируются до тех пор, пока не будет возвращено `ChannelReader`.</span><span class="sxs-lookup"><span data-stu-id="3b671-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="3b671-125">Переносить логику в `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="3b671-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="3b671-126">Завершите `Channel` в `catch` и вне `catch`, чтобы убедиться, что вызов метода концентратора завершен правильно.</span><span class="sxs-lookup"><span data-stu-id="3b671-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

::: moniker range=">= aspnetcore-3.0"

[!code-csharp[Streaming hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/samples/2.2/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/samples/2.1/Hubs/StreamHub.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3b671-127">Методы концентратора потоковой передачи "сервер-клиент" могут принимать параметр `CancellationToken`, который активируется, когда клиент отменяет подписывание из потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="3b671-128">Используйте этот маркер, чтобы прекратить работу сервера и освободить все ресурсы, если клиент отключится до конца потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="3b671-129">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="3b671-129">Client-to-server streaming</span></span>

<span data-ttu-id="3b671-130">Метод концентратора автоматически превращается в метод концентратора потоковой передачи клиента в сервер, если он принимает один или несколько объектов типа <xref:System.Threading.Channels.ChannelReader%601> или <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="3b671-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="3b671-131">В следующем примере показаны основные сведения о чтении потоковых данных, отправленных клиентом.</span><span class="sxs-lookup"><span data-stu-id="3b671-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="3b671-132">Каждый раз, когда клиент выполняет запись в <xref:System.Threading.Channels.ChannelWriter%601>, данные записываются в `ChannelReader` на сервере, с которого считывается метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="3b671-133">Ниже приведена <xref:System.Collections.Generic.IAsyncEnumerable%601>ная версия метода.</span><span class="sxs-lookup"><span data-stu-id="3b671-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<string> stream)
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="3b671-134">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="3b671-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="3b671-135">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="3b671-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3b671-136">Методы `StreamAsync` и `StreamAsChannelAsync` в `HubConnection` используются для вызова методов потоковой передачи "сервер-клиент".</span><span class="sxs-lookup"><span data-stu-id="3b671-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="3b671-137">Передайте имя и аргументы метода концентратора, определенные в методе Hub, в `StreamAsync` или `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="3b671-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="3b671-138">Универсальный параметр в `StreamAsync<T>` и `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="3b671-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="3b671-139">Объект типа `IAsyncEnumerable<T>` или `ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.</span><span class="sxs-lookup"><span data-stu-id="3b671-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="3b671-140">`StreamAsync` пример, который возвращает `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="3b671-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var stream = await hubConnection.StreamAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

await foreach (var count in stream)
{
    Console.WriteLine($"{count}");
}

Console.WriteLine("Streaming completed");
```

<span data-ttu-id="3b671-141">Соответствующий пример `StreamAsChannelAsync`, который возвращает `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="3b671-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="3b671-142">Метод `StreamAsChannelAsync` в `HubConnection` используется для вызова метода потоковой передачи "сервер-клиент".</span><span class="sxs-lookup"><span data-stu-id="3b671-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="3b671-143">Передайте имя метода концентратора и аргументы, определенные в методе концентратора, в `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="3b671-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="3b671-144">Универсальный параметр в `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="3b671-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="3b671-145">`ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.</span><span class="sxs-lookup"><span data-stu-id="3b671-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to
// the server, which will trigger the corresponding token in the hub method.
var cancellationTokenSource = new CancellationTokenSource();
var channel = await hubConnection.StreamAsChannelAsync<int>(
    "Counter", 10, 500, cancellationTokenSource.Token);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="3b671-146">Метод `StreamAsChannelAsync` в `HubConnection` используется для вызова метода потоковой передачи "сервер-клиент".</span><span class="sxs-lookup"><span data-stu-id="3b671-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="3b671-147">Передайте имя метода концентратора и аргументы, определенные в методе концентратора, в `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="3b671-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="3b671-148">Универсальный параметр в `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="3b671-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="3b671-149">`ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.</span><span class="sxs-lookup"><span data-stu-id="3b671-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

```csharp
var channel = await hubConnection
    .StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

// Wait asynchronously for data to become available
while (await channel.WaitToReadAsync())
{
    // Read all currently available data synchronously, before waiting for more data
    while (channel.TryRead(out var count))
    {
        Console.WriteLine($"{count}");
    }
}

Console.WriteLine("Streaming completed");
```

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="3b671-150">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="3b671-150">Client-to-server streaming</span></span>

<span data-ttu-id="3b671-151">Существует два способа вызвать метод концентратора потоковой передачи данных "клиент-сервер" из клиента .NET.</span><span class="sxs-lookup"><span data-stu-id="3b671-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="3b671-152">Можно передать `IAsyncEnumerable<T>` или `ChannelReader` в качестве аргумента в `SendAsync`, `InvokeAsync`или `StreamAsChannelAsync`в зависимости от вызываемого метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="3b671-153">Каждый раз, когда данные записываются в `IAsyncEnumerable` или `ChannelWriter` объект, метод концентратора на сервере получает новый элемент с данными клиента.</span><span class="sxs-lookup"><span data-stu-id="3b671-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="3b671-154">Если используется объект `IAsyncEnumerable`, поток завершается после выхода из метода, возвращающего элементы потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
async IAsyncEnumerable<string> clientStreamData()
{
    for (var i = 0; i < 5; i++)
    {
        var data = await FetchSomeData();
        yield return data;
    }
    //After the for loop has completed and the local function exits the stream completion will be sent.
}

await connection.SendAsync("UploadStream", clientStreamData());
```

<span data-ttu-id="3b671-155">Если вы используете `ChannelWriter`, вы можете выполнить канал с `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="3b671-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="3b671-156">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b671-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="3b671-157">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="3b671-157">Server-to-client streaming</span></span>

<span data-ttu-id="3b671-158">Клиенты JavaScript вызывают потоковые методы "сервер-клиент" в концентраторах с `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="3b671-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="3b671-159">Метод `stream` принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="3b671-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="3b671-160">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-160">The name of the hub method.</span></span> <span data-ttu-id="3b671-161">В следующем примере имя метода концентратора — `Counter`.</span><span class="sxs-lookup"><span data-stu-id="3b671-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="3b671-162">Аргументы, определенные в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="3b671-163">В следующем примере аргументы представляют число получаемых элементов потока и задержку между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="3b671-164">`connection.stream` возвращает `IStreamResult`, который содержит метод `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="3b671-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="3b671-165">Передайте `IStreamSubscriber` `subscribe` и задайте обратные вызовы `next`, `error`и `complete`, чтобы получать уведомления от вызова `stream`.</span><span class="sxs-lookup"><span data-stu-id="3b671-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="3b671-166">Чтобы завершить поток от клиента, вызовите метод `dispose` для `ISubscription`, возвращаемого методом `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="3b671-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="3b671-167">Вызов этого метода приводит к отмене параметра `CancellationToken` метода концентратора, если он указан.</span><span class="sxs-lookup"><span data-stu-id="3b671-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="3b671-168">Чтобы завершить поток от клиента, вызовите метод `dispose` для `ISubscription`, возвращаемого методом `subscribe`.</span><span class="sxs-lookup"><span data-stu-id="3b671-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="3b671-169">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="3b671-169">Client-to-server streaming</span></span>

<span data-ttu-id="3b671-170">Клиенты JavaScript вызывают методы потоковой передачи клиента на сервер на концентраторах, передавая `Subject` в качестве аргумента для `send`, `invoke`или `stream`в зависимости от вызываемого метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="3b671-171">`Subject` — это класс, который выглядит как `Subject`.</span><span class="sxs-lookup"><span data-stu-id="3b671-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="3b671-172">Например, в Рксжс можно использовать класс [subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) из этой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="3b671-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="3b671-173">Вызов `subject.next(item)` с элементом записывает элемент в поток, а метод концентратора получает элемент на сервере.</span><span class="sxs-lookup"><span data-stu-id="3b671-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="3b671-174">Чтобы завершить поток, вызовите `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="3b671-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="3b671-175">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="3b671-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="3b671-176">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="3b671-176">Server-to-client streaming</span></span>

<span data-ttu-id="3b671-177">Клиент SignalR Java использует метод `stream` для вызова методов потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="3b671-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="3b671-178">`stream` принимает три или более аргументов:</span><span class="sxs-lookup"><span data-stu-id="3b671-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="3b671-179">Ожидаемый тип элементов потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="3b671-180">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-180">The name of the hub method.</span></span>
* <span data-ttu-id="3b671-181">Аргументы, определенные в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="3b671-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="3b671-182">Метод `stream` для `HubConnection` Возвращает наблюдаемый тип элемента потока.</span><span class="sxs-lookup"><span data-stu-id="3b671-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="3b671-183">`subscribe` метод наблюдаемого типа, в котором определены обработчики `onNext`, `onError` и `onCompleted`.</span><span class="sxs-lookup"><span data-stu-id="3b671-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="3b671-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3b671-184">Additional resources</span></span>

* [<span data-ttu-id="3b671-185">Центры</span><span class="sxs-lookup"><span data-stu-id="3b671-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="3b671-186">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="3b671-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="3b671-187">Клиент на JavaScript</span><span class="sxs-lookup"><span data-stu-id="3b671-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="3b671-188">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="3b671-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
