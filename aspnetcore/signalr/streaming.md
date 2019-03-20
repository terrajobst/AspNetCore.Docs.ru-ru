---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как для получения потоков из значений из методов концентратора на сервере и используют потоки, с помощью клиентов .NET и JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: 7c176e3f21ffca7b97d9d3c2e8861032f22587b8
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264305"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="88409-103">Использовать потоковую передачу в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="88409-103">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="88409-104">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="88409-104">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="88409-105">ASP.NET Core SignalR поддерживает потоковой передачи возвращаемые значения методов сервера.</span><span class="sxs-lookup"><span data-stu-id="88409-105">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="88409-106">Это полезно для сценариев, источник фрагментах данных со временем.</span><span class="sxs-lookup"><span data-stu-id="88409-106">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="88409-107">При возвращаемое значение передается клиенту, каждый фрагмент отправляется клиенту, как только она становится доступны, вместо ожидания возвращения всех данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="88409-107">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="88409-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="88409-108">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="88409-109">Настройка концентратора</span><span class="sxs-lookup"><span data-stu-id="88409-109">Set up the hub</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88409-110">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, или `Task<IAsyncEnumerable<T>>`.</span><span class="sxs-lookup"><span data-stu-id="88409-110">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, or `Task<IAsyncEnumerable<T>>`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="88409-111">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="88409-111">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="88409-112">В ASP.NET Core 3.0 или более поздней версии, могут возвращать потоковой передачи методов концентратора `IAsyncEnumerable<T>` в дополнение к `ChannelReader<T>`.</span><span class="sxs-lookup"><span data-stu-id="88409-112">In ASP.NET Core 3.0 or later, streaming hub methods can return `IAsyncEnumerable<T>` in addition to `ChannelReader<T>`.</span></span> <span data-ttu-id="88409-113">Самый простой способ возврата `IAsyncEnumerable<T>` заключается в изменении метода концентратора асинхронный метод итератора, как показано в следующем примере.</span><span class="sxs-lookup"><span data-stu-id="88409-113">The simplest way to return `IAsyncEnumerable<T>` is by making the hub method an async iterator method as the following sample demonstrates.</span></span> <span data-ttu-id="88409-114">Методы итератора async центра может принять `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока.</span><span class="sxs-lookup"><span data-stu-id="88409-114">Hub async iterator methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="88409-115">Асинхронные методы итератора легко избежать распространенных проблем с каналами, например не возвращает `ChannelReader` достаточно рано или выход из метода, не завершая `ChannelWriter`.</span><span class="sxs-lookup"><span data-stu-id="88409-115">Async iterator methods easily avoid problems common with Channels such as not returning the `ChannelReader` early enough or exiting the method without completing the `ChannelWriter`.</span></span>

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

