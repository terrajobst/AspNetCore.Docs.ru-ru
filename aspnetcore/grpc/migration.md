---
title: Миграция gRPC Services с C-Core на ASP.NET Core
author: juntaoluo
description: Узнайте, как переместить существующее приложение gRPC на основе C-Core для выполнения на вершине стека ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 39aa711a1a47cf11ec5b08903b4130c7caa1501c
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022304"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="08bbb-103">Миграция gRPC Services с C-Core на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08bbb-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="08bbb-104">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="08bbb-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="08bbb-105">Из-за реализации базового стека не все функции работают таким же образом, как и приложения [gRPC на основе C-Core](https://grpc.io/blog/grpc-stacks) и приложения на основе ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08bbb-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="08bbb-106">В этом документе описываются основные различия между двумя стеками.</span><span class="sxs-lookup"><span data-stu-id="08bbb-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="08bbb-107">время существования реализации службы gRPC</span><span class="sxs-lookup"><span data-stu-id="08bbb-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="08bbb-108">В стеке ASP.NET Core службы gRPC по умолчанию создаются с временем существования с [областью действия](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="08bbb-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="08bbb-109">В отличие от этого, gRPC C-Core по умолчанию привязывается к службе с одноэлементным [жизненным циклом](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="08bbb-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="08bbb-110">Время существования с заданной областью позволяет реализации службы разрешать другие службы с ограниченным временем существования.</span><span class="sxs-lookup"><span data-stu-id="08bbb-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="08bbb-111">Например, время существования с областью действия также `DbContext` можно разрешить из контейнера di посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="08bbb-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="08bbb-112">Использование ограниченного времени существования:</span><span class="sxs-lookup"><span data-stu-id="08bbb-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="08bbb-113">Новый экземпляр реализации службы создается для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="08bbb-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="08bbb-114">Невозможно совместно использовать состояние между запросами через члены экземпляра в типе реализации.</span><span class="sxs-lookup"><span data-stu-id="08bbb-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="08bbb-115">Ожидание заключается в хранении общих состояний в одноэлементной службе в контейнере внедрения.</span><span class="sxs-lookup"><span data-stu-id="08bbb-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="08bbb-116">Хранимые общие состояния разрешаются в конструкторе реализации службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="08bbb-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="08bbb-117">Дополнительные сведения о времени существования службы см. <xref:fundamentals/dependency-injection#service-lifetimes>в разделе.</span><span class="sxs-lookup"><span data-stu-id="08bbb-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="08bbb-118">Добавление одноэлементной службы</span><span class="sxs-lookup"><span data-stu-id="08bbb-118">Add a singleton service</span></span>

<span data-ttu-id="08bbb-119">Чтобы упростить переход от реализации gRPC C-Core к ASP.NET Core, можно изменить время существования службы реализации службы с ограниченного на Singleton.</span><span class="sxs-lookup"><span data-stu-id="08bbb-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="08bbb-120">Это включает добавление экземпляра реализации службы в контейнер DI:</span><span class="sxs-lookup"><span data-stu-id="08bbb-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="08bbb-121">Однако реализация службы с одноэлементным жизненным циклом больше не может разрешать службы с заданной областью посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="08bbb-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="08bbb-122">Настройка параметров служб gRPC Services</span><span class="sxs-lookup"><span data-stu-id="08bbb-122">Configure gRPC services options</span></span>

<span data-ttu-id="08bbb-123">В приложениях на основе C-Core параметры, такие как `grpc.max_receive_message_length` и `grpc.max_send_message_length` , настраиваются `ChannelOption` с помощью при [создании экземпляра сервера](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="08bbb-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="08bbb-124">В ASP.NET Core gRPC предоставляет конфигурацию с помощью `GrpcServiceOptions` типа.</span><span class="sxs-lookup"><span data-stu-id="08bbb-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="08bbb-125">Например, максимальный размер входящего сообщения службы gRPC можно настроить с помощью `AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="08bbb-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

<span data-ttu-id="08bbb-126">Дополнительные сведения о конфигурации см. в <xref:grpc/configuration>разделе.</span><span class="sxs-lookup"><span data-stu-id="08bbb-126">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="08bbb-127">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="08bbb-127">Logging</span></span>

<span data-ttu-id="08bbb-128">Приложения на основе C-Core используют `GrpcEnvironment` для [настройки средства ведения журнала](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="08bbb-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="08bbb-129">Стек ASP.NET Core предоставляет эту функцию через [API ведения журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="08bbb-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="08bbb-130">Например, средство ведения журнала можно добавить в службу gRPC посредством внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="08bbb-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="08bbb-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="08bbb-131">HTTPS</span></span>

<span data-ttu-id="08bbb-132">Приложения на основе C-Core настраивают HTTPS через [свойство Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="08bbb-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="08bbb-133">Аналогичная концепция используется для настройки серверов в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08bbb-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="08bbb-134">Например, Kestrel использует [конфигурацию конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration) для этой функции.</span><span class="sxs-lookup"><span data-stu-id="08bbb-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="08bbb-135">Перехватчики и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="08bbb-135">Interceptors and Middleware</span></span>

<span data-ttu-id="08bbb-136">ASP.NET Core по [промежуточного слоя](xref:fundamentals/middleware/index) предлагает аналогичные функциональные возможности по сравнению с перехватчиками в приложениях gRPC на основе C-Core.</span><span class="sxs-lookup"><span data-stu-id="08bbb-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="08bbb-137">По промежуточного слоя и перехватчиков концептуально совпадают и используются для создания конвейера, обрабатывающего запрос gRPC.</span><span class="sxs-lookup"><span data-stu-id="08bbb-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="08bbb-138">Они позволяют выполнять работу до или после следующего компонента в конвейере.</span><span class="sxs-lookup"><span data-stu-id="08bbb-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="08bbb-139">Однако ASP.NET Core по промежуточного слоя работает с базовыми сообщениями HTTP/2, тогда как перехватчики работают с уровнем абстракции gRPC с помощью [серверкаллконтекст](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="08bbb-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="08bbb-140">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="08bbb-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
