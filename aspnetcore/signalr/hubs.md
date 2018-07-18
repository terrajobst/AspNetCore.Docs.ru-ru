---
title: Использование центров в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать центры в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: be39666373e2b099054bb71f4a7fcf17aeb9a01c
ms.sourcegitcommit: 3ca527f27c88cfc9d04688db5499e372fbc2c775
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/17/2018
ms.locfileid: "39095285"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="0171b-103">Использование центров в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0171b-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="0171b-104">По [Рейчел Аппель](https://twitter.com/rachelappel) и [Кевин Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="0171b-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="0171b-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="0171b-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="0171b-106">Что такое концентратор SignalR</span><span class="sxs-lookup"><span data-stu-id="0171b-106">What is a SignalR hub</span></span>

<span data-ttu-id="0171b-107">API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="0171b-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="0171b-108">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="0171b-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="0171b-109">В клиентском коде определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="0171b-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="0171b-110">SignalR отвечает за все за кулисами, позволяет в режиме реального времени связи клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="0171b-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="0171b-111">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="0171b-111">Configure SignalR hubs</span></span>

<span data-ttu-id="0171b-112">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="0171b-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="0171b-113">При добавлении приложения ASP.NET Core SignalR функциональные возможности, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="0171b-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="0171b-114">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="0171b-114">Create and use hubs</span></span>

<span data-ttu-id="0171b-115">Создать концентратор, объявляя класс, наследуемый от `Hub`и добавьте в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="0171b-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="0171b-116">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="0171b-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="0171b-117">Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#.</span><span class="sxs-lookup"><span data-stu-id="0171b-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="0171b-118">SignalR обрабатывает сериализации и десериализации сложных объектов и массивов в параметрах и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="0171b-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-clients-object"></a><span data-ttu-id="0171b-119">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="0171b-119">The Clients object</span></span>

<span data-ttu-id="0171b-120">Каждый экземпляр `Hub` класс имеет свойство с именем `Clients` , содержащий следующие элементы для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="0171b-120">Each instance of the `Hub` class has a property named `Clients` that contains the following members for communication between server and client:</span></span>

| <span data-ttu-id="0171b-121">Свойство.</span><span class="sxs-lookup"><span data-stu-id="0171b-121">Property</span></span> | <span data-ttu-id="0171b-122">Описание:</span><span class="sxs-lookup"><span data-stu-id="0171b-122">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="0171b-123">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="0171b-123">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="0171b-124">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="0171b-124">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="0171b-125">Вызывает метод на все подключенные клиенты, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="0171b-125">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="0171b-126">Кроме того `Hub.Clients` содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="0171b-126">Additionally, `Hub.Clients` contains the following methods:</span></span>

| <span data-ttu-id="0171b-127">Метод</span><span class="sxs-lookup"><span data-stu-id="0171b-127">Method</span></span> | <span data-ttu-id="0171b-128">Описание:</span><span class="sxs-lookup"><span data-stu-id="0171b-128">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="0171b-129">Вызывает метод на все подключенные клиенты, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="0171b-129">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="0171b-130">Вызывает метод для определенного подключенного клиента</span><span class="sxs-lookup"><span data-stu-id="0171b-130">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="0171b-131">Вызывает метод для конкретных подключенных клиентов</span><span class="sxs-lookup"><span data-stu-id="0171b-131">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="0171b-132">Вызывает метод для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="0171b-132">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="0171b-133">Вызывает метод для всех подключений, в состав указанной группы, за исключением указанным подключениям</span><span class="sxs-lookup"><span data-stu-id="0171b-133">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="0171b-134">Вызывает метод в несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="0171b-134">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="0171b-135">Вызывает метод для группы соединений, за исключением клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="0171b-135">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="0171b-136">Вызывает метод для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="0171b-136">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="0171b-137">Вызывает метод для всех подключений, связанных с определенными пользователями</span><span class="sxs-lookup"><span data-stu-id="0171b-137">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="0171b-138">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="0171b-138">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="0171b-139">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="0171b-139">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="0171b-140">Отправить сообщения клиентам</span><span class="sxs-lookup"><span data-stu-id="0171b-140">Send messages to clients</span></span>

<span data-ttu-id="0171b-141">Чтобы выполнять вызовы для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="0171b-141">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="0171b-142">В следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="0171b-142">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="0171b-143">`SendMessageToGroups` Метод отправляет сообщение в группы, хранящиеся в `List` с именем `groups`.</span><span class="sxs-lookup"><span data-stu-id="0171b-143">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="0171b-144">Обработка событий для подключения</span><span class="sxs-lookup"><span data-stu-id="0171b-144">Handle events for a connection</span></span>

<span data-ttu-id="0171b-145">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="0171b-145">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="0171b-146">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к центру, таких как добавление его в группу.</span><span class="sxs-lookup"><span data-stu-id="0171b-146">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="0171b-147">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="0171b-147">Handle errors</span></span>

<span data-ttu-id="0171b-148">Исключения, возникшие в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="0171b-148">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="0171b-149">На стороне клиента JavaScript `invoke` возвращает [обещание JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="0171b-149">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="0171b-150">Когда клиент получает ошибку с обработчиком подключенных к promise с помощью `catch`, он вызывается и передан в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="0171b-150">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="0171b-151">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0171b-151">Related resources</span></span>

* [<span data-ttu-id="0171b-152">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0171b-152">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="0171b-153">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="0171b-153">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="0171b-154">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="0171b-154">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
