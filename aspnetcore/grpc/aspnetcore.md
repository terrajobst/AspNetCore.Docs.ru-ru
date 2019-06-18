---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Изучите основные принципы, при создании gRPC служб с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: ca06478e6168c59d9abf43d99213fa8a7091e178
ms.sourcegitcommit: 756114cab5e24e99ec23de62b0c1c16b15197ac2
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/18/2019
ms.locfileid: "67169523"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="855c6-103">Службы gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="855c6-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="855c6-104">В этом документе показано, как приступить к работе со службами gRPC, с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="855c6-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="855c6-105">Начало работы со службой gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="855c6-105">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="855c6-106">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="855c6-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="855c6-107">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="855c6-107">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="855c6-108">См. в разделе [приступить к работе со службами gRPC](xref:tutorials/grpc/grpc-start) подробные инструкции о том, как создать проект gRPC.</span><span class="sxs-lookup"><span data-stu-id="855c6-108">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="855c6-109">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="855c6-109">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="855c6-110">Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="855c6-110">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="855c6-111">Добавление служб gRPC в приложения ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="855c6-111">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="855c6-112">gRPC требует наличия следующих пакетов:</span><span class="sxs-lookup"><span data-stu-id="855c6-112">gRPC requires the following packages:</span></span>

* [<span data-ttu-id="855c6-113">Grpc.AspNetCore.Server</span><span class="sxs-lookup"><span data-stu-id="855c6-113">Grpc.AspNetCore.Server</span></span>](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* <span data-ttu-id="855c6-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) для protobuf сообщений API-интерфейсы.</span><span class="sxs-lookup"><span data-stu-id="855c6-114">[Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) for protobuf message APIs.</span></span>
* [<span data-ttu-id="855c6-115">Grpc.Tools</span><span class="sxs-lookup"><span data-stu-id="855c6-115">Grpc.Tools</span></span>](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a><span data-ttu-id="855c6-116">Настройка gRPC</span><span class="sxs-lookup"><span data-stu-id="855c6-116">Configure gRPC</span></span>

<span data-ttu-id="855c6-117">gRPC включен с `AddGrpc` метод:</span><span class="sxs-lookup"><span data-stu-id="855c6-117">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="855c6-118">Каждая служба gRPC добавляется в конвейер маршрутизации через `MapGrpcService` метод:</span><span class="sxs-lookup"><span data-stu-id="855c6-118">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="855c6-119">По промежуточного слоя ASP.NET Core и компоненты используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания запроса дополнительные обработчики.</span><span class="sxs-lookup"><span data-stu-id="855c6-119">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="855c6-120">Обработчики дополнительный запрос, например, контроллеры MVC работать параллельно со службами настроенных gRPC.</span><span class="sxs-lookup"><span data-stu-id="855c6-120">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="855c6-121">Интеграция с ASP.NET Core API-интерфейсов</span><span class="sxs-lookup"><span data-stu-id="855c6-121">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="855c6-122">gRPC службы имеют полный доступ к возможностям ASP.NET Core, такие как [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="855c6-122">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="855c6-123">Например реализация службы можно разрешить службу средства ведения журнала из контейнера внедрения Зависимостей через конструктор:</span><span class="sxs-lookup"><span data-stu-id="855c6-123">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="855c6-124">По умолчанию реализация службы gRPC можно разрешить другим службам внедрения Зависимостей с помощью любого времени существования (одноэлементный Scoped и временные).</span><span class="sxs-lookup"><span data-stu-id="855c6-124">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="855c6-125">Разрешить HttpContext в методах gRPC</span><span class="sxs-lookup"><span data-stu-id="855c6-125">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="855c6-126">GRPC API предоставляет доступ к некоторым данным сообщения HTTP/2, например метод, узла, заголовок и прицепов.</span><span class="sxs-lookup"><span data-stu-id="855c6-126">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="855c6-127">Доступ осуществляется через `ServerCallContext` аргумент, переданный к каждому методу gRPC:</span><span class="sxs-lookup"><span data-stu-id="855c6-127">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="855c6-128">`ServerCallContext` не предоставляет полный доступ к `HttpContext` в интерфейсах для ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="855c6-128">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="855c6-129">`GetHttpContext` Метод расширения предоставляет полный доступ к `HttpContext` представляет базовое сообщение HTTP/2 в интерфейсах API ASP.NET:</span><span class="sxs-lookup"><span data-stu-id="855c6-129">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="855c6-130">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="855c6-130">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
