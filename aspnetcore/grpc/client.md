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
# <a name="call-grpc-services-with-the-net-client"></a>Вызов служб gRPC с помощью клиента .NET

Клиентская библиотека .NET gRPC доступна в пакете NuGet для [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) . В этом документе объясняется, как выполнять следующие задачи:

* Настройте клиент gRPC для вызова служб gRPC.
* Сделайте вызовы gRPC унарными, серверной потоковой передачей, потоковой передачей клиента и методами двунаправленной потоковой передачи.

## <a name="configure-grpc-client"></a>Настройка клиента gRPC

Клиенты gRPC являются конкретными типами клиентов, [созданными из файлов с  *\*.* ](xref:grpc/basics#generated-c-assets)именования. Конкретный клиент gRPC имеет методы, которые транслируются в службу gRPC в файле с  *\*расширением.* .

Клиент gRPC создается из канала. Начните с использования `GrpcChannel.ForAddress` для создания канала, а затем используйте канал для создания клиента gRPC:

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Канал представляет долгосрочное подключение к службе gRPC. При создании канала он настраивается с параметрами, связанными с вызовом службы. Например, объект `HttpClient` , используемый для выполнения вызовов, максимальный размер сообщения для отправки и получения, и ведение журнала можно указать в `GrpcChannelOptions` и использовать с `GrpcChannel.ForAddress`. Полный список параметров см. в разделе [Параметры конфигурации клиента](xref:grpc/configuration#configure-client-options).

Создание канала может быть дорогостоящей операцией и повторное использование канала для вызовов gRPC предлагает преимущества производительности. С помощью канала можно создать несколько конкретных клиентов gRPC, включая различные типы клиентов. Типы клиентов конкретных gRPC являются простыми объектами и могут быть созданы при необходимости.

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

`GrpcChannel.ForAddress`не единственный вариант создания клиента gRPC. При вызове gRPC Services из приложения ASP.NET Core рекомендуется [интегрировать фабрику клиента gRPC](xref:grpc/clientfactory). Интеграция gRPC с `HttpClientFactory` предлагает централизованную альтернативу созданию клиентов gRPC.

## <a name="make-grpc-calls"></a>Сделать вызовы gRPC

Вызов gRPC инициируется путем вызова метода на клиенте. Клиент gRPC будет выполнять сериализацию сообщений и направлять вызов gRPC к правильной службе.

gRPC имеет различные типы методов. Способ использования клиента для выполнения вызова gRPC зависит от типа вызываемого метода. Типы методов gRPC:

* Унарный
* Потоковая передача сервера
* Потоковая передача клиента
* Двунаправленная потоковая передача

### <a name="unary-call"></a>Унарный вызов

Унарный вызов начинается с клиента, отправляющего сообщение запроса. После завершения работы службы возвращается ответное сообщение.

```csharp
var client = new Greet.GreeterClient(channel);
var response = await client.SayHelloAsync(new HelloRequest { Name = "World" });

Console.WriteLine("Greeting: " + response.Message);
// Greeting: Hello World
```

Каждый метод унарной службы в  *\*именованном файле приведет* к появлению двух методов .NET для конкретного типа клиента gRPC для вызова метода: асинхронного метода и метода блокировки. Например, `GreeterClient` существует два способа вызова `SayHello`:

* `GreeterClient.SayHelloAsync`— вызывает `Greeter.SayHello` службу асинхронно. Можно ожидать.
* `GreeterClient.SayHello`— вызывает `Greeter.SayHello` службу и блокируется до завершения. Не используйте в асинхронном коде.

### <a name="server-streaming-call"></a>Потоковый вызов сервера

Потоковый вызов сервера начинается с клиента, отправляющего сообщение запроса. `ResponseStream.MoveNext()`считывает сообщения, переданные из службы в поток. Вызов потоковой передачи сервера завершается `ResponseStream.MoveNext()` при `false`возврате.

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

При использовании C# 8 или более поздней версии `await foreach` можно использовать синтаксис для чтения сообщений. Метод `IAsyncStreamReader<T>.ReadAllAsync()` расширения считывает все сообщения из потока ответов:

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

### <a name="client-streaming-call"></a>Вызов потоковой передачи клиента

Клиентский вызов потоковой передачи начинается *без* отправки сообщения клиентом. Клиент может отправлять сообщения с помощью `RequestStream.WriteAsync`. Когда клиент завершит отправку сообщений `RequestStream.CompleteAsync` , необходимо вызвать метод, чтобы уведомить службу. Вызов завершается, когда служба возвращает ответное сообщение.

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

### <a name="bi-directional-streaming-call"></a>Двунаправленный вызов потоковой передачи

Двунаправленный потоковый вызов начинается *без* отправки сообщения клиентом. Клиент может выбрать отправку сообщений `RequestStream.WriteAsync`. Сообщения, переходящие из службы в поток, `ResponseStream.MoveNext()` доступны `ResponseStream.ReadAllAsync()`с помощью или. Двусторонний двунаправленный потоковый вызов завершается, `ResponseStream` когда не содержит сообщений.

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

Во время двунаправленного потокового вызова клиент и служба могут передавать сообщения друг другу в любое время. Лучшая клиентская логика для взаимодействия с двунаправленным вызовом зависит от логики службы.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/clientfactory>
* <xref:grpc/basics>
