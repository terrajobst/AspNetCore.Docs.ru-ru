---
title: Использование центров в ASP.NET Core SignalR
author: tdykstra
description: Узнайте, как использовать центры в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 11/07/2018
uid: signalr/hubs
ms.openlocfilehash: 0413d354307208726f4252f431ac59526effed08
ms.sourcegitcommit: 408921a932448f66cb46fd53c307a864f5323fe5
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2018
ms.locfileid: "51569923"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="e5407-103">Использование центров в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="e5407-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="e5407-104">По [Рейчел Аппель](https://twitter.com/rachelappel) и [Кевин Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="e5407-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="e5407-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="e5407-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="e5407-106">Что такое концентратор SignalR</span><span class="sxs-lookup"><span data-stu-id="e5407-106">What is a SignalR hub</span></span>

<span data-ttu-id="e5407-107">API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="e5407-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="e5407-108">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="e5407-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="e5407-109">В клиентском коде определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="e5407-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="e5407-110">SignalR отвечает за все за кулисами, позволяет в режиме реального времени связи клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="e5407-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="e5407-111">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="e5407-111">Configure SignalR hubs</span></span>

<span data-ttu-id="e5407-112">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="e5407-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="e5407-113">При добавлении приложения ASP.NET Core SignalR функциональные возможности, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="e5407-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="e5407-114">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="e5407-114">Create and use hubs</span></span>

<span data-ttu-id="e5407-115">Создать концентратор, объявляя класс, наследуемый от `Hub`и добавьте в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="e5407-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="e5407-116">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="e5407-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="e5407-117">Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#.</span><span class="sxs-lookup"><span data-stu-id="e5407-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="e5407-118">SignalR обрабатывает сериализации и десериализации сложных объектов и массивов в параметрах и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="e5407-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="e5407-119">Концентраторы являются временными:</span><span class="sxs-lookup"><span data-stu-id="e5407-119">Hubs are transient:</span></span>
> * <span data-ttu-id="e5407-120">Не храните состояние в свойство класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="e5407-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="e5407-121">Каждый вызов метода концентратора выполняется с использованием нового экземпляра концентратора.</span><span class="sxs-lookup"><span data-stu-id="e5407-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="e5407-122">Используйте `await` при вызове асинхронных методов, которые зависят от концентратора, остаются в активном состоянии.</span><span class="sxs-lookup"><span data-stu-id="e5407-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="e5407-123">Например, метода, такого как `Clients.All.SendAsync(...)` может завершиться ошибкой, если он вызывается без `await` и завершения метода концентратора, прежде чем `SendAsync` завершения.</span><span class="sxs-lookup"><span data-stu-id="e5407-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="e5407-124">Объект контекста</span><span class="sxs-lookup"><span data-stu-id="e5407-124">The Context object</span></span>

