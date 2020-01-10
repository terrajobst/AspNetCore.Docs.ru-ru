---
title: Вызов служб gRPC с помощью клиента .NET
author: jamesnk
description: Узнайте, как вызывать gRPC Services с помощью клиента .NET gRPC.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 1e7887388a752fb35d00e65db210c3924c6ab192
ms.sourcegitcommit: 7dfe6cc8408ac6a4549c29ca57b0c67ec4baa8de
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/09/2020
ms.locfileid: "75829105"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="28384-103">Вызов служб gRPC с помощью клиента .NET</span><span class="sxs-lookup"><span data-stu-id="28384-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="28384-104">Клиентская библиотека .NET gRPC доступна в пакете NuGet для [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) .</span><span class="sxs-lookup"><span data-stu-id="28384-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="28384-105">В этом документе объясняется, как выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="28384-105">This document explains how to:</span></span>

* <span data-ttu-id="28384-106">Настройте клиент gRPC для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="28384-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="28384-107">Сделайте вызовы gRPC унарными, серверной потоковой передачей, потоковой передачей клиента и методами двунаправленной потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="28384-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="28384-108">Настройка клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="28384-108">Configure gRPC client</span></span>

<span data-ttu-id="28384-109">Клиенты gRPC являются конкретными типами клиентов, [создаваемыми в файлах *\*.proto*](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="28384-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="28384-110">Конкретный клиент gRPC использует методы, которые выполняют преобразование для служб gRPC в файле *\*.proto*.</span><span class="sxs-lookup"><span data-stu-id="28384-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="28384-111">Клиент gRPC создается из канала.</span><span class="sxs-lookup"><span data-stu-id="28384-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="28384-112">Начните с использования `GrpcChannel.ForAddress` для создания канала, а затем используйте канал для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="28384-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="28384-113">Канал представляет долгосрочное подключение к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="28384-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="28384-114">При создании канала он настраивается с параметрами, связанными с вызовом службы.</span><span class="sxs-lookup"><span data-stu-id="28384-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="28384-115">Например, `HttpClient`, используемые для выполнения вызовов, максимальный размер сообщения отправки и получения, и ведение журнала можно указать в `GrpcChannelOptions` и использовать с `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="28384-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="28384-116">Полный список параметров см. в разделе [Параметры конфигурации клиента](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="28384-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="28384-117">Производительность и использование канала и клиента:</span><span class="sxs-lookup"><span data-stu-id="28384-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="28384-118">Создание канала может быть дорогостоящей операцией.</span><span class="sxs-lookup"><span data-stu-id="28384-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="28384-119">Повторное использование канала для вызовов gRPC обеспечивает выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="28384-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="28384-120">Клиенты gRPC создаются с помощью каналов.</span><span class="sxs-lookup"><span data-stu-id="28384-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="28384-121">Клиенты gRPC являются легковесными объектами и не нуждаются в кэшировании или повторном использовании.</span><span class="sxs-lookup"><span data-stu-id="28384-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="28384-122">Из канала можно создать несколько клиентов gRPC, включая различные типы клиентов.</span><span class="sxs-lookup"><span data-stu-id="28384-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="28384-123">Канал и клиенты, созданные из канала, могут безопасно использоваться несколькими потоками.</span><span class="sxs-lookup"><span data-stu-id="28384-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="28384-124">Клиенты, созданные из канала, могут выполнять несколько одновременных вызовов.</span><span class="sxs-lookup"><span data-stu-id="28384-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="28384-125">`GrpcChannel.ForAddress` не единственный вариант создания клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="28384-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="28384-126">Если вы вызываете gRPC Services из приложения ASP.NET Core, рассмотрите возможность [интеграции фабрики клиента gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="28384-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="28384-127">Интеграция gRPC с `HttpClientFactory` предлагает централизованную альтернативу созданию клиентов gRPC.</span><span class="sxs-lookup"><span data-stu-id="28384-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="28384-128">Для [вызова незащищенных служб gRPC с клиентом .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="28384-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="28384-129">Сделать вызовы gRPC</span><span class="sxs-lookup"><span data-stu-id="28384-129">Make gRPC calls</span></span>

<span data-ttu-id="28384-130">Вызов gRPC инициируется путем вызова метода на клиенте.</span><span class="sxs-lookup"><span data-stu-id="28384-130">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="28384-131">Клиент gRPC будет выполнять сериализацию сообщений и направлять вызов gRPC к правильной службе.</span><span class="sxs-lookup"><span data-stu-id="28384-131">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="28384-132">gRPC имеет различные типы методов.</span><span class="sxs-lookup"><span data-stu-id="28384-132">gRPC has different types of methods.</span></span> <span data-ttu-id="28384-133">Способ использования клиента для выполнения вызова gRPC зависит от типа вызываемого метода.</span><span class="sxs-lookup"><span data-stu-id="28384-133">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="28384-134">Типы методов gRPC:</span><span class="sxs-lookup"><span data-stu-id="28384-134">The gRPC method types are:</span></span>

* <span data-ttu-id="28384-135">Унарный</span><span class="sxs-lookup"><span data-stu-id="28384-135">Unary</span></span>
* <span data-ttu-id="28384-136">Потоковая передача сервера</span><span class="sxs-lookup"><span data-stu-id="28384-136">Server streaming</span></span>
* <span data-ttu-id="28384-137">Потоковая передача клиента</span><span class="sxs-lookup"><span data-stu-id="28384-137">Client streaming</span></span>
* <span data-ttu-id="28384-138">Двунаправленная потоковая передача</span><span class="sxs-lookup"><span data-stu-id="28384-138">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="28384-139">Унарный вызов</span><span class="sxs-lookup"><span data-stu-id="28384-139">Unary call</span></span>

<span data-ttu-id="28384-140">Унарный вызов начинается с клиента, отправляющего сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="28384-140">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="28384-141">После завершения работы службы возвращается ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="28384-141">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="28384-142">Каждый метод унарной службы в файле *\*.* Typ приведет к появлению двух методов .NET для конкретного типа клиента gRPC для вызова метода: асинхронного метода и блокирующего метода.</span><span class="sxs-lookup"><span data-stu-id="28384-142">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="28384-143">Например, в `GreeterClient` существует два способа вызова `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="28384-143">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="28384-144">`GreeterClient.SayHelloAsync` — асинхронный вызов `Greeter.SayHello` службы.</span><span class="sxs-lookup"><span data-stu-id="28384-144">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="28384-145">Можно ожидать.</span><span class="sxs-lookup"><span data-stu-id="28384-145">Can be awaited.</span></span>
* <span data-ttu-id="28384-146">`GreeterClient.SayHello` — вызывает службу `Greeter.SayHello` и блокируется до завершения.</span><span class="sxs-lookup"><span data-stu-id="28384-146">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="28384-147">Не используйте в асинхронном коде.</span><span class="sxs-lookup"><span data-stu-id="28384-147">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="28384-148">Потоковый вызов сервера</span><span class="sxs-lookup"><span data-stu-id="28384-148">Server streaming call</span></span>

<span data-ttu-id="28384-149">Потоковый вызов сервера начинается с клиента, отправляющего сообщение запроса.</span><span class="sxs-lookup"><span data-stu-id="28384-149">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="28384-150">`ResponseStream.MoveNext()` считывает сообщения, переданные из службы в поток.</span><span class="sxs-lookup"><span data-stu-id="28384-150">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="28384-151">Потоковый вызов сервера завершается, когда `ResponseStream.MoveNext()` возвращает `false`.</span><span class="sxs-lookup"><span data-stu-id="28384-151">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="28384-152">Если используется C# 8 или более поздней версии, для чтения сообщений можно использовать синтаксис `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="28384-152">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="28384-153">Метод расширения `IAsyncStreamReader<T>.ReadAllAsync()` считывает все сообщения из потока ответов:</span><span class="sxs-lookup"><span data-stu-id="28384-153">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="28384-154">Вызов потоковой передачи клиента</span><span class="sxs-lookup"><span data-stu-id="28384-154">Client streaming call</span></span>

<span data-ttu-id="28384-155">Клиентский вызов потоковой передачи начинается *без* отправки сообщения клиентом.</span><span class="sxs-lookup"><span data-stu-id="28384-155">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="28384-156">Клиент может выбрать отправку сообщений с помощью `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="28384-156">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="28384-157">Когда клиент завершит отправку сообщений, `RequestStream.CompleteAsync` следует вызвать, чтобы уведомить службу.</span><span class="sxs-lookup"><span data-stu-id="28384-157">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="28384-158">Вызов завершается, когда служба возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="28384-158">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="28384-159">Двунаправленный вызов потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="28384-159">Bi-directional streaming call</span></span>

<span data-ttu-id="28384-160">Двунаправленный потоковый вызов начинается *без* отправки сообщения клиентом.</span><span class="sxs-lookup"><span data-stu-id="28384-160">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="28384-161">Клиент может выбрать отправку сообщений с помощью `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="28384-161">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="28384-162">Сообщения, переходящие из службы в поток, доступны с `ResponseStream.MoveNext()` или `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="28384-162">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="28384-163">Двусторонний двунаправленный потоковый вызов завершается, когда `ResponseStream` не содержит сообщений.</span><span class="sxs-lookup"><span data-stu-id="28384-163">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="28384-164">Во время двунаправленного потокового вызова клиент и служба могут передавать сообщения друг другу в любое время.</span><span class="sxs-lookup"><span data-stu-id="28384-164">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="28384-165">Лучшая клиентская логика для взаимодействия с двунаправленным вызовом зависит от логики службы.</span><span class="sxs-lookup"><span data-stu-id="28384-165">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="28384-166">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="28384-166">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
