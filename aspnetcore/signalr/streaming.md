---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как выполнять потоковый обмен данными между клиентом и сервером.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: a75156f398e113393ddb891d16eec3f09de80c09
ms.sourcegitcommit: e7e04a45195d4e0527af6f7cf1807defb56dc3c3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/06/2019
ms.locfileid: "66750187"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="c7ee7-103">Использовать потоковую передачу в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c7ee7-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c7ee7-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="c7ee7-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7ee7-105">ASP.NET Core SignalR поддерживает потоковой передачи от клиента к серверу и от сервера клиенту.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="c7ee7-106">Это полезно в сценариях, где фрагментов данных поступают по времени.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="c7ee7-107">При потоковой передаче, каждый фрагмент отправляется на клиенте или сервере как только она становится доступны, вместо ожидания возвращения всех данных станут доступны.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c7ee7-108">ASP.NET Core SignalR поддерживает потоковой передачи возвращаемые значения методов сервера.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="c7ee7-109">Это полезно в сценариях, где фрагментов данных поступают по времени.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="c7ee7-110">При возвращаемое значение передается клиенту, каждый фрагмент отправляется клиенту, как только она становится доступны, вместо ожидания возвращения всех данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="c7ee7-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7ee7-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="c7ee7-112">Настройка концентратора для потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7ee7-113">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="c7ee7-114">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает <xref:System.Threading.Channels.ChannelReader%601> или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="c7ee7-115">Клиентом и сервером потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7ee7-116">Потоковая передача методов концентратора может возвращать `IAsyncEnumerable<T>` в дополнение к `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="c7ee7-117">Самый простой способ возврата `IAsyncEnumerable<T>` заключается в изменении метода концентратора асинхронный метод итератора, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="c7ee7-118">Методы итератора async центра может принять `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="c7ee7-119">Асинхронные методы итератора избежать распространенных проблем с каналами, например не возвращают `ChannelReader` достаточно рано или выход из метода, не завершая <xref:System.Threading.Channels.ChannelWriter`1>.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="c7ee7-120">В следующем примере показано с основами потоковой передачи данных на клиент с помощью каналов.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="c7ee7-121">Каждый раз, когда объект записывается <xref:System.Threading.Channels.ChannelWriter%601>, объект немедленно отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="c7ee7-122">В конце `ChannelWriter` завершения, чтобы сообщить клиенту поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="c7ee7-123">Запись `ChannelWriter<T>` в фоновом потоке и возврат `ChannelReader` как можно скорее.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="c7ee7-124">Другие вызовы концентратора будут заблокированы, пока `ChannelReader` возвращается.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="c7ee7-125">Перенос логики в `try ... catch`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="c7ee7-126">Завершить `Channel` в `catch` так и вне `catch` чтобы убедиться, что концентратор вызов метода завершается должным образом.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="c7ee7-127">Может принимать-клиентом и сервером потоковой передачи методов концентратора `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="c7ee7-128">Используйте этот маркер для остановки работы сервера и освободить все ресурсы, если клиент отключается до окончания потока.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="c7ee7-129">Клиент сервер потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-129">Client-to-server streaming</span></span>

<span data-ttu-id="c7ee7-130">Метод концентратора, автоматически становится клиентом и сервером потоковой передачи метода концентратора, если он принимает один или несколько объектов типа <xref:System.Threading.Channels.ChannelReader%601> или <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="c7ee7-131">В следующем примере показано основные данные потоковой передачи, отправленные клиентом.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="c7ee7-132">Каждый раз, когда клиент записывает <xref:System.Threading.Channels.ChannelWriter%601>, данные записываются в `ChannelReader` на сервере, из которой считывает метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="c7ee7-133"><xref:System.Collections.Generic.IAsyncEnumerable%601> Соответствует версии метода.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

```csharp
public async Task UploadStream(IAsyncEnumerable<Stream> stream) 
{
    await foreach (var item in stream)
    {
        Console.WriteLine(item);
    }
}
```

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="c7ee7-134">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="c7ee7-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="c7ee7-135">Клиентом и сервером потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="c7ee7-136">`StreamAsync` И `StreamAsChannelAsync` методы `HubConnection` используются для вызова методов потоковой передачи-клиентом и сервером.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="c7ee7-137">Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsync` или `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="c7ee7-138">Универсальный параметр на `StreamAsync<T>` и `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="c7ee7-139">Объект типа `IAsyncEnumerable<T>` или `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="c7ee7-140">Объект `StreamAsync` пример, который возвращает `IAsyncEnumerable<int>`:</span><span class="sxs-lookup"><span data-stu-id="c7ee7-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

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

