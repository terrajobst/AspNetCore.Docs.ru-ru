---
title: Вызов служб gRPC с помощью клиента .NET
author: jamesnk
description: Узнайте, как вызывать gRPC Services с помощью клиента .NET gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 27e4b3e7307ae49bacb01a46fbc1b55b6967c7a0
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773694"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="d5fcd-103">Вызов служб gRPC с помощью клиента .NET</span><span class="sxs-lookup"><span data-stu-id="d5fcd-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="d5fcd-104">Клиентская библиотека .NET gRPC доступна в пакете NuGet для [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="d5fcd-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="d5fcd-105">В этом документе объясняется, как выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="d5fcd-105">This document explains how to:</span></span>

* <span data-ttu-id="d5fcd-106">Настройте клиент gRPC для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="d5fcd-107">Сделайте вызовы gRPC унарными, серверной потоковой передачей, потоковой передачей клиента и методами двунаправленной потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="d5fcd-108">Настройка клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="d5fcd-108">Configure gRPC client</span></span>

<span data-ttu-id="d5fcd-109">Клиенты gRPC являются конкретными типами клиентов, [созданными из файлов с  *\*.* ](xref:grpc/basics#generated-c-assets)именования.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="d5fcd-110">Конкретный клиент gRPC имеет методы, которые транслируются в службу gRPC в файле с  *\*расширением.* .</span><span class="sxs-lookup"><span data-stu-id="d5fcd-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="d5fcd-111">Клиент gRPC создается из канала.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="d5fcd-112">Начните с использования `GrpcChannel.ForAddress` для создания канала, а затем используйте канал для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="d5fcd-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="d5fcd-113">Канал представляет долгосрочное подключение к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="d5fcd-114">При создании канала он настраивается с параметрами, связанными с вызовом службы.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-114">When a channel is created it is configured with options related to calling a service.</span></span> <span data-ttu-id="d5fcd-115">Например, объект `HttpClient` , используемый для выполнения вызовов, максимальный размер сообщения для отправки и получения, и ведение журнала можно указать в `GrpcChannelOptions` и использовать с `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="d5fcd-116">Полный список параметров см. в разделе [Параметры конфигурации клиента](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="d5fcd-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

<span data-ttu-id="d5fcd-117">Создание канала может быть дорогостоящей операцией и повторное использование канала для вызовов gRPC предлагает преимущества производительности.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-117">Creating a channel can be an expensive operation and reusing a channel for gRPC calls offers performance benefits.</span></span> <span data-ttu-id="d5fcd-118">С помощью канала можно создать несколько конкретных клиентов gRPC, включая различные типы клиентов.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-118">Multiple concrete gRPC clients can be created from a channel, including different types of clients.</span></span> <span data-ttu-id="d5fcd-119">Типы клиентов конкретных gRPC являются простыми объектами и могут быть созданы при необходимости.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-119">Concrete gRPC client types are lightweight objects and can be created when needed.</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="d5fcd-120">`GrpcChannel.ForAddress`не единственный вариант создания клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-120">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="d5fcd-121">При вызове gRPC Services из приложения ASP.NET Core рекомендуется [интегрировать фабрику клиента gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="d5fcd-121">If you are calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="d5fcd-122">Интеграция gRPC с `HttpClientFactory` предлагает централизованную альтернативу созданию клиентов gRPC.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-122">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="d5fcd-123">Сделать вызовы gRPC</span><span class="sxs-lookup"><span data-stu-id="d5fcd-123">Make gRPC calls</span></span>

<span data-ttu-id="d5fcd-124">Вызов gRPC инициируется путем вызова метода на клиенте.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-124">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="d5fcd-125">Клиент gRPC будет выполнять сериализацию сообщений и направлять вызов gRPC к правильной службе.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-125">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="d5fcd-126">gRPC имеет различные типы методов.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-126">gRPC has different types of methods.</span></span> <span data-ttu-id="d5fcd-127">Способ использования клиента для выполнения вызова gRPC зависит от типа вызываемого метода.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-127">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="d5fcd-128">Типы методов gRPC:</span><span class="sxs-lookup"><span data-stu-id="d5fcd-128">The gRPC method types are:</span></span>

* <span data-ttu-id="d5fcd-129">Унарный</span><span class="sxs-lookup"><span data-stu-id="d5fcd-129">Unary</span></span>
* <span data-ttu-id="d5fcd-130">Потоковая передача сервера</span><span class="sxs-lookup"><span data-stu-id="d5fcd-130">Server streaming</span></span>
* <span data-ttu-id="d5fcd-131">Потоковая передача клиента</span><span class="sxs-lookup"><span data-stu-id="d5fcd-131">Client streaming</span></span>
* <span data-ttu-id="d5fcd-132">Двунаправленная потоковая передача</span><span class="sxs-lookup"><span data-stu-id="d5fcd-132">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="d5fcd-133">Унарный вызов</span><span class="sxs-lookup"><span data-stu-id="d5fcd-133">Unary call</span></span>

<span data-ttu-id="d5fcd-134">Унарный вызов начинается с клиента, отправляющего сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-134">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="d5fcd-135">После завершения работы службы возвращается ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-135">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="d5fcd-136">Каждый метод унарной службы в  *\*именованном файле приведет* к появлению двух методов .NET для конкретного типа клиента gRPC для вызова метода: асинхронного метода и метода блокировки.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-136">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="d5fcd-137">Например, `GreeterClient` существует два способа вызова `SayHello`:</span><span class="sxs-lookup"><span data-stu-id="d5fcd-137">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="d5fcd-138">`GreeterClient.SayHelloAsync`— вызывает `Greeter.SayHello` службу асинхронно.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-138">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="d5fcd-139">Можно ожидать.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-139">Can be awaited.</span></span>
* <span data-ttu-id="d5fcd-140">`GreeterClient.SayHello`— вызывает `Greeter.SayHello` службу и блокируется до завершения.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-140">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="d5fcd-141">Не используйте в асинхронном коде.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-141">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="d5fcd-142">Потоковый вызов сервера</span><span class="sxs-lookup"><span data-stu-id="d5fcd-142">Server streaming call</span></span>

<span data-ttu-id="d5fcd-143">Потоковый вызов сервера начинается с клиента, отправляющего сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-143">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="d5fcd-144">`ResponseStream.MoveNext()`считывает сообщения, переданные из службы в поток.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-144">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="d5fcd-145">Вызов потоковой передачи сервера завершается `ResponseStream.MoveNext()` при `false`возврате.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-145">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="d5fcd-146">При использовании C# 8 или более поздней версии `await foreach` можно использовать синтаксис для чтения сообщений.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-146">If you are using C# 8 or later then the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="d5fcd-147">Метод `IAsyncStreamReader<T>.ReadAllAsync()` расширения считывает все сообщения из потока ответов:</span><span class="sxs-lookup"><span data-stu-id="d5fcd-147">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="d5fcd-148">Вызов потоковой передачи клиента</span><span class="sxs-lookup"><span data-stu-id="d5fcd-148">Client streaming call</span></span>

<span data-ttu-id="d5fcd-149">Клиентский вызов потоковой передачи начинается *без* отправки сообщения клиентом.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-149">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="d5fcd-150">Клиент может отправлять сообщения с помощью `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-150">The client can choose to send sends messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="d5fcd-151">Когда клиент завершит отправку сообщений `RequestStream.CompleteAsync` , необходимо вызвать метод, чтобы уведомить службу.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-151">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="d5fcd-152">Вызов завершается, когда служба возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-152">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="d5fcd-153">Двунаправленный вызов потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="d5fcd-153">Bi-directional streaming call</span></span>

<span data-ttu-id="d5fcd-154">Двунаправленный потоковый вызов начинается *без* отправки сообщения клиентом.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-154">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="d5fcd-155">Клиент может выбрать отправку сообщений `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-155">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="d5fcd-156">Сообщения, переходящие из службы в поток, `ResponseStream.MoveNext()` доступны `ResponseStream.ReadAllAsync()`с помощью или.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-156">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="d5fcd-157">Двусторонний двунаправленный потоковый вызов завершается, `ResponseStream` когда не содержит сообщений.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-157">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="d5fcd-158">Во время двунаправленного потокового вызова клиент и служба могут передавать сообщения друг другу в любое время.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-158">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="d5fcd-159">Лучшая клиентская логика для взаимодействия с двунаправленным вызовом зависит от логики службы.</span><span class="sxs-lookup"><span data-stu-id="d5fcd-159">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d5fcd-160">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d5fcd-160">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
