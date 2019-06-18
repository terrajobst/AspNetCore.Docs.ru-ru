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
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="13424-103">Клиент .NET SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="13424-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="13424-104">Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с концентраторами SignalR из приложений .NET.</span><span class="sxs-lookup"><span data-stu-id="13424-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="13424-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="13424-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="13424-106">В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="13424-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="13424-107">Установка пакета клиента SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="13424-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="13424-108">`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="13424-108">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="13424-109">Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:</span><span class="sxs-lookup"><span data-stu-id="13424-109">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="13424-110">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="13424-110">Connect to a hub</span></span>

<span data-ttu-id="13424-111">Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`.</span><span class="sxs-lookup"><span data-stu-id="13424-111">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="13424-112">URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="13424-112">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="13424-113">Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`.</span><span class="sxs-lookup"><span data-stu-id="13424-113">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="13424-114">Установить подключение с `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="13424-114">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="13424-115">Дескриптор была потеряна связь</span><span class="sxs-lookup"><span data-stu-id="13424-115">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="13424-116">Автоматическое переподключение</span><span class="sxs-lookup"><span data-stu-id="13424-116">Automatically reconnect</span></span>

<span data-ttu-id="13424-117"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> Можно настроить для автоматического повторного подключения с помощью `WithAutomaticReconnect` метод <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="13424-117">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="13424-118">Он не будет повторное соединение производится автоматически по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13424-118">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="13424-119">Без параметров `WithAutomaticReconnect()` служит для настройки клиента в течение 0, 2, 10 и 30 секунд соответственно, перед попыткой попытками повторного подключения после четырех неудачных попыток.</span><span class="sxs-lookup"><span data-stu-id="13424-119">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="13424-120">Прежде чем выполнять любые попытки повторного подключения `HubConnection` будет переход к `HubConnectionState.Reconnecting` состояния и инициировать `Reconnecting` событий.</span><span class="sxs-lookup"><span data-stu-id="13424-120">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="13424-121">Это дает возможность предупредить пользователей о том, что соединение было потеряно и отключать элементы пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="13424-121">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="13424-122">Неинтерактивный режим приложений можно запустить, очереди или удаление сообщения.</span><span class="sxs-lookup"><span data-stu-id="13424-122">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="13424-123">Если клиент успешном повторном подключении в первые четыре попытки, `HubConnection` перейдет к `Connected` состояния и инициировать `Reconnected` событий.</span><span class="sxs-lookup"><span data-stu-id="13424-123">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="13424-124">Это дает возможность для информирования пользователей, подключение будет восстановлено и вывода из очереди все сообщения из очереди.</span><span class="sxs-lookup"><span data-stu-id="13424-124">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="13424-125">Так как подключение выглядит совершенно новое на сервер новый `ConnectionId` будет предоставляться `Reconnected` обработчики событий.</span><span class="sxs-lookup"><span data-stu-id="13424-125">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="13424-126">`Reconnected` Обработчик событий `connectionId` параметр будет иметь значение null, если `HubConnection` был настроен для [пропустить согласования](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="13424-126">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="13424-127">`WithAutomaticReconnect()` не настроить `HubConnection` попытки при сбоях начальной загрузки, поэтому при сбоях загрузки нужно обрабатывать вручную:</span><span class="sxs-lookup"><span data-stu-id="13424-127">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="13424-128">Если клиент не повторного подключения успешно в первые четыре попытки, `HubConnection` будет переход к `Disconnected` состояния и инициировать <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий.</span><span class="sxs-lookup"><span data-stu-id="13424-128">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="13424-129">Это обеспечивает возможность перезапустить соединение вручную или сообщите пользователям, что соединение было безвозвратно утрачены.</span><span class="sxs-lookup"><span data-stu-id="13424-129">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="13424-130">Чтобы настроить настраиваемое количество попыток повторного подключения до отключения или повторного подключения этот интервал, `WithAutomaticReconnect` принимает массив чисел, представляющий задержку в миллисекундах для ожидания перед началом каждой попытки повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="13424-130">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="13424-131">В предыдущем примере настраивается `HubConnection` запустить попытки повторных подключений сразу же после потери связи.</span><span class="sxs-lookup"><span data-stu-id="13424-131">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="13424-132">Это справедливо также для настройки по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13424-132">This is also true for the default configuration.</span></span>

