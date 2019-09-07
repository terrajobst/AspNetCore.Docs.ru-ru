---
title: Интеграция фабрики клиента gRPC в .NET Core
author: jamesnk
description: Узнайте, как создавать клиенты gRPC с помощью фабрики клиента.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 08/21/2019
uid: grpc/clientfactory
ms.openlocfilehash: 5d719893e96ae017e2de0ee1744003d2d67a49c9
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773668"
---
# <a name="grpc-client-factory-integration-in-net-core"></a><span data-ttu-id="23b09-103">Интеграция фабрики клиента gRPC в .NET Core</span><span class="sxs-lookup"><span data-stu-id="23b09-103">gRPC client factory integration in .NET Core</span></span>

<span data-ttu-id="23b09-104">Интеграция gRPC с `HttpClientFactory` предлагает централизованный способ создания клиентов gRPC.</span><span class="sxs-lookup"><span data-stu-id="23b09-104">gRPC integration with `HttpClientFactory` offers a centralized way to create gRPC clients.</span></span> <span data-ttu-id="23b09-105">Его можно использовать в качестве альтернативы [настройке изолированных экземпляров клиента gRPC](xref:grpc/client).</span><span class="sxs-lookup"><span data-stu-id="23b09-105">It can be used as an alternative to [configuring stand-alone gRPC client instances](xref:grpc/client).</span></span> <span data-ttu-id="23b09-106">Заводская интеграция доступна в пакете NuGet [GRPC .NET. клиентфактори](https://www.nuget.org/packages/Grpc.Net.ClientFactory) .</span><span class="sxs-lookup"><span data-stu-id="23b09-106">Factory integration is available in the [Grpc.Net.ClientFactory](https://www.nuget.org/packages/Grpc.Net.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="23b09-107">Фабрика предоставляет следующие преимущества:</span><span class="sxs-lookup"><span data-stu-id="23b09-107">The factory offers the following benefits:</span></span>

* <span data-ttu-id="23b09-108">Предоставляет центральное расположение для настройки экземпляров клиентов логических gRPC</span><span class="sxs-lookup"><span data-stu-id="23b09-108">Provides a central location for configuring logical gRPC client instances</span></span>
* <span data-ttu-id="23b09-109">Управляет временем существования базового`HttpClientMessageHandler`</span><span class="sxs-lookup"><span data-stu-id="23b09-109">Manages the lifetime of the underlying `HttpClientMessageHandler`</span></span>
* <span data-ttu-id="23b09-110">Автоматическое распространение крайнего срока и отмены в службе ASP.NET Core gRPC</span><span class="sxs-lookup"><span data-stu-id="23b09-110">Automatic propagation of deadline and cancellation in an ASP.NET Core gRPC service</span></span>

## <a name="register-grpc-clients"></a><span data-ttu-id="23b09-111">Регистрация клиентов gRPC</span><span class="sxs-lookup"><span data-stu-id="23b09-111">Register gRPC clients</span></span>

<span data-ttu-id="23b09-112">Чтобы зарегистрировать клиент gRPC, можно использовать универсальный `AddGrpcClient` метод расширения в `Startup.ConfigureServices`, указав gRPC типизированный клиентский класс и адрес службы:</span><span class="sxs-lookup"><span data-stu-id="23b09-112">To register a gRPC client, the generic `AddGrpcClient` extension method can be used within `Startup.ConfigureServices`, specifying the gRPC typed client class and service address:</span></span>

```csharp
services.AddGrpcClient<Greeter.GreeterClient>(o =>
{
    o.Address = new Uri("https://localhost:5001");
});
```

<span data-ttu-id="23b09-113">Тип клиента gRPC регистрируется как временный с внедрением зависимостей (DI).</span><span class="sxs-lookup"><span data-stu-id="23b09-113">The gRPC client type is registered as transient with dependency injection (DI).</span></span> <span data-ttu-id="23b09-114">Теперь клиент может внедряться и потребляться непосредственно в типах, созданных с помощью DI.</span><span class="sxs-lookup"><span data-stu-id="23b09-114">The client can now be injected and consumed directly in types created by DI.</span></span> <span data-ttu-id="23b09-115">ASP.NET Core контроллеры MVC, концентраторы SignalR и службы gRPC — это места, где можно автоматически внедрять клиенты gRPC:</span><span class="sxs-lookup"><span data-stu-id="23b09-115">ASP.NET Core MVC controllers, SignalR hubs and gRPC services are places where gRPC clients can automatically be injected:</span></span>

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

## <a name="configure-httpclient"></a><span data-ttu-id="23b09-116">Настройка HttpClient</span><span class="sxs-lookup"><span data-stu-id="23b09-116">Configure HttpClient</span></span>

<span data-ttu-id="23b09-117">`HttpClientFactory`Создает объект `HttpClient` , используемый клиентом gRPC.</span><span class="sxs-lookup"><span data-stu-id="23b09-117">`HttpClientFactory` creates the `HttpClient` used by the gRPC client.</span></span> <span data-ttu-id="23b09-118">Стандартные `HttpClientFactory` методы можно использовать для добавления по промежуточного слоя исходящего запроса или для `HttpClientHandler` настройки базового `HttpClient`компонента.</span><span class="sxs-lookup"><span data-stu-id="23b09-118">Standard `HttpClientFactory` methods can be used to add outgoing request middleware or to configure the underlying `HttpClientHandler` of the `HttpClient`:</span></span>

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

<span data-ttu-id="23b09-119">Дополнительные сведения см. в разделе [make HTTP-запросы с помощью ихттпклиентфактори](xref:fundamentals/http-requests).</span><span class="sxs-lookup"><span data-stu-id="23b09-119">For more information, see [Make HTTP requests using IHttpClientFactory](xref:fundamentals/http-requests).</span></span>

## <a name="configure-channel-and-interceptors"></a><span data-ttu-id="23b09-120">Настройка канала и перехватчиков</span><span class="sxs-lookup"><span data-stu-id="23b09-120">Configure Channel and Interceptors</span></span>

<span data-ttu-id="23b09-121">методы, относящиеся к gRPC, доступны для:</span><span class="sxs-lookup"><span data-stu-id="23b09-121">gRPC-specific methods are available to:</span></span>

* <span data-ttu-id="23b09-122">Настройка базового канала клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="23b09-122">Configure a gRPC client's underlying channel.</span></span>
* <span data-ttu-id="23b09-123">Добавьте `Interceptor` экземпляры, которые будут использоваться клиентом при выполнении вызовов gRPC.</span><span class="sxs-lookup"><span data-stu-id="23b09-123">Add `Interceptor` instances that the client will use when making gRPC calls.</span></span>

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

## <a name="deadline-and-cancellation-propagation"></a><span data-ttu-id="23b09-124">Распространение крайнего срока и отмены</span><span class="sxs-lookup"><span data-stu-id="23b09-124">Deadline and cancellation propagation</span></span>

<span data-ttu-id="23b09-125">Клиенты gRPC, созданные фабрикой в службе gRPC, могут быть настроены `EnableCallContextPropagation()` на автоматическое распространение крайнего срока и токена отмены в дочерние вызовы.</span><span class="sxs-lookup"><span data-stu-id="23b09-125">gRPC clients created by the factory in a gRPC service can be configured with `EnableCallContextPropagation()` to automatically propagate the deadline and cancellation token to child calls.</span></span> <span data-ttu-id="23b09-126">Метод расширения доступен в пакете NuGet [GRPC. AspNetCore. Server. клиентфактори.](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) `EnableCallContextPropagation()`</span><span class="sxs-lookup"><span data-stu-id="23b09-126">The `EnableCallContextPropagation()` extension method is available in the [Grpc.AspNetCore.Server.ClientFactory](https://www.nuget.org/packages/Grpc.AspNetCore.Server.ClientFactory) NuGet package.</span></span>

<span data-ttu-id="23b09-127">Распространение контекста вызова выполняется путем считывания крайнего срока и токена отмены из текущего контекста запроса gRPC и автоматического распространения их на исходящие вызовы, выполненные клиентом gRPC.</span><span class="sxs-lookup"><span data-stu-id="23b09-127">Call context propagation works by reading the deadline and cancellation token from the current gRPC request context and automatically propagating them to outgoing calls made by the gRPC client.</span></span> <span data-ttu-id="23b09-128">Распространение контекста вызова — это отличный способ гарантировать, что сложные, вложенные сценарии gRPC всегда распространяют крайний срок и отмену.</span><span class="sxs-lookup"><span data-stu-id="23b09-128">Call context propagation is an excellent way of ensuring that complex, nested gRPC scenarios always propagate the deadline and cancellation.</span></span>

```csharp
services
    .AddGrpcClient<Greeter.GreeterClient>(o =>
    {
        o.Address = new Uri("https://localhost:5001");
    })
    .EnableCallContextPropagation();
```

<span data-ttu-id="23b09-129">Дополнительные сведения о крайних сроках и отмене RPC см. в разделе [жизненный цикл RPC](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span><span class="sxs-lookup"><span data-stu-id="23b09-129">For more information about deadlines and RPC cancellation, see [RPC life cycle](https://www.grpc.io/docs/guides/concepts/#rpc-life-cycle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="23b09-130">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="23b09-130">Additional resources</span></span>

* <xref:grpc/client>
* <xref:fundamentals/http-requests>