<span data-ttu-id="88409-116">В следующем примере показано с основами потоковой передачи данных на клиент с помощью каналов.</span><span class="sxs-lookup"><span data-stu-id="88409-116">The following sample shows the basics of streaming data to the client using Channels.</span></span> <span data-ttu-id="88409-117">Каждый раз, когда объект записывается `ChannelWriter` этого объекта немедленно отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="88409-117">Whenever an object is written to the `ChannelWriter` that object is immediately sent to the client.</span></span> <span data-ttu-id="88409-118">В конце `ChannelWriter` завершения, чтобы сообщить клиенту поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="88409-118">At the end, the `ChannelWriter` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="88409-119">Запись `ChannelWriter` в фоновом потоке и возврат `ChannelReader` как можно скорее.</span><span class="sxs-lookup"><span data-stu-id="88409-119">Write to the `ChannelWriter` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="88409-120">Другие вызовы концентратора будут заблокированы до `ChannelReader` возвращается.</span><span class="sxs-lookup"><span data-stu-id="88409-120">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="88409-121">Перенос логики в `try ... catch` и завершите `Channel` в catch и за ее пределами catch, чтобы убедиться, что концентратор вызов метода завершается должным образом.</span><span class="sxs-lookup"><span data-stu-id="88409-121">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="88409-122">В ASP.NET Core 2.2 или более поздней версии, могут принимать потоковой передачи методов концентратора `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока.</span><span class="sxs-lookup"><span data-stu-id="88409-122">In ASP.NET Core 2.2 or later, streaming hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="88409-123">Используйте этот маркер для остановки работы сервера и освободить все ресурсы, если клиент отключается до окончания потока.</span><span class="sxs-lookup"><span data-stu-id="88409-123">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="88409-124">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="88409-124">.NET client</span></span>

<span data-ttu-id="88409-125">`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="88409-125">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="88409-126">Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="88409-126">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="88409-127">Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="88409-127">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="88409-128">Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="88409-128">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="88409-129">Чтобы считать данные, распространенный шаблон — запустить цикл для `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны.</span><span class="sxs-lookup"><span data-stu-id="88409-129">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="88409-130">Цикл завершится, когда поток был закрыт сервером или маркер отмены, переданный `StreamAsChannelAsync` отменяется.</span><span class="sxs-lookup"><span data-stu-id="88409-130">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

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

## <a name="javascript-client"></a><span data-ttu-id="88409-131">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="88409-131">JavaScript client</span></span>

<span data-ttu-id="88409-132">Клиенты JavaScript вызывать методы потоковой передачи концентраторов с помощью `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="88409-132">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="88409-133">`stream` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="88409-133">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="88409-134">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="88409-134">The name of the hub method.</span></span> <span data-ttu-id="88409-135">В следующем примере, является имя метода концентратора `Counter`.</span><span class="sxs-lookup"><span data-stu-id="88409-135">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="88409-136">Аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="88409-136">Arguments defined in the hub method.</span></span> <span data-ttu-id="88409-137">В следующем примере аргументы являются: счетчик для числа элементов потока для получения и задержку между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="88409-137">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="88409-138">`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="88409-138">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="88409-139">Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений из `stream` вызова.</span><span class="sxs-lookup"><span data-stu-id="88409-139">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="88409-140">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="88409-140">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="88409-141">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="88409-141">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="88409-142">Вызов этого метода приведет к `CancellationToken` параметр метода концентратора (Если вы указали один) отменяется.</span><span class="sxs-lookup"><span data-stu-id="88409-142">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

## <a name="java-client"></a><span data-ttu-id="88409-143">Клиент Java</span><span class="sxs-lookup"><span data-stu-id="88409-143">Java client</span></span>

<span data-ttu-id="88409-144">Клиент SignalR Java использует `stream` метод для вызова методов потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="88409-144">The SignalR Java client uses the `stream` method to invoke streaming methods.</span></span> <span data-ttu-id="88409-145">Он принимает три или более аргументов:</span><span class="sxs-lookup"><span data-stu-id="88409-145">It accepts three or more arguments:</span></span>

* <span data-ttu-id="88409-146">Ожидаемый тип элементов потока</span><span class="sxs-lookup"><span data-stu-id="88409-146">The expected type of the stream items</span></span>
* <span data-ttu-id="88409-147">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="88409-147">The name of the hub method.</span></span>
* <span data-ttu-id="88409-148">Аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="88409-148">Arguments defined in the hub method.</span></span>

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

<span data-ttu-id="88409-149">`stream` Метод `HubConnection` Возвращает наблюдаемый объект типа элемента потока.</span><span class="sxs-lookup"><span data-stu-id="88409-149">The `stream` method on `HubConnection` returns an Observable of the stream item type.</span></span> <span data-ttu-id="88409-150">Наблюдаемого типа `subscribe` метод является определение вашего `onNext`, `onError` и `onCompleted` обработчиков.</span><span class="sxs-lookup"><span data-stu-id="88409-150">The Observable type's `subscribe` method is where you define your `onNext`,  `onError` and  `onCompleted` handlers.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="88409-151">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="88409-151">Related resources</span></span>

* [<span data-ttu-id="88409-152">Центры</span><span class="sxs-lookup"><span data-stu-id="88409-152">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="88409-153">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="88409-153">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="88409-154">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="88409-154">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="88409-155">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="88409-155">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
