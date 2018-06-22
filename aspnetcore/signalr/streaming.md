---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: ae0e733dddfb48db07d77ea73f4673cf8f783b88
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36275854"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="c22dd-102">Использовать потоковую передачу в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c22dd-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c22dd-103">По [Brennan Conroy](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="c22dd-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="c22dd-104">SignalR ASP.NET Core поддерживает потоковой передачи возвращаемых значений методов сервера.</span><span class="sxs-lookup"><span data-stu-id="c22dd-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="c22dd-105">Это полезно для сценариев, источник фрагментов данных динамике.</span><span class="sxs-lookup"><span data-stu-id="c22dd-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="c22dd-106">При потоковой значение, возвращаемое клиенту каждый фрагмент отправляется клиенту сразу становится доступной, а не ожидая все данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="c22dd-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="c22dd-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c22dd-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="c22dd-108">Настройка концентратора</span><span class="sxs-lookup"><span data-stu-id="c22dd-108">Set up the hub</span></span>

<span data-ttu-id="c22dd-109">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="c22dd-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="c22dd-110">Ниже приведен образец, демонстрирующий основы потоковой передачи данных клиенту.</span><span class="sxs-lookup"><span data-stu-id="c22dd-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="c22dd-111">Каждый раз, когда объект записывается `ChannelReader` этот объект будет немедленно отправлено клиенту.</span><span class="sxs-lookup"><span data-stu-id="c22dd-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="c22dd-112">В конце `ChannelReader` завершения, чтобы сообщить клиенту поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="c22dd-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="c22dd-113">Запись `ChannelReader` на фоновом потоке и возврат `ChannelReader` как можно быстрее.</span><span class="sxs-lookup"><span data-stu-id="c22dd-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="c22dd-114">Другие вызовы концентратора, блокируются до `ChannelReader` возвращается.</span><span class="sxs-lookup"><span data-stu-id="c22dd-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/hubs/streamhub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="c22dd-115">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="c22dd-115">.NET client</span></span>

<span data-ttu-id="c22dd-116">`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c22dd-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="c22dd-117">Передайте имя метода концентратора и аргументы, заданные в методе концентратора для `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="c22dd-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="c22dd-118">Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c22dd-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="c22dd-119">Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток, на клиентском компьютере.</span><span class="sxs-lookup"><span data-stu-id="c22dd-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="c22dd-120">Для чтения данных, распространенный подход заключается в цикла `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны.</span><span class="sxs-lookup"><span data-stu-id="c22dd-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="c22dd-121">Цикл завершается, когда поток был закрыт сервером, или токен отмены, передаваемый `StreamAsChannelAsync` отменяется.</span><span class="sxs-lookup"><span data-stu-id="c22dd-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

```csharp
var channel = await hubConnection.StreamAsChannelAsync<int>("Counter", 10, 500, CancellationToken.None);

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

## <a name="javascript-client"></a><span data-ttu-id="c22dd-122">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c22dd-122">JavaScript client</span></span>

<span data-ttu-id="c22dd-123">Клиентам JavaScript вызывать методы потоковой передачи для концентраторов с помощью `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="c22dd-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="c22dd-124">`stream` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="c22dd-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="c22dd-125">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c22dd-125">The name of the hub method.</span></span> <span data-ttu-id="c22dd-126">В следующем примере, является имя метода концентратора `Counter`.</span><span class="sxs-lookup"><span data-stu-id="c22dd-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="c22dd-127">Аргументы, заданные в метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="c22dd-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="c22dd-128">В следующем примере аргументы являются: количество элементов потока для получения и задержки между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="c22dd-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="c22dd-129">`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c22dd-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="c22dd-130">Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений от `stream` вызова.</span><span class="sxs-lookup"><span data-stu-id="c22dd-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="c22dd-131">Чтобы завершить поток из вызова клиента `dispose` метод `ISubscription` возвращенный `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c22dd-131">To end the stream from the client call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="c22dd-132">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c22dd-132">Related resources</span></span>

* [<span data-ttu-id="c22dd-133">Центры</span><span class="sxs-lookup"><span data-stu-id="c22dd-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c22dd-134">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="c22dd-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c22dd-135">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c22dd-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c22dd-136">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="c22dd-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)