---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC Services с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 2507ce6df05403cb19e8bfa2565d410d6140b144
ms.sourcegitcommit: 73e255e846e414821b8cc20ffa3aec946735cd4e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/03/2019
ms.locfileid: "71925069"
---
# <a name="grpc-services-with-aspnet-core"></a><span data-ttu-id="15686-103">Службы gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15686-103">gRPC services with ASP.NET Core</span></span>

<span data-ttu-id="15686-104">В этом документе показано, как приступить к работе с gRPC Services с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15686-104">This document shows how to get started with gRPC services using ASP.NET Core.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="15686-105">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="15686-105">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15686-106">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15686-106">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="15686-107">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="15686-107">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="15686-108">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="15686-108">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a><span data-ttu-id="15686-109">Начало работы со службой gRPC в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15686-109">Get started with gRPC service in ASP.NET Core</span></span>

<span data-ttu-id="15686-110">[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).</span><span class="sxs-lookup"><span data-stu-id="15686-110">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="15686-111">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="15686-111">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="15686-112">Подробные инструкции по созданию проекта gRPC см. в статье Начало [работы с gRPC Services](xref:tutorials/grpc/grpc-start) .</span><span class="sxs-lookup"><span data-stu-id="15686-112">See [Get started with gRPC services](xref:tutorials/grpc/grpc-start) for detailed instructions on how to create a gRPC project.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="15686-113">Visual Studio Code/Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="15686-113">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="15686-114">Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.</span><span class="sxs-lookup"><span data-stu-id="15686-114">Run `dotnet new grpc -o GrpcGreeter` from the command line.</span></span>

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a><span data-ttu-id="15686-115">Добавление gRPC Services в приложение ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="15686-115">Add gRPC services to an ASP.NET Core app</span></span>

