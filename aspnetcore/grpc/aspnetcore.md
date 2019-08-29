---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC Services с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/28/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 128f5b36eac9112460c33693db5537134a077476
ms.sourcegitcommit: 23f79bd71d49c4efddb56377c1f553cc993d781b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/29/2019
ms.locfileid: "70130706"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="474f2-103">Службы gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="474f2-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="474f2-104">В этом документе показано, как приступить к работе с gRPC Services с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="474f2-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="474f2-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="474f2-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="474f2-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="474f2-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="474f2-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="474f2-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="474f2-108">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="474f2-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="474f2-109">Начало работы со службой gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="474f2-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="474f2-110">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="474f2-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="474f2-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="474f2-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="474f2-112">Подробные инструкции по созданию проекта gRPC см. в статье Начало [работы с gRPC Services](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="474f2-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="474f2-113">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="474f2-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="474f2-114">Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="474f2-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="474f2-115">Добавление gRPC Services в приложение ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="474f2-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="474f2-116">для gRPC требуется пакет [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="474f2-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="474f2-117">Настройка gRPC</span><span class="sxs-lookup"><span data-stu-id="474f2-117">Configure gRPC</span></span>

<span data-ttu-id="474f2-118">В файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="474f2-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="474f2-119">gRPC включается с помощью `AddGrpc` метода.</span><span class="sxs-lookup"><span data-stu-id="474f2-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="474f2-120">Каждая служба gRPC добавляется в конвейер маршрутизации с помощью `MapGrpcService` метода.</span><span class="sxs-lookup"><span data-stu-id="474f2-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="474f2-121">ASP.NET Core по промежуточного слоя и компоненты совместно используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания дополнительных обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="474f2-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="474f2-122">Дополнительные обработчики запросов, такие как контроллеры MVC, работают параллельно с настроенными службами gRPC.</span><span class="sxs-lookup"><span data-stu-id="474f2-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="474f2-123">Настройка Kestrel</span><span class="sxs-lookup"><span data-stu-id="474f2-123">Configure Kestrel</span></span>

<span data-ttu-id="474f2-124">Конечные точки gRPC Kestrel:</span><span class="sxs-lookup"><span data-stu-id="474f2-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="474f2-125">Требовать HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="474f2-125">Require HTTP/2.</span></span>
* <span data-ttu-id="474f2-126">Должен быть защищен с помощью протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="474f2-126">Should be secured with HTTPS.</span></span>

#### <a name="http2"></a><span data-ttu-id="474f2-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="474f2-127">HTTP/2</span></span>

<span data-ttu-id="474f2-128">Kestrel [поддерживает HTTP/2](xref:fundamentals/servers/kestrel#http2-support) в большинстве современных операционных систем.</span><span class="sxs-lookup"><span data-stu-id="474f2-128">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="474f2-129">Конечные точки Kestrel настроены для поддержки подключений HTTP/1.1 и HTTP/2 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="474f2-129">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

> [!NOTE]
> <span data-ttu-id="474f2-130">macOS не поддерживает ASP.NET Core gRPC с [протоколом TLS](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="474f2-130">macOS doesn't support ASP.NET Core gRPC with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span> <span data-ttu-id="474f2-131">Для успешного запуска служб gRPC в macOS требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="474f2-131">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="474f2-132">Дополнительные сведения см. в статье [Не удается запустить приложение ASP.NET Core gRPC в macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="474f2-132">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

#### <a name="https"></a><span data-ttu-id="474f2-133">HTTPS</span><span class="sxs-lookup"><span data-stu-id="474f2-133">HTTPS</span></span>

<span data-ttu-id="474f2-134">Конечные точки Kestrel, используемые для gRPC, должны быть защищены с помощью протокола HTTPS.</span><span class="sxs-lookup"><span data-stu-id="474f2-134">Kestrel endpoints used for gRPC should be secured with HTTPS.</span></span> <span data-ttu-id="474f2-135">В процессе разработки конечная точка HTTPS создается `https://localhost:5001` автоматически при наличии сертификата разработки ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="474f2-135">In development, an HTTPS endpoint is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="474f2-136">Настройка не требуется.</span><span class="sxs-lookup"><span data-stu-id="474f2-136">No configuration is required.</span></span>

<span data-ttu-id="474f2-137">В рабочей среде необходимо явно настроить HTTPS.</span><span class="sxs-lookup"><span data-stu-id="474f2-137">In production, HTTPS must be explicitly configured.</span></span> <span data-ttu-id="474f2-138">В следующем примере *appSettings. JSON* предоставляется точка HTTP/2, защищенная с помощью HTTPS:</span><span class="sxs-lookup"><span data-stu-id="474f2-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with HTTPS is provided:</span></span>

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

<span data-ttu-id="474f2-139">Кроме того, Kestrel ендспоинтс можно настроить в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="474f2-139">Alternatively, Kestrel endspoints can be configured in *Program.cs*:</span></span>

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

<span data-ttu-id="474f2-140">Дополнительные сведения о включении HTTP/2 и HTTPS с помощью Kestrel см. в разделе [Конфигурация конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="474f2-140">For more information on enabling HTTP/2 and HTTPS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="474f2-141">Интеграция с ASP.NET Core API</span><span class="sxs-lookup"><span data-stu-id="474f2-141">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="474f2-142">gRPC Services имеют полный доступ к ASP.NET Coreным функциям, таким как [внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="474f2-142">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="474f2-143">Например, реализация службы может разрешить службу ведения журнала из контейнера DI с помощью конструктора:</span><span class="sxs-lookup"><span data-stu-id="474f2-143">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="474f2-144">По умолчанию реализация службы gRPC может разрешать другие службы DI с любым временем существования (singleton, Scope или временно).</span><span class="sxs-lookup"><span data-stu-id="474f2-144">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="474f2-145">Разрешение HttpContext в методах gRPC</span><span class="sxs-lookup"><span data-stu-id="474f2-145">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="474f2-146">API gRPC предоставляет доступ к некоторым данным сообщений HTTP/2, таким как метод, узел, заголовок и трейлер.</span><span class="sxs-lookup"><span data-stu-id="474f2-146">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="474f2-147">Доступ осуществляется посредством `ServerCallContext` аргумента, передаваемого в каждый метод gRPC:</span><span class="sxs-lookup"><span data-stu-id="474f2-147">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="474f2-148">`ServerCallContext`не предоставляет полный доступ ко `HttpContext` всем API-интерфейсам ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="474f2-148">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="474f2-149">Метод расширения предоставляет полный доступ к элементу `HttpContext` , представляющему базовое сообщение HTTP/2 в API-интерфейсах ASP.NET: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="474f2-149">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="474f2-150">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="474f2-150">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
