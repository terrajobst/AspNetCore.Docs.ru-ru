---
title: Миграция gRPC Services с C-Core на ASP.NET Core
author: juntaoluo
description: Узнайте, как переместить существующее приложение gRPC на основе C-Core для выполнения на вершине стека ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: 451171a041f7bbb3711babd73d2fa2e245aadd28
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75355130"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="3ba4c-103">Миграция gRPC Services с C-Core на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3ba4c-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="3ba4c-104">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="3ba4c-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="3ba4c-105">Из-за реализации базового стека не все функции работают таким же образом, как и приложения [gRPC на основе C-Core](https://grpc.io/blog/grpc-stacks) и приложения на основе ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="3ba4c-106">В этом документе описываются основные различия между двумя стеками.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="3ba4c-107">время существования реализации службы gRPC</span><span class="sxs-lookup"><span data-stu-id="3ba4c-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="3ba4c-108">В стеке ASP.NET Core службы gRPC по умолчанию создаются с [временем существования с областью действия](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="3ba4c-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="3ba4c-109">В отличие от этого, gRPC C-Core по умолчанию привязывается к службе с [одноэлементным жизненным циклом](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="3ba4c-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="3ba4c-110">Время существования с заданной областью позволяет реализации службы разрешать другие службы с ограниченным временем существования.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="3ba4c-111">Например, время существования в области может также разрешать `DbContext` из контейнера DI посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-111">For example, a scoped lifetime can also resolve `DbContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="3ba4c-112">Использование ограниченного времени существования:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="3ba4c-113">Новый экземпляр реализации службы создается для каждого запроса.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="3ba4c-114">Невозможно совместно использовать состояние между запросами через члены экземпляра в типе реализации.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="3ba4c-115">Ожидание заключается в хранении общих состояний в одноэлементной службе в контейнере внедрения.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="3ba4c-116">Хранимые общие состояния разрешаются в конструкторе реализации службы gRPC.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="3ba4c-117">Дополнительные сведения о времени существования службы см. в разделе <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="3ba4c-118">Добавление одноэлементной службы</span><span class="sxs-lookup"><span data-stu-id="3ba4c-118">Add a singleton service</span></span>

<span data-ttu-id="3ba4c-119">Чтобы упростить переход от реализации gRPC C-Core к ASP.NET Core, можно изменить время существования службы реализации службы с ограниченного на Singleton.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="3ba4c-120">Это включает добавление экземпляра реализации службы в контейнер DI:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="3ba4c-121">Однако реализация службы с одноэлементным жизненным циклом больше не может разрешать службы с заданной областью посредством внедрения конструктора.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="3ba4c-122">Настройка параметров служб gRPC Services</span><span class="sxs-lookup"><span data-stu-id="3ba4c-122">Configure gRPC services options</span></span>

<span data-ttu-id="3ba4c-123">В приложениях на основе C-Core такие параметры, как `grpc.max_receive_message_length` и `grpc.max_send_message_length`, настраиваются с помощью `ChannelOption` при [создании экземпляра сервера](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="3ba4c-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="3ba4c-124">В ASP.NET Core gRPC предоставляет конфигурацию с помощью `GrpcServiceOptions` типа.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-124">In ASP.NET Core, gRPC provides configuration through the `GrpcServiceOptions` type.</span></span> <span data-ttu-id="3ba4c-125">Например, максимальный размер входящего сообщения службы gRPC можно настроить с помощью `AddGrpc`.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-125">For example, a gRPC service's the maximum incoming message size can be configured via `AddGrpc`.</span></span> <span data-ttu-id="3ba4c-126">В следующем примере `MaxReceiveMessageSize` по умолчанию для 4 МБ изменяется на 16 МБ:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-126">The following example changes the default `MaxReceiveMessageSize` of 4 MB to 16 MB:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

<span data-ttu-id="3ba4c-127">Дополнительные сведения о конфигурации см. в разделе <xref:grpc/configuration>.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-127">For more information on configuration, see <xref:grpc/configuration>.</span></span>

## <a name="logging"></a><span data-ttu-id="3ba4c-128">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="3ba4c-128">Logging</span></span>

<span data-ttu-id="3ba4c-129">Приложения на основе C-Core используют `GrpcEnvironment` для [настройки средства ведения журнала](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) в целях отладки.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-129">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="3ba4c-130">Стек ASP.NET Core предоставляет эту функцию через [API ведения журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="3ba4c-130">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="3ba4c-131">Например, средство ведения журнала можно добавить в службу gRPC посредством внедрения конструктора:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-131">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="3ba4c-132">HTTPS</span><span class="sxs-lookup"><span data-stu-id="3ba4c-132">HTTPS</span></span>

<span data-ttu-id="3ba4c-133">Приложения на основе C-Core настраивают HTTPS через [свойство Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="3ba4c-133">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="3ba4c-134">Аналогичная концепция используется для настройки серверов в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-134">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="3ba4c-135">Например, Kestrel использует [конфигурацию конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration) для этой функции.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-135">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="grpc-interceptors-vs-middleware"></a><span data-ttu-id="3ba4c-136">gRPC перехватчики VS по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="3ba4c-136">gRPC Interceptors vs Middleware</span></span>

<span data-ttu-id="3ba4c-137">ASP.NET Core по [промежуточного слоя](xref:fundamentals/middleware/index) предлагает аналогичные функциональные возможности по сравнению с перехватчиками в приложениях gRPC на основе C-Core.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-137">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="3ba4c-138">ASP.NET Core по промежуточного слоя и перехватчики концептуально похожи.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-138">ASP.NET Core middleware and interceptors are conceptually similar.</span></span> <span data-ttu-id="3ba4c-139">Оба.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-139">Both:</span></span>

* <span data-ttu-id="3ba4c-140">Используются для создания конвейера, обрабатывающего запрос gRPC.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-140">Are used to construct a pipeline that handles a gRPC request.</span></span>
* <span data-ttu-id="3ba4c-141">Разрешить выполнение работы до или после следующего компонента в конвейере.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-141">Allow work to be performed before or after the next component in the pipeline.</span></span>
* <span data-ttu-id="3ba4c-142">Предоставление доступа к `HttpContext`:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-142">Provide access to `HttpContext`:</span></span>
  * <span data-ttu-id="3ba4c-143">В промежуточном слое `HttpContext` является параметром.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-143">In middleware the `HttpContext` is a parameter.</span></span>
  * <span data-ttu-id="3ba4c-144">В перехватчиках доступ к `HttpContext` можно получить с помощью параметра `ServerCallContext` с помощью метода расширения `ServerCallContext.GetHttpContext`.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-144">In interceptors the `HttpContext` can be accessed using the `ServerCallContext` parameter with the `ServerCallContext.GetHttpContext` extension method.</span></span> <span data-ttu-id="3ba4c-145">Обратите внимание, что эта функция относится к перехватчикам, выполняемым в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-145">Note that this feature is specific to interceptors running in ASP.NET Core.</span></span>

<span data-ttu-id="3ba4c-146">отличия gRPC от по промежуточного слоя ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-146">gRPC Interceptor differences from ASP.NET Core Middleware:</span></span>

* <span data-ttu-id="3ba4c-147">Перехватчики</span><span class="sxs-lookup"><span data-stu-id="3ba4c-147">Interceptors:</span></span>
  * <span data-ttu-id="3ba4c-148">Работа с уровнем абстракции gRPC с помощью [серверкаллконтекст](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="3ba4c-148">Operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>
  * <span data-ttu-id="3ba4c-149">Предоставить доступ к:</span><span class="sxs-lookup"><span data-stu-id="3ba4c-149">Provide access to:</span></span>
    * <span data-ttu-id="3ba4c-150">Десериализованное сообщение, отправленное в вызов.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-150">The deserialized message sent to a call.</span></span>
    * <span data-ttu-id="3ba4c-151">Сообщение, возвращаемое из вызова перед его сериализацией.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-151">The message being returned from the call before it is serialized.</span></span>
  * <span data-ttu-id="3ba4c-152">Может перехватывать и справляться с исключениями, созданными из служб gRPC.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-152">Can catch and handle exceptions thrown from gRPC services.</span></span>
* <span data-ttu-id="3ba4c-153">По промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="3ba4c-153">Middleware:</span></span>
  * <span data-ttu-id="3ba4c-154">Выполняется перед перехватчиками gRPC.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-154">Runs before gRPC interceptors.</span></span>
  * <span data-ttu-id="3ba4c-155">Работает с базовыми сообщениями HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-155">Operates on the underlying HTTP/2 messages.</span></span>
  * <span data-ttu-id="3ba4c-156">Может получать доступ только к байтам из потоков запросов и ответов.</span><span class="sxs-lookup"><span data-stu-id="3ba4c-156">Can only access bytes from the request and response streams.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="3ba4c-157">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="3ba4c-157">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
