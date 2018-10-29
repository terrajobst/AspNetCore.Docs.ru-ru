---
title: Использование центров в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать центры в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/12/2018
uid: signalr/hubs
ms.openlocfilehash: be42314afad4ff43d2fcf1abbc96c5b78c773977
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/29/2018
ms.locfileid: "50206020"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="c9f01-103">Использование центров в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c9f01-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="c9f01-104">По [Рейчел Аппель](https://twitter.com/rachelappel) и [Кевин Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="c9f01-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="c9f01-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="c9f01-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="c9f01-106">Что такое концентратор SignalR</span><span class="sxs-lookup"><span data-stu-id="c9f01-106">What is a SignalR hub</span></span>

<span data-ttu-id="c9f01-107">API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="c9f01-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="c9f01-108">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="c9f01-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="c9f01-109">В клиентском коде определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="c9f01-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="c9f01-110">SignalR отвечает за все за кулисами, позволяет в режиме реального времени связи клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="c9f01-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="c9f01-111">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="c9f01-111">Configure SignalR hubs</span></span>

<span data-ttu-id="c9f01-112">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="c9f01-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="c9f01-113">При добавлении приложения ASP.NET Core SignalR функциональные возможности, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="c9f01-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="c9f01-114">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="c9f01-114">Create and use hubs</span></span>

<span data-ttu-id="c9f01-115">Создать концентратор, объявляя класс, наследуемый от `Hub`и добавьте в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="c9f01-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="c9f01-116">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="c9f01-116">Clients can call methods that are defined as `public`.</span></span>

[!code-csharp[Create and use hubs](hubs/sample/hubs/chathub.cs?range=8-37)]

<span data-ttu-id="c9f01-117">Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#.</span><span class="sxs-lookup"><span data-stu-id="c9f01-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="c9f01-118">SignalR обрабатывает сериализации и десериализации сложных объектов и массивов в параметрах и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="c9f01-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="c9f01-119">Объект контекста</span><span class="sxs-lookup"><span data-stu-id="c9f01-119">The Context object</span></span>

<span data-ttu-id="c9f01-120">`Hub` Класс имеет `Context` свойство, которое содержит следующие свойства, используя сведения о подключении:</span><span class="sxs-lookup"><span data-stu-id="c9f01-120">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="c9f01-121">Свойство.</span><span class="sxs-lookup"><span data-stu-id="c9f01-121">Property</span></span> | <span data-ttu-id="c9f01-122">Описание</span><span class="sxs-lookup"><span data-stu-id="c9f01-122">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="c9f01-123">Получает уникальный идентификатор для подключения, назначенный SignalR.</span><span class="sxs-lookup"><span data-stu-id="c9f01-123">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="c9f01-124">Есть один идентификатор подключения для каждого подключения.</span><span class="sxs-lookup"><span data-stu-id="c9f01-124">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="c9f01-125">Получает [идентификатор пользователя](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="c9f01-125">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="c9f01-126">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="c9f01-126">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="c9f01-127">Получает `ClaimsPrincipal` связанный с текущим пользователем.</span><span class="sxs-lookup"><span data-stu-id="c9f01-127">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="c9f01-128">Получает коллекцию ключей и значений, можно использовать для совместного использования данных в рамках этого подключения.</span><span class="sxs-lookup"><span data-stu-id="c9f01-128">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="c9f01-129">Данные могут храниться в этой коллекции и будет сохраняться для подключения через вызовы методов концентратора на другой.</span><span class="sxs-lookup"><span data-stu-id="c9f01-129">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="c9f01-130">Получает коллекцию функций, доступных для соединения.</span><span class="sxs-lookup"><span data-stu-id="c9f01-130">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="c9f01-131">Сейчас этой коллекции не требуется в большинстве сценариев, поэтому он не подробно задокументированы в еще.</span><span class="sxs-lookup"><span data-stu-id="c9f01-131">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="c9f01-132">Получает `CancellationToken` , уведомляет, когда подключение будет прервано.</span><span class="sxs-lookup"><span data-stu-id="c9f01-132">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="c9f01-133">`Hub.Context` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="c9f01-133">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="c9f01-134">Метод</span><span class="sxs-lookup"><span data-stu-id="c9f01-134">Method</span></span> | <span data-ttu-id="c9f01-135">Описание</span><span class="sxs-lookup"><span data-stu-id="c9f01-135">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="c9f01-136">Возвращает `HttpContext` подключения или `null` Если соединение не ассоциировано с HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="c9f01-136">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="c9f01-137">Для подключений по протоколу HTTP можно использовать этот метод для получения сведений, таких как HTTP-заголовки и строки запросов.</span><span class="sxs-lookup"><span data-stu-id="c9f01-137">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="c9f01-138">Прерывает подключение.</span><span class="sxs-lookup"><span data-stu-id="c9f01-138">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="c9f01-139">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="c9f01-139">The Clients object</span></span>

<span data-ttu-id="c9f01-140">`Hub` Класс имеет `Clients` свойство, которое содержит следующие свойства для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="c9f01-140">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="c9f01-141">Свойство.</span><span class="sxs-lookup"><span data-stu-id="c9f01-141">Property</span></span> | <span data-ttu-id="c9f01-142">Описание</span><span class="sxs-lookup"><span data-stu-id="c9f01-142">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="c9f01-143">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="c9f01-143">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="c9f01-144">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="c9f01-144">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="c9f01-145">Вызывает метод на все подключенные клиенты, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="c9f01-145">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="c9f01-146">`Hub.Clients` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="c9f01-146">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="c9f01-147">Метод</span><span class="sxs-lookup"><span data-stu-id="c9f01-147">Method</span></span> | <span data-ttu-id="c9f01-148">Описание</span><span class="sxs-lookup"><span data-stu-id="c9f01-148">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="c9f01-149">Вызывает метод на все подключенные клиенты, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="c9f01-149">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="c9f01-150">Вызывает метод для определенного подключенного клиента</span><span class="sxs-lookup"><span data-stu-id="c9f01-150">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="c9f01-151">Вызывает метод для конкретных подключенных клиентов</span><span class="sxs-lookup"><span data-stu-id="c9f01-151">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="c9f01-152">Вызывает метод для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="c9f01-152">Calls a method to all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="c9f01-153">Вызывает метод для всех подключений, в состав указанной группы, за исключением указанным подключениям</span><span class="sxs-lookup"><span data-stu-id="c9f01-153">Calls a method to all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="c9f01-154">Вызывает метод в несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="c9f01-154">Calls a method to multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="c9f01-155">Вызывает метод для группы соединений, за исключением клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="c9f01-155">Calls a method to a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="c9f01-156">Вызывает метод для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="c9f01-156">Calls a method to all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="c9f01-157">Вызывает метод для всех подключений, связанных с определенными пользователями</span><span class="sxs-lookup"><span data-stu-id="c9f01-157">Calls a method to all connections associated with the specified users</span></span> |

<span data-ttu-id="c9f01-158">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="c9f01-158">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="c9f01-159">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="c9f01-159">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="c9f01-160">Отправить сообщения клиентам</span><span class="sxs-lookup"><span data-stu-id="c9f01-160">Send messages to clients</span></span>

<span data-ttu-id="c9f01-161">Чтобы выполнять вызовы для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="c9f01-161">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="c9f01-162">В следующем примере `SendMessageToCaller` метод демонстрирует отправку сообщения к подключению, вызвавшему метод концентратора.</span><span class="sxs-lookup"><span data-stu-id="c9f01-162">In the following example, the `SendMessageToCaller` method demonstrates sending a message to the connection that invoked the hub method.</span></span> <span data-ttu-id="c9f01-163">`SendMessageToGroups` Метод отправляет сообщение в группы, хранящиеся в `List` с именем `groups`.</span><span class="sxs-lookup"><span data-stu-id="c9f01-163">The `SendMessageToGroups` method sends a message to the groups stored in a `List` named `groups`.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?range=15-24)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="c9f01-164">Строго типизированные концентраторов</span><span class="sxs-lookup"><span data-stu-id="c9f01-164">Strongly typed hubs</span></span>

<span data-ttu-id="c9f01-165">Недостаток использования `SendAsync` — что оно полагается на магической строки для указания метода клиента для вызова.</span><span class="sxs-lookup"><span data-stu-id="c9f01-165">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="c9f01-166">При этом остается открытым кодом для ошибки времени выполнения, если неправильно указано имя метода или отсутствует от клиента.</span><span class="sxs-lookup"><span data-stu-id="c9f01-166">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="c9f01-167">Альтернативой использованию `SendAsync` — для задания строго типизированных `Hub` с <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="c9f01-167">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="c9f01-168">В следующем примере `ChatHub` методы клиента были извлечены out в интерфейсе `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="c9f01-168">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="c9f01-169">Этот интерфейс можно использовать рефакторинг предыдущих `ChatHub` пример.</span><span class="sxs-lookup"><span data-stu-id="c9f01-169">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="c9f01-170">С помощью `Hub<IChatClient>` позволяет клиентских методов проверки во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="c9f01-170">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="c9f01-171">Это позволяет предотвратить проблемы, из-за применение магических строк, так как `Hub<T>` только обеспечить доступ к методам, определенным в интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="c9f01-171">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="c9f01-172">С помощью строго типизированного `Hub<T>` отключает возможность использования `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="c9f01-172">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="c9f01-173">Обработка событий для подключения</span><span class="sxs-lookup"><span data-stu-id="c9f01-173">Handle events for a connection</span></span>

<span data-ttu-id="c9f01-174">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="c9f01-174">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="c9f01-175">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к центру, таких как добавление его в группу.</span><span class="sxs-lookup"><span data-stu-id="c9f01-175">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle events](hubs/sample/hubs/chathub.cs?range=26-36)]

## <a name="handle-errors"></a><span data-ttu-id="c9f01-176">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="c9f01-176">Handle errors</span></span>

<span data-ttu-id="c9f01-177">Исключения, возникшие в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="c9f01-177">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="c9f01-178">На стороне клиента JavaScript `invoke` возвращает [обещание JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="c9f01-178">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="c9f01-179">Когда клиент получает ошибку с обработчиком подключенных к promise с помощью `catch`, он вызывается и передан в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="c9f01-179">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

## <a name="related-resources"></a><span data-ttu-id="c9f01-180">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="c9f01-180">Related resources</span></span>

* [<span data-ttu-id="c9f01-181">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="c9f01-181">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="c9f01-182">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="c9f01-182">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="c9f01-183">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="c9f01-183">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
