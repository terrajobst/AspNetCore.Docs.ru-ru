---
title: Использование центров в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как использовать центры в ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/20/2018
uid: signalr/hubs
ms.openlocfilehash: 244ddc40e647bfcc3ca8cda2797c51bc49174822
ms.sourcegitcommit: 088e6744cd67a62f214f25146313a53949b17d35
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320151"
---
# <a name="use-hubs-in-signalr-for-aspnet-core"></a><span data-ttu-id="78b68-103">Использование центров в SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="78b68-103">Use hubs in SignalR for ASP.NET Core</span></span>

<span data-ttu-id="78b68-104">По [Рейчел Аппель](https://twitter.com/rachelappel) и [Кевин Гриффин](https://twitter.com/1kevgriff)</span><span class="sxs-lookup"><span data-stu-id="78b68-104">By [Rachel Appel](https://twitter.com/rachelappel) and [Kevin Griffin](https://twitter.com/1kevgriff)</span></span>

<span data-ttu-id="78b68-105">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="78b68-105">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/hubs/sample/ ) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="what-is-a-signalr-hub"></a><span data-ttu-id="78b68-106">Что такое концентратор SignalR</span><span class="sxs-lookup"><span data-stu-id="78b68-106">What is a SignalR hub</span></span>

<span data-ttu-id="78b68-107">API концентраторов SignalR позволяет вызывать методы для подключенных клиентов с сервера.</span><span class="sxs-lookup"><span data-stu-id="78b68-107">The SignalR Hubs API enables you to call methods on connected clients from the server.</span></span> <span data-ttu-id="78b68-108">В серверном коде определить методы, вызываемые клиентом.</span><span class="sxs-lookup"><span data-stu-id="78b68-108">In the server code, you define methods that are called by client.</span></span> <span data-ttu-id="78b68-109">В клиентском коде определить методы, вызываемые с сервера.</span><span class="sxs-lookup"><span data-stu-id="78b68-109">In the client code, you define methods that are called from the server.</span></span> <span data-ttu-id="78b68-110">SignalR отвечает за все за кулисами, позволяет в режиме реального времени связи клиент сервер и сервер клиент.</span><span class="sxs-lookup"><span data-stu-id="78b68-110">SignalR takes care of everything behind the scenes that makes real-time client-to-server and server-to-client communications possible.</span></span>

## <a name="configure-signalr-hubs"></a><span data-ttu-id="78b68-111">Настройка концентраторов SignalR</span><span class="sxs-lookup"><span data-stu-id="78b68-111">Configure SignalR hubs</span></span>

<span data-ttu-id="78b68-112">По промежуточного слоя SignalR требует некоторых служб, которые настраиваются путем вызова `services.AddSignalR`.</span><span class="sxs-lookup"><span data-stu-id="78b68-112">The SignalR middleware requires some services, which are configured by calling `services.AddSignalR`.</span></span>

[!code-csharp[Configure service](hubs/sample/startup.cs?range=38)]

