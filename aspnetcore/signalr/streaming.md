---
title: Использование потоковой передачи в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как выполнять потоковую передачу данных между клиентом и сервером.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 06/05/2019
uid: signalr/streaming
ms.openlocfilehash: d520c8eec3e777acb9604bdcb5969268deabf8da
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71186933"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="2f047-103">Использование потоковой передачи в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="2f047-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="2f047-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="2f047-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f047-105">ASP.NET Core SignalR поддерживает потоковую передачу от клиента серверу и от сервера к клиенту.</span><span class="sxs-lookup"><span data-stu-id="2f047-105">ASP.NET Core SignalR supports streaming from client to server and from server to client.</span></span> <span data-ttu-id="2f047-106">Это полезно для сценариев, в которых фрагменты данных поступают с течением времени.</span><span class="sxs-lookup"><span data-stu-id="2f047-106">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="2f047-107">При потоковой передаче каждый фрагмент отправляется клиенту или серверу сразу после того, как он становится доступным, а не ожидает, пока все данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="2f047-107">When streaming, each fragment is sent to the client or server as soon as it becomes available, rather than waiting for all of the data to become available.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f047-108">ASP.NET Core SignalR поддерживает потоковые возвращаемые значения методов сервера.</span><span class="sxs-lookup"><span data-stu-id="2f047-108">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="2f047-109">Это полезно для сценариев, в которых фрагменты данных поступают с течением времени.</span><span class="sxs-lookup"><span data-stu-id="2f047-109">This is useful for scenarios where fragments of data arrive over time.</span></span> <span data-ttu-id="2f047-110">Когда возвращаемое значение передается клиенту в потоке, каждый фрагмент отправляется клиенту, как только он становится доступным, а не ожидает, пока все данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="2f047-110">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

::: moniker-end

