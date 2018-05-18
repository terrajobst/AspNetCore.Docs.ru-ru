---
title: Использование концентраторов в ASP.NET Core SignalR
author: rachelappel
description: Сведения об использовании концентраторы в ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/01/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 5c477dd64c4cf8b7d6da1f121a290b00f3864f45
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="c26e9-103">Использование концентраторов в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c26e9-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="c26e9-104">По [Рэйчел Аппель](https://twitter.com/rachelappel) и [Kevin Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="c26e9-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="c26e9-105">[Просмотреть или загрузить образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(как для загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c26e9-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="c26e9-106">Что такое концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="c26e9-106">What is a SignalR hub</span></span>

<span data-ttu-id="c26e9-107">Интерфейс API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="c26e9-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="c26e9-108">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="c26e9-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="c26e9-109">В коде клиента определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="c26e9-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="c26e9-110">SignalR обеспечивает все за кулисами, делает возможным обмен данными в режиме реального времени клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="c26e9-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="c26e9-111">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="c26e9-111">Configure SignalR hubs</span></span>

<span data-ttu-id="c26e9-112">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="c26e9-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="c26e9-113">При добавлении SignalR функциональность приложения ASP.NET Core, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="c26e9-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="c26e9-114">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="c26e9-114">Create and use hubs</span></span>

<span data-ttu-id="c26e9-115">Создание концентратора путем объявления класса, который наследует от `Hub`и добавить в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="c26e9-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="c26e9-116">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="c26e9-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="c26e9-117">Можно указать тип возвращаемого значения и параметры, включая сложные типы массивов и, как и в любой метод C#.</span><span class="sxs-lookup"><span data-stu-id="c26e9-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c26e9-118">SignalR обрабатывает сериализация и десериализация сложные объекты и массивы параметров и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="c26e9-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="c26e9-119">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="c26e9-119">The Clients object</span></span>

<span data-ttu-id="c26e9-120">Каждый экземпляр `Hub` класс имеет свойство с именем `Clients` , содержащий следующие члены для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="c26e9-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="c26e9-121">Свойство.</span><span class="sxs-lookup"><span data-stu-id="c26e9-121">Property</span></span> | <span data-ttu-id="c26e9-122">Описание</span><span class="sxs-lookup"><span data-stu-id="c26e9-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="c26e9-123">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="c26e9-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="c26e9-124">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="c26e9-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="c26e9-125">Вызывает метод для всех подключенных клиентов, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="c26e9-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="c26e9-126">Кроме того `Hub.Clients` содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="c26e9-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="c26e9-127">Метод</span><span class="sxs-lookup"><span data-stu-id="c26e9-127">Method</span></span> | <span data-ttu-id="c26e9-128">Описание</span><span class="sxs-lookup"><span data-stu-id="c26e9-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="c26e9-129">Вызывает метод для всех подключенных клиентов, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="c26e9-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="c26e9-130">Вызывает метод для конкретных подключенный клиент</span><span class="sxs-lookup"><span data-stu-id="c26e9-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="c26e9-131">Вызывает метод для конкретных подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="c26e9-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="c26e9-132">Отправляет сообщение для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="c26e9-132">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="c26e9-133">Отправляет сообщение для всех подключений в указанной группе, за исключением указанного соединения</span><span class="sxs-lookup"><span data-stu-id="c26e9-133">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="c26e9-134">Отправляет сообщение в несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="c26e9-134">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="c26e9-135">Отправляет сообщение в группу подключения, исключая клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="c26e9-135">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="c26e9-136">Отправляет сообщение для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="c26e9-136">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="c26e9-137">Отправляет сообщение для всех подключений, связанный с указанным пользователям</span><span class="sxs-lookup"><span data-stu-id="c26e9-137">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="c26e9-138">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="c26e9-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="c26e9-139">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="c26e9-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="c26e9-140">Отправлять сообщения для клиентов</span><span class="sxs-lookup"><span data-stu-id="c26e9-140">Send messages to clients</span></span>

<span data-ttu-id="c26e9-141">Для выполнения вызовов для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="c26e9-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="c26e9-142">В следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="c26e9-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="c26e9-143">`SendMessageToGroups` Метод отправляет сообщение в группах, хранящихся в `List` с именем `groups`.</span><span class="sxs-lookup"><span data-stu-id="c26e9-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="c26e9-144">Обрабатывать события для подключения</span><span class="sxs-lookup"><span data-stu-id="c26e9-144">Handle events for a connection</span></span>

<span data-ttu-id="c26e9-145">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="c26e9-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="c26e9-146">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к концентратору, такие как добавление в группу.</span><span class="sxs-lookup"><span data-stu-id="c26e9-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="c26e9-147">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="c26e9-147">Handle errors</span></span>

<span data-ttu-id="c26e9-148">Исключения, создаваемые в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="c26e9-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="c26e9-149">На стороне клиента JavaScript `invoke` возвращает [объекта обещания JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="c26e9-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="c26e9-150">Когда клиент получает сообщение об ошибке с обработчик присоединяется к promise с помощью `catch`, его вызова, переданного в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="c26e9-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="c26e9-151">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c26e9-151">Related resources</span></span>

* [<span data-ttu-id="c26e9-152">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c26e9-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="c26e9-153">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c26e9-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c26e9-154">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="c26e9-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
