---
title: Клиент .NET SignalR ASP.NET Core
author: bradygaster
description: Сведения о клиенте .NET, ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 97c553874cb1e4b678fa0e5cd65074f135193861
ms.sourcegitcommit: 4ef0362ef8b6e5426fc5af18f22734158fe587e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/17/2019
ms.locfileid: "67153118"
---
# <a name="aspnet-core-signalr-net-client"></a>Клиент .NET SignalR ASP.NET Core

Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с концентраторами SignalR из приложений .NET.

[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Установка пакета клиента SignalR .NET

`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR. Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`. URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения. Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`. Установить подключение с `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Дескриптор была потеряна связь

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Автоматическое переподключение

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> Можно настроить для автоматического повторного подключения с помощью `WithAutomaticReconnect` метод <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. Он не будет повторное соединение производится автоматически по умолчанию.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Без параметров `WithAutomaticReconnect()` служит для настройки клиента в течение 0, 2, 10 и 30 секунд соответственно, перед попыткой попытками повторного подключения после четырех неудачных попыток.

Прежде чем выполнять любые попытки повторного подключения `HubConnection` будет переход к `HubConnectionState.Reconnecting` состояния и инициировать `Reconnecting` событий.  Это дает возможность предупредить пользователей о том, что соединение было потеряно и отключать элементы пользовательского интерфейса. Неинтерактивный режим приложений можно запустить, очереди или удаление сообщения.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Если клиент успешном повторном подключении в первые четыре попытки, `HubConnection` перейдет к `Connected` состояния и инициировать `Reconnected` событий. Это дает возможность для информирования пользователей, подключение будет восстановлено и вывода из очереди все сообщения из очереди.

Так как подключение выглядит совершенно новое на сервер новый `ConnectionId` будет предоставляться `Reconnected` обработчики событий.

> [!WARNING]
> `Reconnected` Обработчик событий `connectionId` параметр будет иметь значение null, если `HubConnection` был настроен для [пропустить согласования](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` не настроить `HubConnection` попытки при сбоях начальной загрузки, поэтому при сбоях загрузки нужно обрабатывать вручную:

```csharp
public static async Task<bool> ConnectWithRetryAsync(HubConnection connection, CancellationToken token)
{
    // Keep trying to until we can start or the token is canceled.
    while (true)
    {
        try
        {
            await connection.StartAsync(token);
            Debug.Assert(connection.State == HubConnectionState.Connected);
            return true;
        }
        catch when (token.IsCancellationRequested)
        {
            return false;
        }
        catch
        {
            // Failed to connect, trying again in 5000 ms.
            Debug.Assert(connection.State == HubConnectionState.Disconnected);
            await Task.Delay(5000);
        }
    }
}
```

Если клиент не повторного подключения успешно в первые четыре попытки, `HubConnection` будет переход к `Disconnected` состояния и инициировать <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий. Это обеспечивает возможность перезапустить соединение вручную или сообщите пользователям, что соединение было безвозвратно утрачены.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Чтобы настроить настраиваемое количество попыток повторного подключения до отключения или повторного подключения этот интервал, `WithAutomaticReconnect` принимает массив чисел, представляющий задержку в миллисекундах для ожидания перед началом каждой попытки повторного подключения.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

В предыдущем примере настраивается `HubConnection` запустить попытки повторных подключений сразу же после потери связи. Это справедливо также для настройки по умолчанию.

В случае с первой попытки повторного подключения со второй попытки повторного подключения будут также применены немедленно вместо ожидания 2 секунды, как в конфигурации по умолчанию.

В случае со второй попытки повторного подключения третьей попытки повторного подключения начнется через 10 секунд и снова станет как конфигурацию по умолчанию.

Пользовательское поведение затем еще раз расходится со всеми поведение по умолчанию путем остановки после сбоя попытки третий повторное соединение. В конфигурации по умолчанию существует должен быть один более повторного подключения попытки еще на 30 секунд.

Если вы хотите еще лучше контролировать время и число автоматическое переподключение попыток, `WithAutomaticReconnect` принимает объект, реализующий интерфейс `IRetryPolicy` интерфейс, который содержит один метод с именем `NextRetryDelay`.

`NextRetryDelay` принимает один аргумент с типом `RetryContext`. `RetryContext` Имеет три свойства: `PreviousRetryCount`, `ElapsedTime` и `RetryReason` , `long`, `TimeSpan` и `Exception` соответственно. Перед первой попытки повторного подключения оба `PreviousRetryCount` и `ElapsedTime` будут равны нулю и `RetryReason` будет исключение, вызвавшее разрыв соединения. После каждой попытки повтора неудачных `PreviousRetryCount` увеличивается на единицу, `ElapsedTime` будут обновлены в соответствии с временной интервал повторного подключения на данный момент и `RetryReason` будет исключение, вызвавшее сбой последней попытки повторного подключения.

`NextRetryDelay` должен возвращать либо интервал времени, представляющий время ожидания перед следующей попыткой повторного подключения или `null` Если `HubConnection` должна прекратиться, повторное подключение.

```csharp
public class RandomRetryPolicy : IRetryPolicy
{
    private readonly Random _random = new Random();

    public TimeSpan? NextRetryDelay(RetryContext retryContext)
    {
        // If we've been reconnecting for less than 60 seconds so far,
        // wait between 0 and 10 seconds before the next reconnect attempt.
        if (retryContext.ElapsedTime < TimeSpan.FromSeconds(60))
        {
            return TimeSpan.FromSeconds(_random.Next() * 10);
        }
        else
        {
            // If we've been reconnecting for more than 60 seconds so far, stop reconnecting.
            return null;
        }
    }
}
```

```csharp
HubConnection connection = new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new RandomRetryPolicy())
    .Build();
```

Кроме того, можно написать код, который позволит приложению повторно подключиться клиент вручную, как показано в [вручную переподключить](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Вручную переподключить

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> До версии 3.0 клиент .NET для SignalR не повторное соединение производится автоматически. Необходимо написать код, который позволит приложению повторно подключиться клиент вручную.

::: moniker-end

Используйте <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий реагировать на потерю соединения. Например можно автоматизировать повторное подключение.

`Closed` Событие требует делегат, который возвращает `Task`, что позволяет асинхронного кода для выполнения без использования `async void`. Для удовлетворения сигнатуре делегата в `Closed` обработчик событий, который выполняется синхронно, возвращают `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Основная причина для поддержки асинхронных является, поэтому вы можете перезапустить подключение. Подключения — это действие async.

В `Closed` обработчик, который перезапускает соединения, рассмотрим некоторые случайные задержки предотвратить перегрузку сервера, как показано в следующем примере:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Вызов методов концентратора из клиента

`InvokeAsync` вызывает методы концентратора. Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`. SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync` Возвращает метод `Task` которой завершается при возврате метода сервера. Возвращаемое значение, если таковой имеется, предоставляется в результате `Task`. Любые исключения, создаваемые на сервере метод создания непредвиденное `Task`. Используйте `await` синтаксис для ожидания завершения метода сервера и `try...catch` синтаксис для обработки ошибок.

`SendAsync` Возвращает метод `Task` которого завершается, когда сообщение отправлено на сервер. Возвращаемое значение не указано с этого момента `Task` не приходится ждать завершения работы метода сервера. Все исключения, возникшие на стороне клиента при отправке сообщения создают непредвиденное `Task`. Используйте `await` и `try...catch` синтаксис для обработки ошибок отправки.

> [!NOTE]
> Если вы используете службу Azure SignalR в *бессерверной режим*, нельзя вызывать методы концентратора клиента. Дополнительные сведения см. в разделе [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Вызывать методы клиента от концентратора

Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Обработка ошибок с помощью оператора try-catch. Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [SignalR бессерверной документация по службе Azure](/azure/azure-signalr/signalr-concept-serverless-development-config)
