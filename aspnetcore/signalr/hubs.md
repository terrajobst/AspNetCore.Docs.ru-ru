---
title: Использование центров в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать центры в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 05/01/2018
uid: signalr/hubs
ms.openlocfilehash: e583676ab0eed45aeaf6391d8cdf8c1485aa914e
ms.sourcegitcommit: e7e1e531b80b3f4117ff119caadbebf4dcf5dcb7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/12/2018
ms.locfileid: "44510341"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="a7f2e-103">Использование центров в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a7f2e-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="a7f2e-104">По [Рейчел Аппель](https://twitter.com/rachelappel) и [Кевин Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="a7f2e-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="a7f2e-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(способ загрузки)](xref:tutorials/index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="a7f2e-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:tutorials/index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="a7f2e-106">Что такое концентратор SignalR</span><span class="sxs-lookup"><span data-stu-id="a7f2e-106">What is a SignalR hub</span></span>

<span data-ttu-id="a7f2e-107">API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="a7f2e-108">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="a7f2e-109">В клиентском коде определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="a7f2e-110">SignalR отвечает за все за кулисами, позволяет в режиме реального времени связи клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="a7f2e-111">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="a7f2e-111">Configure SignalR hubs</span></span>

<span data-ttu-id="a7f2e-112">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="a7f2e-113">При добавлении приложения ASP.NET Core SignalR функциональные возможности, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="a7f2e-114">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="a7f2e-114">Create and use hubs</span></span>

<span data-ttu-id="a7f2e-115">Создать концентратор, объявляя класс, наследуемый от `Hub`и добавьте в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="a7f2e-116">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="a7f2e-117">Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="a7f2e-118">SignalR обрабатывает сериализации и десериализации сложных объектов и массивов в параметрах и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="a7f2e-119">Объект контекста</span><span class="sxs-lookup"><span data-stu-id="a7f2e-119">The Context object</span></span>

<span data-ttu-id="a7f2e-120">`Hub` Класс имеет `Context` свойство, которое содержит следующие свойства, используя сведения о подключении:</span><span class="sxs-lookup"><span data-stu-id="a7f2e-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="a7f2e-121">Свойство.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-121">Property</span></span> | <span data-ttu-id="a7f2e-122">Описание</span><span class="sxs-lookup"><span data-stu-id="a7f2e-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="a7f2e-123">Получает уникальный идентификатор для подключения, назначенный SignalR.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="a7f2e-124">Есть один идентификатор подключения для каждого подключения.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="a7f2e-125">Получает [идентификатор пользователя](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="a7f2e-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="a7f2e-126">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="a7f2e-127">Получает `ClaimsPrincipal` связанный с текущим пользователем.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="a7f2e-128">Получает коллекцию ключей и значений, можно использовать для совместного использования данных в рамках этого подключения.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="a7f2e-129">Данные могут храниться в этой коллекции и будет сохраняться для подключения через вызовы методов концентратора на другой.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="a7f2e-130">Получает коллекцию функций, доступных для соединения.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="a7f2e-131">Сейчас этой коллекции не требуется в большинстве сценариев, поэтому он не подробно задокументированы в еще.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="a7f2e-132">Получает `CancellationToken` , уведомляет, когда подключение будет прервано.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="a7f2e-133">`Hub.Context` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="a7f2e-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="a7f2e-134">Метод</span><span class="sxs-lookup"><span data-stu-id="a7f2e-134">Method</span></span> | <span data-ttu-id="a7f2e-135">Описание</span><span class="sxs-lookup"><span data-stu-id="a7f2e-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="a7f2e-136">Возвращает `HttpContext` подключения или `null` Если соединение не ассоциировано с HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="a7f2e-137">Для подключений по протоколу HTTP можно использовать этот метод для получения сведений, таких как HTTP-заголовки и строки запросов.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="a7f2e-138">Прерывает подключение.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="a7f2e-139">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="a7f2e-139">The Clients object</span></span>

<span data-ttu-id="a7f2e-140">`Hub` Класс имеет `Clients` свойство, которое содержит следующие свойства для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="a7f2e-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="a7f2e-141">Свойство.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-141">Property</span></span> | <span data-ttu-id="a7f2e-142">Описание</span><span class="sxs-lookup"><span data-stu-id="a7f2e-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="a7f2e-143">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="a7f2e-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="a7f2e-144">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="a7f2e-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="a7f2e-145">Вызывает метод на все подключенные клиенты, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="a7f2e-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="a7f2e-146">`Hub.Clients` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="a7f2e-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="a7f2e-147">Метод</span><span class="sxs-lookup"><span data-stu-id="a7f2e-147">Method</span></span> | <span data-ttu-id="a7f2e-148">Описание</span><span class="sxs-lookup"><span data-stu-id="a7f2e-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="a7f2e-149">Вызывает метод на все подключенные клиенты, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="a7f2e-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="a7f2e-150">Вызывает метод для определенного подключенного клиента</span><span class="sxs-lookup"><span data-stu-id="a7f2e-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="a7f2e-151">Вызывает метод для конкретных подключенных клиентов</span><span class="sxs-lookup"><span data-stu-id="a7f2e-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="a7f2e-152">Вызывает метод для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="a7f2e-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="a7f2e-153">Вызывает метод для всех подключений, в состав указанной группы, за исключением указанным подключениям</span><span class="sxs-lookup"><span data-stu-id="a7f2e-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="a7f2e-154">Вызывает метод в несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="a7f2e-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="a7f2e-155">Вызывает метод для группы соединений, за исключением клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="a7f2e-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="a7f2e-156">Вызывает метод для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="a7f2e-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="a7f2e-157">Вызывает метод для всех подключений, связанных с определенными пользователями</span><span class="sxs-lookup"><span data-stu-id="a7f2e-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="a7f2e-158">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="a7f2e-159">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="a7f2e-160">Отправить сообщения клиентам</span><span class="sxs-lookup"><span data-stu-id="a7f2e-160">Send messages to clients</span></span>

<span data-ttu-id="a7f2e-161">Чтобы выполнять вызовы для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="a7f2e-162">В следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="a7f2e-163">`SendMessageToGroups` Метод отправляет сообщение в группы, хранящиеся в `List` с именем `groups`.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="a7f2e-164">Обработка событий для подключения</span><span class="sxs-lookup"><span data-stu-id="a7f2e-164">Handle events for a connection</span></span>

<span data-ttu-id="a7f2e-165">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-165">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="a7f2e-166">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к центру, таких как добавление его в группу.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-166">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="a7f2e-167">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="a7f2e-167">Handle errors</span></span>

<span data-ttu-id="a7f2e-168">Исключения, возникшие в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-168">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="a7f2e-169">На стороне клиента JavaScript `invoke` возвращает [обещание JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="a7f2e-169">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="a7f2e-170">Когда клиент получает ошибку с обработчиком подключенных к promise с помощью `catch`, он вызывается и передан в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="a7f2e-170">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="a7f2e-171">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a7f2e-171">Related resources</span></span>

* [<span data-ttu-id="a7f2e-172">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="a7f2e-172">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="a7f2e-173">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="a7f2e-173">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="a7f2e-174">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="a7f2e-174">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