<span data-ttu-id="78b68-113">При добавлении приложения ASP.NET Core SignalR функциональные возможности, настроить маршруты SignalR путем вызова `app.UseSignalR` в `Startup.Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="78b68-113">When adding SignalR functionality to an ASP.NET Core app, setup SignalR routes by calling `app.UseSignalR` in the `Startup.Configure` method.</span></span>

[!code-csharp[Configure routes to hubs](hubs/sample/startup.cs?range=57-60)]

## <a name="create-and-use-hubs"></a><span data-ttu-id="78b68-114">Создание и использование концентраторов</span><span class="sxs-lookup"><span data-stu-id="78b68-114">Create and use hubs</span></span>

<span data-ttu-id="78b68-115">Создать концентратор, объявляя класс, наследуемый от `Hub`и добавьте в него открытые методы.</span><span class="sxs-lookup"><span data-stu-id="78b68-115">Create a hub by declaring a class that inherits from `Hub`, and add public methods to it.</span></span> <span data-ttu-id="78b68-116">Клиенты могут вызывать методы, которые определены как `public`.</span><span class="sxs-lookup"><span data-stu-id="78b68-116">Clients can call methods that are defined as `public`.</span></span>

```csharp
public class ChatHub : Hub
{
    public Task SendMessage(string user, string message)
    {
        return Clients.All.SendAsync("ReceiveMessage", user, message);
    }
}
```

<span data-ttu-id="78b68-117">Можно указать тип возвращаемого значения и параметры, включая сложные типы и массивы, как это делается в любом методе C#.</span><span class="sxs-lookup"><span data-stu-id="78b68-117">You can specify a return type and parameters, including complex types and arrays, as you would in any C# method.</span></span> <span data-ttu-id="78b68-118">SignalR обрабатывает сериализации и десериализации сложных объектов и массивов в параметрах и возвращаемых значений.</span><span class="sxs-lookup"><span data-stu-id="78b68-118">SignalR handles the serialization and deserialization of complex objects and arrays in your parameters and return values.</span></span>

> [!NOTE]
> <span data-ttu-id="78b68-119">Концентраторы являются временными:</span><span class="sxs-lookup"><span data-stu-id="78b68-119">Hubs are transient:</span></span>
>
> * <span data-ttu-id="78b68-120">Не храните состояние в свойство класса концентратора.</span><span class="sxs-lookup"><span data-stu-id="78b68-120">Don't store state in a property on the hub class.</span></span> <span data-ttu-id="78b68-121">Каждый вызов метода концентратора выполняется с использованием нового экземпляра концентратора.</span><span class="sxs-lookup"><span data-stu-id="78b68-121">Every hub method call is executed on a new hub instance.</span></span>  
> * <span data-ttu-id="78b68-122">Используйте `await` при вызове асинхронных методов, которые зависят от концентратора, остаются в активном состоянии.</span><span class="sxs-lookup"><span data-stu-id="78b68-122">Use `await` when calling asynchronous methods that depend on the hub staying alive.</span></span> <span data-ttu-id="78b68-123">Например, метода, такого как `Clients.All.SendAsync(...)` может завершиться ошибкой, если он вызывается без `await` и завершения метода концентратора, прежде чем `SendAsync` завершения.</span><span class="sxs-lookup"><span data-stu-id="78b68-123">For example, a method such as `Clients.All.SendAsync(...)` can fail if it's called without `await` and the hub method completes before `SendAsync` finishes.</span></span>

## <a name="the-context-object"></a><span data-ttu-id="78b68-124">Объект контекста</span><span class="sxs-lookup"><span data-stu-id="78b68-124">The Context object</span></span>

<span data-ttu-id="78b68-125">`Hub` Класс имеет `Context` свойство, которое содержит следующие свойства, используя сведения о подключении:</span><span class="sxs-lookup"><span data-stu-id="78b68-125">The `Hub` class has a `Context` property that contains the following properties with information about the connection:</span></span>

| <span data-ttu-id="78b68-126">Свойство.</span><span class="sxs-lookup"><span data-stu-id="78b68-126">Property</span></span> | <span data-ttu-id="78b68-127">Описание</span><span class="sxs-lookup"><span data-stu-id="78b68-127">Description</span></span> |
| ------ | ----------- |
| `ConnectionId` | <span data-ttu-id="78b68-128">Получает уникальный идентификатор для подключения, назначенный SignalR.</span><span class="sxs-lookup"><span data-stu-id="78b68-128">Gets the unique ID for the connection, assigned by SignalR.</span></span> <span data-ttu-id="78b68-129">Есть один идентификатор подключения для каждого подключения.</span><span class="sxs-lookup"><span data-stu-id="78b68-129">There is one connection ID for each connection.</span></span>|
| `UserIdentifier` | <span data-ttu-id="78b68-130">Получает [идентификатор пользователя](xref:signalr/groups).</span><span class="sxs-lookup"><span data-stu-id="78b68-130">Gets the [user identifier](xref:signalr/groups).</span></span> <span data-ttu-id="78b68-131">По умолчанию использует SignalR `ClaimTypes.NameIdentifier` из `ClaimsPrincipal` связан с соединением в качестве идентификатора пользователя.</span><span class="sxs-lookup"><span data-stu-id="78b68-131">By default, SignalR uses the `ClaimTypes.NameIdentifier` from the `ClaimsPrincipal` associated with the connection as the user identifier.</span></span> |
| `User` | <span data-ttu-id="78b68-132">Получает `ClaimsPrincipal` связанный с текущим пользователем.</span><span class="sxs-lookup"><span data-stu-id="78b68-132">Gets the `ClaimsPrincipal` associated with the current user.</span></span> |
| `Items` | <span data-ttu-id="78b68-133">Получает коллекцию ключей и значений, можно использовать для совместного использования данных в рамках этого подключения.</span><span class="sxs-lookup"><span data-stu-id="78b68-133">Gets a key/value collection that can be used to share data within the scope of this connection.</span></span> <span data-ttu-id="78b68-134">Данные могут храниться в этой коллекции и будет сохраняться для подключения через вызовы методов концентратора на другой.</span><span class="sxs-lookup"><span data-stu-id="78b68-134">Data can be stored in this collection and it will persist for the connection across different hub method invocations.</span></span> |
| `Features` | <span data-ttu-id="78b68-135">Получает коллекцию функций, доступных для соединения.</span><span class="sxs-lookup"><span data-stu-id="78b68-135">Gets the collection of features available on the connection.</span></span> <span data-ttu-id="78b68-136">Сейчас этой коллекции не требуется в большинстве сценариев, поэтому он не подробно задокументированы в еще.</span><span class="sxs-lookup"><span data-stu-id="78b68-136">For now, this collection isn't needed in most scenarios, so it isn't documented in detail yet.</span></span> |
| `ConnectionAborted` | <span data-ttu-id="78b68-137">Получает `CancellationToken` , уведомляет, когда подключение будет прервано.</span><span class="sxs-lookup"><span data-stu-id="78b68-137">Gets a `CancellationToken` that notifies when the connection is aborted.</span></span> |

<span data-ttu-id="78b68-138">`Hub.Context` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="78b68-138">`Hub.Context` also contains the following methods:</span></span>

| <span data-ttu-id="78b68-139">Метод</span><span class="sxs-lookup"><span data-stu-id="78b68-139">Method</span></span> | <span data-ttu-id="78b68-140">Описание:</span><span class="sxs-lookup"><span data-stu-id="78b68-140">Description</span></span> |
| ------ | ----------- |
| `GetHttpContext` | <span data-ttu-id="78b68-141">Возвращает `HttpContext` подключения или `null` Если соединение не ассоциировано с HTTP-запроса.</span><span class="sxs-lookup"><span data-stu-id="78b68-141">Returns the `HttpContext` for the connection, or `null` if the connection is not associated with an HTTP request.</span></span> <span data-ttu-id="78b68-142">Для подключений по протоколу HTTP можно использовать этот метод для получения сведений, таких как HTTP-заголовки и строки запросов.</span><span class="sxs-lookup"><span data-stu-id="78b68-142">For HTTP connections, you can use this method to get information such as HTTP headers and query strings.</span></span> |
| `Abort` | <span data-ttu-id="78b68-143">Прерывает подключение.</span><span class="sxs-lookup"><span data-stu-id="78b68-143">Aborts the connection.</span></span> |

## <a name="the-clients-object"></a><span data-ttu-id="78b68-144">Объект клиентов</span><span class="sxs-lookup"><span data-stu-id="78b68-144">The Clients object</span></span>

<span data-ttu-id="78b68-145">`Hub` Класс имеет `Clients` свойство, которое содержит следующие свойства для обмена данными между сервером и клиентом:</span><span class="sxs-lookup"><span data-stu-id="78b68-145">The `Hub` class has a `Clients` property that contains the following properties for communication between server and client:</span></span>

| <span data-ttu-id="78b68-146">Свойство</span><span class="sxs-lookup"><span data-stu-id="78b68-146">Property</span></span> | <span data-ttu-id="78b68-147">Описание:</span><span class="sxs-lookup"><span data-stu-id="78b68-147">Description</span></span> |
| ------ | ----------- |
| `All` | <span data-ttu-id="78b68-148">Вызывает метод на все подключенные клиенты</span><span class="sxs-lookup"><span data-stu-id="78b68-148">Calls a method on all connected clients</span></span> |
| `Caller` | <span data-ttu-id="78b68-149">Вызывает метод на стороне клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="78b68-149">Calls a method on the client that invoked the hub method</span></span> |
| `Others` | <span data-ttu-id="78b68-150">Вызывает метод на все подключенные клиенты, кроме клиента, который вызывает метод</span><span class="sxs-lookup"><span data-stu-id="78b68-150">Calls a method on all connected clients except the client that invoked the method</span></span> |

<span data-ttu-id="78b68-151">`Hub.Clients` также содержит следующие методы:</span><span class="sxs-lookup"><span data-stu-id="78b68-151">`Hub.Clients` also contains the following methods:</span></span>

| <span data-ttu-id="78b68-152">Метод</span><span class="sxs-lookup"><span data-stu-id="78b68-152">Method</span></span> | <span data-ttu-id="78b68-153">Описание:</span><span class="sxs-lookup"><span data-stu-id="78b68-153">Description</span></span> |
| ------ | ----------- |
| `AllExcept` | <span data-ttu-id="78b68-154">Вызывает метод на все подключенные клиенты, за исключением указанного соединений</span><span class="sxs-lookup"><span data-stu-id="78b68-154">Calls a method on all connected clients except for the specified connections</span></span> |
| `Client` | <span data-ttu-id="78b68-155">Вызывает метод для определенного подключенного клиента</span><span class="sxs-lookup"><span data-stu-id="78b68-155">Calls a method on a specific connected client</span></span> |
| `Clients` | <span data-ttu-id="78b68-156">Вызывает метод для конкретных подключенных клиентов</span><span class="sxs-lookup"><span data-stu-id="78b68-156">Calls a method on specific connected clients</span></span> |
| `Group` | <span data-ttu-id="78b68-157">Вызывает метод для всех подключений в указанной группе</span><span class="sxs-lookup"><span data-stu-id="78b68-157">Calls a method on all connections in the specified group</span></span>  |
| `GroupExcept` | <span data-ttu-id="78b68-158">Вызывает метод для всех подключений, в состав указанной группы, за исключением указанным подключениям</span><span class="sxs-lookup"><span data-stu-id="78b68-158">Calls a method on all connections in the specified group, except the specified connections</span></span> |
| `Groups` | <span data-ttu-id="78b68-159">Вызывает метод на несколько групп соединений</span><span class="sxs-lookup"><span data-stu-id="78b68-159">Calls a method on multiple groups of connections</span></span>  |
| `OthersInGroup` | <span data-ttu-id="78b68-160">Вызывает метод группы соединений, за исключением клиента, вызвавшему метод концентратора</span><span class="sxs-lookup"><span data-stu-id="78b68-160">Calls a method on a group of connections, excluding the client that invoked the hub method</span></span>  |
| `User` | <span data-ttu-id="78b68-161">Вызывает метод для всех подключений, связанных с конкретным пользователем</span><span class="sxs-lookup"><span data-stu-id="78b68-161">Calls a method on all connections associated with a specific user</span></span> |
| `Users` | <span data-ttu-id="78b68-162">Вызывает метод для всех подключений, связанных с определенными пользователями</span><span class="sxs-lookup"><span data-stu-id="78b68-162">Calls a method on all connections associated with the specified users</span></span> |

<span data-ttu-id="78b68-163">Каждого свойства или метода в таблицах выше возвращает объект с `SendAsync` метод.</span><span class="sxs-lookup"><span data-stu-id="78b68-163">Each property or method in the preceding tables returns an object with a `SendAsync` method.</span></span> <span data-ttu-id="78b68-164">`SendAsync` Метод позволяет указать имя и параметры метода для вызова клиента.</span><span class="sxs-lookup"><span data-stu-id="78b68-164">The `SendAsync` method allows you to supply the name and parameters of the client method to call.</span></span>

## <a name="send-messages-to-clients"></a><span data-ttu-id="78b68-165">Отправить сообщения клиентам</span><span class="sxs-lookup"><span data-stu-id="78b68-165">Send messages to clients</span></span>

<span data-ttu-id="78b68-166">Чтобы выполнять вызовы для конкретных клиентов, используйте свойства `Clients` объекта.</span><span class="sxs-lookup"><span data-stu-id="78b68-166">To make calls to specific clients, use the properties of the `Clients` object.</span></span> <span data-ttu-id="78b68-167">В следующем примере существует три метода концентратора:</span><span class="sxs-lookup"><span data-stu-id="78b68-167">In the following example, there are three Hub methods:</span></span>

* <span data-ttu-id="78b68-168">`SendMessage` отправляет сообщение всем подключенным клиентам, с помощью `Clients.All`.</span><span class="sxs-lookup"><span data-stu-id="78b68-168">`SendMessage` sends a message to all connected clients, using `Clients.All`.</span></span>
* <span data-ttu-id="78b68-169">`SendMessageToCaller` отправляет сообщение обратно в вызывающий объект, с помощью `Clients.Caller`.</span><span class="sxs-lookup"><span data-stu-id="78b68-169">`SendMessageToCaller` sends a message back to the caller, using `Clients.Caller`.</span></span>
* <span data-ttu-id="78b68-170">`SendMessageToGroups` отправляет сообщение всем клиентам в `SignalR Users` группы.</span><span class="sxs-lookup"><span data-stu-id="78b68-170">`SendMessageToGroups` sends a message to all clients in the `SignalR Users` group.</span></span>

[!code-csharp[Send messages](hubs/sample/hubs/chathub.cs?name=HubMethods)]

## <a name="strongly-typed-hubs"></a><span data-ttu-id="78b68-171">Строго типизированные концентраторов</span><span class="sxs-lookup"><span data-stu-id="78b68-171">Strongly typed hubs</span></span>

<span data-ttu-id="78b68-172">Недостаток использования `SendAsync` — что оно полагается на магической строки для указания метода клиента для вызова.</span><span class="sxs-lookup"><span data-stu-id="78b68-172">A drawback of using `SendAsync` is that it relies on a magic string to specify the client method to be called.</span></span> <span data-ttu-id="78b68-173">При этом остается открытым кодом для ошибки времени выполнения, если неправильно указано имя метода или отсутствует от клиента.</span><span class="sxs-lookup"><span data-stu-id="78b68-173">This leaves code open to runtime errors if the method name is misspelled or missing from the client.</span></span>

<span data-ttu-id="78b68-174">Альтернативой использованию `SendAsync` — для задания строго типизированных `Hub` с <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span><span class="sxs-lookup"><span data-stu-id="78b68-174">An alternative to using `SendAsync` is to strongly type the `Hub` with <xref:Microsoft.AspNetCore.SignalR.Hub`1>.</span></span> <span data-ttu-id="78b68-175">В следующем примере `ChatHub` методы клиента были извлечены out в интерфейсе `IChatClient`.</span><span class="sxs-lookup"><span data-stu-id="78b68-175">In the following example, the `ChatHub` client methods have been extracted out into an interface called `IChatClient`.</span></span>  

