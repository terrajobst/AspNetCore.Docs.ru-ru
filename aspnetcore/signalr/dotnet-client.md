---
title: ASP.NET Core SignalR клиента .NET
author: bradygaster
description: Сведения о ASP.NET Core SignalR клиента .NET
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/dotnet-client
ms.openlocfilehash: 28e8fcf808406cd0251ba94e2ef97ab04841fcd0
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963971"
---
# <a name="aspnet-core-opno-locsignalr-net-client"></a><span data-ttu-id="17dc4-103">ASP.NET Core SignalR клиента .NET</span><span class="sxs-lookup"><span data-stu-id="17dc4-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="17dc4-104">Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с SignalRными концентраторами из приложений .NET.</span><span class="sxs-lookup"><span data-stu-id="17dc4-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

<span data-ttu-id="17dc4-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="17dc4-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="17dc4-106">Пример кода в этой статье — это приложение WPF, которое использует ASP.NET Core SignalR клиента .NET.</span><span class="sxs-lookup"><span data-stu-id="17dc4-106">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-opno-locsignalr-net-client-package"></a><span data-ttu-id="17dc4-107">Установка клиентского пакета SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="17dc4-107">Install the SignalR .NET client package</span></span>

<span data-ttu-id="17dc4-108">Объект [Microsoft. AspNetCore.SignalR. ](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client)Для подключения клиентов .NET к SignalRным концентраторам требуется клиентский пакет.</span><span class="sxs-lookup"><span data-stu-id="17dc4-108">The [Microsoft.AspNetCore.SignalR.Client](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR.Client) package is required for .NET clients to connect to SignalR hubs.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="17dc4-109">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="17dc4-109">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="17dc4-110">Чтобы установить клиентскую библиотеку, в окне **консоли диспетчера пакетов** выполните следующую команду:</span><span class="sxs-lookup"><span data-stu-id="17dc4-110">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="17dc4-111">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="17dc4-111">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="17dc4-112">Чтобы установить клиентскую библиотеку, выполните в командной оболочке следующую команду:</span><span class="sxs-lookup"><span data-stu-id="17dc4-112">To install the client library, run the following command in a command shell:</span></span>

```dotnetcli
dotnet add package Microsoft.AspNetCore.SignalR.Client
```

---

## <a name="connect-to-a-hub"></a><span data-ttu-id="17dc4-113">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="17dc4-113">Connect to a hub</span></span>

<span data-ttu-id="17dc4-114">Чтобы установить соединение, создайте `HubConnectionBuilder` и вызовите `Build`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-114">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="17dc4-115">При создании соединения можно настроить URL-адрес концентратора, протокол, тип транспорта, уровень ведения журнала, заголовки и другие параметры.</span><span class="sxs-lookup"><span data-stu-id="17dc4-115">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="17dc4-116">Настройте все необходимые параметры, вставив любой из `HubConnectionBuilder` методов в `Build`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-116">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="17dc4-117">Запустите подключение с `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-117">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="17dc4-118">Обработано потерянное подключение</span><span class="sxs-lookup"><span data-stu-id="17dc4-118">Handle lost connection</span></span>

::: moniker range=">= aspnetcore-3.0"

### <a name="automatically-reconnect"></a><span data-ttu-id="17dc4-119">Автоматическое повторное подключение</span><span class="sxs-lookup"><span data-stu-id="17dc4-119">Automatically reconnect</span></span>

<span data-ttu-id="17dc4-120"><xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> можно настроить для автоматического повторного подключения с помощью метода `WithAutomaticReconnect` на <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span><span class="sxs-lookup"><span data-stu-id="17dc4-120">The <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection> can be configured to automatically reconnect using the `WithAutomaticReconnect` method on the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnectionBuilder>.</span></span> <span data-ttu-id="17dc4-121">По умолчанию оно не будет автоматически восстановлено.</span><span class="sxs-lookup"><span data-stu-id="17dc4-121">It won't automatically reconnect by default.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect()
    .Build();
```

