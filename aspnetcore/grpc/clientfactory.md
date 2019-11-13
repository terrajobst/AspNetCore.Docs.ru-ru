---
title: Интеграция фабрики клиента gRPC в .NET Core
author: jamesnk
description: Узнайте, как создавать клиенты gRPC с помощью фабрики клиента.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 11/12/2019
no-loc:
- SignalR
uid: grpc/clientfactory
ms.openlocfilehash: 3042bb61367f8b9a9f3142217ad329270ab2cca5
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963683"
---
# <a name="grpc-client-factory-integration-in-net-core"></a>Интеграция фабрики клиента gRPC в .NET Core

Интеграция gRPC с `HttpClientFactory` предлагает централизованный способ создания клиентов gRPC. Его можно использовать в качестве альтернативы [настройке изолированных экземпляров клиента gRPC](xref:grpc/client). Заводская интеграция доступна в пакете NuGet [GRPC .NET. клиентфактори](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .

Фабрика предоставляет следующие преимущества:

* Предоставляет центральное расположение для настройки экземпляров клиентов логических gRPC
* Управляет временем существования базового `HttpClientMessageHandler`
* Автоматическое распространение крайнего срока и отмены в службе ASP.NET Core gRPC

## <a name="register-grpc-clients"></a>Регистрация клиентов gRPC

Чтобы зарегистрировать клиент gRPC, можно использовать универсальный метод расширения `AddGrpcClient` в `Startup.ConfigureServices`, указав типизированный класс клиента и адрес службы gRPC:

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

Тип клиента gRPC регистрируется как временный с внедрением зависимостей (DI). Теперь клиент может внедряться и потребляться непосредственно в типах, созданных с помощью DI. ASP.NET Core контроллерах MVC SignalR концентраторы и службы gRPC — это места, где клиенты gRPC могут быть автоматически добавлены:

```csharp
public class AggregatorService : Aggregator.AggregatorBase
{
    private readonly Greeter.GreeterClient _client;

    public AggregatorService(Greeter.GreeterClient client)
    {
        _client = client;
    }

    public override async Task SayHellos(HelloRequest request,
        IServerStreamWriter<HelloReply> responseStream, ServerCallContext context)
    {
        // Forward the call on to the greeter service
        using (var call = _client.SayHellos(request))
        {
            await foreach (var response in call.ResponseStream.ReadAllAsync())
            {
                await responseStream.WriteAsync(response);
            }
        }
    }
}
```

## <a name="configure-httpclient"></a>Настройка HttpClient

`HttpClientFactory` создает `HttpClient`, используемый клиентом gRPC. Стандартные методы `HttpClientFactory` можно использовать для добавления по промежуточного слоя исходящего запроса или для настройки базового `HttpClientHandler` `HttpClient`:

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .ConfigurePrimaryHttpMessageHandler(() =>
    {
        var handler = new HttpClientHandler();
        handler.ClientCertificates.Add(LoadCertificate());
        return handler;
    });
```

Дополнительные сведения см. в разделе [make HTTP-запросы с помощью ихттпклиентфактори](xref:fundamentals/http-requests).

## <a name="configure-channel-and-interceptors"></a>Настройка канала и перехватчиков

методы, относящиеся к gRPC, доступны для:

* Настройка базового канала клиента gRPC.
* Добавьте `Interceptor` экземпляры, которые клиент будет использовать при выполнении вызовов gRPC.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .AddInterceptor(() => new LoggingInterceptor())
    .ConfigureChannel(o =>
    {
        o.Credentials = new CustomCredentials();
    });
```

## <a name="deadline-and-cancellation-propagation"></a>Распространение крайнего срока и отмены

Клиенты gRPC, созданные фабрикой в службе gRPC, можно настроить с помощью `EnableCallContextPropagation()`, чтобы автоматически распространить крайний срок и токен отмены в дочерние вызовы. Метод расширения `EnableCallContextPropagation()` доступен в пакете NuGet [GRPC. AspNetCore. Server. клиентфактори](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) .

Распространение контекста вызова выполняется путем считывания крайнего срока и токена отмены из текущего контекста запроса gRPC и автоматического распространения их на исходящие вызовы, выполненные клиентом gRPC. Распространение контекста вызова — это отличный способ гарантировать, что сложные, вложенные сценарии gRPC всегда распространяют крайний срок и отмену.

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

Дополнительные сведения о крайних сроках и отмене RPC см. в разделе [жизненный цикл RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