<span data-ttu-id="13424-133">В случае с первой попытки повторного подключения со второй попытки повторного подключения будут также применены немедленно вместо ожидания 2 секунды, как в конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13424-133">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="13424-134">В случае со второй попытки повторного подключения третьей попытки повторного подключения начнется через 10 секунд и снова станет как конфигурацию по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="13424-134">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="13424-135">Пользовательское поведение затем еще раз расходится со всеми поведение по умолчанию путем остановки после сбоя попытки третий повторное соединение.</span><span class="sxs-lookup"><span data-stu-id="13424-135">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="13424-136">В конфигурации по умолчанию существует должен быть один более повторного подключения попытки еще на 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="13424-136">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="13424-137">Если вы хотите еще лучше контролировать время и число автоматическое переподключение попыток, `WithAutomaticReconnect` принимает объект, реализующий интерфейс `IRetryPolicy` интерфейс, который содержит один метод с именем `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="13424-137">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="13424-138">`NextRetryDelay` принимает один аргумент с типом `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="13424-138">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="13424-139">`RetryContext` Имеет три свойства: `PreviousRetryCount`, `ElapsedTime` и `RetryReason` , `long`, `TimeSpan` и `Exception` соответственно.</span><span class="sxs-lookup"><span data-stu-id="13424-139">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="13424-140">Перед первой попытки повторного подключения оба `PreviousRetryCount` и `ElapsedTime` будут равны нулю и `RetryReason` будет исключение, вызвавшее разрыв соединения.</span><span class="sxs-lookup"><span data-stu-id="13424-140">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="13424-141">После каждой попытки повтора неудачных `PreviousRetryCount` увеличивается на единицу, `ElapsedTime` будут обновлены в соответствии с временной интервал повторного подключения на данный момент и `RetryReason` будет исключение, вызвавшее сбой последней попытки повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="13424-141">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="13424-142">`NextRetryDelay` должен возвращать либо интервал времени, представляющий время ожидания перед следующей попыткой повторного подключения или `null` Если `HubConnection` должна прекратиться, повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="13424-142">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="13424-143">Кроме того, можно написать код, который позволит приложению повторно подключиться клиент вручную, как показано в [вручную переподключить](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="13424-143">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="13424-144">Вручную переподключить</span><span class="sxs-lookup"><span data-stu-id="13424-144">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="13424-145">До версии 3.0 клиент .NET для SignalR не повторное соединение производится автоматически.</span><span class="sxs-lookup"><span data-stu-id="13424-145">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="13424-146">Необходимо написать код, который позволит приложению повторно подключиться клиент вручную.</span><span class="sxs-lookup"><span data-stu-id="13424-146">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="13424-147">Используйте <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий реагировать на потерю соединения.</span><span class="sxs-lookup"><span data-stu-id="13424-147">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="13424-148">Например можно автоматизировать повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="13424-148">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="13424-149">`Closed` Событие требует делегат, который возвращает `Task`, что позволяет асинхронного кода для выполнения без использования `async void`.</span><span class="sxs-lookup"><span data-stu-id="13424-149">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="13424-150">Для удовлетворения сигнатуре делегата в `Closed` обработчик событий, который выполняется синхронно, возвращают `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="13424-150">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="13424-151">Основная причина для поддержки асинхронных является, поэтому вы можете перезапустить подключение.</span><span class="sxs-lookup"><span data-stu-id="13424-151">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="13424-152">Подключения — это действие async.</span><span class="sxs-lookup"><span data-stu-id="13424-152">Starting a connection is an async action.</span></span>

<span data-ttu-id="13424-153">В `Closed` обработчик, который перезапускает соединения, рассмотрим некоторые случайные задержки предотвратить перегрузку сервера, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="13424-153">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="13424-154">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="13424-154">Call hub methods from client</span></span>

<span data-ttu-id="13424-155">`InvokeAsync` вызывает методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="13424-155">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="13424-156">Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="13424-156">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="13424-157">SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.</span><span class="sxs-lookup"><span data-stu-id="13424-157">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="13424-158">`InvokeAsync` Возвращает метод `Task` которой завершается при возврате метода сервера.</span><span class="sxs-lookup"><span data-stu-id="13424-158">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="13424-159">Возвращаемое значение, если таковой имеется, предоставляется в результате `Task`.</span><span class="sxs-lookup"><span data-stu-id="13424-159">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="13424-160">Любые исключения, создаваемые на сервере метод создания непредвиденное `Task`.</span><span class="sxs-lookup"><span data-stu-id="13424-160">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="13424-161">Используйте `await` синтаксис для ожидания завершения метода сервера и `try...catch` синтаксис для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="13424-161">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="13424-162">`SendAsync` Возвращает метод `Task` которого завершается, когда сообщение отправлено на сервер.</span><span class="sxs-lookup"><span data-stu-id="13424-162">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="13424-163">Возвращаемое значение не указано с этого момента `Task` не приходится ждать завершения работы метода сервера.</span><span class="sxs-lookup"><span data-stu-id="13424-163">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="13424-164">Все исключения, возникшие на стороне клиента при отправке сообщения создают непредвиденное `Task`.</span><span class="sxs-lookup"><span data-stu-id="13424-164">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="13424-165">Используйте `await` и `try...catch` синтаксис для обработки ошибок отправки.</span><span class="sxs-lookup"><span data-stu-id="13424-165">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="13424-166">Если вы используете службу Azure SignalR в *бессерверной режим*, нельзя вызывать методы концентратора клиента.</span><span class="sxs-lookup"><span data-stu-id="13424-166">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="13424-167">Дополнительные сведения см. в разделе [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="13424-167">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="13424-168">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="13424-168">Call client methods from hub</span></span>

<span data-ttu-id="13424-169">Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.</span><span class="sxs-lookup"><span data-stu-id="13424-169">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="13424-170">Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="13424-170">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="13424-171">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="13424-171">Error handling and logging</span></span>

<span data-ttu-id="13424-172">Обработка ошибок с помощью оператора try-catch.</span><span class="sxs-lookup"><span data-stu-id="13424-172">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="13424-173">Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="13424-173">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="13424-174">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="13424-174">Additional resources</span></span>

* [<span data-ttu-id="13424-175">Центры</span><span class="sxs-lookup"><span data-stu-id="13424-175">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="13424-176">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="13424-176">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="13424-177">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="13424-177">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="13424-178">SignalR бессерверной документация по службе Azure</span><span class="sxs-lookup"><span data-stu-id="13424-178">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
