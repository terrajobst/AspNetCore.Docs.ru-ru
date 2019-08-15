---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC Services с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 38111c152c581c50767f9cd4e5fa257bd3fd804e
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022313"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="7950e-103">Службы gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7950e-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="7950e-104">В этом документе показано, как приступить к работе с gRPC Services с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7950e-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7950e-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="7950e-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7950e-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7950e-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="7950e-107">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="7950e-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="7950e-108">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7950e-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="7950e-109">Начало работы со службой gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7950e-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="7950e-110">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="7950e-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="7950e-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="7950e-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="7950e-112">Подробные инструкции по созданию проекта gRPC см. в статье Начало [работы с gRPC Services](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="7950e-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="7950e-113">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="7950e-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="7950e-114">Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="7950e-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="7950e-115">Добавление gRPC Services в приложение ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7950e-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="7950e-116">для gRPC требуется пакет [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="7950e-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="7950e-117">Настройка gRPC</span><span class="sxs-lookup"><span data-stu-id="7950e-117">Configure gRPC</span></span>

<span data-ttu-id="7950e-118">gRPC включается с помощью `AddGrpc` метода:</span><span class="sxs-lookup"><span data-stu-id="7950e-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="7950e-119">Каждая служба gRPC добавляется в конвейер маршрутизации с помощью `MapGrpcService` метода:</span><span class="sxs-lookup"><span data-stu-id="7950e-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="7950e-120">ASP.NET Core по промежуточного слоя и компоненты совместно используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания дополнительных обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="7950e-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="7950e-121">Дополнительные обработчики запросов, такие как контроллеры MVC, работают параллельно с настроенными службами gRPC.</span><span class="sxs-lookup"><span data-stu-id="7950e-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="7950e-122">Интеграция с ASP.NET Core API</span><span class="sxs-lookup"><span data-stu-id="7950e-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="7950e-123">gRPC Services имеют полный доступ к ASP.NET Coreным функциям, таким как [внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="7950e-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="7950e-124">Например, реализация службы может разрешить службу ведения журнала из контейнера DI с помощью конструктора:</span><span class="sxs-lookup"><span data-stu-id="7950e-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="7950e-125">По умолчанию реализация службы gRPC может разрешать другие службы DI с любым временем существования (singleton, Scope или временно).</span><span class="sxs-lookup"><span data-stu-id="7950e-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="7950e-126">Разрешение HttpContext в методах gRPC</span><span class="sxs-lookup"><span data-stu-id="7950e-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="7950e-127">API gRPC предоставляет доступ к некоторым данным сообщений HTTP/2, таким как метод, узел, заголовок и трейлер.</span><span class="sxs-lookup"><span data-stu-id="7950e-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="7950e-128">Доступ осуществляется посредством `ServerCallContext` аргумента, передаваемого в каждый метод gRPC:</span><span class="sxs-lookup"><span data-stu-id="7950e-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="7950e-129">`ServerCallContext`не предоставляет полный доступ ко `HttpContext` всем API-интерфейсам ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="7950e-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="7950e-130">Метод расширения предоставляет полный доступ к элементу `HttpContext` , представляющему базовое сообщение HTTP/2 в API-интерфейсах ASP.NET: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="7950e-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="7950e-131">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7950e-131">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
