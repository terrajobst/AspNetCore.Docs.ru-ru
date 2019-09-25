---
title: Ведение журналов и диагностика в gRPC на .NET
author: jamesnk
description: Узнайте, как собирать диагностические сведения из приложения gRPC на платформе .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250740"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="5a0b9-103">Ведение журналов и диагностика в gRPC на .NET</span><span class="sxs-lookup"><span data-stu-id="5a0b9-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="5a0b9-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="5a0b9-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="5a0b9-105">В этой статье приводятся рекомендации по сбору диагностических данных из приложения gRPC для устранения проблем.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-105">This article provides guidance for gathering diagnostics from your gRPC app to help troubleshoot issues.</span></span>

## <a name="grpc-services-logging"></a><span data-ttu-id="5a0b9-106">ведение журнала gRPC Services</span><span class="sxs-lookup"><span data-stu-id="5a0b9-106">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="5a0b9-107">Журналы на стороне сервера могут содержать конфиденциальные сведения из приложения.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-107">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="5a0b9-108">**Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-108">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="5a0b9-109">Поскольку службы gRPC размещаются на ASP.NET Core, используется система ведения журнала ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-109">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="5a0b9-110">В конфигурации по умолчанию gRPC регистрирует очень мало информации, но это может быть настроено.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-110">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="5a0b9-111">Дополнительные сведения о настройке ведения журнала ASP.NET Core см. в документации по [ASP.NET Core ведению журнала](xref:fundamentals/logging/index#configuration) .</span><span class="sxs-lookup"><span data-stu-id="5a0b9-111">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="5a0b9-112">gRPC добавляет журналы в `Grpc` категорию.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-112">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="5a0b9-113">Чтобы включить `Grpc` подробные журналы из gRPC, настройте префиксы `Debug` для уровня в файле *appSettings. JSON* `LogLevel` , добавив следующие элементы в подраздел в `Logging`:</span><span class="sxs-lookup"><span data-stu-id="5a0b9-113">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

<span data-ttu-id="5a0b9-114">Это также можно настроить в *Startup.CS* с помощью `ConfigureLogging`:</span><span class="sxs-lookup"><span data-stu-id="5a0b9-114">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

<span data-ttu-id="5a0b9-115">Если конфигурация на основе JSON не используется, установите следующее значение конфигурации в системе конфигурации:</span><span class="sxs-lookup"><span data-stu-id="5a0b9-115">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="5a0b9-116">Ознакомьтесь с документацией по системе конфигурации, чтобы определить, как указать вложенные значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-116">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="5a0b9-117">Например, при использовании переменных среды вместо параметра `_` `:` (например, `Logging__LogLevel__Grpc`) используются два символа.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-117">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="5a0b9-118">Рекомендуется использовать `Debug` уровень при сборе более подробных диагностических сведений о приложении.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-118">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="5a0b9-119">`Trace` Уровень обеспечивает очень низкую диагностику и редко требуется для диагностики проблем в приложении.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-119">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

### <a name="sample-logging-output"></a><span data-ttu-id="5a0b9-120">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="5a0b9-120">Sample logging output</span></span>

<span data-ttu-id="5a0b9-121">Ниже приведен пример вывода на консоль на `Debug` уровне службы gRPC:</span><span class="sxs-lookup"><span data-stu-id="5a0b9-121">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

```console
info: Microsoft.AspNetCore.Hosting.Diagnostics[1]
      Request starting HTTP/2 POST https://localhost:5001/Greet.Greeter/SayHello application/grpc
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[0]
      Executing endpoint 'gRPC - /Greet.Greeter/SayHello'
dbug: Grpc.AspNetCore.Server.ServerCallHandler[1]
      Reading message.
info: GrpcService.GreeterService[0]
      Hello World
dbug: Grpc.AspNetCore.Server.ServerCallHandler[6]
      Sending message.
info: Microsoft.AspNetCore.Routing.EndpointMiddleware[1]
      Executed endpoint 'gRPC - /Greet.Greeter/SayHello'
info: Microsoft.AspNetCore.Hosting.Diagnostics[2]
      Request finished in 1.4113ms 200 application/grpc
```

