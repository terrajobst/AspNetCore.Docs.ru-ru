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
ms.openlocfilehash: 7825beba55cefb6236fd8d8e332d030a7e4fc6df
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963888"
---
# <a name="use-streaming-in-aspnet-core-opno-locsignalr"></a>Использование потоковой передачи в ASP.NET Core SignalR

По [Бреннан Конрой](https://github.com/BrennanConroy)

::: moniker range=">= aspnetcore-3.0"

ASP.NET Core SignalR поддерживает потоковую передачу от клиента к серверу и от сервера к клиенту. Это полезно для сценариев, в которых фрагменты данных поступают с течением времени. При потоковой передаче каждый фрагмент отправляется клиенту или серверу сразу после того, как он становится доступным, а не ожидает, пока все данные станут доступны.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

ASP.NET Core SignalR поддерживает потоковые возвращаемые значения методов сервера. Это полезно для сценариев, в которых фрагменты данных поступают с течением времени. Когда возвращаемое значение передается клиенту в потоке, каждый фрагмент отправляется клиенту, как только он становится доступным, а не ожидает, пока все данные станут доступны.

::: moniker-end

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/live/aspnetcore/signalr/streaming/samples/) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="set-up-a-hub-for-streaming"></a>Настройка концентратора для потоковой передачи

::: moniker range=">= aspnetcore-3.0"

Метод концентратора автоматически превращается в метод концентратора потоковой передачи, когда он возвращает <xref:System.Collections.Generic.IAsyncEnumerable`1>, <xref:System.Threading.Channels.ChannelReader%601>, `Task<IAsyncEnumerable<T>>`или `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

При возврате <xref:System.Threading.Channels.ChannelReader%601> или `Task<ChannelReader<T>>`метод концентратора автоматически превращается в метод концентратора потоковой передачи.

::: moniker-end

### <a name="server-to-client-streaming"></a>Потоковая передача из сервера в клиент

::: moniker range=">= aspnetcore-3.0"

Методы концентратора потоковой передачи могут возвращать `IAsyncEnumerable<T>` в дополнение к `ChannelReader<T>`. Самый простой способ вернуть `IAsyncEnumerable<T>` — сделать метод концентратора асинхронным методом итератора, как показано в следующем примере. Методы асинхронного итератора концентратора могут принимать параметр `CancellationToken`, который активируется, когда клиент отменяет подписывание из потока. Методы асинхронных итераторов позволяют избежать проблем, распространенных с помощью каналов, например, не возвращая `ChannelReader` ранних или завершающих работу метода без завершения <xref:System.Threading.Channels.ChannelWriter`1>.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/samples/3.0/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

В следующем примере показаны основные сведения о потоковой передаче данных клиенту с помощью каналов. Каждый раз, когда объект записывается в <xref:System.Threading.Channels.ChannelWriter%601>, объект немедленно отправляется клиенту. В конце `ChannelWriter` завершается, чтобы сообщить клиенту, что поток закрыт.

> [!NOTE]
> Запись в `ChannelWriter<T>` в фоновом потоке и возврат `ChannelReader` как можно скорее. Другие вызовы концентратора блокируются до тех пор, пока не будет возвращено `ChannelReader`.
>
> Переносить логику в `try ... catch`. Завершите `Channel` в `catch` и вне `catch`, чтобы убедиться, что вызов метода концентратора завершен правильно.

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

Методы концентратора потоковой передачи "сервер-клиент" могут принимать параметр `CancellationToken`, который активируется, когда клиент отменяет подписывание из потока. Используйте этот маркер, чтобы прекратить работу сервера и освободить все ресурсы, если клиент отключится до конца потока.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Потоковая передача клиента в сервер

Метод концентратора автоматически превращается в метод концентратора потоковой передачи клиента в сервер, если он принимает один или несколько объектов типа <xref:System.Threading.Channels.ChannelReader%601> или <xref:System.Collections.Generic.IAsyncEnumerable%601>. В следующем примере показаны основные сведения о чтении потоковых данных, отправленных клиентом. Каждый раз, когда клиент выполняет запись в <xref:System.Threading.Channels.ChannelWriter%601>, данные записываются в `ChannelReader` на сервере, с которого считывается метод концентратора.

[!code-csharp[Streaming upload hub method](streaming/samples/3.0/Hubs/StreamHub.cs?name=snippet2)]

Ниже приведена <xref:System.Collections.Generic.IAsyncEnumerable%601>ная версия метода.

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

## <a name="net-client"></a>Клиент .NET

### <a name="server-to-client-streaming"></a>Потоковая передача из сервера в клиент


::: moniker range=">= aspnetcore-3.0"

Методы `StreamAsync` и `StreamAsChannelAsync` в `HubConnection` используются для вызова методов потоковой передачи "сервер-клиент". Передайте имя и аргументы метода концентратора, определенные в методе Hub, в `StreamAsync` или `StreamAsChannelAsync`. Универсальный параметр в `StreamAsync<T>` и `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи. Объект типа `IAsyncEnumerable<T>` или `ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.

`StreamAsync` пример, который возвращает `IAsyncEnumerable<int>`:

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

Соответствующий пример `StreamAsChannelAsync`, который возвращает `ChannelReader<int>`:

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

Метод `StreamAsChannelAsync` в `HubConnection` используется для вызова метода потоковой передачи "сервер-клиент". Передайте имя метода концентратора и аргументы, определенные в методе концентратора, в `StreamAsChannelAsync`. Универсальный параметр в `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи. `ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.

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

Метод `StreamAsChannelAsync` в `HubConnection` используется для вызова метода потоковой передачи "сервер-клиент". Передайте имя метода концентратора и аргументы, определенные в методе концентратора, в `StreamAsChannelAsync`. Универсальный параметр в `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи. `ChannelReader<T>` возвращается из вызова потока и представляет поток на клиенте.

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

