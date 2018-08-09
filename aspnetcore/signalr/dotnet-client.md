---
title: Клиент .NET SignalR ASP.NET Core
author: tdykstra
description: Сведения о клиенте .NET, ASP.NET Core SignalR
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/07/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 970888a410b2486a20f98ce77a8674f8ec357f50
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655256"
---
# <a name="aspnet-core-signalr-net-client"></a><span data-ttu-id="24f62-103">Клиент .NET SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="24f62-103">ASP.NET Core SignalR .NET Client</span></span>

<span data-ttu-id="24f62-104">Автор: [Рэйчел Аппель](http://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="24f62-104">By [Rachel Appel](http://twitter.com/rachelappel)</span></span>

<span data-ttu-id="24f62-105">Клиент ASP.NET Core SignalR .NET можно использовать в приложениях Xamarin, WPF, Windows Forms, консоли и .NET Core.</span><span class="sxs-lookup"><span data-stu-id="24f62-105">The ASP.NET Core SignalR .NET client can be used by Xamarin, WPF, Windows Forms, Console, and .NET Core apps.</span></span> <span data-ttu-id="24f62-106">Как и [клиента JavaScript](xref:signalr/javascript-client), клиент .NET позволяет получать и отправлять и получать сообщения в концентратор в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="24f62-106">Like the [JavaScript client](xref:signalr/javascript-client), the .NET client enables you to receive and send and receive messages to a hub in real time.</span></span>

> [!NOTE]
> <span data-ttu-id="24f62-107">Xamarin имеет особые требования для версии Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="24f62-107">Xamarin has special requirements for Visual Studio version.</span></span> <span data-ttu-id="24f62-108">Дополнительные сведения см. в разделе [клиентом SignalR 2.1.1 в Xamarin](https://github.com/aspnet/Announcements/issues/305).</span><span class="sxs-lookup"><span data-stu-id="24f62-108">For more information, see [SignalR Client 2.1.1 in Xamarin](https://github.com/aspnet/Announcements/issues/305).</span></span>

<span data-ttu-id="24f62-109">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="24f62-109">[View or download sample code](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="24f62-110">В образце кода в этой статье показана приложения WPF, использующее клиент ASP.NET Core SignalR .NET.</span><span class="sxs-lookup"><span data-stu-id="24f62-110">The code sample in this article is a WPF app that uses the ASP.NET Core SignalR .NET client.</span></span>

## <a name="install-the-signalr-net-client-package"></a><span data-ttu-id="24f62-111">Установка пакета клиента SignalR .NET</span><span class="sxs-lookup"><span data-stu-id="24f62-111">Install the SignalR .NET client package</span></span>

<span data-ttu-id="24f62-112">`Microsoft.AspNetCore.SignalR.Client` Пакет требуется для клиентов .NET для подключения к концентраторов SignalR.</span><span class="sxs-lookup"><span data-stu-id="24f62-112">The `Microsoft.AspNetCore.SignalR.Client` package is needed for .NET clients to connect to SignalR hubs.</span></span> <span data-ttu-id="24f62-113">Чтобы установить клиентскую библиотеку, выполните следующую команду **консоль диспетчера пакетов** окна:</span><span class="sxs-lookup"><span data-stu-id="24f62-113">To install the client library, run the following command in the **Package Manager Console** window:</span></span>

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a><span data-ttu-id="24f62-114">Подключение к концентратору</span><span class="sxs-lookup"><span data-stu-id="24f62-114">Connect to a hub</span></span>

<span data-ttu-id="24f62-115">Чтобы установить подключение, создайте `HubConnectionBuilder` и вызвать `Build`.</span><span class="sxs-lookup"><span data-stu-id="24f62-115">To establish a connection, create a `HubConnectionBuilder` and call `Build`.</span></span> <span data-ttu-id="24f62-116">URL-адрес концентратора, протокола, тип транспорта, уровень ведения журнала, заголовки и другие параметры можно настроить при создании подключения.</span><span class="sxs-lookup"><span data-stu-id="24f62-116">The hub URL, protocol, transport type, log level, headers, and other options can be configured while building a connection.</span></span> <span data-ttu-id="24f62-117">Настройте все необходимые параметры, вставляя `HubConnectionBuilder` методы в `Build`.</span><span class="sxs-lookup"><span data-stu-id="24f62-117">Configure any required options by inserting any of the `HubConnectionBuilder` methods into `Build`.</span></span> <span data-ttu-id="24f62-118">Установить подключение с `StartAsync`.</span><span class="sxs-lookup"><span data-stu-id="24f62-118">Start the connection with `StartAsync`.</span></span>

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a><span data-ttu-id="24f62-119">Вызов методов концентратора из клиента</span><span class="sxs-lookup"><span data-stu-id="24f62-119">Call hub methods from client</span></span>

<span data-ttu-id="24f62-120">`InvokeAsync` вызывает методы концентратора.</span><span class="sxs-lookup"><span data-stu-id="24f62-120">`InvokeAsync` calls methods on the hub.</span></span> <span data-ttu-id="24f62-121">Передайте имя метода концентратора и все аргументы, заданные в методе концентратора, чтобы `InvokeAsync`.</span><span class="sxs-lookup"><span data-stu-id="24f62-121">Pass the hub method name and any arguments defined in the hub method to `InvokeAsync`.</span></span> <span data-ttu-id="24f62-122">SignalR является асинхронным, поэтому используйте `async` и `await` при выполнении вызовов.</span><span class="sxs-lookup"><span data-stu-id="24f62-122">SignalR is asynchronous, so use `async` and `await` when making the calls.</span></span>

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a><span data-ttu-id="24f62-123">Вызывать методы клиента от концентратора</span><span class="sxs-lookup"><span data-stu-id="24f62-123">Call client methods from hub</span></span>

<span data-ttu-id="24f62-124">Определите методы концентратора вызовы с использованием `connection.On` после сборки, но прежде чем выполнять подключение.</span><span class="sxs-lookup"><span data-stu-id="24f62-124">Define methods the hub calls using `connection.On` after building, but before starting the connection.</span></span>

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

<span data-ttu-id="24f62-125">Предыдущий код в `connection.On` выполняется, когда серверный код вызывает ее с помощью `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="24f62-125">The preceding code in `connection.On` runs when server-side code calls it using the `SendAsync` method.</span></span>

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a><span data-ttu-id="24f62-126">Обработка ошибок и ведение журнала</span><span class="sxs-lookup"><span data-stu-id="24f62-126">Error handling and logging</span></span>

<span data-ttu-id="24f62-127">Обработка ошибок с помощью оператора try-catch.</span><span class="sxs-lookup"><span data-stu-id="24f62-127">Handle errors with a try-catch statement.</span></span> <span data-ttu-id="24f62-128">Проверьте `Exception` объектом, чтобы определить правильное действие, предпринимаемое после возникновения ошибки.</span><span class="sxs-lookup"><span data-stu-id="24f62-128">Inspect the `Exception` object to determine the proper action to take after an error occurs.</span></span>

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a><span data-ttu-id="24f62-129">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="24f62-129">Additional resources</span></span>

* [<span data-ttu-id="24f62-130">Центры</span><span class="sxs-lookup"><span data-stu-id="24f62-130">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="24f62-131">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="24f62-131">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="24f62-132">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="24f62-132">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
