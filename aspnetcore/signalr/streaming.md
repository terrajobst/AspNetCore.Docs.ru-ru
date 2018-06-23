---
title: Использовать потоковую передачу в ASP.NET Core SignalR
author: rachelappel
description: ''
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/07/2018
uid: signalr/streaming
ms.openlocfilehash: 08ddea4fb83150bab27a9e2685c75ff34565606b
ms.sourcegitcommit: 79b756ea03eae77a716f500ef88253ee9b1464d2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/22/2018
ms.locfileid: "36327497"
---
# <a name="use-streaming-in-aspnet-core-signalr"></a>Использовать потоковую передачу в ASP.NET Core SignalR

По [Brennan Conroy](https://github.com/BrennanConroy)

SignalR ASP.NET Core поддерживает потоковой передачи возвращаемых значений методов сервера. Это полезно для сценариев, источник фрагментов данных динамике. При потоковой значение, возвращаемое клиенту каждый фрагмент отправляется клиенту сразу становится доступной, а не ожидая все данные станут доступны.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/streaming/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="set-up-the-hub"></a>Настройка концентратора

Метод концентратора автоматически становится потоковой передачи метода концентратора, если он возвращает `ChannelReader<T>` или `Task<ChannelReader<T>>`. Ниже приведен образец, демонстрирующий основы потоковой передачи данных клиенту. Каждый раз, когда объект записывается `ChannelReader` этот объект будет немедленно отправлено клиенту. В конце `ChannelReader` завершения, чтобы сообщить клиенту поток закрыт.

> [!NOTE]
> Запись `ChannelReader` на фоновом потоке и возврат `ChannelReader` как можно быстрее. Другие вызовы концентратора, блокируются до `ChannelReader` возвращается.

[!code-csharp[Streaming hub method](streaming/sample/Hubs/StreamHub.cs?range=10-34)]

## <a name="net-client"></a>Клиент .NET

`StreamAsChannelAsync` Метод `HubConnection` используется для вызова метода потоковой передачи. Передайте имя метода концентратора и аргументы, заданные в методе концентратора для `StreamAsChannelAsync`. Универсальный параметр на `StreamAsChannelAsync<T>` указывает тип объектов, возвращаемых методом потоковой передачи. Объект `ChannelReader<T>` возвращается из вызова потока и представляет собой поток, на клиентском компьютере. Для чтения данных, распространенный подход заключается в цикла `WaitToReadAsync` и вызвать `TryRead` когда данные недоступны. Цикл завершается, когда поток был закрыт сервером, или токен отмены, передаваемый `StreamAsChannelAsync` отменяется.

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

Клиентам JavaScript вызывать методы потоковой передачи для концентраторов с помощью `connection.stream`. `stream` Метод принимает два аргумента:

* Имя метода концентратора. В следующем примере, является имя метода концентратора `Counter`.
* Аргументы, заданные в метод концентратора. В следующем примере аргументы являются: количество элементов потока для получения и задержки между элементами потока.

`connection.stream` Возвращает `IStreamResult` , содержащее `subscribe` метод. Передайте `IStreamSubscriber` для `subscribe` и задайте `next`, `error`, и `complete` обратные вызовы для получения уведомлений от `stream` вызова.

[!code-javascript[Streaming javascript](streaming/sample/wwwroot/js/stream.js?range=19-36)]

Чтобы завершить поток из вызова клиента `dispose` метод `ISubscription` возвращенный `subscribe` метод.

## <a name="related-resources"></a>Связанные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент .NET](xref:signalr/dotnet-client)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