<span data-ttu-id="2f047-111">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="2f047-111">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-a-hub-for-streaming"></a><span data-ttu-id="2f047-112">Настройка концентратора для потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="2f047-112">Set up a hub for streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f047-113">Метод концентратора автоматически превращается в метод концентратора потоковой <xref:System.Collections.Generic.IAsyncEnumerable`1>передачи <xref:System.Threading.Channels.ChannelReader%601> `Task<IAsyncEnumerable<T>>`, когда он `Task<ChannelReader<T>>`возвращает,, или.</span><span class="sxs-lookup"><span data-stu-id="2f047-113">A hub method automatically becomes a streaming hub method when it returns <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`, or `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f047-114">Метод концентратора автоматически превращается в метод концентратора потоковой передачи <xref:System.Threading.Channels.ChannelReader%601> , когда `Task<ChannelReader<T>>`он возвращает или.</span><span class="sxs-lookup"><span data-stu-id="2f047-114">A hub method automatically becomes a streaming hub method when it returns a <xref:System.Threading.Channels.ChannelReader%601> or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

### <a name="server-to-client-streaming"></a><span data-ttu-id="2f047-115">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="2f047-115">Server-to-client streaming</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f047-116">Методы концентратора потоковой `IAsyncEnumerable<T>` передачи могут возвращать в дополнение к. `ChannelReader<T>`</span><span class="sxs-lookup"><span data-stu-id="2f047-116">Streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="2f047-117">Самый простой способ вернуть `IAsyncEnumerable<T>` — сделать метод концентратора асинхронным методом итератора, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="2f047-117">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="2f047-118">Методы асинхронного итератора концентратора `CancellationToken` могут принимать параметр, который активируется, когда клиент отменяет подписывание из потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-118">Hub async iterator methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="2f047-119">Методы асинхронных итераторов позволяют избежать проблем, распространенных с помощью каналов, `ChannelReader` таких как недостаточное раннее <xref:System.Threading.Channels.ChannelWriter`1>выполнение или выход из метода без завершения.</span><span class="sxs-lookup"><span data-stu-id="2f047-119">Async iterator methods avoid problems common with Channels, such as not returning the `ChannelReader` early enough or exiting the method without completing the <xref:System.Threading.Channels.ChannelWriter`1>.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="2f047-120">В следующем примере показаны основные сведения о потоковой передаче данных клиенту с помощью каналов.</span><span class="sxs-lookup"><span data-stu-id="2f047-120">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="2f047-121">При записи <xref:System.Threading.Channels.ChannelWriter%601>объекта в объект немедленно отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="2f047-121">Whenever an object is written to the <xref:System.Threading.Channels.ChannelWriter%601>, the object is immediately sent to the client.</span></span> <span data-ttu-id="2f047-122">В конце завершается, `ChannelWriter` чтобы сообщить клиенту, что поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="2f047-122">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="2f047-123">Запись в фоновом потоке и `ChannelReader` возврат как можно быстрее. `ChannelWriter<T>`</span><span class="sxs-lookup"><span data-stu-id="2f047-123">Write to the `ChannelWriter<T>` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="2f047-124">Другие вызовы концентратора блокируются до тех пор `ChannelReader` , пока не будет возвращен.</span><span class="sxs-lookup"><span data-stu-id="2f047-124">Other hub invocations are blocked until a `ChannelReader` is returned.</span></span>
>
> <span data-ttu-id="2f047-125">Переносить логику `try ... catch`в.</span><span class="sxs-lookup"><span data-stu-id="2f047-125">Wrap logic in a `try ... catch`.</span></span> <span data-ttu-id="2f047-126">Завершите работу `catch` `catch` в и вне, чтобы убедиться, что вызов метода концентратора завершен правильно. `Channel`</span><span class="sxs-lookup"><span data-stu-id="2f047-126">Complete the `Channel` in the `catch` and outside the `catch` to make sure the hub method invocation is completed properly.</span></span>

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

<span data-ttu-id="2f047-127">Методы концентратора потоковой передачи "сервер-клиент" `CancellationToken` могут принимать параметр, который активируется, когда клиент отменяет подписывание из потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-127">Server-to-client streaming hub methods can accept a `CancellationToken` parameter that's triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="2f047-128">Используйте этот маркер, чтобы прекратить работу сервера и освободить все ресурсы, если клиент отключится до конца потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-128">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="2f047-129">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="2f047-129">Client-to-server streaming</span></span>

<span data-ttu-id="2f047-130">Метод концентратора автоматически превращается в метод концентратора потоковой передачи клиента в сервер, если он принимает один или несколько <xref:System.Threading.Channels.ChannelReader%601> объектов <xref:System.Collections.Generic.IAsyncEnumerable%601>типа или.</span><span class="sxs-lookup"><span data-stu-id="2f047-130">A hub method automatically becomes a client-to-server streaming hub method when it accepts one or more objects of type <xref:System.Threading.Channels.ChannelReader%601> or <xref:System.Collections.Generic.IAsyncEnumerable%601>.</span></span> <span data-ttu-id="2f047-131">В следующем примере показаны основные сведения о чтении потоковых данных, отправленных клиентом.</span><span class="sxs-lookup"><span data-stu-id="2f047-131">The following sample shows the basics of reading streaming data sent from the client.</span></span> <span data-ttu-id="2f047-132">Каждый раз <xref:System.Threading.Channels.ChannelWriter%601>, когда клиент выполняет запись в, данные записываются `ChannelReader` на сервер, с которого считывается метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f047-132">Whenever the client writes to the <xref:System.Threading.Channels.ChannelWriter%601>, the data is written into the `ChannelReader` on the server from which the hub method is reading.</span></span>

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

<span data-ttu-id="2f047-133">Ниже <xref:System.Collections.Generic.IAsyncEnumerable%601> приведена версия метода.</span><span class="sxs-lookup"><span data-stu-id="2f047-133">An <xref:System.Collections.Generic.IAsyncEnumerable%601> version of the method follows.</span></span>

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

## <a name="net-client"></a><span data-ttu-id="2f047-134">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="2f047-134">.NET client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="2f047-135">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="2f047-135">Server-to-client streaming</span></span>


::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f047-136">Методы `StreamAsync` и`HubConnection` используются для вызова методов потоковой передачи "сервер-клиент". `StreamAsChannelAsync`</span><span class="sxs-lookup"><span data-stu-id="2f047-136">The `StreamAsync` and `StreamAsChannelAsync` methods on `HubConnection` are used to invoke server-to-client streaming methods.</span></span> <span data-ttu-id="2f047-137">Передайте имя метода концентратора и аргументы, определенные в методе `StreamAsync` концентратора, в или `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="2f047-137">Pass the hub method name and arguments defined in the hub method to `StreamAsync` or `StreamAsChannelAsync`.</span></span> <span data-ttu-id="2f047-138">Универсальный параметр в `StreamAsync<T>` и `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="2f047-138">The generic parameter on `StreamAsync<T>` and `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="2f047-139">Объект типа `IAsyncEnumerable<T>` или `ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.</span><span class="sxs-lookup"><span data-stu-id="2f047-139">An object of type `IAsyncEnumerable<T>` or `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

<span data-ttu-id="2f047-140">Пример, возвращающий `IAsyncEnumerable<int>`: `StreamAsync`</span><span class="sxs-lookup"><span data-stu-id="2f047-140">A `StreamAsync` example that returns `IAsyncEnumerable<int>`:</span></span>

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

<span data-ttu-id="2f047-141">Соответствующий `StreamAsChannelAsync` пример, возвращающий `ChannelReader<int>`:</span><span class="sxs-lookup"><span data-stu-id="2f047-141">A corresponding `StreamAsChannelAsync` example that returns `ChannelReader<int>`:</span></span>

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

