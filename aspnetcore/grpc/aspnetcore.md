---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC Services с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862931"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="4f776-103">Службы gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f776-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="4f776-104">В этом документе показано, как приступить к работе с gRPC Services с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="4f776-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="4f776-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="4f776-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f776-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f776-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="4f776-107">Visual Studio Code.</span><span class="sxs-lookup"><span data-stu-id="4f776-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="4f776-108">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f776-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="4f776-109">Начало работы со службой gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f776-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="4f776-110">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="4f776-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="4f776-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="4f776-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="4f776-112">Подробные инструкции по созданию проекта gRPC см. в статье Начало [работы с gRPC Services](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="4f776-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="4f776-113">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="4f776-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="4f776-114">Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="4f776-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="4f776-115">Добавление gRPC Services в приложение ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="4f776-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="4f776-116">для gRPC требуется пакет [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="4f776-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="4f776-117">Настройка gRPC</span><span class="sxs-lookup"><span data-stu-id="4f776-117">Configure gRPC</span></span>

<span data-ttu-id="4f776-118">gRPC включается с помощью `AddGrpc` метода:</span><span class="sxs-lookup"><span data-stu-id="4f776-118">gRPC is enabled with the `AddGrpc` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

<span data-ttu-id="4f776-119">Каждая служба gRPC добавляется в конвейер маршрутизации с помощью `MapGrpcService` метода:</span><span class="sxs-lookup"><span data-stu-id="4f776-119">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method:</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

<span data-ttu-id="4f776-120">ASP.NET Core по промежуточного слоя и компоненты совместно используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания дополнительных обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="4f776-120">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="4f776-121">Дополнительные обработчики запросов, такие как контроллеры MVC, работают параллельно с настроенными службами gRPC.</span><span class="sxs-lookup"><span data-stu-id="4f776-121">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="4f776-122">Интеграция с ASP.NET Core API</span><span class="sxs-lookup"><span data-stu-id="4f776-122">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="4f776-123">gRPC Services имеют полный доступ к ASP.NET Coreным функциям, таким как [внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="4f776-123">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="4f776-124">Например, реализация службы может разрешить службу ведения журнала из контейнера DI с помощью конструктора:</span><span class="sxs-lookup"><span data-stu-id="4f776-124">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="4f776-125">По умолчанию реализация службы gRPC может разрешать другие службы DI с любым временем существования (singleton, Scope или временно).</span><span class="sxs-lookup"><span data-stu-id="4f776-125">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="4f776-126">Разрешение HttpContext в методах gRPC</span><span class="sxs-lookup"><span data-stu-id="4f776-126">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="4f776-127">API gRPC предоставляет доступ к некоторым данным сообщений HTTP/2, таким как метод, узел, заголовок и трейлер.</span><span class="sxs-lookup"><span data-stu-id="4f776-127">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="4f776-128">Доступ осуществляется посредством `ServerCallContext` аргумента, передаваемого в каждый метод gRPC:</span><span class="sxs-lookup"><span data-stu-id="4f776-128">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="4f776-129">`ServerCallContext`не предоставляет полный доступ ко `HttpContext` всем API-интерфейсам ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="4f776-129">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="4f776-130">Метод расширения предоставляет полный доступ к элементу `HttpContext` , представляющему базовое сообщение HTTP/2 в API-интерфейсах ASP.NET: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="4f776-130">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="grpc-and-aspnet-core-on-macos"></a><span data-ttu-id="4f776-131">gRPC и ASP.NET Core на macOS</span><span class="sxs-lookup"><span data-stu-id="4f776-131">gRPC and ASP.NET Core on macOS</span></span>

<span data-ttu-id="4f776-132">Kestrel не поддерживает HTTP/2 с протоколом [TLS](https://tools.ietf.org/html/rfc5246) в macOS.</span><span class="sxs-lookup"><span data-stu-id="4f776-132">Kestrel doesn't support HTTP/2 with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) on macOS.</span></span> <span data-ttu-id="4f776-133">Шаблон ASP.NET Core gRPC и примеры по умолчанию используют протокол TLS.</span><span class="sxs-lookup"><span data-stu-id="4f776-133">The ASP.NET Core gRPC template and samples use TLS by default.</span></span> <span data-ttu-id="4f776-134">При попытке запустить сервер gRPC появится следующее сообщение об ошибке:</span><span class="sxs-lookup"><span data-stu-id="4f776-134">You'll see the following error message when you attempt to start the gRPC server:</span></span>

> <span data-ttu-id="4f776-135">Не удалось выполнить привязку к https://localhost:5001 интерфейсу замыкания на себя IPv4: "HTTP/2 через TLS не поддерживается в macOS из-за отсутствия поддержки АЛПН".</span><span class="sxs-lookup"><span data-stu-id="4f776-135">Unable to bind to https://localhost:5001 on the IPv4 loopback interface: 'HTTP/2 over TLS is not supported on macOS due to missing ALPN support.'.</span></span>

<span data-ttu-id="4f776-136">Чтобы обойти эту ошибку, настройте Kestrel и клиент gRPC для использования HTTP/2 **без** TLS.</span><span class="sxs-lookup"><span data-stu-id="4f776-136">To work around this issue, configure Kestrel and the gRPC client to use HTTP/2 **without** TLS.</span></span> <span data-ttu-id="4f776-137">Это следует делать только во время разработки.</span><span class="sxs-lookup"><span data-stu-id="4f776-137">You should only do this during development.</span></span> <span data-ttu-id="4f776-138">Отсутствие использования TLS приведет к тому, что сообщения gRPC будут отправляться без шифрования.</span><span class="sxs-lookup"><span data-stu-id="4f776-138">Not using TLS will result in gRPC messages being sent without encryption.</span></span>

<span data-ttu-id="4f776-139">Kestrel должен настроить конечную точку HTTP/2 без TLS `Program.cs`в:</span><span class="sxs-lookup"><span data-stu-id="4f776-139">Kestrel must configure a HTTP/2 endpoint without TLS in `Program.cs`:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="4f776-140">Клиент gRPC должен задать `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` для `true` параметра значение и использовать `http` в адресе сервера:</span><span class="sxs-lookup"><span data-stu-id="4f776-140">The gRPC client must set the `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` switch to `true` and use `http` in the server address:</span></span>

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> <span data-ttu-id="4f776-141">HTTP/2 без TLS следует использовать только во время разработки приложений.</span><span class="sxs-lookup"><span data-stu-id="4f776-141">HTTP/2 without TLS should only be used during app development.</span></span> <span data-ttu-id="4f776-142">В рабочих приложениях всегда следует использовать безопасность транспорта.</span><span class="sxs-lookup"><span data-stu-id="4f776-142">Production applications should always use transport security.</span></span> <span data-ttu-id="4f776-143">Дополнительные сведения см. [в разделе вопросы безопасности в gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span><span class="sxs-lookup"><span data-stu-id="4f776-143">For more information, see [Security considerations in gRPC for ASP.NET Core](xref:grpc/security#transport-security).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4f776-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="4f776-144">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
