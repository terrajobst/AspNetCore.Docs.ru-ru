---
title: ASP.NET Core SignalR клиента .NET
author: bradygaster
description: Сведения о ASP.NET Core SignalR клиента .NET
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 01/14/2020
no-loc:
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: a9583c9d6df52ff81a402df03e663ccc3847e51f
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78652132"
---
# <a name="aspnet-core-signalr-net-client"></a>Клиентский ASP.NET Core SignalR .NET

Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с концентраторами SignalR из приложений .NET.

[Просмотреть или скачать образец кода](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))

Пример кода в этой статье — это приложение WPF, которое использует клиентский ASP.NET Core SignalR .NET.

## <a name="install-the-signalr-net-client-package"></a>Установка пакета клиента SignalR .NET

Пакет [Microsoft. AspNetCore. SignalR. Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) необходим для подключения клиентов .NET к концентраторам SignalR.

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

Чтобы установить клиентскую библиотеку, в окне **консоли диспетчера пакетов** выполните следующую команду:

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-cli"></a>[Интерфейс командной строки .NET Core](#tab/netcore-cli)

Чтобы установить клиентскую библиотеку, выполните в командной оболочке следующую команду:

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a>Подключение к концентратору

Чтобы установить соединение, создайте `HubConnectionBuilder` и вызовите `Build`. При создании соединения можно настроить URL-адрес концентратора, протокол, тип транспорта, уровень ведения журнала, заголовки и другие параметры. Настройте все необходимые параметры, вставив любой из `HubConnectionBuilder` методов в `Build`. Запустите подключение с `StartAsync`.

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>Обработано потерянное подключение

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a>Автоматическое повторное подключение

<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> можно настроить для автоматического повторного подключения с помощью метода `WithAutomaticReconnect` на <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>. По умолчанию оно не будет автоматически восстановлено.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

Без параметров `WithAutomaticReconnect()` настраивает клиент на ожидание 0, 2, 10 и 30 секунд соответственно, прежде чем пытаться выполнить каждую попытку повторного подключения, остановить после четырех неудачных попыток.

Перед запуском всех попыток повторного подключения `HubConnection` переходит в состояние `HubConnectionState.Reconnecting` и запускает событие `Reconnecting`.  Это дает возможность предупредить пользователей о потере подключения и отключении элементов пользовательского интерфейса. Неинтерактивные приложения могут запускать постановку в очередь или удалять сообщения.

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

Если клиент успешно подключится в течение первых четырех попыток, `HubConnection` вернется в состояние `Connected` и начнет событие `Reconnected`. Это дает возможность информировать пользователей о том, что подключение было восстановлено, и выводит сообщения из очереди.

Так как соединение выглядит совершенно только на сервере, в обработчики `Reconnected` событий будет предоставлен новый `ConnectionId`.