<span data-ttu-id="15686-116">для gRPC требуется пакет [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .</span><span class="sxs-lookup"><span data-stu-id="15686-116">gRPC requires the [Grpc.AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) package.</span></span>

### <a name="configure-grpc"></a><span data-ttu-id="15686-117">Настройка gRPC</span><span class="sxs-lookup"><span data-stu-id="15686-117">Configure gRPC</span></span>

<span data-ttu-id="15686-118">В файле *Startup.cs*:</span><span class="sxs-lookup"><span data-stu-id="15686-118">In *Startup.cs*:</span></span>

* <span data-ttu-id="15686-119">gRPC включается с помощью `AddGrpc` метода.</span><span class="sxs-lookup"><span data-stu-id="15686-119">gRPC is enabled with the `AddGrpc` method.</span></span>
* <span data-ttu-id="15686-120">Каждая служба gRPC добавляется в конвейер маршрутизации с помощью `MapGrpcService` метода.</span><span class="sxs-lookup"><span data-stu-id="15686-120">Each gRPC service is added to the routing pipeline through the `MapGrpcService` method.</span></span>

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

<span data-ttu-id="15686-121">ASP.NET Core по промежуточного слоя и компоненты совместно используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания дополнительных обработчиков запросов.</span><span class="sxs-lookup"><span data-stu-id="15686-121">ASP.NET Core middlewares and features share the routing pipeline, therefore an app can be configured to serve additional request handlers.</span></span> <span data-ttu-id="15686-122">Дополнительные обработчики запросов, такие как контроллеры MVC, работают параллельно с настроенными службами gRPC.</span><span class="sxs-lookup"><span data-stu-id="15686-122">The additional request handlers, such as MVC controllers, work in parallel with the configured gRPC services.</span></span>

### <a name="configure-kestrel"></a><span data-ttu-id="15686-123">Настройка Kestrel</span><span class="sxs-lookup"><span data-stu-id="15686-123">Configure Kestrel</span></span>

<span data-ttu-id="15686-124">Конечные точки gRPC Kestrel:</span><span class="sxs-lookup"><span data-stu-id="15686-124">Kestrel gRPC endpoints:</span></span>

* <span data-ttu-id="15686-125">Требовать HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="15686-125">Require HTTP/2.</span></span>
* <span data-ttu-id="15686-126">Следует защищать с помощью [протокола TLS](https://tools.ietf.org/html/rfc5246).</span><span class="sxs-lookup"><span data-stu-id="15686-126">Should be secured with [Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246).</span></span>

#### <a name="http2"></a><span data-ttu-id="15686-127">HTTP/2</span><span class="sxs-lookup"><span data-stu-id="15686-127">HTTP/2</span></span>

<span data-ttu-id="15686-128">для gRPC требуется HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="15686-128">gRPC requires HTTP/2.</span></span> <span data-ttu-id="15686-129">gRPC для ASP.NET Core проверяет [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) — `HTTP/2`.</span><span class="sxs-lookup"><span data-stu-id="15686-129">gRPC for ASP.NET Core validates [HttpRequest.Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) is `HTTP/2`.</span></span>

<span data-ttu-id="15686-130">Kestrel [поддерживает HTTP/2](xref:fundamentals/servers/kestrel#http2-support) в большинстве современных операционных систем.</span><span class="sxs-lookup"><span data-stu-id="15686-130">Kestrel [supports HTTP/2](xref:fundamentals/servers/kestrel#http2-support) on most modern operating systems.</span></span> <span data-ttu-id="15686-131">Конечные точки Kestrel настроены для поддержки подключений HTTP/1.1 и HTTP/2 по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="15686-131">Kestrel endpoints are configured to support HTTP/1.1 and HTTP/2 connections by default.</span></span>

#### <a name="tls"></a><span data-ttu-id="15686-132">TLS</span><span class="sxs-lookup"><span data-stu-id="15686-132">TLS</span></span>

<span data-ttu-id="15686-133">Конечные точки Kestrel, используемые для gRPC, должны быть защищены с помощью TLS.</span><span class="sxs-lookup"><span data-stu-id="15686-133">Kestrel endpoints used for gRPC should be secured with TLS.</span></span> <span data-ttu-id="15686-134">В разработке конечная точка, защищенная с помощью TLS, `https://localhost:5001` автоматически создается при наличии сертификата разработки ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="15686-134">In development, an endpoint secured with TLS is automatically created at `https://localhost:5001` when the ASP.NET Core development certificate is present.</span></span> <span data-ttu-id="15686-135">Настройка не требуется.</span><span class="sxs-lookup"><span data-stu-id="15686-135">No configuration is required.</span></span> <span data-ttu-id="15686-136">`https` Префикс проверяет, что конечная точка Kestrel использует TLS.</span><span class="sxs-lookup"><span data-stu-id="15686-136">An `https` prefix verifies the Kestrel endpoint is using TLS.</span></span>

<span data-ttu-id="15686-137">В рабочей среде протокол TLS должен быть настроен явным образом.</span><span class="sxs-lookup"><span data-stu-id="15686-137">In production, TLS must be explicitly configured.</span></span> <span data-ttu-id="15686-138">В следующем примере *appSettings. JSON* предоставляется точка HTTP/2, защищенная с помощью TLS:</span><span class="sxs-lookup"><span data-stu-id="15686-138">In the following *appsettings.json* example, an HTTP/2 endpoint secured with TLS is provided:</span></span>

[!code-json[](~/grpc/aspnetcore/sample/appsettings.json?highlight=4)]

<span data-ttu-id="15686-139">Кроме того, конечные точки Kestrel можно настроить в *Program.CS*:</span><span class="sxs-lookup"><span data-stu-id="15686-139">Alternatively, Kestrel endpoints can be configured in *Program.cs*:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/Program.cs?highlight=7&name=snippet)]

#### <a name="protocol-negotiation"></a><span data-ttu-id="15686-140">Согласование протокола</span><span class="sxs-lookup"><span data-stu-id="15686-140">Protocol negotiation</span></span>

<span data-ttu-id="15686-141">Протокол TLS используется не только для защиты обмена данными.</span><span class="sxs-lookup"><span data-stu-id="15686-141">TLS is used for more than securing communication.</span></span> <span data-ttu-id="15686-142">Подтверждение [согласования протокола приложения TLS (алпн)](https://tools.ietf.org/html/rfc7301#section-3) используется для согласования протокола подключения между клиентом и сервером, если конечная точка поддерживает несколько протоколов.</span><span class="sxs-lookup"><span data-stu-id="15686-142">The TLS [Application-Layer Protocol Negotiation (ALPN)](https://tools.ietf.org/html/rfc7301#section-3) handshake is used to negotiate the connection protocol between the client and the server when an endpoint supports multiple protocols.</span></span> <span data-ttu-id="15686-143">Это согласование определяет, использует ли соединение HTTP/1.1 или HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="15686-143">This negotiation determines whether the connection uses HTTP/1.1 or HTTP/2.</span></span>

<span data-ttu-id="15686-144">Если конечная точка HTTP/2 настроена без TLS, [листеноптионс. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) должна иметь значение `HttpProtocols.Http2`.</span><span class="sxs-lookup"><span data-stu-id="15686-144">If an HTTP/2 endpoint is configured without TLS, the endpoint's [ListenOptions.Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) must be set to `HttpProtocols.Http2`.</span></span> <span data-ttu-id="15686-145">Конечная точка с несколькими протоколами (например `HttpProtocols.Http1AndHttp2`,) не может использоваться без TLS, так как отсутствует согласование.</span><span class="sxs-lookup"><span data-stu-id="15686-145">An endpoint with multiple protocols (for example, `HttpProtocols.Http1AndHttp2`) can't be used without TLS because there is no negotiation.</span></span> <span data-ttu-id="15686-146">Все подключения к незащищенной конечной точке по умолчанию заключаются в HTTP/1.1, а вызовы gRPC завершаются ошибкой.</span><span class="sxs-lookup"><span data-stu-id="15686-146">All connections to the unsecured endpoint default to HTTP/1.1, and gRPC calls fail.</span></span>

<span data-ttu-id="15686-147">Дополнительные сведения о включении HTTP/2 и TLS с помощью Kestrel см. в разделе [Конфигурация конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="15686-147">For more information on enabling HTTP/2 and TLS with Kestrel, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

> [!NOTE]
> <span data-ttu-id="15686-148">macOS не поддерживает ASP.NET Core gRPC с TLS.</span><span class="sxs-lookup"><span data-stu-id="15686-148">macOS doesn't support ASP.NET Core gRPC with TLS.</span></span> <span data-ttu-id="15686-149">Для успешного запуска служб gRPC в macOS требуется дополнительная настройка.</span><span class="sxs-lookup"><span data-stu-id="15686-149">Additional configuration is required to successfully run gRPC services on macOS.</span></span> <span data-ttu-id="15686-150">Дополнительные сведения см. в статье [Не удается запустить приложение ASP.NET Core gRPC в macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span><span class="sxs-lookup"><span data-stu-id="15686-150">For more information, see [Unable to start ASP.NET Core gRPC app on macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).</span></span>

## <a name="integration-with-aspnet-core-apis"></a><span data-ttu-id="15686-151">Интеграция с ASP.NET Core API</span><span class="sxs-lookup"><span data-stu-id="15686-151">Integration with ASP.NET Core APIs</span></span>

<span data-ttu-id="15686-152">gRPC Services имеют полный доступ к ASP.NET Coreным функциям, таким как [внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="15686-152">gRPC services have full access to the ASP.NET Core features such as [Dependency Injection](xref:fundamentals/dependency-injection) (DI) and [Logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="15686-153">Например, реализация службы может разрешить службу ведения журнала из контейнера DI с помощью конструктора:</span><span class="sxs-lookup"><span data-stu-id="15686-153">For example, the service implementation can resolve a logger service from the DI container via the constructor:</span></span>

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

<span data-ttu-id="15686-154">По умолчанию реализация службы gRPC может разрешать другие службы DI с любым временем существования (singleton, Scope или временно).</span><span class="sxs-lookup"><span data-stu-id="15686-154">By default, the gRPC service implementation can resolve other DI services with any lifetime (Singleton, Scoped, or Transient).</span></span>

### <a name="resolve-httpcontext-in-grpc-methods"></a><span data-ttu-id="15686-155">Разрешение HttpContext в методах gRPC</span><span class="sxs-lookup"><span data-stu-id="15686-155">Resolve HttpContext in gRPC methods</span></span>

<span data-ttu-id="15686-156">API gRPC предоставляет доступ к некоторым данным сообщений HTTP/2, таким как метод, узел, заголовок и трейлер.</span><span class="sxs-lookup"><span data-stu-id="15686-156">The gRPC API provides access to some HTTP/2 message data, such as the method, host, header, and trailers.</span></span> <span data-ttu-id="15686-157">Доступ осуществляется посредством `ServerCallContext` аргумента, передаваемого в каждый метод gRPC:</span><span class="sxs-lookup"><span data-stu-id="15686-157">Access is through the `ServerCallContext` argument passed to each gRPC method:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

<span data-ttu-id="15686-158">`ServerCallContext`не предоставляет полный доступ ко `HttpContext` всем API-интерфейсам ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="15686-158">`ServerCallContext` does not provide full access to `HttpContext` in all ASP.NET APIs.</span></span> <span data-ttu-id="15686-159">Метод расширения предоставляет полный доступ к элементу `HttpContext` , представляющему базовое сообщение HTTP/2 в API-интерфейсах ASP.NET: `GetHttpContext`</span><span class="sxs-lookup"><span data-stu-id="15686-159">The `GetHttpContext` extension method provides full access to the `HttpContext` representing the underlying HTTP/2 message in ASP.NET APIs:</span></span>

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

[!INCLUDE[](~/includes/gRPCazure.md)]

## <a name="additional-resources"></a><span data-ttu-id="15686-160">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="15686-160">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
