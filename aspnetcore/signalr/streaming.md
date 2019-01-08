---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: e0d201a7ffebbbe387a874c6d788994faa2be7a5
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098809"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="4db61-102">Использовать потоковую передачу в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="4db61-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="4db61-103">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="4db61-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="4db61-104">ASP.NET Core SignalR поддерживает потоковой передачи возвращаемые значения методов сервера.</span><span class="sxs-lookup"><span data-stu-id="4db61-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="4db61-105">Это полезно для сценариев, источник фрагментах данных со временем.</span><span class="sxs-lookup"><span data-stu-id="4db61-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="4db61-106">При возвращаемое значение передается клиенту, каждый фрагмент отправляется клиенту, как только она становится доступны, вместо ожидания возвращения всех данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="4db61-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="4db61-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="4db61-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="4db61-108">Настройка концентратора</span><span class="sxs-lookup"><span data-stu-id="4db61-108">Set up the hub</span></span>

<span data-ttu-id="4db61-109">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="4db61-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="4db61-110">Ниже приведен пример, показывающий, с основами потоковой передачи данных клиенту.</span><span class="sxs-lookup"><span data-stu-id="4db61-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="4db61-111">Каждый раз, когда объект записывается `ChannelReader` этого объекта немедленно отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="4db61-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="4db61-112">В конце `ChannelReader` завершения, чтобы сообщить клиенту поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="4db61-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> * <span data-ttu-id="4db61-113">Запись `ChannelReader` в фоновом потоке и возврат `ChannelReader` как можно скорее.</span><span class="sxs-lookup"><span data-stu-id="4db61-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="4db61-114">Другие вызовы концентратора будут заблокированы до `ChannelReader` возвращается.</span><span class="sxs-lookup"><span data-stu-id="4db61-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>
> * <span data-ttu-id="4db61-115">Перенос логики в `try ... catch` и завершите `Channel` в catch и за ее пределами catch, чтобы убедиться, что концентратор вызов метода завершается должным образом.</span><span class="sxs-lookup"><span data-stu-id="4db61-115">Wrap your logic in a `try ... catch` and complete the `Channel` in the catch and outside the catch to make sure the hub method invocation is completed properly.</span></span>

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

<span data-ttu-id="4db61-116">В ASP.NET Core 2.2 или более поздней версии, могут принимать потоковой передачи методов концентратора `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока.</span><span class="sxs-lookup"><span data-stu-id="4db61-116">In ASP.NET Core 2.2 or later, streaming Hub methods can accept a `CancellationToken` parameter that will be triggered when the client unsubscribes from the stream.</span></span> <span data-ttu-id="4db61-117">Используйте этот маркер для остановки работы сервера и освободить все ресурсы, если клиент отключается до окончания потока.</span><span class="sxs-lookup"><span data-stu-id="4db61-117">Use this token to stop the server operation and release any resources if the client disconnects before the end of the stream.</span></span>

::: moniker-end

## <a name="net-client"></a><span data-ttu-id="4db61-118">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="4db61-118">.NET client</span></span>

<span data-ttu-id="4db61-119">`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="4db61-119">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="4db61-120">Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="4db61-120">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="4db61-121">Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="4db61-121">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="4db61-122">Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="4db61-122">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="4db61-123">Чтобы считать данные, распространенный шаблон — запустить цикл для `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны.</span><span class="sxs-lookup"><span data-stu-id="4db61-123">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="4db61-124">Цикл завершится, когда поток был закрыт сервером или маркер отмены, переданный `StreamAsChannelAsync` отменяется.</span><span class="sxs-lookup"><span data-stu-id="4db61-124">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

::: moniker range=">= aspnetcore-2.2"

```csharp
// Call "Cancel" on this CancellationTokenSource to send a cancellation message to 
// the server, which will trigger the corresponding token in the Hub method.
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

## <a name="javascript-client"></a><span data-ttu-id="4db61-125">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="4db61-125">JavaScript client</span></span>

<span data-ttu-id="4db61-126">Клиенты JavaScript вызывать методы потоковой передачи концентраторов с помощью `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="4db61-126">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="4db61-127">`stream` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="4db61-127">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="4db61-128">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="4db61-128">The name of the hub method.</span></span> <span data-ttu-id="4db61-129">В следующем примере, является имя метода концентратора `Counter`.</span><span class="sxs-lookup"><span data-stu-id="4db61-129">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="4db61-130">Аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="4db61-130">Arguments defined in the hub method.</span></span> <span data-ttu-id="4db61-131">В следующем примере аргументы являются: счетчик для числа элементов потока для получения и задержку между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="4db61-131">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="4db61-132">`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="4db61-132">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="4db61-133">Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений из `stream` вызова.</span><span class="sxs-lookup"><span data-stu-id="4db61-133">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="4db61-134">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="4db61-134">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

<span data-ttu-id="4db61-135">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="4db61-135">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span> <span data-ttu-id="4db61-136">Вызов этого метода приведет к `CancellationToken` параметр метода концентратора (Если вы указали один) отменяется.</span><span class="sxs-lookup"><span data-stu-id="4db61-136">Calling this method will cause the `CancellationToken` parameter of the Hub method (if you provided one) to be canceled.</span></span>

::: moniker-end

## <a name="related-resources"></a><span data-ttu-id="4db61-137">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4db61-137">Related resources</span></span>

* [<span data-ttu-id="4db61-138">Центры</span><span class="sxs-lookup"><span data-stu-id="4db61-138">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="4db61-139">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="4db61-139">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="4db61-140">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="4db61-140">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="4db61-141">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="4db61-141">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