> [!WARNING]
> Параметр `connectionId` обработчика событий `Reconnected` будет иметь значение null, если `HubConnection` был настроен на [пропуск согласования](xref:signalr/configuration#configure-client-options).

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

`WithAutomaticReconnect()` не настраивает `HubConnection` для повторного выполнения начальных ошибок запуска, поэтому ошибки запуска должны быть обработаны вручную:

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

Если клиент не подключится в течение первых четырех попыток, `HubConnection` переходит в состояние `Disconnected` и запускает событие <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>. Это дает возможность попытаться вручную перезапустить подключение или сообщить пользователям, что подключение было безвозвратно утрачено.

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

Чтобы настроить настраиваемое число попыток повторного подключения перед отключением или изменением времени повторного подключения, `WithAutomaticReconnect` принимает массив чисел, представляющих задержку в миллисекундах, прежде чем начинать каждую попытку повторного подключения.

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

В предыдущем примере выполняется настройка `HubConnection` для начала попытки повторного подключения сразу после потери соединения. Это также справедливо для конфигурации по умолчанию.

Если первая попытка повторного подключения завершается неудачно, вторая попытка повторного подключения также начнется немедленно, а не подождать 2 секунды, как в конфигурации по умолчанию.

Если вторая попытка повторного подключения завершается неудачно, третья попытка повторного подключения начнется через 10 секунд, что опять же, как и конфигурация по умолчанию.

Затем пользовательское поведение снова рассходится от поведения по умолчанию, останавливаясь после третьего сбоя попытки повторного подключения. В конфигурации по умолчанию будет предпринята еще одна повторная попытки подключения через 30 секунд.

Если требуется еще более контролировать время и число попыток автоматического повторного подключения, `WithAutomaticReconnect` принимает объект, реализующий интерфейс `IRetryPolicy`, который имеет один метод с именем `NextRetryDelay`.

`NextRetryDelay` принимает один аргумент с типом `RetryContext`. `RetryContext` имеет три свойства: `PreviousRetryCount`, `ElapsedTime` и `RetryReason`, которые являются `long`, `TimeSpan` и `Exception` соответственно. Перед первой попыткой повторного подключения оба `PreviousRetryCount` и `ElapsedTime` будут равны нулю, а `RetryReason` будет исключением, вызвавшим потерю соединения. После каждой неудачной попытки повторного выполнения `PreviousRetryCount` будет увеличиваться на единицу, `ElapsedTime` будет обновлено с учетом времени, затраченного на повторное подключение, а `RetryReason` будет исключением, которое привело к сбою последней попытки повторного подключения.

`NextRetryDelay` должен возвращать интервал времени, представляющий время ожидания перед следующей попыткой повторного подключения, или `null`, если `HubConnection` должен переустановить подключение.

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
            return TimeSpan.FromSeconds(_random.NextDouble() * 10);
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

Кроме того, можно написать код, который будет повторно подключаться к клиенту вручную, как показано в [подсоединении вручную](#manually-reconnect).

::: moniker-end

### <a name="manually-reconnect"></a>Повторное подключение вручную

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> До 3,0 клиент .NET для SignalR не выполняет автоматическое повторное подключение. Необходимо написать код, который будет повторно подключаться к клиенту вручную.

::: moniker-end

Используйте событие <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> для реагирования на потерянное подключение. Например, может потребоваться автоматизировать повторное подключение.

Для события `Closed` требуется делегат, возвращающий `Task`, который позволяет выполнять асинхронный код без использования `async void`. Чтобы удовлетворить сигнатуру делегата в обработчике событий `Closed`, который выполняется синхронно, возвращайте `Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

Основная причина поддержки асинхронности заключается в том, что вы можете перезапустить подключение. Запуск соединения является асинхронным действием.

В обработчике `Closed`, который перезапускает подключение, рассмотрите возможность ожидания некоторой случайной задержки, чтобы предотвратить перегрузку сервера, как показано в следующем примере:

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>Методы концентратора вызовов от клиента

`InvokeAsync` вызывает методы в концентраторе. Передайте имя метода концентратора и все аргументы, определенные в методе Hub, в `InvokeAsync`. SignalR является асинхронным, поэтому при выполнении вызовов используйте `async` и `await`.

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

Метод `InvokeAsync` возвращает `Task`, который завершается, когда метод сервера возвращает значение. Возвращаемое значение, если таковое имеется, предоставляется в качестве результата `Task`. Все исключения, вызванные методом на сервере, вызывают сбой `Task`. Используйте синтаксис `await`, чтобы дождаться завершения метода сервера и `try...catch` синтаксис для обработки ошибок.

Метод `SendAsync` возвращает `Task`, который завершается, когда сообщение было отправлено на сервер. Возвращаемое значение не предоставляется, так как эта `Task` не ждет завершения метода сервера. Любые исключения, возникающие на клиенте при отправке сообщения, вызывают сбой `Task`. Для управления ошибками отправки используйте синтаксис `await` и `try...catch`.

> [!NOTE]
> Если вы используете службу SignalR Azure в *Бессерверном режиме*, вы не можете вызывать методы концентратора из клиента. Дополнительные сведения см. в [документации по службеSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).

## <a name="call-client-methods-from-hub"></a>Вызов методов клиента из концентратора

Определите методы, вызываемые центром с помощью `connection.On` после сборки, но перед запуском соединения.

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

Приведенный выше код в `connection.On` выполняется, когда код на стороне сервера вызывает его с помощью метода `SendAsync`.

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>Обработка ошибок и ведение журнала

Обрабатывайте ошибки с помощью оператора try-catch. Проверьте объект `Exception`, чтобы определить необходимые действия после возникновения ошибки.

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* [Центры](xref:signalr/hubs)
* [Клиент на JavaScript](xref:signalr/javascript-client)
* [Публикация в Azure](xref:signalr/publish-to-azure-web-app)
* [Документация по Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)
