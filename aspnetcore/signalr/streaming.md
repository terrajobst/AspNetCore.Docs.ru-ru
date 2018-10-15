---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: tdykstra
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 3ae9b83d60019eaa3196f35645bf9b4b03f6d8c6
ms.sourcegitcommit: 4bdf7703aed86ebd56b9b4bae9ad5700002af32d
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2018
ms.locfileid: "49325644"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Использовать потоковую передачу в ASP.NET Core SignalR

По [Бреннан Конрой](https://github.com/BrennanConroy)

ASP.NET Core SignalR поддерживает потоковой передачи возвращаемые значения методов сервера. Это полезно для сценариев, источник фрагментах данных со временем. При возвращаемое значение передается клиенту, каждый фрагмент отправляется клиенту, как только она становится доступны, вместо ожидания возвращения всех данные станут доступны.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Настройка концентратора

Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`. Ниже приведен пример, показывающий, с основами потоковой передачи данных клиенту. Каждый раз, когда объект записывается `ChannelReader` этого объекта немедленно отправляется клиенту. В конце `ChannelReader` завершения, чтобы сообщить клиенту поток закрыт.

> [!NOTE]
> Запись `ChannelReader` в фоновом потоке и возврат `ChannelReader` как можно скорее. Другие вызовы концентратора будут заблокированы до `ChannelReader` возвращается.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>Клиент .NET

`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи. Передайте имя метода концентратора и аргументы, заданные в методе концентратора, чтобы `StreamAsChannelAsync`. Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи. Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток на стороне клиента. Чтобы считать данные, распространенный шаблон — запустить цикл для `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны. Цикл завершится, когда поток был закрыт сервером или маркер отмены, переданный `StreamAsChannelAsync` отменяется.

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

## <a name="javascript-client"></a>Клиент JavaScript

Клиенты JavaScript вызывать методы потоковой передачи концентраторов с помощью `connection.stream`. `stream` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере, является имя метода концентратора `Counter`.
* Аргументы, определенный в методе концентратора. В следующем примере аргументы являются: счетчик для числа элементов потока для получения и задержку между элементами потока.

`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод. Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений из `stream` вызова.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Чтобы завершить поток, из клиента, вызовите `dispose` метод `ISubscription` , возвращаемый методом `subscribe` метод.

## <a name="related-resources"></a>Связанные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
