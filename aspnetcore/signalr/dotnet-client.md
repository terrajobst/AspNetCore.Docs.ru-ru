---
title: Клиент .NET SignalR ASP.NET Core
author: tdykstra
description: Сведения о клиенте .NET, ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 488d2ec31ce71534eeff4b9428262cc9ca00d992
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50205956"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="15c8c-103">Клиент .NET SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15c8c-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="15c8c-104">Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с концентраторами SignalR из приложений .NET.</span><span class="sxs-lookup"><span data-stu-id="15c8c-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="15c8c-105">Xamarin имеет особые требования для версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="15c8c-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="15c8c-106">Дополнительные сведения см. в разделе [клиентом SignalR 2.1.1 в Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="15c8c-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="15c8c-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="15c8c-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="15c8c-108">В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="15c8c-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="15c8c-109">Установка пакета клиента SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="15c8c-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="15c8c-110">`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="15c8c-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="15c8c-111">Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:</span><span class="sxs-lookup"><span data-stu-id="15c8c-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="15c8c-112">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="15c8c-112">Connect to a hub</span></span>

<span data-ttu-id="15c8c-113">Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`.</span><span class="sxs-lookup"><span data-stu-id="15c8c-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="15c8c-114">URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="15c8c-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="15c8c-115">Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`.</span><span class="sxs-lookup"><span data-stu-id="15c8c-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="15c8c-116">Установить подключение с `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="15c8c-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="15c8c-117">Дескриптор была потеряна связь</span><span class="sxs-lookup"><span data-stu-id="15c8c-117">Handle lost connection</span></span>

<span data-ttu-id="15c8c-118">Используйте <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий реагировать на потерю соединения.</span><span class="sxs-lookup"><span data-stu-id="15c8c-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="15c8c-119">Например можно автоматизировать повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="15c8c-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="15c8c-120">`Closed` Событие требует делегат, который возвращает `Task`, что позволяет асинхронного кода для выполнения без использования `async void`.</span><span class="sxs-lookup"><span data-stu-id="15c8c-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="15c8c-121">Для удовлетворения сигнатуре делегата в `Closed` обработчик событий, который выполняется синхронно, возвращают `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="15c8c-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="15c8c-122">Основная причина для поддержки асинхронных является, поэтому вы можете перезапустить подключение.</span><span class="sxs-lookup"><span data-stu-id="15c8c-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="15c8c-123">Подключения — это действие async.</span><span class="sxs-lookup"><span data-stu-id="15c8c-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="15c8c-124">В `Closed` обработчик, который перезапускает соединения, рассмотрим некоторые случайные задержки предотвратить перегрузку сервера, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="15c8c-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="15c8c-125">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="15c8c-125">Call hub methods from client</span></span>

<span data-ttu-id="15c8c-126">`InvokeAsync` вызывает методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="15c8c-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="15c8c-127">Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="15c8c-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="15c8c-128">SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.</span><span class="sxs-lookup"><span data-stu-id="15c8c-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="15c8c-129">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="15c8c-129">Call client methods from hub</span></span>

<span data-ttu-id="15c8c-130">Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.</span><span class="sxs-lookup"><span data-stu-id="15c8c-130">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="15c8c-131">Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="15c8c-131">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="15c8c-132">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="15c8c-132">Error handling and logging</span></span>

<span data-ttu-id="15c8c-133">Обработка ошибок с помощью оператора try-catch.</span><span class="sxs-lookup"><span data-stu-id="15c8c-133">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="15c8c-134">Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="15c8c-134">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="15c8c-135">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="15c8c-135">Additional resources</span></span>

* [<span data-ttu-id="15c8c-136">Центры</span><span class="sxs-lookup"><span data-stu-id="15c8c-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="15c8c-137">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="15c8c-137">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="15c8c-138">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="15c8c-138">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