<span data-ttu-id="17dc4-122">Без параметров `WithAutomaticReconnect()` настраивает клиент на ожидание 0, 2, 10 и 30 секунд соответственно, прежде чем пытаться выполнить каждую попытку повторного подключения, остановить после четырех неудачных попыток.</span><span class="sxs-lookup"><span data-stu-id="17dc4-122">Without any parameters, `WithAutomaticReconnect()` configures the client to wait 0, 2, 10, and 30 seconds respectively before trying each reconnect attempt, stopping after four failed attempts.</span></span>

<span data-ttu-id="17dc4-123">Перед запуском всех попыток повторного подключения `HubConnection` переходит в состояние `HubConnectionState.Reconnecting` и запускает событие `Reconnecting`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-123">Before starting any reconnect attempts, the `HubConnection` will transition to the `HubConnectionState.Reconnecting` state and fire the `Reconnecting` event.</span></span>  <span data-ttu-id="17dc4-124">Это дает возможность предупредить пользователей о потере подключения и отключении элементов пользовательского интерфейса.</span><span class="sxs-lookup"><span data-stu-id="17dc4-124">This provides an opportunity to warn users that the connection has been lost and to disable UI elements.</span></span> <span data-ttu-id="17dc4-125">Неинтерактивные приложения могут запускать постановку в очередь или удалять сообщения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-125">Non-interactive apps can start queuing or dropping messages.</span></span>

```csharp
connection.Reconnecting += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Reconnecting);

    // Notify users the connection was lost and the client is reconnecting.
    // Start queuing or dropping messages.

    return Task.CompletedTask;
};
```

<span data-ttu-id="17dc4-126">Если клиент успешно подключится в течение первых четырех попыток, `HubConnection` вернется в состояние `Connected` и начнет событие `Reconnected`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-126">If the client successfully reconnects within its first four attempts, the `HubConnection` will transition back to the `Connected` state and fire the `Reconnected` event.</span></span> <span data-ttu-id="17dc4-127">Это дает возможность информировать пользователей о том, что подключение было восстановлено, и выводит сообщения из очереди.</span><span class="sxs-lookup"><span data-stu-id="17dc4-127">This provides an opportunity to inform users the connection has been reestablished and dequeue any queued messages.</span></span>

<span data-ttu-id="17dc4-128">Так как соединение выглядит совершенно только на сервере, в обработчики `Reconnected` событий будет предоставлен новый `ConnectionId`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-128">Since the connection looks entirely new to the server, a new `ConnectionId` will be provided to the `Reconnected` event handlers.</span></span>

