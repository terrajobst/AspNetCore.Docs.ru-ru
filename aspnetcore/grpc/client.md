---
title: Вызов служб gRPC с помощью клиента .NET
author: jamesnk
description: Узнайте, как вызывать службы gRPC с помощью клиента gRPC .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/client
ms.openlocfilehash: 6a6a649f7194354b16f3d67160be02428cc01170
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/06/2020
ms.locfileid: "78650806"
---
# <a name="call-grpc-services-with-the-net-client"></a><span data-ttu-id="0c7cf-103">Вызов служб gRPC с помощью клиента .NET</span><span class="sxs-lookup"><span data-stu-id="0c7cf-103">Call gRPC services with the .NET client</span></span>

<span data-ttu-id="0c7cf-104">Клиентская библиотека .NET gRPC доступна в пакете NuGet [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client).</span><span class="sxs-lookup"><span data-stu-id="0c7cf-104">A .NET gRPC client library is available in the [Grpc.Net.Client](https://www.nuget.org/packages/Grpc.Net.Client) NuGet package.</span></span> <span data-ttu-id="0c7cf-105">В этом документе объясняется, как выполнять следующие задачи:</span><span class="sxs-lookup"><span data-stu-id="0c7cf-105">This document explains how to:</span></span>

* <span data-ttu-id="0c7cf-106">Настройка клиента gRPC для вызова служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-106">Configure a gRPC client to call gRPC services.</span></span>
* <span data-ttu-id="0c7cf-107">Вызовы gRPC для унарного метода, методов потоковой передачи сервера, потоковой передачи клиента и двунаправленной потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-107">Make gRPC calls to unary, server streaming, client streaming, and bi-directional streaming methods.</span></span>

## <a name="configure-grpc-client"></a><span data-ttu-id="0c7cf-108">Настройка клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="0c7cf-108">Configure gRPC client</span></span>

<span data-ttu-id="0c7cf-109">Клиенты gRPC являются конкретными типами клиентов, [создаваемыми в файлах *\*.proto*](xref:grpc/basics#generated-c-assets).</span><span class="sxs-lookup"><span data-stu-id="0c7cf-109">gRPC clients are concrete client types that are [generated from *\*.proto* files](xref:grpc/basics#generated-c-assets).</span></span> <span data-ttu-id="0c7cf-110">Конкретный клиент gRPC использует методы, которые выполняют преобразование для служб gRPC в файле *\*.proto*.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-110">The concrete gRPC client has methods that translate to the gRPC service in the *\*.proto* file.</span></span>

<span data-ttu-id="0c7cf-111">Клиент gRPC создается из канала.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-111">A gRPC client is created from a channel.</span></span> <span data-ttu-id="0c7cf-112">Для начала воспользуйтесь `GrpcChannel.ForAddress`, чтобы создать канал, а затем используйте канал для создания клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="0c7cf-112">Start by using `GrpcChannel.ForAddress` to create a channel, and then use the channel to create a gRPC client:</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

<span data-ttu-id="0c7cf-113">Канал представляет собой долгосрочное подключение к службе gRPC.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-113">A channel represents a long-lived connection to a gRPC service.</span></span> <span data-ttu-id="0c7cf-114">При создании канала он настраивается с параметрами, связанными с вызовом службы.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-114">When a channel is created, it is configured with options related to calling a service.</span></span> <span data-ttu-id="0c7cf-115">Например, `HttpClient`, используемый для выполнения вызовов, максимальный размер сообщения для отправки и получения, а также ведение журнала можно указать в `GrpcChannelOptions` и использовать с `GrpcChannel.ForAddress`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-115">For example, the `HttpClient` used to make calls, the maximum send and receive message size, and logging can be specified on `GrpcChannelOptions` and used with `GrpcChannel.ForAddress`.</span></span> <span data-ttu-id="0c7cf-116">Полный список параметров см. в разделе, [посвященном параметрам конфигурации клиента](xref:grpc/configuration#configure-client-options).</span><span class="sxs-lookup"><span data-stu-id="0c7cf-116">For a complete list of options, see [client configuration options](xref:grpc/configuration#configure-client-options).</span></span>

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

<span data-ttu-id="0c7cf-117">Производительность и использование канала и клиента:</span><span class="sxs-lookup"><span data-stu-id="0c7cf-117">Channel and client performance and usage:</span></span>

* <span data-ttu-id="0c7cf-118">Создание канала может потребовать значительных ресурсов.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-118">Creating a channel can be an expensive operation.</span></span> <span data-ttu-id="0c7cf-119">Повторное использование канала для вызовов gRPC обеспечивает выигрыш в производительности.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-119">Reusing a channel for gRPC calls provides performance benefits.</span></span>
* <span data-ttu-id="0c7cf-120">Клиенты gRPC создаются с помощью каналов.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-120">gRPC clients are created with channels.</span></span> <span data-ttu-id="0c7cf-121">Клиенты gRPC являются облегченными объектами и не нуждаются в кэшировании или повторном использовании.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-121">gRPC clients are lightweight objects and don't need to be cached or reused.</span></span>
* <span data-ttu-id="0c7cf-122">Из одного канала можно создать несколько клиентов gRPC, включая различные типы клиентов.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-122">Multiple gRPC clients can be created from a channel, including different types of clients.</span></span>
* <span data-ttu-id="0c7cf-123">Канал и клиенты, созданные из канала, могут безопасно использоваться несколькими потоками.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-123">A channel and clients created from the channel can safely be used by multiple threads.</span></span>
* <span data-ttu-id="0c7cf-124">Клиенты, созданные из канала, могут выполнять несколько одновременных вызовов.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-124">Clients created from the channel can make multiple simultaneous calls.</span></span>

<span data-ttu-id="0c7cf-125">`GrpcChannel.ForAddress` — не единственный вариант создания клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-125">`GrpcChannel.ForAddress` isn't the only option for creating a gRPC client.</span></span> <span data-ttu-id="0c7cf-126">Если вы вызываете службы gRPC из приложения ASP.NET Core, рассмотрите возможность [интеграции фабрики клиента gRPC](xref:grpc/clientfactory).</span><span class="sxs-lookup"><span data-stu-id="0c7cf-126">If you're calling gRPC services from an ASP.NET Core app, consider [gRPC client factory integration](xref:grpc/clientfactory).</span></span> <span data-ttu-id="0c7cf-127">Интеграция gRPC с `HttpClientFactory` предлагает централизованную альтернативу созданию клиентов gRPC.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-127">gRPC integration with `HttpClientFactory` offers a centralized alternative to creating gRPC clients.</span></span>

> [!NOTE]
> <span data-ttu-id="0c7cf-128">Для [вызова незащищенных служб gRPC с клиентом .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client) требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-128">Additional configuration is required to [call insecure gRPC services with the .NET client](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client).</span></span>

> [!NOTE]
> <span data-ttu-id="0c7cf-129">Вызов gRPC через HTTP/2 с `Grpc.Net.Client` в настоящее время не поддерживается в Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-129">Calling gRPC over HTTP/2 with `Grpc.Net.Client` is currently not supported on Xamarin.</span></span> <span data-ttu-id="0c7cf-130">Мы работаем над улучшением поддержки HTTP/2 в будущих выпусках Xamarin.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-130">We are working to improve HTTP/2 support in a future Xamarin release.</span></span> <span data-ttu-id="0c7cf-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) и [gRPC-Web](xref:grpc/browser) являются приемлемыми работающими альтернативами, которые доступны на сегодняшний день.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-131">[Grpc.Core](https://www.nuget.org/packages/Grpc.Core) and [gRPC-Web](xref:grpc/browser) are viable alternatives that work today.</span></span>

## <a name="make-grpc-calls"></a><span data-ttu-id="0c7cf-132">Вызовы gRPC</span><span class="sxs-lookup"><span data-stu-id="0c7cf-132">Make gRPC calls</span></span>

<span data-ttu-id="0c7cf-133">Вызов gRPC инициируется путем вызова метода в клиенте.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-133">A gRPC call is initiated by calling a method on the client.</span></span> <span data-ttu-id="0c7cf-134">Клиент gRPC будет выполнять сериализацию сообщений и направлять вызов gRPC к правильной службе.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-134">The gRPC client will handle message serialization and addressing the gRPC call to the correct service.</span></span>

<span data-ttu-id="0c7cf-135">gRPC имеет различные типы методов.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-135">gRPC has different types of methods.</span></span> <span data-ttu-id="0c7cf-136">Способ использования клиента для выполнения вызова gRPC зависит от типа вызываемого метода.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-136">How you use the client to make a gRPC call depends on the type of method you are calling.</span></span> <span data-ttu-id="0c7cf-137">Типы методов gRPC:</span><span class="sxs-lookup"><span data-stu-id="0c7cf-137">The gRPC method types are:</span></span>

* <span data-ttu-id="0c7cf-138">Унарный</span><span class="sxs-lookup"><span data-stu-id="0c7cf-138">Unary</span></span>
* <span data-ttu-id="0c7cf-139">Потоковая передача сервера</span><span class="sxs-lookup"><span data-stu-id="0c7cf-139">Server streaming</span></span>
* <span data-ttu-id="0c7cf-140">Потоковая передача клиента</span><span class="sxs-lookup"><span data-stu-id="0c7cf-140">Client streaming</span></span>
* <span data-ttu-id="0c7cf-141">Двунаправленная потоковая передача</span><span class="sxs-lookup"><span data-stu-id="0c7cf-141">Bi-directional streaming</span></span>

### <a name="unary-call"></a><span data-ttu-id="0c7cf-142">Унарный вызов</span><span class="sxs-lookup"><span data-stu-id="0c7cf-142">Unary call</span></span>

<span data-ttu-id="0c7cf-143">Унарный вызов начинается с клиента, отправляющего сообщение с запросом.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-143">A unary call starts with the client sending a request message.</span></span> <span data-ttu-id="0c7cf-144">После завершения работы службы возвращается ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-144">A response message is returned when the service finishes.</span></span>

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

<span data-ttu-id="0c7cf-145">Каждый унарный метод службы в файле *\*PROTO* приведет к появлению двух методов .NET в конкретном типе клиента gRPC для вызова метода: асинхронного метода и блокирующего метода.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-145">Each unary service method in the *\*.proto* file will result in two .NET methods on the concrete gRPC client type for calling the method: an asynchronous method and a blocking method.</span></span> <span data-ttu-id="0c7cf-146">Например, в `GreeterClient` существует два способа вызова `SayHello`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-146">For example, on `GreeterClient` there are two ways of calling `SayHello`:</span></span>

* <span data-ttu-id="0c7cf-147">`GreeterClient.SayHelloAsync` — асинхронный вызов службы `Greeter.SayHello`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-147">`GreeterClient.SayHelloAsync` - calls `Greeter.SayHello` service asynchronously.</span></span> <span data-ttu-id="0c7cf-148">Может быть ожидаемым.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-148">Can be awaited.</span></span>
* <span data-ttu-id="0c7cf-149">`GreeterClient.SayHello` — вызов службы `Greeter.SayHello` и блокировка до завершения.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-149">`GreeterClient.SayHello` - calls `Greeter.SayHello` service and blocks until complete.</span></span> <span data-ttu-id="0c7cf-150">Не используйте его в асинхронном коде.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-150">Don't use in asynchronous code.</span></span>

### <a name="server-streaming-call"></a><span data-ttu-id="0c7cf-151">Вызов потоковой передачи сервера</span><span class="sxs-lookup"><span data-stu-id="0c7cf-151">Server streaming call</span></span>

<span data-ttu-id="0c7cf-152">Вызов потоковой передачи сервера начинается с клиента, отправляющего сообщение с запросом.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-152">A server streaming call starts with the client sending a request message.</span></span> <span data-ttu-id="0c7cf-153">`ResponseStream.MoveNext()` считывает сообщения, переданные в службу путем потоковой передачи.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-153">`ResponseStream.MoveNext()` reads messages streamed from the service.</span></span> <span data-ttu-id="0c7cf-154">Вызов потоковой передачи сервера завершается, когда `ResponseStream.MoveNext()` возвращает `false`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-154">The server streaming call is complete when `ResponseStream.MoveNext()` returns `false`.</span></span>

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

<span data-ttu-id="0c7cf-155">Если используется C# 8 или более поздней версии, для чтения сообщений можно использовать синтаксис `await foreach`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-155">If you are using C# 8 or later, the `await foreach` syntax can be used to read messages.</span></span> <span data-ttu-id="0c7cf-156">Метод расширения `IAsyncStreamReader<T>.ReadAllAsync()` считывает все сообщения из потока ответов:</span><span class="sxs-lookup"><span data-stu-id="0c7cf-156">The `IAsyncStreamReader<T>.ReadAllAsync()` extension method reads all messages from the response stream:</span></span>

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

### <a name="client-streaming-call"></a><span data-ttu-id="0c7cf-157">Вызов потоковой передачи клиента</span><span class="sxs-lookup"><span data-stu-id="0c7cf-157">Client streaming call</span></span>

<span data-ttu-id="0c7cf-158">Вызов потоковой передачи клиента начинается *без* клиента, отправляющего сообщение с запросом.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-158">A client streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="0c7cf-159">Клиент может выбрать отправку сообщений с помощью `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-159">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="0c7cf-160">Когда клиент завершит отправку сообщений, следует вызвать `RequestStream.CompleteAsync`, чтобы уведомить службу.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-160">When the client has finished sending messages `RequestStream.CompleteAsync` should be called to notify the service.</span></span> <span data-ttu-id="0c7cf-161">Вызов завершается, когда служба возвращает ответное сообщение.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-161">The call is finished when the service returns a response message.</span></span>

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

### <a name="bi-directional-streaming-call"></a><span data-ttu-id="0c7cf-162">Вызов двунаправленной потоковой передачи</span><span class="sxs-lookup"><span data-stu-id="0c7cf-162">Bi-directional streaming call</span></span>

<span data-ttu-id="0c7cf-163">Вызов двунаправленной потоковой передачи начинается *без* клиента, отправляющего сообщение с запросом.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-163">A bi-directional streaming call starts *without* the client sending a message.</span></span> <span data-ttu-id="0c7cf-164">Клиент может выбрать отправку сообщений с помощью `RequestStream.WriteAsync`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-164">The client can choose to send messages with `RequestStream.WriteAsync`.</span></span> <span data-ttu-id="0c7cf-165">Сообщения, переданные в службу путем потоковой передачи, доступны с `ResponseStream.MoveNext()` или `ResponseStream.ReadAllAsync()`.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-165">Messages streamed from the service are accessible with `ResponseStream.MoveNext()` or `ResponseStream.ReadAllAsync()`.</span></span> <span data-ttu-id="0c7cf-166">Вызов двунаправленной потоковой передачи завершается, когда `ResponseStream` больше не содержит сообщений.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-166">The bi-directional streaming call is complete when the `ResponseStream` has no more messages.</span></span>

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

<span data-ttu-id="0c7cf-167">Во время вызова двунаправленной потоковой передачи клиент и служба могут обмениваться сообщениями в любое время.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-167">During a bi-directional streaming call, the client and service can send messages to each other at any time.</span></span> <span data-ttu-id="0c7cf-168">Наиболее подходящая логика клиента для взаимодействия с вызовом двунаправленной потоковой передачи зависит от логики службы.</span><span class="sxs-lookup"><span data-stu-id="0c7cf-168">The best client logic for interacting with a bi-directional call varies depending upon the service logic.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="0c7cf-169">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0c7cf-169">Additional resources</span></span>

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
