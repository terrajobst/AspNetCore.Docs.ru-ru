---
title: Миграция служб gRPC из C-core на ASP.NET Core
author: juntaoluo
description: Узнайте, как переместить существующее приложение на основе gRPC C core для запуска на вершине стека ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 4d489b5aecf2e15fbbe3ac472b991a4365cd47c1
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672623"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a><span data-ttu-id="7af26-103">Миграция служб gRPC из C-core на ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7af26-103">Migrating gRPC services from C-core to ASP.NET Core</span></span>

<span data-ttu-id="7af26-104">Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)</span><span class="sxs-lookup"><span data-stu-id="7af26-104">By [John Luo](https://github.com/juntaoluo)</span></span>

<span data-ttu-id="7af26-105">Из-за реализации нижележащего стека, не все функции работают одинаково между [C-ядро на основе gRPC](https://grpc.io/blog/grpc-stacks) приложения и приложения на основе ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7af26-105">Due to the implementation of the underlying stack, not all features work in the same way between [C-core-based gRPC](https://grpc.io/blog/grpc-stacks) apps and ASP.NET Core-based apps.</span></span> <span data-ttu-id="7af26-106">В этом документе описываются ключевые различия по миграции между двумя стеками.</span><span class="sxs-lookup"><span data-stu-id="7af26-106">This document highlights the key differences for migrating between the two stacks.</span></span>

## <a name="grpc-service-implementation-lifetime"></a><span data-ttu-id="7af26-107">время существования реализации службы gRPC</span><span class="sxs-lookup"><span data-stu-id="7af26-107">gRPC service implementation lifetime</span></span>

<span data-ttu-id="7af26-108">В стеке ASP.NET Core gRPC службы, по умолчанию создаются с помощью [области времени существования](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7af26-108">In the ASP.NET Core stack, gRPC services, by default, are created with a [scoped lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="7af26-109">Напротив, gRPC C-core по умолчанию привязывается к службе с помощью [времени существования singleton](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="7af26-109">In contrast, gRPC C-core by default binds to a service with a [singleton lifetime](xref:fundamentals/dependency-injection#service-lifetimes).</span></span>

<span data-ttu-id="7af26-110">Время существования областью действия позволяет реализации службы, чтобы разрешить другим службам с помощью области времени существования.</span><span class="sxs-lookup"><span data-stu-id="7af26-110">A scoped lifetime allows the service implementation to resolve other services with scoped lifetimes.</span></span> <span data-ttu-id="7af26-111">Например, время существования областью действия для устранения `DBContext` из контейнера внедрения Зависимостей через конструктор injection.</span><span class="sxs-lookup"><span data-stu-id="7af26-111">For example, a scoped lifetime can also resolve `DBContext` from the DI container through constructor injection.</span></span> <span data-ttu-id="7af26-112">С помощью области времени существования:</span><span class="sxs-lookup"><span data-stu-id="7af26-112">Using scoped lifetime:</span></span>

* <span data-ttu-id="7af26-113">Для каждого запроса создается новый экземпляр реализации службы.</span><span class="sxs-lookup"><span data-stu-id="7af26-113">A new instance of the service implementation is constructed for each request.</span></span>
* <span data-ttu-id="7af26-114">Невозможно получить совместного использования состояния между запросами через члены экземпляра в реализации типа.</span><span class="sxs-lookup"><span data-stu-id="7af26-114">It isn't possible to share state between requests via instance members on the implementation type.</span></span>
* <span data-ttu-id="7af26-115">Ожидается, для хранения общих состояний в это отдельная служба в контейнере внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7af26-115">The expectation is to store shared states in a singleton service in the DI container.</span></span> <span data-ttu-id="7af26-116">В конструкторе класса реализации службы gRPC разрешаются хранимые общих состояний.</span><span class="sxs-lookup"><span data-stu-id="7af26-116">The stored shared states are resolved in the constructor of the gRPC service implementation.</span></span>

<span data-ttu-id="7af26-117">Дополнительные сведения о времени существования службы, см. в разделе <xref:fundamentals/dependency-injection#service-lifetimes>.</span><span class="sxs-lookup"><span data-stu-id="7af26-117">For more information on service lifetimes, see <xref:fundamentals/dependency-injection#service-lifetimes>.</span></span>

### <a name="add-a-singleton-service"></a><span data-ttu-id="7af26-118">Добавьте это отдельная служба</span><span class="sxs-lookup"><span data-stu-id="7af26-118">Add a singleton service</span></span>

<span data-ttu-id="7af26-119">Для облегчения перехода от реализации C-core gRPC на ASP.NET Core, его можно изменить время существования службы из реализации службы за один элемент с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="7af26-119">To facilitate the transition from a gRPC C-core implementation to ASP.NET Core, it's possible to change the service lifetime of the service implementation from scoped to singleton.</span></span> <span data-ttu-id="7af26-120">Для этого необходимо добавить экземпляр реализации службы в контейнер внедрения Зависимостей.</span><span class="sxs-lookup"><span data-stu-id="7af26-120">This involves adding an instance of the service implementation to the DI container:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

<span data-ttu-id="7af26-121">Тем не менее реализация службы со временем существования singleton больше не может разрешить ограниченных служб через внедрение через конструктор.</span><span class="sxs-lookup"><span data-stu-id="7af26-121">However, a service implementation with a singleton lifetime is no longer able to resolve scoped services through constructor injection.</span></span>

## <a name="configure-grpc-services-options"></a><span data-ttu-id="7af26-122">Настройка параметров служб gRPC</span><span class="sxs-lookup"><span data-stu-id="7af26-122">Configure gRPC services options</span></span>

<span data-ttu-id="7af26-123">В приложениях на основе C-core, параметры, такие как `grpc.max_receive_message_length` и `grpc.max_send_message_length` должны быть настроены `ChannelOption` при [конструирование экземпляра сервера](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span><span class="sxs-lookup"><span data-stu-id="7af26-123">In C-core-based apps, settings such as `grpc.max_receive_message_length` and `grpc.max_send_message_length` are configured with `ChannelOption` when [constructing the Server instance](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).</span></span>

<span data-ttu-id="7af26-124">В ASP.NET Core `GrpcServiceOptions` предоставляет способ для настройки этих параметров.</span><span class="sxs-lookup"><span data-stu-id="7af26-124">In ASP.NET Core, `GrpcServiceOptions` provides a way to configure these settings.</span></span> <span data-ttu-id="7af26-125">Параметры могут применяться глобально ко всем службам gRPC, или к типу реализации отдельной службы.</span><span class="sxs-lookup"><span data-stu-id="7af26-125">The settings can be applied globally to all gRPC services or to an individual service implementation type.</span></span> <span data-ttu-id="7af26-126">Параметры, заданные для типов реализации отдельной службы переопределить глобальные параметры, при настройке.</span><span class="sxs-lookup"><span data-stu-id="7af26-126">Options specified for individual service implementation types override global settings when configured.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a><span data-ttu-id="7af26-127">Ведение журнала</span><span class="sxs-lookup"><span data-stu-id="7af26-127">Logging</span></span>

<span data-ttu-id="7af26-128">C-ядро на основе приложения зависят от `GrpcEnvironment` для [настроить средство ведения журнала](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) для целей отладки.</span><span class="sxs-lookup"><span data-stu-id="7af26-128">C-core-based apps rely on the `GrpcEnvironment` to [configure the logger](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) for debugging purposes.</span></span> <span data-ttu-id="7af26-129">Стек ASP.NET Core предоставляет следующие функциональные возможности через [API ведения журналов](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="7af26-129">The ASP.NET Core stack provides this functionality through the [Logging API](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="7af26-130">Например средство ведения журнала можно добавить к службе gRPC через внедрение через конструктор:</span><span class="sxs-lookup"><span data-stu-id="7af26-130">For example, a logger can be added to the gRPC service via constructor injection:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a><span data-ttu-id="7af26-131">HTTPS</span><span class="sxs-lookup"><span data-stu-id="7af26-131">HTTPS</span></span>

<span data-ttu-id="7af26-132">Приложения на основе C-core Настройка протокола HTTPS через [Server.Ports свойство](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span><span class="sxs-lookup"><span data-stu-id="7af26-132">C-core-based apps configure HTTPS through the [Server.Ports property](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports).</span></span> <span data-ttu-id="7af26-133">Настройка серверов в ASP.NET Core используется похожая концепция.</span><span class="sxs-lookup"><span data-stu-id="7af26-133">A similar concept is used to configure servers in ASP.NET Core.</span></span> <span data-ttu-id="7af26-134">Например, использует Kestrel [конфигурации конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration) для использования этой функции.</span><span class="sxs-lookup"><span data-stu-id="7af26-134">For example, Kestrel uses [endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration) for this functionality.</span></span>

## <a name="interceptors-and-middleware"></a><span data-ttu-id="7af26-135">Перехватчики и по промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="7af26-135">Interceptors and Middleware</span></span>

<span data-ttu-id="7af26-136">ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index) предлагает перехватчики в приложениях C-ядро на основе gRPC по сравнению с аналогичными функциональными возможностями.</span><span class="sxs-lookup"><span data-stu-id="7af26-136">ASP.NET Core [middleware](xref:fundamentals/middleware/index) offers similar functionalities compared to interceptors in C-core-based gRPC apps.</span></span> <span data-ttu-id="7af26-137">По промежуточного слоя и перехватчики аналогичны концептуально оба используются для создания конвейера, где обрабатывается запрос gRPC.</span><span class="sxs-lookup"><span data-stu-id="7af26-137">Middleware and interceptors are conceptually the same as both are used to construct a pipeline that handles a gRPC request.</span></span> <span data-ttu-id="7af26-138">Они оба допускают работы до или после следующему компоненту в конвейере.</span><span class="sxs-lookup"><span data-stu-id="7af26-138">They both allow work to be performed before or after the next component in the pipeline.</span></span> <span data-ttu-id="7af26-139">Тем не менее, по промежуточного слоя ASP.NET Core работает в соответствующих сообщениях HTTP/2, пока перехватчики оперируют gRPC уровень абстракции с помощью [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span><span class="sxs-lookup"><span data-stu-id="7af26-139">However, ASP.NET Core middleware operates on the underlying HTTP/2 messages, while interceptors operate on the gRPC layer of abstraction using the [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="7af26-140">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7af26-140">Additional resources</span></span>

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