> [!WARNING]
> <span data-ttu-id="17dc4-129">Параметр `connectionId` обработчика событий `Reconnected` будет иметь значение null, если `HubConnection` был настроен на [пропуск согласования](xref:signalr/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="17dc4-129">The `Reconnected` event handler's `connectionId` parameter will be null if the `HubConnection` was configured to [skip negotiation](xref:signalr/configuration#configure-client-options).</span></span>

```csharp
connection.Reconnected += connectionId =>
{
    Debug.Assert(connection.State == HubConnectionState.Connected);

    // Notify users the connection was reestablished.
    // Start dequeuing messages queued while reconnecting if any.

    return Task.CompletedTask;
};
```

<span data-ttu-id="17dc4-130">`WithAutomaticReconnect()` не настраивает `HubConnection` для повторного выполнения начальных ошибок запуска, поэтому ошибки запуска должны быть обработаны вручную:</span><span class="sxs-lookup"><span data-stu-id="17dc4-130">`WithAutomaticReconnect()` won't configure the `HubConnection` to retry initial start failures, so start failures need to be handled manually:</span></span>

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

<span data-ttu-id="17dc4-131">Если клиент не подключится в течение первых четырех попыток, `HubConnection` переходит в состояние `Disconnected` и запускает событие <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>.</span><span class="sxs-lookup"><span data-stu-id="17dc4-131">If the client doesn't successfully reconnect within its first four attempts, the `HubConnection` will transition to the `Disconnected` state and fire the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event.</span></span> <span data-ttu-id="17dc4-132">Это дает возможность попытаться вручную перезапустить подключение или сообщить пользователям, что подключение было безвозвратно утрачено.</span><span class="sxs-lookup"><span data-stu-id="17dc4-132">This provides an opportunity to attempt to restart the connection manually or inform users the connection has been permanently lost.</span></span>

```csharp
connection.Closed += error =>
{
    Debug.Assert(connection.State == HubConnectionState.Disconnected);

    // Notify users the connection has been closed or manually try to restart the connection.

    return Task.CompletedTask;
};
```

<span data-ttu-id="17dc4-133">Чтобы настроить настраиваемое число попыток повторного подключения перед отключением или изменением времени повторного подключения, `WithAutomaticReconnect` принимает массив чисел, представляющих задержку в миллисекундах, прежде чем начинать каждую попытку повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-133">In order to configure a custom number of reconnect attempts before disconnecting or change the reconnect timing, `WithAutomaticReconnect` accepts an array of numbers representing the delay in milliseconds to wait before starting each reconnect attempt.</span></span>

```csharp
HubConnection connection= new HubConnectionBuilder()
    .WithUrl(new Uri("http://127.0.0.1:5000/chatHub"))
    .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.Zero, TimeSpan.FromSeconds(10) })
    .Build();

    // .WithAutomaticReconnect(new[] { TimeSpan.Zero, TimeSpan.FromSeconds(2), TimeSpan.FromSeconds(10), TimeSpan.FromSeconds(30) }) yields the default behavior.
```

<span data-ttu-id="17dc4-134">В предыдущем примере выполняется настройка `HubConnection` для начала попытки повторного подключения сразу после потери соединения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-134">The preceding example configures the `HubConnection` to start attempting reconnects immediately after the connection is lost.</span></span> <span data-ttu-id="17dc4-135">Это также справедливо для конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="17dc4-135">This is also true for the default configuration.</span></span>

<span data-ttu-id="17dc4-136">Если первая попытка повторного подключения завершается неудачно, вторая попытка повторного подключения также начнется немедленно, а не подождать 2 секунды, как в конфигурации по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="17dc4-136">If the first reconnect attempt fails, the second reconnect attempt will also start immediately instead of waiting 2 seconds like it would in the default configuration.</span></span>

<span data-ttu-id="17dc4-137">Если вторая попытка повторного подключения завершается неудачно, третья попытка повторного подключения начнется через 10 секунд, что опять же, как и конфигурация по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="17dc4-137">If the second reconnect attempt fails, the third reconnect attempt will start in 10 seconds which is again like the default configuration.</span></span>

<span data-ttu-id="17dc4-138">Затем пользовательское поведение снова рассходится от поведения по умолчанию, останавливаясь после третьего сбоя попытки повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-138">The custom behavior then diverges again from the default behavior by stopping after the third reconnect attempt failure.</span></span> <span data-ttu-id="17dc4-139">В конфигурации по умолчанию будет предпринята еще одна повторная попытки подключения через 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="17dc4-139">In the default configuration there would be one more reconnect attempt in another 30 seconds.</span></span>

<span data-ttu-id="17dc4-140">Если требуется еще более контролировать время и число попыток автоматического повторного подключения, `WithAutomaticReconnect` принимает объект, реализующий интерфейс `IRetryPolicy`, который имеет один метод с именем `NextRetryDelay`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-140">If you want even more control over the timing and number of automatic reconnect attempts, `WithAutomaticReconnect` accepts an object implementing the `IRetryPolicy` interface, which has a single method named `NextRetryDelay`.</span></span>

<span data-ttu-id="17dc4-141">`NextRetryDelay` принимает один аргумент с типом `RetryContext`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-141">`NextRetryDelay` takes a single argument with the type `RetryContext`.</span></span> <span data-ttu-id="17dc4-142">`RetryContext` имеет три свойства: `PreviousRetryCount`, `ElapsedTime` и `RetryReason`, которые являются `long`, `TimeSpan` и `Exception` соответственно.</span><span class="sxs-lookup"><span data-stu-id="17dc4-142">The `RetryContext` has three properties: `PreviousRetryCount`, `ElapsedTime` and `RetryReason` which are a `long`, a `TimeSpan` and an `Exception` respectively.</span></span> <span data-ttu-id="17dc4-143">Перед первой попыткой повторного подключения оба `PreviousRetryCount` и `ElapsedTime` будут равны нулю, а `RetryReason` будет исключением, вызвавшим потерю соединения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-143">Before the first reconnect attempt, both `PreviousRetryCount` and `ElapsedTime` will be zero, and the `RetryReason` will be the Exception that caused the connection to be lost.</span></span> <span data-ttu-id="17dc4-144">После каждой неудачной попытки повторного выполнения `PreviousRetryCount` будет увеличиваться на единицу, `ElapsedTime` будет обновлено с учетом времени, затраченного на повторное подключение, а `RetryReason` будет исключением, которое привело к сбою последней попытки повторного подключения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-144">After each failed retry attempt, `PreviousRetryCount` will be incremented by one, `ElapsedTime` will be updated to reflect the amount of time spent reconnecting so far, and the `RetryReason` will be the Exception that caused the last reconnect attempt to fail.</span></span>

<span data-ttu-id="17dc4-145">`NextRetryDelay` должен возвращать интервал времени, представляющий время ожидания перед следующей попыткой повторного подключения, или `null`, если `HubConnection` должен переустановить подключение.</span><span class="sxs-lookup"><span data-stu-id="17dc4-145">`NextRetryDelay` must return either a TimeSpan representing the time to wait before the next reconnect attempt or `null` if the `HubConnection` should stop reconnecting.</span></span>

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

<span data-ttu-id="17dc4-146">Кроме того, можно написать код, который будет повторно подключаться к клиенту вручную, как показано в [подсоединении вручную](#manually-reconnect).</span><span class="sxs-lookup"><span data-stu-id="17dc4-146">Alternatively, you can write code that will reconnect your client manually as demonstrated in [Manually reconnect](#manually-reconnect).</span></span>

::: moniker-end

### <a name="manually-reconnect"></a><span data-ttu-id="17dc4-147">Повторное подключение вручную</span><span class="sxs-lookup"><span data-stu-id="17dc4-147">Manually reconnect</span></span>

::: moniker range="< aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="17dc4-148">До 3,0 клиент .NET для SignalR не выполняет автоматическое повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="17dc4-148">Prior to 3.0, the .NET client for SignalR doesn't automatically reconnect.</span></span> <span data-ttu-id="17dc4-149">Необходимо написать код, который будет повторно подключаться к клиенту вручную.</span><span class="sxs-lookup"><span data-stu-id="17dc4-149">You must write code that will reconnect your client manually.</span></span>

::: moniker-end

<span data-ttu-id="17dc4-150">Используйте событие <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> для реагирования на потерянное подключение.</span><span class="sxs-lookup"><span data-stu-id="17dc4-150">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="17dc4-151">Например, может потребоваться автоматизировать повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="17dc4-151">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="17dc4-152">Для события `Closed` требуется делегат, возвращающий `Task`, который позволяет выполнять асинхронный код без использования `async void`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-152">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="17dc4-153">Чтобы удовлетворить сигнатуру делегата в обработчике событий `Closed`, который выполняется синхронно, возвращайте `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="17dc4-153">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="17dc4-154">Основная причина поддержки асинхронности заключается в том, что вы можете перезапустить подключение.</span><span class="sxs-lookup"><span data-stu-id="17dc4-154">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="17dc4-155">Запуск соединения является асинхронным действием.</span><span class="sxs-lookup"><span data-stu-id="17dc4-155">Starting a connection is an async action.</span></span>

<span data-ttu-id="17dc4-156">В обработчике `Closed`, который перезапускает подключение, рассмотрите возможность ожидания некоторой случайной задержки, чтобы предотвратить перегрузку сервера, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="17dc4-156">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="17dc4-157">Методы концентратора вызовов от клиента</span><span class="sxs-lookup"><span data-stu-id="17dc4-157">Call hub methods from client</span></span>

<span data-ttu-id="17dc4-158">`InvokeAsync` вызывает методы в концентраторе.</span><span class="sxs-lookup"><span data-stu-id="17dc4-158">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="17dc4-159">Передайте имя метода концентратора и все аргументы, определенные в методе Hub, в `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-159">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> SignalR<span data-ttu-id="17dc4-160"> является асинхронным, поэтому при выполнении вызовов используйте `async` и `await`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-160"> is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="17dc4-161">Метод `InvokeAsync` возвращает `Task`, который завершается, когда метод сервера возвращает значение.</span><span class="sxs-lookup"><span data-stu-id="17dc4-161">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="17dc4-162">Возвращаемое значение, если таковое имеется, предоставляется в качестве результата `Task`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-162">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="17dc4-163">Все исключения, вызванные методом на сервере, вызывают сбой `Task`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-163">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="17dc4-164">Используйте синтаксис `await`, чтобы дождаться завершения метода сервера и `try...catch` синтаксис для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="17dc4-164">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="17dc4-165">Метод `SendAsync` возвращает `Task`, который завершается, когда сообщение было отправлено на сервер.</span><span class="sxs-lookup"><span data-stu-id="17dc4-165">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="17dc4-166">Возвращаемое значение не предоставляется, так как эта `Task` не ждет завершения метода сервера.</span><span class="sxs-lookup"><span data-stu-id="17dc4-166">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="17dc4-167">Любые исключения, возникающие на клиенте при отправке сообщения, вызывают сбой `Task`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-167">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="17dc4-168">Для управления ошибками отправки используйте синтаксис `await` и `try...catch`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-168">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="17dc4-169">Если вы используете службу SignalR Azure в *Бессерверном режиме*, вы не можете вызывать методы концентратора из клиента.</span><span class="sxs-lookup"><span data-stu-id="17dc4-169">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="17dc4-170">Дополнительные сведения см. в [документации по службеSignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="17dc4-170">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="17dc4-171">Вызов методов клиента из концентратора</span><span class="sxs-lookup"><span data-stu-id="17dc4-171">Call client methods from hub</span></span>

<span data-ttu-id="17dc4-172">Определите методы, вызываемые центром с помощью `connection.On` после сборки, но перед запуском соединения.</span><span class="sxs-lookup"><span data-stu-id="17dc4-172">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="17dc4-173">Приведенный выше код в `connection.On` выполняется, когда код на стороне сервера вызывает его с помощью метода `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="17dc4-173">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="17dc4-174">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="17dc4-174">Error handling and logging</span></span>

<span data-ttu-id="17dc4-175">Обрабатывайте ошибки с помощью оператора try-catch.</span><span class="sxs-lookup"><span data-stu-id="17dc4-175">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="17dc4-176">Проверьте объект `Exception`, чтобы определить необходимые действия после возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="17dc4-176">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="17dc4-177">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="17dc4-177">Additional resources</span></span>

* [<span data-ttu-id="17dc4-178">Центры</span><span class="sxs-lookup"><span data-stu-id="17dc4-178">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="17dc4-179">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="17dc4-179">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="17dc4-180">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="17dc4-180">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* <span data-ttu-id="17dc4-181">[Документация по Azure SignalR Service](/azure/azure-signalr/signalr-concept-serverless-development-config)</span><span class="sxs-lookup"><span data-stu-id="17dc4-181">[Azure SignalR Service serverless documentation](/azure/azure-signalr/signalr-concept-serverless-development-config)</span></span>
