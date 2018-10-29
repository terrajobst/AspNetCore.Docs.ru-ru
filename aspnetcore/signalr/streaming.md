---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 70f12999b7f4230147b9ea43f6f7730b0816c43a
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206392"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a><span data-ttu-id="c7064-102">Использовать потоковую передачу в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c7064-102">Use streaming in ASP.NET Core SignalR</span></span>

<span data-ttu-id="c7064-103">По [Бреннан Конрой](https://github.com/BrennanConroy)</span><span class="sxs-lookup"><span data-stu-id="c7064-103">By [Brennan Conroy](https://github.com/BrennanConroy)</span></span>

<span data-ttu-id="c7064-104">ASP.NET Core SignalR поддерживает потоковой передачи возвращаемые значения методов сервера.</span><span class="sxs-lookup"><span data-stu-id="c7064-104">ASP.NET Core SignalR supports streaming return values of server methods.</span></span> <span data-ttu-id="c7064-105">Это полезно для сценариев, источник фрагментах данных со временем.</span><span class="sxs-lookup"><span data-stu-id="c7064-105">This is useful for scenarios where fragments of data will come in over time.</span></span> <span data-ttu-id="c7064-106">При возвращаемое значение передается клиенту, каждый фрагмент отправляется клиенту, как только она становится доступны, вместо ожидания возвращения всех данные станут доступны.</span><span class="sxs-lookup"><span data-stu-id="c7064-106">When a return value is streamed to the client, each fragment is sent to the client as soon as it becomes available, rather than waiting for all the data to become available.</span></span>

<span data-ttu-id="c7064-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="c7064-107">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="set-up-the-hub"></a><span data-ttu-id="c7064-108">Настройка концентратора</span><span class="sxs-lookup"><span data-stu-id="c7064-108">Set up the hub</span></span>

<span data-ttu-id="c7064-109">Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`.</span><span class="sxs-lookup"><span data-stu-id="c7064-109">A hub method automatically becomes a streaming hub method when it returns a `ChannelReader<T>` or a `Task<ChannelReader<T>>`.</span></span> <span data-ttu-id="c7064-110">Ниже приведен пример, показывающий, с основами потоковой передачи данных клиенту.</span><span class="sxs-lookup"><span data-stu-id="c7064-110">Below is a sample that shows the basics of streaming data to the client.</span></span> <span data-ttu-id="c7064-111">Каждый раз, когда объект записывается `ChannelReader` этого объекта немедленно отправляется клиенту.</span><span class="sxs-lookup"><span data-stu-id="c7064-111">Whenever an object is written to the `ChannelReader` that object is immediately sent to the client.</span></span> <span data-ttu-id="c7064-112">В конце `ChannelReader` завершения, чтобы сообщить клиенту поток закрыт.</span><span class="sxs-lookup"><span data-stu-id="c7064-112">At the end, the `ChannelReader` is completed to tell the client the stream is closed.</span></span>

> [!NOTE]
> <span data-ttu-id="c7064-113">Запись `ChannelReader` в фоновом потоке и возврат `ChannelReader` как можно скорее.</span><span class="sxs-lookup"><span data-stu-id="c7064-113">Write to the `ChannelReader` on a background thread and return the `ChannelReader` as soon as possible.</span></span> <span data-ttu-id="c7064-114">Другие вызовы концентратора будут заблокированы до `ChannelReader` возвращается.</span><span class="sxs-lookup"><span data-stu-id="c7064-114">Other hub invocations will be blocked until a `ChannelReader` is returned.</span></span>

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a><span data-ttu-id="c7064-115">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="c7064-115">.NET client</span></span>

<span data-ttu-id="c7064-116">`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7064-116">The `StreamAsChannelAsync` method on `HubConnection` is used to invoke a streaming method.</span></span> <span data-ttu-id="c7064-117">Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`.</span><span class="sxs-lookup"><span data-stu-id="c7064-117">Pass the hub method name, and arguments defined in the hub method to `StreamAsChannelAsync`.</span></span> <span data-ttu-id="c7064-118">Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="c7064-118">The generic parameter on `StreamAsChannelAsync<T>` specifies the type of objects returned by the streaming method.</span></span> <span data-ttu-id="c7064-119">Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="c7064-119">A `ChannelReader<T>` is returned from the stream invocation, and represents the stream on the client.</span></span> <span data-ttu-id="c7064-120">Чтобы считать данные, распространенный шаблон — запустить цикл для `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны.</span><span class="sxs-lookup"><span data-stu-id="c7064-120">To read data, a common pattern is to loop over `WaitToReadAsync` and call `TryRead` when data is available.</span></span> <span data-ttu-id="c7064-121">Цикл завершится, когда поток был закрыт сервером или маркер отмены, переданный `StreamAsChannelAsync` отменяется.</span><span class="sxs-lookup"><span data-stu-id="c7064-121">The loop will end when the stream has been closed by the server, or the cancellation token passed to `StreamAsChannelAsync` is canceled.</span></span>

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

## <a name="javascript-client"></a><span data-ttu-id="c7064-122">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c7064-122">JavaScript client</span></span>

<span data-ttu-id="c7064-123">Клиенты JavaScript вызывать методы потоковой передачи концентраторов с помощью `connection.stream`.</span><span class="sxs-lookup"><span data-stu-id="c7064-123">JavaScript clients call streaming methods on hubs by using `connection.stream`.</span></span> <span data-ttu-id="c7064-124">`stream` Метод принимает два аргумента:</span><span class="sxs-lookup"><span data-stu-id="c7064-124">The `stream` method accepts two arguments:</span></span>

* <span data-ttu-id="c7064-125">Имя метода концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7064-125">The name of the hub method.</span></span> <span data-ttu-id="c7064-126">В следующем примере, является имя метода концентратора `Counter`.</span><span class="sxs-lookup"><span data-stu-id="c7064-126">In the following example, the hub method name is `Counter`.</span></span>
* <span data-ttu-id="c7064-127">Аргументы, определенный в методе концентратора.</span><span class="sxs-lookup"><span data-stu-id="c7064-127">Arguments defined in the hub method.</span></span> <span data-ttu-id="c7064-128">В следующем примере аргументы являются: счетчик для числа элементов потока для получения и задержку между элементами потока.</span><span class="sxs-lookup"><span data-stu-id="c7064-128">In the following example, the arguments are: a count for the number of stream items to receive, and the delay between stream items.</span></span>

<span data-ttu-id="c7064-129">`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c7064-129">`connection.stream` returns an `IStreamResult` which contains a `subscribe` method.</span></span> <span data-ttu-id="c7064-130">Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений из `stream` вызова.</span><span class="sxs-lookup"><span data-stu-id="c7064-130">Pass an `IStreamSubscriber` to `subscribe` and set the `next`, `error`, and `complete` callbacks to get notifications from the `stream` invocation.</span></span>

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

<span data-ttu-id="c7064-131">Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.</span><span class="sxs-lookup"><span data-stu-id="c7064-131">To end the stream from the client, call the `dispose` method on the `ISubscription` that is returned from the `subscribe` method.</span></span>

## <a name="related-resources"></a><span data-ttu-id="c7064-132">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c7064-132">Related resources</span></span>

* [<span data-ttu-id="c7064-133">Центры</span><span class="sxs-lookup"><span data-stu-id="c7064-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="c7064-134">Клиент .NET</span><span class="sxs-lookup"><span data-stu-id="c7064-134">.NET client</span></span>](xref:signalr/dotnet-client)
* [<span data-ttu-id="c7064-135">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c7064-135">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c7064-136">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="c7064-136">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