[!code-csharp[Interface for IChatClient](hubs/sample/hubs/ichatclient.cs?name=snippet_IChatClient)]

<span data-ttu-id="78b68-176">Этот интерфейс можно использовать рефакторинг предыдущих `ChatHub` пример.</span><span class="sxs-lookup"><span data-stu-id="78b68-176">This interface can be used to refactor the preceding `ChatHub` example.</span></span>

[!code-csharp[Strongly typed ChatHub](hubs/sample/hubs/StronglyTypedChatHub.cs?range=8-18,36)]

<span data-ttu-id="78b68-177">С помощью `Hub<IChatClient>` позволяет клиентских методов проверки во время компиляции.</span><span class="sxs-lookup"><span data-stu-id="78b68-177">Using `Hub<IChatClient>` enables compile-time checking of the client methods.</span></span> <span data-ttu-id="78b68-178">Это позволяет предотвратить проблемы, из-за применение магических строк, так как `Hub<T>` только обеспечить доступ к методам, определенным в интерфейсе.</span><span class="sxs-lookup"><span data-stu-id="78b68-178">This prevents issues caused by using magic strings, since `Hub<T>` can only provide access to the methods defined in the interface.</span></span>

<span data-ttu-id="78b68-179">С помощью строго типизированного `Hub<T>` отключает возможность использования `SendAsync`.</span><span class="sxs-lookup"><span data-stu-id="78b68-179">Using a strongly typed `Hub<T>` disables the ability to use `SendAsync`.</span></span> <span data-ttu-id="78b68-180">Любые методы, определенные в интерфейсе по-прежнему могут быть определены как асинхронные.</span><span class="sxs-lookup"><span data-stu-id="78b68-180">Any methods defined on the interface can still be defined as asynchronous.</span></span> <span data-ttu-id="78b68-181">На самом деле, каждый из этих методов должна возвращать `Task`.</span><span class="sxs-lookup"><span data-stu-id="78b68-181">In fact, each of these methods should return a `Task`.</span></span> <span data-ttu-id="78b68-182">Так как он является интерфейсом, не используйте `async` ключевое слово.</span><span class="sxs-lookup"><span data-stu-id="78b68-182">Since it's an interface, don't use the `async` keyword.</span></span> <span data-ttu-id="78b68-183">Пример:</span><span class="sxs-lookup"><span data-stu-id="78b68-183">For example:</span></span>