<span data-ttu-id="c7ee7-141">Соответствующий `StreamAsChannelAsync` пример, который возвращает `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="c7ee7-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

<span data-ttu-id="c7ee7-142">`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода клиентом и сервером потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="c7ee7-143">Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="c7ee7-144">Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="c7ee7-145">Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="c7ee7-146">`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода клиентом и сервером потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="c7ee7-147">Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="c7ee7-148">Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="c7ee7-149">Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="c7ee7-150">Клиент сервер потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-150">Client-to-server streaming</span></span>

<span data-ttu-id="c7ee7-151">Существует два способа вызова клиентом и сервером потоковой передачи метода концентратора от клиента .NET.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="c7ee7-152">Вы можете либо передайте в `IAsyncEnumerable<T>` или `ChannelReader` как аргумент `SendAsync`, `InvokeAsync`, или `StreamAsChannelAsync`в зависимости от вызова метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="c7ee7-153">Каждый раз, когда данные записываются в `IAsyncEnumerable` или `ChannelWriter` объекта, метод концентратора на сервере получает новый элемент с данными от клиента.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="c7ee7-154">При использовании `IAsyncEnumerable` объекта потока заканчивается после метода, возвращающего поток элементов завершает работу.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

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

<span data-ttu-id="c7ee7-155">Или если вы используете `ChannelWriter`, выполнения канал с `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="c7ee7-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="c7ee7-156">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c7ee7-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="c7ee7-157">Клиентом и сервером потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-157">Server-to-client streaming</span></span>

<span data-ttu-id="c7ee7-158">Клиенты JavaScript вызывать-клиентом и сервером потоковой передачи методов Hub с помощью `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="c7ee7-159">`stream` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="c7ee7-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="c7ee7-160">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-160">The name of the hub method.</span></span> <span data-ttu-id="c7ee7-161">В следующем примере, является имя метода концентратора `Counter`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="c7ee7-162">Аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="c7ee7-163">В следующем примере аргументы являются подсчет числа элементов потока для получения и задержку между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="c7ee7-164">`connection.stream` Возвращает `IStreamResult`, который содержит `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="c7ee7-165">Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений из `stream` вызова.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="c7ee7-166">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="c7ee7-167">Вызов этого метода приводит к отмене `CancellationToken` параметр метода концентратора, если вы указали один.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="c7ee7-168">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="c7ee7-169">Клиент сервер потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-169">Client-to-server streaming</span></span>

<span data-ttu-id="c7ee7-170">Клиенты JavaScript вызывать клиентом и сервером потоковой передачи методов концентраторов, передав `Subject` как аргумент `send`, `invoke`, или `stream`в зависимости от вызова метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="c7ee7-171">`Subject` — Это класс, который выглядит как `Subject`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="c7ee7-172">Например в RxJS, можно использовать [субъекта](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) класс из этой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="c7ee7-173">Вызов `subject.next(item)` с элемент записывает элемент в поток, а метод концентратора получает элемента на сервере.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="c7ee7-174">Чтобы завершить поток, вызовите `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="c7ee7-175">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="c7ee7-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="c7ee7-176">Клиентом и сервером потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="c7ee7-176">Server-to-client streaming</span></span>

<span data-ttu-id="c7ee7-177">Клиент SignalR Java использует `stream` метод для вызова методов потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="c7ee7-178">`stream` принимает три или более аргументов:</span><span class="sxs-lookup"><span data-stu-id="c7ee7-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="c7ee7-179">Ожидаемый тип элементов потока.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="c7ee7-180">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-180">The name of the hub method.</span></span>
* <span data-ttu-id="c7ee7-181">Аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="c7ee7-182">`stream` Метод `HubConnection` Возвращает наблюдаемый объект типа элемента потока.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="c7ee7-183">Наблюдаемого типа `subscribe` метод именно `onNext`, `onError` и `onCompleted` определены обработчики.</span><span class="sxs-lookup"><span data-stu-id="c7ee7-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="c7ee7-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c7ee7-184">Additional resources</span></span>

* [<span data-ttu-id="c7ee7-185">Центры</span><span class="sxs-lookup"><span data-stu-id="c7ee7-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c7ee7-186">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="c7ee7-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c7ee7-187">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c7ee7-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c7ee7-188">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="c7ee7-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