<span data-ttu-id="2f047-142">`StreamAsChannelAsync` Метод в`HubConnection` используется для вызова метода потоковой передачи "сервер-клиент".</span><span class="sxs-lookup"><span data-stu-id="2f047-142">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="2f047-143">Передайте имя метода концентратора и аргументы, определенные в методе `StreamAsChannelAsync`концентратора, в.</span><span class="sxs-lookup"><span data-stu-id="2f047-143">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="2f047-144">Универсальный параметр для `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="2f047-144">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="2f047-145">`ChannelReader<T>` Возвращается из вызова потока и представляет поток на клиенте.</span><span class="sxs-lookup"><span data-stu-id="2f047-145">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

<span data-ttu-id="2f047-146">`StreamAsChannelAsync` Метод в`HubConnection` используется для вызова метода потоковой передачи "сервер-клиент".</span><span class="sxs-lookup"><span data-stu-id="2f047-146">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a server-to-client streaming method.</span></span> <span data-ttu-id="2f047-147">Передайте имя метода концентратора и аргументы, определенные в методе `StreamAsChannelAsync`концентратора, в.</span><span class="sxs-lookup"><span data-stu-id="2f047-147">Pass the hub method name and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="2f047-148">Универсальный параметр для `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="2f047-148">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="2f047-149">`ChannelReader<T>` Возвращается из вызова потока и представляет поток на клиенте.</span><span class="sxs-lookup"><span data-stu-id="2f047-149">A `ChannelReader<T>` is returned from the stream invocation and represents the stream on the client.</span></span>

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

### <a name="client-to-server-streaming"></a><span data-ttu-id="2f047-150">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="2f047-150">Client-to-server streaming</span></span>

<span data-ttu-id="2f047-151">Существует два способа вызвать метод концентратора потоковой передачи данных "клиент-сервер" из клиента .NET.</span><span class="sxs-lookup"><span data-stu-id="2f047-151">There are two ways to invoke a client-to-server streaming hub method from the .NET client.</span></span> <span data-ttu-id="2f047-152">`IAsyncEnumerable<T>` Можно передать `SendAsync` `InvokeAsync` `StreamAsChannelAsync`или в качествеаргументав,или,взависимостиотвызываемогометодаконцентратора.`ChannelReader`</span><span class="sxs-lookup"><span data-stu-id="2f047-152">You can either pass in an `IAsyncEnumerable<T>` or a `ChannelReader` as an argument to `SendAsync`, `InvokeAsync`, or `StreamAsChannelAsync`, depending on the hub method invoked.</span></span>

<span data-ttu-id="2f047-153">Каждый раз, когда данные записываются `IAsyncEnumerable` в объект или `ChannelWriter` , метод концентратора на сервере получает новый элемент с данными от клиента.</span><span class="sxs-lookup"><span data-stu-id="2f047-153">Whenever data is written to the `IAsyncEnumerable` or `ChannelWriter` object, the hub method on the server receives a new item with the data from the client.</span></span>

<span data-ttu-id="2f047-154">При использовании `IAsyncEnumerable` объекта поток завершается после выхода из метода, возвращающего элементы потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-154">If using an `IAsyncEnumerable` object, the stream ends after the method returning stream items exits.</span></span>

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

<span data-ttu-id="2f047-155">Если вы используете `ChannelWriter`, вы можете выполнить канал с помощью `channel.Writer.Complete()`:</span><span class="sxs-lookup"><span data-stu-id="2f047-155">Or if you're using a `ChannelWriter`, you complete the channel with `channel.Writer.Complete()`:</span></span>

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a><span data-ttu-id="2f047-156">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f047-156">JavaScript client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="2f047-157">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="2f047-157">Server-to-client streaming</span></span>