```csharp
public interface IClient
{
    Task ClientMethod();
}
```

> [!NOTE]
> <span data-ttu-id="78b68-184">`Async` Суффикс не удаляются из имени метода.</span><span class="sxs-lookup"><span data-stu-id="78b68-184">The `Async` suffix isn't stripped from the method name.</span></span> <span data-ttu-id="78b68-185">Если не использовать метод клиентов определяется с помощью `.on('MyMethodAsync')`, не следует использовать `MyMethodAsync` как имя.</span><span class="sxs-lookup"><span data-stu-id="78b68-185">Unless your client method is defined with `.on('MyMethodAsync')`, you shouldn't use `MyMethodAsync` as a name.</span></span>

## <a name="change-the-name-of-a-hub-method"></a><span data-ttu-id="78b68-186">Измените имя метода концентратора</span><span class="sxs-lookup"><span data-stu-id="78b68-186">Change the name of a hub method</span></span>

<span data-ttu-id="78b68-187">По умолчанию имя сервера центра метод является имя метода .NET.</span><span class="sxs-lookup"><span data-stu-id="78b68-187">By default, a server hub method name is the name of the .NET method.</span></span> <span data-ttu-id="78b68-188">Тем не менее, можно использовать [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) атрибут, чтобы изменить это значение по умолчанию и вручную указывать имя метода.</span><span class="sxs-lookup"><span data-stu-id="78b68-188">However, you can use the [HubMethodName](xref:Microsoft.AspNetCore.SignalR.HubMethodNameAttribute) attribute to change this default and manually specify a name for the method.</span></span> <span data-ttu-id="78b68-189">Клиент должен использовать это имя вместо имени метода в .NET, при вызове метода.</span><span class="sxs-lookup"><span data-stu-id="78b68-189">The client should use this name, instead of the .NET method name, when invoking the method.</span></span>

