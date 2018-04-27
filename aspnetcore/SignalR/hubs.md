---
title: Использование концентраторов в ASP.NET Core SignalR
author: rachelappel
description: Сведения об использовании концентраторы в ASP.NET Core SignalR.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/30/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/hubs
ms.openlocfilehash: 7da0c4832b1aa6a844172bf751a46b280a02f37a
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="84eb8-103">Использование концентраторов в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="84eb8-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="84eb8-104">По [Рэйчел Аппель](https://twitter.com/rachelappel) и [Kevin Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="84eb8-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="84eb8-105">Что такое концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="84eb8-105">What is a SignalR hub</span></span>

<span data-ttu-id="84eb8-106">Интерфейс API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="84eb8-106">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="84eb8-107">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="84eb8-107">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="84eb8-108">В коде клиента определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="84eb8-108">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="84eb8-109">SignalR обеспечивает все за кулисами, делает возможным обмен данными в режиме реального времени клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="84eb8-109">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="84eb8-110">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="84eb8-110">Configure SignalR hubs</span></span>

<span data-ttu-id="84eb8-111">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="84eb8-111">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=35)]

<span data-ttu-id="84eb8-112">При добавлении SignalR функциональность приложения ASP.NET Core, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="84eb8-112">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=55-58)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="84eb8-113">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="84eb8-113">Create and use hubs</span></span>

<span data-ttu-id="84eb8-114">Создание концентратора путем объявления класса, который наследует от `Hub`и добавить в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="84eb8-114">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="84eb8-115">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="84eb8-115">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/chathub.cs?range=10-13)]

<span data-ttu-id="84eb8-116">Можно указать тип возвращаемого значения и параметры, включая сложные типы массивов и, как и в любой метод C#.</span><span class="sxs-lookup"><span data-stu-id="84eb8-116">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="84eb8-117">SignalR обрабатывает сериализация и десериализация сложные объекты и массивы параметров и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="84eb8-117">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="84eb8-118">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="84eb8-118">The Clients object</span></span>

<span data-ttu-id="84eb8-119">Каждый экземпляр `Hub` класс имеет свойство с именем `Clients` , содержащий следующие члены для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="84eb8-119">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="84eb8-120">Свойство.</span><span class="sxs-lookup"><span data-stu-id="84eb8-120">Property</span></span> | <span data-ttu-id="84eb8-121">Описание</span><span class="sxs-lookup"><span data-stu-id="84eb8-121">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="84eb8-122">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="84eb8-122">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="84eb8-123">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="84eb8-123">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="84eb8-124">Вызывает метод для всех подключенных клиентов, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="84eb8-124">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="84eb8-125">Кроме того `Hub` класс содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="84eb8-125">Additionally, the `Hub` class contains the following methods:</span></span>

| <span data-ttu-id="84eb8-126">Метод</span><span class="sxs-lookup"><span data-stu-id="84eb8-126">Method</span></span> | <span data-ttu-id="84eb8-127">Описание</span><span class="sxs-lookup"><span data-stu-id="84eb8-127">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="84eb8-128">Вызывает метод для всех подключенных клиентов, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="84eb8-128">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="84eb8-129">Вызывает метод для конкретных подключенный клиент</span><span class="sxs-lookup"><span data-stu-id="84eb8-129">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="84eb8-130">Вызывает метод для конкретных подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="84eb8-130">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="84eb8-131">Отправляет сообщение для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="84eb8-131">Sends a message to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="84eb8-132">Отправляет сообщение для всех подключений в указанной группе, за исключением указанного соединения</span><span class="sxs-lookup"><span data-stu-id="84eb8-132">Sends a message to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="84eb8-133">Отправляет сообщение в несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="84eb8-133">Sends a message to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="84eb8-134">Отправляет сообщение в группу подключения, исключая клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="84eb8-134">Sends a message to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="84eb8-135">Отправляет сообщение для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="84eb8-135">Sends a message to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="84eb8-136">Отправляет сообщение для всех подключений, связанный с указанным пользователям</span><span class="sxs-lookup"><span data-stu-id="84eb8-136">Sends a message to all connections associated with the specified users</span></span> |

<span data-ttu-id="84eb8-137">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="84eb8-137">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="84eb8-138">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="84eb8-138">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="84eb8-139">Отправлять сообщения для клиентов</span><span class="sxs-lookup"><span data-stu-id="84eb8-139">Send messages to clients</span></span>

<span data-ttu-id="84eb8-140">Для выполнения вызовов для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="84eb8-140">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="84eb8-141">В следующем в следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="84eb8-141">In the following In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="84eb8-142">`SendMessageToGroups` Метод отправляет сообщение в группах, хранящихся в `List` с именем `groups`.</span><span class="sxs-lookup"><span data-stu-id="84eb8-142">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="84eb8-143">Обрабатывать события для подключения</span><span class="sxs-lookup"><span data-stu-id="84eb8-143">Handle events for a connection</span></span>

<span data-ttu-id="84eb8-144">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="84eb8-144">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="84eb8-145">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к концентратору, такие как добавление в группу.</span><span class="sxs-lookup"><span data-stu-id="84eb8-145">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/chathub.cs?range=26-30)]

## <a name="handle-errors"></a><span data-ttu-id="84eb8-146">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="84eb8-146">Handle errors</span></span>

<span data-ttu-id="84eb8-147">Исключения, создаваемые в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="84eb8-147">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="84eb8-148">На стороне клиента JavaScript `invoke` возвращает [объекта обещания JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="84eb8-148">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="84eb8-149">Когда клиент получает сообщение об ошибке с обработчик присоединяется к promise с помощью `catch`, его вызова, переданного в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="84eb8-149">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/chat.js?range=20)]
[!code-javascript[Error](hubs/sample/chat.js?range=16-18)]

## <a name="related-resources"></a><span data-ttu-id="84eb8-150">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="84eb8-150">Related resources</span></span>

[<span data-ttu-id="84eb8-151">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="84eb8-151">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)