### <a name="client-to-server-streaming"></a>Потоковая передача клиента в сервер

Существует два способа вызвать метод концентратора потоковой передачи данных "клиент-сервер" из клиента .NET. Можно передать `IAsyncEnumerable<T>` или `ChannelReader` в качестве аргумента в `SendAsync`, `InvokeAsync`или `StreamAsChannelAsync`в зависимости от вызываемого метода концентратора.

Каждый раз, когда данные записываются в `IAsyncEnumerable` или `ChannelWriter` объект, метод концентратора на сервере получает новый элемент с данными клиента.

Если используется объект `IAsyncEnumerable`, поток завершается после выхода из метода, возвращающего элементы потока.

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

Если вы используете `ChannelWriter`, вы можете выполнить канал с `channel.Writer.Complete()`:

```csharp
var channel = Channel.CreateBounded<string>(10);
await connection.SendAsync("UploadStream", channel.Reader);
await channel.Writer.WriteAsync("some data");
await channel.Writer.WriteAsync("some more data");
channel.Writer.Complete();
```

::: moniker-end

## <a name="javascript-client"></a>Клиент JavaScript

### <a name="server-to-client-streaming"></a>Потоковая передача из сервера в клиент

Клиенты JavaScript вызывают потоковые методы "сервер-клиент" в концентраторах с `connection.stream`. Метод `stream` принимает два аргумента:

* Имя метода концентратора. В следующем примере имя метода концентратора — `Counter`.
* Аргументы, определенные в методе концентратора. В следующем примере аргументы представляют число получаемых элементов потока и задержку между элементами потока.

`connection.stream` возвращает `IStreamResult`, который содержит метод `subscribe`. Передайте `IStreamSubscriber` `subscribe` и задайте обратные вызовы `next`, `error`и `complete`, чтобы получать уведомления от вызова `stream`.

::: moniker range=">= aspnetcore-2.2"

[!code-javascript[Streaming javascript](streaming/samples/2.2/wwwroot/js/stream.js?range=19-36)]

Чтобы завершить поток от клиента, вызовите метод `dispose` для `ISubscription`, возвращаемого методом `subscribe`. Вызов этого метода приводит к отмене параметра `CancellationToken` метода концентратора, если он указан.

::: moniker-end

::: moniker range="= aspnetcore-2.1"

[!code-javascript[Streaming javascript](streaming/samples/2.1/wwwroot/js/stream.js?range=19-36)]

Чтобы завершить поток от клиента, вызовите метод `dispose` для `ISubscription`, возвращаемого методом `subscribe`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

### <a name="client-to-server-streaming"></a>Потоковая передача клиента в сервер

Клиенты JavaScript вызывают методы потоковой передачи клиента на сервер на концентраторах, передавая `Subject` в качестве аргумента для `send`, `invoke`или `stream`в зависимости от вызываемого метода концентратора. `Subject` — это класс, который выглядит как `Subject`. Например, в Рксжс можно использовать класс [subject](https://rxjs-dev.firebaseapp.com/api/index/class/Subject) из этой библиотеки.

[!code-javascript[Upload javascript](streaming/samples/3.0/wwwroot/js/stream.js?range=41-51)]

Вызов `subject.next(item)` с элементом записывает элемент в поток, а метод концентратора получает элемент на сервере.

Чтобы завершить поток, вызовите `subject.complete()`.

## <a name="java-client"></a>Клиент Java

### <a name="server-to-client-streaming"></a>Потоковая передача из сервера в клиент

Клиент SignalR Java использует метод `stream` для вызова методов потоковой передачи. `stream` принимает три или более аргументов:

* Ожидаемый тип элементов потока.
* Имя метода концентратора.
* Аргументы, определенные в методе концентратора.

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```

Метод `stream` для `HubConnection` Возвращает наблюдаемый тип элемента потока. `subscribe` метод наблюдаемого типа, в котором определены обработчики `onNext`, `onError` и `onCompleted`.

::: moniker-end

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