[!code-csharp[HubMethodName attribute](hubs/sample/hubs/chathub.cs?name=HubMethodName&highlight=1)]

## <a name="handle-events-for-a-connection"></a><span data-ttu-id="78b68-190">Обработка событий для подключения</span><span class="sxs-lookup"><span data-stu-id="78b68-190">Handle events for a connection</span></span>

<span data-ttu-id="78b68-191">Предоставляет API концентраторов SignalR `OnConnectedAsync` и `OnDisconnectedAsync` виртуальные методы для управления и отслеживания подключений.</span><span class="sxs-lookup"><span data-stu-id="78b68-191">The SignalR Hubs API provides the `OnConnectedAsync` and `OnDisconnectedAsync` virtual methods to manage and track connections.</span></span> <span data-ttu-id="78b68-192">Переопределить `OnConnectedAsync` виртуальный метод для выполнения действий, когда клиент подключается к центру, таких как добавление его в группу.</span><span class="sxs-lookup"><span data-stu-id="78b68-192">Override the `OnConnectedAsync` virtual method to perform actions when a client connects to the Hub, such as adding it to a group.</span></span>

[!code-csharp[Handle connection](hubs/sample/hubs/chathub.cs?name=OnConnectedAsync)]

<span data-ttu-id="78b68-193">Переопределить `OnDisconnectedAsync` виртуальный метод для выполнения действий при отключении клиента.</span><span class="sxs-lookup"><span data-stu-id="78b68-193">Override the `OnDisconnectedAsync` virtual method to perform actions when a client disconnects.</span></span> <span data-ttu-id="78b68-194">Если клиент отключается намеренно (путем вызова `connection.stop()`, например), `exception` параметр будет иметь `null`.</span><span class="sxs-lookup"><span data-stu-id="78b68-194">If the client disconnects intentionally (by calling `connection.stop()`, for example), the `exception` parameter will be `null`.</span></span> <span data-ttu-id="78b68-195">Тем не менее, если клиент отключен из-за ошибки (например, сбой сети), `exception` параметр будет содержать исключение, описывающие неудачу.</span><span class="sxs-lookup"><span data-stu-id="78b68-195">However, if the client is disconnected due to an error (such as a network failure), the `exception` parameter will contain an exception describing the failure.</span></span>

