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
# <a name="call-grpc-services-with-the-net-client"></a>Вызов служб gRPC с помощью клиента .NET

Клиентская библиотека .NET gRPC доступна в пакете NuGet для [gRPC .NET. Client](https://www.nuget.org/packages/Grpc.Net.Client) . В этом документе объясняется, как выполнять следующие задачи:

* Настройте клиент gRPC для вызова служб gRPC.
* Сделайте вызовы gRPC унарными, серверной потоковой передачей, потоковой передачей клиента и методами двунаправленной потоковой передачи.

## <a name="configure-grpc-client"></a>Настройка клиента gRPC

Клиенты gRPC являются конкретными типами клиентов, [создаваемыми в файлах *\*.proto*](xref:grpc/basics#generated-c-assets). Конкретный клиент gRPC использует методы, которые выполняют преобразование для служб gRPC в файле *\*.proto*.

Клиент gRPC создается из канала. Начните с использования `GrpcChannel.ForAddress` для создания канала, а затем используйте канал для создания клиента gRPC:

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");
var client = new Greet.GreeterClient(channel);
```

Канал представляет долгосрочное подключение к службе gRPC. При создании канала он настраивается с параметрами, связанными с вызовом службы. Например, `HttpClient`, используемые для выполнения вызовов, максимальный размер сообщения отправки и получения, и ведение журнала можно указать в `GrpcChannelOptions` и использовать с `GrpcChannel.ForAddress`. Полный список параметров см. в разделе [Параметры конфигурации клиента](xref:grpc/configuration#configure-client-options).

```csharp
var channel = GrpcChannel.ForAddress("https://localhost:5001");

var greeterClient = new Greet.GreeterClient(channel);
var counterClient = new Count.CounterClient(channel);

// Use clients to call gRPC services
```

Производительность и использование канала и клиента:

* Создание канала может быть дорогостоящей операцией. Повторное использование канала для вызовов gRPC обеспечивает выигрыш в производительности.
* Клиенты gRPC создаются с помощью каналов. Клиенты gRPC являются легковесными объектами и не нуждаются в кэшировании или повторном использовании.
* Из канала можно создать несколько клиентов gRPC, включая различные типы клиентов.
* Канал и клиенты, созданные из канала, могут безопасно использоваться несколькими потоками.
* Клиенты, созданные из канала, могут выполнять несколько одновременных вызовов.

`GrpcChannel.ForAddress` не единственный вариант создания клиента gRPC. Если вы вызываете gRPC Services из приложения ASP.NET Core, рассмотрите возможность [интеграции фабрики клиента gRPC](xref:grpc/clientfactory). Интеграция gRPC с `HttpClientFactory` предлагает централизованную альтернативу созданию клиентов gRPC.

> [!NOTE]
> Для [вызова незащищенных служб gRPC с клиентом .NET](xref:grpc/troubleshoot#call-insecure-grpc-services-with-net-core-client)требуется дополнительная настройка.

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

Каждый метод унарной службы в файле *\*.* Typ приведет к появлению двух методов .NET для конкретного типа клиента gRPC для вызова метода: асинхронного метода и блокирующего метода. Например, в `GreeterClient` существует два способа вызова `SayHello`.

* `GreeterClient.SayHelloAsync` — асинхронный вызов `Greeter.SayHello` службы. Можно ожидать.
* `GreeterClient.SayHello` — вызывает службу `Greeter.SayHello` и блокируется до завершения. Не используйте в асинхронном коде.

### <a name="server-streaming-call"></a>Потоковый вызов сервера

Потоковый вызов сервера начинается с клиента, отправляющего сообщение запроса. `ResponseStream.MoveNext()` считывает сообщения, переданные из службы в поток. Потоковый вызов сервера завершается, когда `ResponseStream.MoveNext()` возвращает `false`.

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

Если используется C# 8 или более поздней версии, для чтения сообщений можно использовать синтаксис `await foreach`. Метод расширения `IAsyncStreamReader<T>.ReadAllAsync()` считывает все сообщения из потока ответов:

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

Клиентский вызов потоковой передачи начинается *без* отправки сообщения клиентом. Клиент может выбрать отправку сообщений с помощью `RequestStream.WriteAsync`. Когда клиент завершит отправку сообщений, `RequestStream.CompleteAsync` следует вызвать, чтобы уведомить службу. Вызов завершается, когда служба возвращает ответное сообщение.

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

Двунаправленный потоковый вызов начинается *без* отправки сообщения клиентом. Клиент может выбрать отправку сообщений с помощью `RequestStream.WriteAsync`. Сообщения, переходящие из службы в поток, доступны с `ResponseStream.MoveNext()` или `ResponseStream.ReadAllAsync()`. Двусторонний двунаправленный потоковый вызов завершается, когда `ResponseStream` не содержит сообщений.

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
