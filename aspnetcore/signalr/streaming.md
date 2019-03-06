---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как для получения потоков из значений из методов концентратора на сервере и используют потоки, с помощью клиентов .NET и JavaScript.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/14/2018
uid: signalr/streaming
ms.openlocfilehash: fb7183f7189d62c181f69ffdb170e3da25612919
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345591"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Использовать потоковую передачу в ASP.NET Core SignalR

По [Бреннан Конрой](https://github.com/BrennanConroy)

ASP.NET Core SignalR поддерживает потоковой передачи возвращаемые значения методов сервера. Это полезно для сценариев, источник фрагментах данных со временем. При возвращаемое значение передается клиенту, каждый фрагмент отправляется клиенту, как только она становится доступны, вместо ожидания возвращения всех данные станут доступны.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Настройка концентратора

::: moniker range=">= aspnetcore-3.0"

Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>`, `IAsyncEnumerable<T>`, `Task<ChannelReader<T>>`, или `Task<IAsyncEnumerable<T>>`.

::: moniker-end

::: moniker range="< aspnetcore-3.0"

Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

В ASP.NET Core 3.0 или более поздней версии, могут возвращать потоковой передачи методов концентратора `IAsyncEnumerable<T>` в дополнение к `ChannelReader<T>`. Самый простой способ возврата `IAsyncEnumerable<T>` заключается в изменении метода концентратора асинхронный метод итератора, как показано в следующем примере. Методы итератора async центра может принять `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока. Асинхронные методы итератора легко избежать распространенных проблем с каналами, например не возвращает `ChannelReader` достаточно рано или выход из метода, не завершая `ChannelWriter`.

[!INCLUDE[](~/includes/csharp-8-required.md)]

[!code-csharp[Streaming hub async iterator method](streaming/sample/Hubs/AsyncEnumerableHub.cs?name=snippet_AsyncIterator)]

::: moniker-end

В следующем примере показано с основами потоковой передачи данных на клиент с помощью каналов. Каждый раз, когда объект записывается `ChannelWriter` этого объекта немедленно отправляется клиенту. В конце `ChannelWriter` завершения, чтобы сообщить клиенту поток закрыт.

> [!NOTE]
> * Запись `ChannelWriter` в фоновом потоке и возврат `ChannelReader` как можно скорее. Другие вызовы концентратора будут заблокированы до `ChannelReader` возвращается.
> * Перенос логики в `try ... catch` и завершите `Channel` в catch и за ее пределами catch, чтобы убедиться, что концентратор вызов метода завершается должным образом.

::: moniker range="= aspnetcore-2.1"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.aspnetcore21.cs?name=snippet1)]

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?name=snippet1)]

В ASP.NET Core 2.2 или более поздней версии, могут принимать потоковой передачи методов концентратора `CancellationToken` параметр, который активируется, когда клиент отменяет подписку из потока. Используйте этот маркер для остановки работы сервера и освободить все ресурсы, если клиент отключается до окончания потока.

::: moniker-end

## <a name="net-client"></a>Клиент .NET

`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи. Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`. Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи. Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента. Чтобы считать данные, распространенный шаблон — запустить цикл для `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны. Цикл завершится, когда поток был закрыт сервером или маркер отмены, переданный `StreamAsChannelAsync` отменяется.

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

## <a name="javascript-client"></a>Клиент JavaScript

Клиенты JavaScript вызывать методы потоковой передачи концентраторов с помощью `connection.stream`. `stream` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере, является имя метода концентратора `Counter`.
* Аргументы, определенный в методе концентратора. В следующем примере аргументы являются: счетчик для числа элементов потока для получения и задержку между элементами потока.

`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод. Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений из `stream` вызова.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

::: moniker range="= aspnetcore-2.1"

Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод. Вызов этого метода приведет к `CancellationToken` параметр метода концентратора (Если вы указали один) отменяется.

::: moniker-end

::: moniker range=">= aspnetcore-3.0"
## <a name="java-client"></a>Клиент Java
Клиент SignalR Java использует `stream` метод для вызова методов потоковой передачи. Он принимает три или более аргументов:

* Ожидаемый тип элементов потока 
* Имя метода концентратора.
* Аргументы, определенный в методе концентратора. 

```java
hubConnection.stream(String.class, "ExampleStreamingHubMethod", "Arg1")
    .subscribe(
        (item) -> {/* Define your onNext handler here. */ },
        (error) -> {/* Define your onError handler here. */},
        () -> {/* Define your onCompleted handler here. */});
```
`stream` Метод `HubConnection` Возвращает наблюдаемый объект типа элемента потока. Наблюдаемого типа `subscribe` метод является определение вашего `onNext`, `onError` и `onCompleted` обработчиков.

::: moniker-end

## <a name="related-resources"></a>Связанные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