[!code-csharp[Handle disconnection](hubs/sample/hubs/chathub.cs?name=OnDisconnectedAsync)]

## <a name="handle-errors"></a><span data-ttu-id="78b68-196">Обработка ошибок</span><span class="sxs-lookup"><span data-stu-id="78b68-196">Handle errors</span></span>

<span data-ttu-id="78b68-197">Исключения, возникшие в методах hub отправляются клиенту, вызвавшему метод.</span><span class="sxs-lookup"><span data-stu-id="78b68-197">Exceptions thrown in your hub methods are sent to the client that invoked the method.</span></span> <span data-ttu-id="78b68-198">На стороне клиента JavaScript `invoke` возвращает [обещание JavaScript](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span><span class="sxs-lookup"><span data-stu-id="78b68-198">On the JavaScript client, the `invoke` method returns a [JavaScript Promise](https://developer.mozilla.org/docs/Web/JavaScript/Guide/Using_promises).</span></span> <span data-ttu-id="78b68-199">Когда клиент получает ошибку с обработчиком подключенных к promise с помощью `catch`, он вызывается и передан в качестве JavaScript `Error` объекта.</span><span class="sxs-lookup"><span data-stu-id="78b68-199">When the client receives an error with a handler attached to the promise using `catch`, it's invoked and passed as a JavaScript `Error` object.</span></span>

[!code-javascript[Error](hubs/sample/wwwroot/js/chat.js?range=23)]

<span data-ttu-id="78b68-200">При возникновении исключения в концентраторе, подключения не закрыты.</span><span class="sxs-lookup"><span data-stu-id="78b68-200">If your Hub throws an exception, connections aren't closed.</span></span> <span data-ttu-id="78b68-201">По умолчанию SignalR возвращает сообщения об ошибке клиенту.</span><span class="sxs-lookup"><span data-stu-id="78b68-201">By default, SignalR returns a generic error message to the client.</span></span> <span data-ttu-id="78b68-202">Пример:</span><span class="sxs-lookup"><span data-stu-id="78b68-202">For example:</span></span>

```
Microsoft.AspNetCore.SignalR.HubException: An unexpected error occurred invoking 'MethodName' on the server.
```

<span data-ttu-id="78b68-203">Непредвиденные исключения часто содержат конфиденциальные сведения, такие как имя сервера базы данных в исключение, возникающее при сбое подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="78b68-203">Unexpected exceptions often contain sensitive information, such as the name of a database server in an exception triggered when the database connection fails.</span></span> <span data-ttu-id="78b68-204">SignalR не предоставляет эти подробные сообщения об ошибках по умолчанию, из соображений безопасности.</span><span class="sxs-lookup"><span data-stu-id="78b68-204">SignalR doesn't expose these detailed error messages by default as a security measure.</span></span> <span data-ttu-id="78b68-205">См. в разделе [статья рекомендации по безопасности](xref:signalr/security#exceptions) Дополнительные сведения о том, почему подавляются сведения об исключении.</span><span class="sxs-lookup"><span data-stu-id="78b68-205">See the [Security considerations article](xref:signalr/security#exceptions) for more information on why exception details are suppressed.</span></span>

<span data-ttu-id="78b68-206">Если у вас есть исключительных условий вы *сделать* потребоваться распространить клиенту, можно использовать `HubException` класса.</span><span class="sxs-lookup"><span data-stu-id="78b68-206">If you have an exceptional condition you *do* want to propagate to the client, you can use the `HubException` class.</span></span> <span data-ttu-id="78b68-207">Если вызывается `HubException` из своего метода концентратора, SignalR **будет** отправлять все сообщение клиенту, без изменений.</span><span class="sxs-lookup"><span data-stu-id="78b68-207">If you throw a `HubException` from your hub method, SignalR **will** send the entire message to the client, unmodified.</span></span>

[!code-csharp[ThrowHubException](hubs/sample/hubs/chathub.cs?name=ThrowHubException&highlight=3)]

> [!NOTE]
> <span data-ttu-id="78b68-208">Отправляет только SignalR `Message` свойство исключения клиенту.</span><span class="sxs-lookup"><span data-stu-id="78b68-208">SignalR only sends the `Message` property of the exception to the client.</span></span> <span data-ttu-id="78b68-209">Трассировка стека и другими свойствами исключение не доступны клиенту.</span><span class="sxs-lookup"><span data-stu-id="78b68-209">The stack trace and other properties on the exception aren't available to the client.</span></span>

## <a name="related-resources"></a><span data-ttu-id="78b68-210">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="78b68-210">Related resources</span></span>

* [<span data-ttu-id="78b68-211">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="78b68-211">Intro to ASP.NET Core SignalR</span></span>](xref:signalr/introduction)
* [<span data-ttu-id="78b68-212">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="78b68-212">JavaScript client</span></span>](xref:signalr/javascript-client)
* [<span data-ttu-id="78b68-213">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="78b68-213">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