<span data-ttu-id="2f047-158">Клиенты JavaScript вызывают потоковые методы "сервер-клиент" на концентраторах с помощью `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="2f047-158">JavaScript clients call server-to-client streaming methods on hubs with `connection.stream`.</span></span> <span data-ttu-id="2f047-159">`stream` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="2f047-159">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="2f047-160">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f047-160">The name of the hub method.</span></span> <span data-ttu-id="2f047-161">В следующем примере имя метода концентратора — `Counter`.</span><span class="sxs-lookup"><span data-stu-id="2f047-161">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="2f047-162">Аргументы, определенные в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f047-162">Arguments defined in the hub method.</span></span> <span data-ttu-id="2f047-163">В следующем примере аргументы представляют число получаемых элементов потока и задержку между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-163">In the following example, the arguments are a count for the number of stream items to receive and the delay between stream items.</span></span>

<span data-ttu-id="2f047-164">`connection.stream`Возвращает объект `IStreamResult`, который `subscribe` содержит метод.</span><span class="sxs-lookup"><span data-stu-id="2f047-164">`connection.stream` returns an `IStreamResult`, which contains a `subscribe` method.</span></span> <span data-ttu-id="2f047-165">`error` `next` `stream` Передайте `subscribe`ви задайте обратные вызовы, `complete` и, чтобы получать уведомления от вызова. `IStreamSubscriber`</span><span class="sxs-lookup"><span data-stu-id="2f047-165">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to receive notifications from the `stream` invocation.</span></span>

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="2f047-166">Чтобы завершить поток от клиента, вызовите `dispose` метод `ISubscription` для объекта `subscribe` , который возвращается из метода.</span><span class="sxs-lookup"><span data-stu-id="2f047-166">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span> <span data-ttu-id="2f047-167">Вызов этого метода приводит к отмене `CancellationToken` параметра метода концентратора, если он указан.</span><span class="sxs-lookup"><span data-stu-id="2f047-167">Calling this method causes cancellation of the `CancellationToken` parameter of the Hub method, if you provided one.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="2f047-168">Чтобы завершить поток от клиента, вызовите `dispose` метод `ISubscription` для объекта `subscribe` , который возвращается из метода.</span><span class="sxs-lookup"><span data-stu-id="2f047-168">To end the stream from the client, call the `dispose` method on the `ISubscription` that's returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a><span data-ttu-id="2f047-169">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="2f047-169">Client-to-server streaming</span></span>

<span data-ttu-id="2f047-170">Клиенты JavaScript вызывают методы потоковой передачи клиента на сервер на концентраторах, передавая `Subject` в качестве `send`аргумента в `invoke`, или `stream`, в зависимости от вызываемого метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f047-170">JavaScript clients call client-to-server streaming methods on hubs by passing in a `Subject` as an argument to `send`, `invoke`, or `stream`, depending on the hub method invoked.</span></span> <span data-ttu-id="2f047-171">— Это класс, который выглядит `Subject`как. `Subject`</span><span class="sxs-lookup"><span data-stu-id="2f047-171">The `Subject` is a class that looks like a `Subject`.</span></span> <span data-ttu-id="2f047-172">Например, в Рксжс можно использовать класс [subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) из этой библиотеки.</span><span class="sxs-lookup"><span data-stu-id="2f047-172">For example in RxJS, you can use the [Subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) class from that library.</span></span>

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

<span data-ttu-id="2f047-173">Вызов `subject.next(item)` с помощью элемента записывает элемент в поток, а метод концентратора получает элемент на сервере.</span><span class="sxs-lookup"><span data-stu-id="2f047-173">Calling `subject.next(item)` with an item writes the item to the stream, and the hub method receives the item on the server.</span></span>

<span data-ttu-id="2f047-174">Чтобы завершить поток, вызовите `subject.complete()`.</span><span class="sxs-lookup"><span data-stu-id="2f047-174">To end the stream, call `subject.complete()`.</span></span>

## <a name="java-client"></a><span data-ttu-id="2f047-175">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="2f047-175">Java client</span></span>

### <a name="server-to-client-streaming"></a><span data-ttu-id="2f047-176">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="2f047-176">Server-to-client streaming</span></span>

<span data-ttu-id="2f047-177">Клиент, использующий SignalR Java `stream` , использует метод для вызова методов потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="2f047-177">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="2f047-178">`stream`принимает три или более аргументов:</span><span class="sxs-lookup"><span data-stu-id="2f047-178">`stream` accepts three or more arguments:</span></span>

* <span data-ttu-id="2f047-179">Ожидаемый тип элементов потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-179">The expected type of the stream items.</span></span>
* <span data-ttu-id="2f047-180">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f047-180">The name of the hub method.</span></span>
* <span data-ttu-id="2f047-181">Аргументы, определенные в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="2f047-181">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="2f047-182">`stream` Метод для`HubConnection` Возвращает наблюдаемый тип элемента потока.</span><span class="sxs-lookup"><span data-stu-id="2f047-182">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="2f047-183">`subscribe` Метод наблюдаемого типа определяет, `onError` где `onNext`и `onCompleted` какие обработчики определены.</span><span class="sxs-lookup"><span data-stu-id="2f047-183">The Observable type's `subscribe` method is where `onNext`, `onError` and `onCompleted` handlers are defined.</span></span>

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2f047-184">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="2f047-184">Additional resources</span></span>

* [<span data-ttu-id="2f047-185">Центры</span><span class="sxs-lookup"><span data-stu-id="2f047-185">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="2f047-186">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="2f047-186">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="2f047-187">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="2f047-187">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="2f047-188">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="2f047-188">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
