---
title: Вызов служб gRPC с помощью клиента .NET
author: jamesnk
description: Узнайте, как вызывать gRPC Services с помощью клиента .NET gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 56f79b303a8d53699e8eb6156d328c0da1259416
ms.sourcegitcommit: dc5b293e08336dc236de66ed1834f7ef78359531
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/16/2019
ms.locfileid: "71011139"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="80929-103">Вызов служб gRPC с помощью клиента .NET</span><span class="sxs-lookup"><span data-stu-id="80929-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="80929-104">Клиентская библиотека .NET gRPC доступна в пакете NuGet для [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="80929-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="80929-105">В этом документе объясняется, как выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="80929-105">This document explains how to:</span></span>

* <span data-ttu-id="80929-106">Настройте клиент gRPC для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="80929-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="80929-107">Сделайте вызовы gRPC унарными, серверной потоковой передачей, потоковой передачей клиента и методами двунаправленной потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="80929-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="80929-108">Настройка клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="80929-108">Configure gRPC client</span></span>

<span data-ttu-id="80929-109">Клиенты gRPC являются конкретными типами клиентов, [созданными из файлов с  *\*.* ](xref:grpc/basics#generated-c-assets)именования.</span><span class="sxs-lookup"><span data-stu-id="80929-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="80929-110">Конкретный клиент gRPC имеет методы, которые транслируются в службу gRPC в файле с  *\*расширением.* .</span><span class="sxs-lookup"><span data-stu-id="80929-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="80929-111">Клиент gRPC создается из канала.</span><span class="sxs-lookup"><span data-stu-id="80929-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="80929-112">Начните с использования `GrpcChannel.ForAddress` для создания канала, а затем используйте канал для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="80929-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="80929-113">Канал представляет долгосрочное подключение к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="80929-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="80929-114">При создании канала он настраивается с параметрами, связанными с вызовом службы.</span><span class="sxs-lookup"><span data-stu-id="80929-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="80929-115">Например, объект `HttpClient` , используемый для выполнения вызовов, максимальный размер сообщения для отправки и получения, и ведение журнала можно указать в `GrpcChannelOptions` и использовать с `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="80929-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="80929-116">Полный список параметров см. в разделе [Параметры конфигурации клиента](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="80929-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="80929-117">Создание канала может быть дорогостоящей операцией и повторное использование канала для вызовов gRPC предлагает преимущества производительности.</span><span class="sxs-lookup"><span data-stu-id="80929-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="80929-118">С помощью канала можно создать несколько конкретных клиентов gRPC, включая различные типы клиентов.</span><span class="sxs-lookup"><span data-stu-id="80929-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="80929-119">Типы клиентов конкретных gRPC являются простыми объектами и могут быть созданы при необходимости.</span><span class="sxs-lookup"><span data-stu-id="80929-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="80929-120">`GrpcChannel.ForAddress`не единственный вариант создания клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="80929-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="80929-121">При вызове gRPC Services из приложения ASP.NET Core рекомендуется [интегрировать фабрику клиента gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="80929-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="80929-122">Интеграция gRPC с `HttpClientFactory` предлагает централизованную альтернативу созданию клиентов gRPC.</span><span class="sxs-lookup"><span data-stu-id="80929-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="80929-123">Для [вызова незащищенных служб gRPC с клиентом .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="80929-123">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="80929-124">Сделать вызовы gRPC</span><span class="sxs-lookup"><span data-stu-id="80929-124">Make gRPC calls</span></span>

<span data-ttu-id="80929-125">Вызов gRPC инициируется путем вызова метода на клиенте.</span><span class="sxs-lookup"><span data-stu-id="80929-125">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="80929-126">Клиент gRPC будет выполнять сериализацию сообщений и направлять вызов gRPC к правильной службе.</span><span class="sxs-lookup"><span data-stu-id="80929-126">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="80929-127">gRPC имеет различные типы методов.</span><span class="sxs-lookup"><span data-stu-id="80929-127">gRPC has different types of methods.</span></span> <span data-ttu-id="80929-128">Способ использования клиента для выполнения вызова gRPC зависит от типа вызываемого метода.</span><span class="sxs-lookup"><span data-stu-id="80929-128">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="80929-129">Типы методов gRPC:</span><span class="sxs-lookup"><span data-stu-id="80929-129">The gRPC method types are:</span></span>

* <span data-ttu-id="80929-130">Унарный</span><span class="sxs-lookup"><span data-stu-id="80929-130">Unary</span></span>
* <span data-ttu-id="80929-131">Потоковая передача сервера</span><span class="sxs-lookup"><span data-stu-id="80929-131">Server streaming</span></span>
* <span data-ttu-id="80929-132">Потоковая передача клиента</span><span class="sxs-lookup"><span data-stu-id="80929-132">Client streaming</span></span>
* <span data-ttu-id="80929-133">Двунаправленная потоковая передача</span><span class="sxs-lookup"><span data-stu-id="80929-133">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="80929-134">Унарный вызов</span><span class="sxs-lookup"><span data-stu-id="80929-134">Unary call</span></span>

<span data-ttu-id="80929-135">Унарный вызов начинается с клиента, отправляющего сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="80929-135">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="80929-136">После завершения работы службы возвращается ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="80929-136">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="80929-137">Каждый метод унарной службы в  *\*именованном файле приведет* к появлению двух методов .NET для конкретного типа клиента gRPC для вызова метода: асинхронного метода и метода блокировки.</span><span class="sxs-lookup"><span data-stu-id="80929-137">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="80929-138">Например, `GreeterClient` существует два способа вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="80929-138">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="80929-139">`GreeterClient.SayHelloAsync`— вызывает `Greeter.SayHello` службу асинхронно.</span><span class="sxs-lookup"><span data-stu-id="80929-139">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="80929-140">Можно ожидать.</span><span class="sxs-lookup"><span data-stu-id="80929-140">Can be awaited.</span></span>
* <span data-ttu-id="80929-141">`GreeterClient.SayHello`— вызывает `Greeter.SayHello` службу и блокируется до завершения.</span><span class="sxs-lookup"><span data-stu-id="80929-141">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="80929-142">Не используйте в асинхронном коде.</span><span class="sxs-lookup"><span data-stu-id="80929-142">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="80929-143">Потоковый вызов сервера</span><span class="sxs-lookup"><span data-stu-id="80929-143">Server streaming call</span></span>

<span data-ttu-id="80929-144">Потоковый вызов сервера начинается с клиента, отправляющего сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="80929-144">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="80929-145">`ResponseStream.MoveNext()`считывает сообщения, переданные из службы в поток.</span><span class="sxs-lookup"><span data-stu-id="80929-145">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="80929-146">Вызов потоковой передачи сервера завершается `ResponseStream.MoveNext()` при `false`возврате.</span><span class="sxs-lookup"><span data-stu-id="80929-146">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    while (await call.ResponseStream.MoveNext())
    {
        Console.WriteLine("Greeting: " + call.ResponseStream.Current.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

<span data-ttu-id="80929-147">При использовании C# 8 или более поздней версии `await foreach` можно использовать синтаксис для чтения сообщений.</span><span class="sxs-lookup"><span data-stu-id="80929-147">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="80929-148">Метод `IAsyncStreamReader<T>.ReadAllAsync()` расширения считывает все сообщения из потока ответов:</span><span class="sxs-lookup"><span data-stu-id="80929-148">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
using (var call = client.SayHellos(new HelloRequest { Name = "World" }))
{
    await foreach (var response in call.ResponseStream.ReadAllAsync())
    {
        Console.WriteLine("Greeting: " + response.Message);
        // "Greeting: Hello World" is written multiple times
    }
}
```

### <a name="client-streaming-call"></a><span data-ttu-id="80929-149">Вызов потоковой передачи клиента</span><span class="sxs-lookup"><span data-stu-id="80929-149">Client streaming call</span></span>

<span data-ttu-id="80929-150">Клиентский вызов потоковой передачи начинается *без* отправки сообщения клиентом.</span><span class="sxs-lookup"><span data-stu-id="80929-150">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="80929-151">Клиент может отправлять сообщения с помощью `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="80929-151">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="80929-152">Когда клиент завершит отправку сообщений `RequestStream.CompleteAsync` , необходимо вызвать метод, чтобы уведомить службу.</span><span class="sxs-lookup"><span data-stu-id="80929-152">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="80929-153">Вызов завершается, когда служба возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="80929-153">The call is finished when the service returns a response message.</span></span>

```csharp
var client = new Counter.CounterClient(channel);
using (var call = client.AccumulateCount())
{
    for (var i = 0; i < 3; i++)
    {
        await call.RequestStream.WriteAsync(new CounterRequest { Count = 1 });
    }
    await call.RequestStream.CompleteAsync();

    var response = await call;
    Console.WriteLine($"Count: {response.Count}");
    // Count: 3
}
```

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="80929-154">Двунаправленный вызов потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="80929-154">Bi-directional streaming call</span></span>

<span data-ttu-id="80929-155">Двунаправленный потоковый вызов начинается *без* отправки сообщения клиентом.</span><span class="sxs-lookup"><span data-stu-id="80929-155">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="80929-156">Клиент может выбрать отправку сообщений `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="80929-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="80929-157">Сообщения, переходящие из службы в поток, `ResponseStream.MoveNext()` доступны `ResponseStream.ReadAllAsync()`с помощью или.</span><span class="sxs-lookup"><span data-stu-id="80929-157">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="80929-158">Двусторонний двунаправленный потоковый вызов завершается, `ResponseStream` когда не содержит сообщений.</span><span class="sxs-lookup"><span data-stu-id="80929-158">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

```csharp
using (var call = client.Echo())
{
    Console.WriteLine("Starting background task to receive messages");
    var readTask = Task.Run(async () =>
    {
        await foreach (var response in call.ResponseStream.ReadAllAsync())
        {
            Console.WriteLine(response.Message);
            // Echo messages sent to the service
        }
    });

    Console.WriteLine("Starting to send messages");
    Console.WriteLine("Type a message to echo then press enter.");
    while (true)
    {
        var result = Console.ReadLine();
        if (string.IsNullOrEmpty(result))
        {
            break;
        }

        await call.RequestStream.WriteAsync(new EchoMessage { Message = result });
    }

    Console.WriteLine("Disconnecting");
    await call.RequestStream.CompleteAsync();
    await readTask;
}
```

<span data-ttu-id="80929-159">Во время двунаправленного потокового вызова клиент и служба могут передавать сообщения друг другу в любое время.</span><span class="sxs-lookup"><span data-stu-id="80929-159">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="80929-160">Лучшая клиентская логика для взаимодействия с двунаправленным вызовом зависит от логики службы.</span><span class="sxs-lookup"><span data-stu-id="80929-160">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="80929-161">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="80929-161">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