<span data-ttu-id="e5407-125">`Hub` Класс имеет `Context` свойство, которое содержит следующие свойства, используя сведения о подключении:</span><span class="sxs-lookup"><span data-stu-id="e5407-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="e5407-126">Свойство.</span><span class="sxs-lookup"><span data-stu-id="e5407-126">Property</span></span> | <span data-ttu-id="e5407-127">Описание</span><span class="sxs-lookup"><span data-stu-id="e5407-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="e5407-128">Получает уникальный идентификатор для подключения, назначенный SignalR.</span><span class="sxs-lookup"><span data-stu-id="e5407-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="e5407-129">Есть один идентификатор подключения для каждого подключения.</span><span class="sxs-lookup"><span data-stu-id="e5407-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="e5407-130">Получает [идентификатор пользователя](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="e5407-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="e5407-131">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="e5407-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="e5407-132">Получает `ClaimsPrincipal` связанный с текущим пользователем.</span><span class="sxs-lookup"><span data-stu-id="e5407-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="e5407-133">Получает коллекцию ключей и значений, можно использовать для совместного использования данных в рамках этого подключения.</span><span class="sxs-lookup"><span data-stu-id="e5407-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="e5407-134">Данные могут храниться в этой коллекции и будет сохраняться для подключения через вызовы методов концентратора на другой.</span><span class="sxs-lookup"><span data-stu-id="e5407-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="e5407-135">Получает коллекцию функций, доступных для соединения.</span><span class="sxs-lookup"><span data-stu-id="e5407-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="e5407-136">Сейчас этой коллекции не требуется в большинстве сценариев, поэтому он не подробно задокументированы в еще.</span><span class="sxs-lookup"><span data-stu-id="e5407-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="e5407-137">Получает `CancellationToken` , уведомляет, когда подключение будет прервано.</span><span class="sxs-lookup"><span data-stu-id="e5407-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="e5407-138">`Hub.Context` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="e5407-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="e5407-139">Метод</span><span class="sxs-lookup"><span data-stu-id="e5407-139">Method</span></span> | <span data-ttu-id="e5407-140">Описание</span><span class="sxs-lookup"><span data-stu-id="e5407-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="e5407-141">Возвращает `HttpContext` подключения или `null` Если соединение не ассоциировано с HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="e5407-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="e5407-142">Для подключений по протоколу HTTP можно использовать этот метод для получения сведений, таких как HTTP-заголовки и строки запросов.</span><span class="sxs-lookup"><span data-stu-id="e5407-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="e5407-143">Прерывает подключение.</span><span class="sxs-lookup"><span data-stu-id="e5407-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="e5407-144">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="e5407-144">The Clients object</span></span>

<span data-ttu-id="e5407-145">`Hub` Класс имеет `Clients` свойство, которое содержит следующие свойства для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="e5407-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="e5407-146">Свойство.</span><span class="sxs-lookup"><span data-stu-id="e5407-146">Property</span></span> | <span data-ttu-id="e5407-147">Описание</span><span class="sxs-lookup"><span data-stu-id="e5407-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="e5407-148">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="e5407-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="e5407-149">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="e5407-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="e5407-150">Вызывает метод на все подключенные клиенты, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="e5407-150">Calls a method on all connected clients except the client that invoked the method</span></span> |


<span data-ttu-id="e5407-151">`Hub.Clients` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="e5407-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="e5407-152">Метод</span><span class="sxs-lookup"><span data-stu-id="e5407-152">Method</span></span> | <span data-ttu-id="e5407-153">Описание</span><span class="sxs-lookup"><span data-stu-id="e5407-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="e5407-154">Вызывает метод на все подключенные клиенты, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="e5407-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="e5407-155">Вызывает метод для определенного подключенного клиента</span><span class="sxs-lookup"><span data-stu-id="e5407-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="e5407-156">Вызывает метод для конкретных подключенных клиентов</span><span class="sxs-lookup"><span data-stu-id="e5407-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="e5407-157">Вызывает метод для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="e5407-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="e5407-158">Вызывает метод для всех подключений, в состав указанной группы, за исключением указанным подключениям</span><span class="sxs-lookup"><span data-stu-id="e5407-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="e5407-159">Вызывает метод на несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="e5407-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="e5407-160">Вызывает метод группы соединений, за исключением клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="e5407-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="e5407-161">Вызывает метод для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="e5407-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="e5407-162">Вызывает метод для всех подключений, связанных с определенными пользователями</span><span class="sxs-lookup"><span data-stu-id="e5407-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="e5407-163">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="e5407-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="e5407-164">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="e5407-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="e5407-165">Отправить сообщения клиентам</span><span class="sxs-lookup"><span data-stu-id="e5407-165">Send messages to clients</span></span>

<span data-ttu-id="e5407-166">Чтобы выполнять вызовы для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="e5407-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="e5407-167">В следующем примере существует три метода концентратора:</span><span class="sxs-lookup"><span data-stu-id="e5407-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="e5407-168">`SendMessage` отправляет сообщение всем подключенным клиентам, с помощью `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="e5407-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="e5407-169">`SendMessageToCaller` отправляет сообщение обратно в вызывающий объект, с помощью `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="e5407-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="e5407-170">`SendMessageToGroups` отправляет сообщение всем клиентам в `SignalR Users` группы.</span><span class="sxs-lookup"><span data-stu-id="e5407-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="e5407-171">Строго типизированные концентраторов</span><span class="sxs-lookup"><span data-stu-id="e5407-171">Strongly typed hubs</span></span>

<span data-ttu-id="e5407-172">Недостаток использования `SendAsync` — что оно полагается на магической строки для указания метода клиента для вызова.</span><span class="sxs-lookup"><span data-stu-id="e5407-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="e5407-173">При этом остается открытым кодом для ошибки времени выполнения, если неправильно указано имя метода или отсутствует от клиента.</span><span class="sxs-lookup"><span data-stu-id="e5407-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="e5407-174">Альтернативой использованию `SendAsync` — для задания строго типизированных `Hub` с <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="e5407-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="e5407-175">В следующем примере `ChatHub` методы клиента были извлечены out в интерфейсе `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="e5407-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="e5407-176">Этот интерфейс можно использовать рефакторинг предыдущих `ChatHub` пример.</span><span class="sxs-lookup"><span data-stu-id="e5407-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="e5407-177">С помощью `Hub<IChatClient>` позволяет клиентских методов проверки во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="e5407-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="e5407-178">Это позволяет предотвратить проблемы, из-за применение магических строк, так как `Hub<T>` только обеспечить доступ к методам, определенным в интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="e5407-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="e5407-179">С помощью строго типизированного `Hub<T>` отключает возможность использования `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="e5407-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="e5407-180">Измените имя метода концентратора</span><span class="sxs-lookup"><span data-stu-id="e5407-180">Change the name of a hub method</span></span>

<span data-ttu-id="e5407-181">По умолчанию имя сервера центра метод является имя метода .NET.</span><span class="sxs-lookup"><span data-stu-id="e5407-181">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="e5407-182">Тем не менее, можно использовать [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) атрибут, чтобы изменить это значение по умолчанию и вручную указывать имя метода.</span><span class="sxs-lookup"><span data-stu-id="e5407-182">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="e5407-183">Клиент должен использовать это имя вместо имени метода в .NET, при вызове метода.</span><span class="sxs-lookup"><span data-stu-id="e5407-183">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="e5407-184">Обработка событий для подключения</span><span class="sxs-lookup"><span data-stu-id="e5407-184">Handle events for a connection</span></span>

<span data-ttu-id="e5407-185">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="e5407-185">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="e5407-186">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к центру, таких как добавление его в группу.</span><span class="sxs-lookup"><span data-stu-id="e5407-186">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="e5407-187">Переопределить `OnDisconnectedAsync` виртуальный метод для выполнения действий при отключении клиента.</span><span class="sxs-lookup"><span data-stu-id="e5407-187">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="e5407-188">Если клиент отключается намеренно (путем вызова `connection.stop()`, например), `exception` параметр будет иметь `null`.</span><span class="sxs-lookup"><span data-stu-id="e5407-188">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="e5407-189">Тем не менее, если клиент отключен из-за ошибки (например, сбой сети), `exception` параметр будет содержать исключение, описывающие неудачу.</span><span class="sxs-lookup"><span data-stu-id="e5407-189">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="e5407-190">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="e5407-190">Handle errors</span></span>

<span data-ttu-id="e5407-191">Исключения, возникшие в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="e5407-191">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="e5407-192">На стороне клиента JavaScript `invoke` возвращает [обещание JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="e5407-192">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="e5407-193">Когда клиент получает ошибку с обработчиком подключенных к promise с помощью `catch`, он вызывается и передан в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="e5407-193">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="e5407-194">По умолчанию если концентратор вызывает исключение, SignalR возвращает сообщения об ошибке клиенту.</span><span class="sxs-lookup"><span data-stu-id="e5407-194">By default, if your Hub throws an exception, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="e5407-195">Пример:</span><span class="sxs-lookup"><span data-stu-id="e5407-195">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="e5407-196">Непредвиденные исключения часто содержат конфиденциальные сведения, такие как имя сервера базы данных в исключение, возникающее при сбое подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="e5407-196">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="e5407-197">SignalR не предоставляет эти подробные сообщения об ошибках по умолчанию, из соображений безопасности.</span><span class="sxs-lookup"><span data-stu-id="e5407-197">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="e5407-198">См. в разделе [статья рекомендации по безопасности](xref:signalr/security#exceptions) Дополнительные сведения о том, почему подавляются сведения об исключении.</span><span class="sxs-lookup"><span data-stu-id="e5407-198">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="e5407-199">Если у вас есть исключительных условий вы *сделать* потребоваться распространить клиенту, можно использовать `HubException` класса.</span><span class="sxs-lookup"><span data-stu-id="e5407-199">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="e5407-200">Если вызывается `HubException` из своего метода концентратора, SignalR **будет** отправлять все сообщение клиенту, без изменений.</span><span class="sxs-lookup"><span data-stu-id="e5407-200">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="e5407-201">Отправляет только SignalR `Message` свойство исключения клиенту.</span><span class="sxs-lookup"><span data-stu-id="e5407-201">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="e5407-202">Трассировка стека и другими свойствами исключение не доступны клиенту.</span><span class="sxs-lookup"><span data-stu-id="e5407-202">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="e5407-203">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="e5407-203">Related resources</span></span>

* [<span data-ttu-id="e5407-204">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="e5407-204">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="e5407-205">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="e5407-205">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="e5407-206">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="e5407-206">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