## <a name="access-server-side-logs"></a><span data-ttu-id="5a0b9-122">Доступ к журналам на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="5a0b9-122">Access server-side logs</span></span>

<span data-ttu-id="5a0b9-123">Доступ к журналам на стороне сервера зависит от среды, в которой выполняется.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-123">How you access server-side logs depends on the environment in which you're running.</span></span>

### <a name="as-a-console-app"></a><span data-ttu-id="5a0b9-124">Как консольное приложение</span><span class="sxs-lookup"><span data-stu-id="5a0b9-124">As a console app</span></span>

<span data-ttu-id="5a0b9-125">Если вы работаете в консольном приложении, [средство ведения журнала консоли](xref:fundamentals/logging/index#console-provider) должно быть включено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-125">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="5a0b9-126">журналы gRPC будут отображаться в консоли.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-126">gRPC logs will appear in the console.</span></span>

### <a name="other-environments"></a><span data-ttu-id="5a0b9-127">Другие среды</span><span class="sxs-lookup"><span data-stu-id="5a0b9-127">Other environments</span></span>

<span data-ttu-id="5a0b9-128">Если приложение развертывается в другой среде (например, Docker, Kubernetes или службе Windows), см <xref:fundamentals/logging/index> . Дополнительные сведения о настройке регистраторов, подходящих для среды.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-128">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

## <a name="grpc-client-logging"></a><span data-ttu-id="5a0b9-129">ведение журнала клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="5a0b9-129">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="5a0b9-130">Журналы на стороне клиента могут содержать конфиденциальные сведения из приложения.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-130">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="5a0b9-131">**Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-131">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="5a0b9-132">Чтобы получить журналы из клиента .NET, можно задать `GrpcChannelOptions.LoggerFactory` свойство при создании канала клиента.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-132">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="5a0b9-133">Если вы вызываете службу gRPC из приложения ASP.NET Core, фабрика журнала может быть разрешена путем внедрения зависимостей (DI):</span><span class="sxs-lookup"><span data-stu-id="5a0b9-133">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="5a0b9-134">Альтернативный способ включения ведения журнала клиента — использование [фабрики клиента gRPC](xref:grpc/clientfactory) для создания клиента.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-134">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="5a0b9-135">Клиент gRPC, зарегистрированный в фабрике клиента и разрешенный из DI, автоматически будет использовать настроенное для приложения ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-135">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="5a0b9-136">Если приложение не использует di, можно создать новый `ILoggerFactory` экземпляр с помощью [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span><span class="sxs-lookup"><span data-stu-id="5a0b9-136">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="5a0b9-137">Чтобы получить доступ к этому методу, добавьте в приложение пакет [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) .</span><span class="sxs-lookup"><span data-stu-id="5a0b9-137">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a><span data-ttu-id="5a0b9-138">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="5a0b9-138">Sample logging output</span></span>

<span data-ttu-id="5a0b9-139">Ниже приведен пример вывода на консоль на `Debug` уровне клиента gRPC.</span><span class="sxs-lookup"><span data-stu-id="5a0b9-139">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

```console
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Starting gRPC call. Method type: 'Unary', URI: 'https://localhost:5001/Greet.Greeter/SayHello'.
dbug: Grpc.Net.Client.Internal.GrpcCall[6]
      Sending message.
dbug: Grpc.Net.Client.Internal.GrpcCall[1]
      Reading message.
dbug: Grpc.Net.Client.Internal.GrpcCall[4]
      Finished gRPC call.
```

## <a name="additional-resources"></a><span data-ttu-id="5a0b9-140">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="5a0b9-140">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
