---
title: Клиент .NET SignalR ASP.NET Core
author: bradygaster
description: Сведения о клиенте .NET, ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: 640d75157e42ffa6d78235c5be03e4846e8dcde9
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982950"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="23197-103">Клиент .NET SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23197-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="23197-104">Клиентская библиотека ASP.NET Core SignalR .NET позволяет взаимодействовать с концентраторами SignalR из приложений .NET.</span><span class="sxs-lookup"><span data-stu-id="23197-104">The ASP.NET Core SignalR .NET client library lets you communicate with SignalR hubs from .NET apps.</span></span>

> [!NOTE]
> <span data-ttu-id="23197-105">Xamarin имеет особые требования для версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="23197-105">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="23197-106">Дополнительные сведения см. в разделе [клиентом SignalR 2.1.1 в Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="23197-106">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="23197-107">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="23197-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="23197-108">В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="23197-108">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="23197-109">Установка пакета клиента SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="23197-109">Install the SignalR .NET client package</span></span>

<span data-ttu-id="23197-110">`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="23197-110">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="23197-111">Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:</span><span class="sxs-lookup"><span data-stu-id="23197-111">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="23197-112">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="23197-112">Connect to a hub</span></span>

<span data-ttu-id="23197-113">Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`.</span><span class="sxs-lookup"><span data-stu-id="23197-113">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="23197-114">URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="23197-114">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="23197-115">Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`.</span><span class="sxs-lookup"><span data-stu-id="23197-115">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="23197-116">Установить подключение с `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="23197-116">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a><span data-ttu-id="23197-117">Дескриптор была потеряна связь</span><span class="sxs-lookup"><span data-stu-id="23197-117">Handle lost connection</span></span>

<span data-ttu-id="23197-118">Используйте <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> событий реагировать на потерю соединения.</span><span class="sxs-lookup"><span data-stu-id="23197-118">Use the <xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed> event to respond to a lost connection.</span></span> <span data-ttu-id="23197-119">Например можно автоматизировать повторное подключение.</span><span class="sxs-lookup"><span data-stu-id="23197-119">For example, you might want to automate reconnection.</span></span>

<span data-ttu-id="23197-120">`Closed` Событие требует делегат, который возвращает `Task`, что позволяет асинхронного кода для выполнения без использования `async void`.</span><span class="sxs-lookup"><span data-stu-id="23197-120">The `Closed` event requires a delegate that returns a `Task`, which allows async code to run without using `async void`.</span></span> <span data-ttu-id="23197-121">Для удовлетворения сигнатуре делегата в `Closed` обработчик событий, который выполняется синхронно, возвращают `Task.CompletedTask`:</span><span class="sxs-lookup"><span data-stu-id="23197-121">To satisfy the delegate signature in a `Closed` event handler that runs synchronously, return `Task.CompletedTask`:</span></span>

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

<span data-ttu-id="23197-122">Основная причина для поддержки асинхронных является, поэтому вы можете перезапустить подключение.</span><span class="sxs-lookup"><span data-stu-id="23197-122">The main reason for the async support is so you can restart the connection.</span></span> <span data-ttu-id="23197-123">Подключения — это действие async.</span><span class="sxs-lookup"><span data-stu-id="23197-123">Starting a connection is an async action.</span></span>

<span data-ttu-id="23197-124">В `Closed` обработчик, который перезапускает соединения, рассмотрим некоторые случайные задержки предотвратить перегрузку сервера, как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="23197-124">In a `Closed` handler that restarts the connection, consider waiting for some random delay to prevent overloading the server, as shown in the following example:</span></span>

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="23197-125">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="23197-125">Call hub methods from client</span></span>

<span data-ttu-id="23197-126">`InvokeAsync` вызывает методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="23197-126">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="23197-127">Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="23197-127">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="23197-128">SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.</span><span class="sxs-lookup"><span data-stu-id="23197-128">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

<span data-ttu-id="23197-129">`InvokeAsync` Возвращает метод `Task` которой завершается при возврате метода сервера.</span><span class="sxs-lookup"><span data-stu-id="23197-129">The `InvokeAsync` method returns a `Task` which completes when the server method returns.</span></span> <span data-ttu-id="23197-130">Возвращаемое значение, если таковой имеется, предоставляется в результате `Task`.</span><span class="sxs-lookup"><span data-stu-id="23197-130">The return value, if any, is provided as the result of the `Task`.</span></span> <span data-ttu-id="23197-131">Любые исключения, создаваемые на сервере метод создания непредвиденное `Task`.</span><span class="sxs-lookup"><span data-stu-id="23197-131">Any exceptions thrown by the method on the server produce a faulted `Task`.</span></span> <span data-ttu-id="23197-132">Используйте `await` синтаксис для ожидания завершения метода сервера и `try...catch` синтаксис для обработки ошибок.</span><span class="sxs-lookup"><span data-stu-id="23197-132">Use `await` syntax to wait for the server method to complete and `try...catch` syntax to handle errors.</span></span>

<span data-ttu-id="23197-133">`SendAsync` Возвращает метод `Task` которого завершается, когда сообщение отправлено на сервер.</span><span class="sxs-lookup"><span data-stu-id="23197-133">The `SendAsync` method returns a `Task` which completes when the message has been sent to the server.</span></span> <span data-ttu-id="23197-134">Возвращаемое значение не указано с этого момента `Task` не приходится ждать завершения работы метода сервера.</span><span class="sxs-lookup"><span data-stu-id="23197-134">No return value is provided since this `Task` doesn't wait until the server method completes.</span></span> <span data-ttu-id="23197-135">Все исключения, возникшие на стороне клиента при отправке сообщения создают непредвиденное `Task`.</span><span class="sxs-lookup"><span data-stu-id="23197-135">Any exceptions thrown on the client while sending the message produce a faulted `Task`.</span></span> <span data-ttu-id="23197-136">Используйте `await` и `try...catch` синтаксис для обработки ошибок отправки.</span><span class="sxs-lookup"><span data-stu-id="23197-136">Use `await` and `try...catch` syntax to handle send errors.</span></span>

> [!NOTE]
> <span data-ttu-id="23197-137">Если вы используете службу Azure SignalR в *бессерверной режим*, нельзя вызывать методы концентратора клиента.</span><span class="sxs-lookup"><span data-stu-id="23197-137">If you're using Azure SignalR Service in *Serverless mode*, you cannot call hub methods from a client.</span></span> <span data-ttu-id="23197-138">Дополнительные сведения см. в разделе [документации по службе SignalR](/azure/azure-signalr/signalr-concept-serverless-development-config).</span><span class="sxs-lookup"><span data-stu-id="23197-138">For more information, see the [SignalR Service documentation](/azure/azure-signalr/signalr-concept-serverless-development-config).</span></span>

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="23197-139">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="23197-139">Call client methods from hub</span></span>

<span data-ttu-id="23197-140">Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.</span><span class="sxs-lookup"><span data-stu-id="23197-140">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

<span data-ttu-id="23197-141">Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="23197-141">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="23197-142">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="23197-142">Error handling and logging</span></span>

<span data-ttu-id="23197-143">Обработка ошибок с помощью оператора try-catch.</span><span class="sxs-lookup"><span data-stu-id="23197-143">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="23197-144">Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="23197-144">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a><span data-ttu-id="23197-145">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="23197-145">Additional resources</span></span>

* [<span data-ttu-id="23197-146">Центры</span><span class="sxs-lookup"><span data-stu-id="23197-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="23197-147">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="23197-147">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="23197-148">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="23197-148">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="23197-149">SignalR бессерверной документация по службе Azure</span><span class="sxs-lookup"><span data-stu-id="23197-149">Azure SignalR Service serverless documentation</span></span>](/azure/azure-signalr/signalr-concept-serverless-development-config)
