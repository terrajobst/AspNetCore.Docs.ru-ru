---
title: Ведение журналов и диагностика в gRPC на .NET
author: jamesnk
description: Узнайте, как собирать диагностические сведения из приложения gRPC на платформе .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 17607b734e6d777de9516aa14e81c97f87b61023
ms.sourcegitcommit: 80286715afb93c4d13c931b008016d6086c0312b
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/07/2020
ms.locfileid: "77074527"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a><span data-ttu-id="fa95e-103">Ведение журналов и диагностика в gRPC на .NET</span><span class="sxs-lookup"><span data-stu-id="fa95e-103">Logging and diagnostics in gRPC on .NET</span></span>

<span data-ttu-id="fa95e-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="fa95e-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="fa95e-105">В этой статье приводятся рекомендации по сбору диагностических данных из приложения gRPC для устранения проблем.</span><span class="sxs-lookup"><span data-stu-id="fa95e-105">This article provides guidance for gathering diagnostics from a gRPC app to help troubleshoot issues.</span></span> <span data-ttu-id="fa95e-106">Здесь рассматриваются такие темы:</span><span class="sxs-lookup"><span data-stu-id="fa95e-106">Topics covered include:</span></span>

* <span data-ttu-id="fa95e-107">Журналы, структурированные **с ведением журнала** , записываются в [журнал .NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="fa95e-107">**Logging** - Structured logs written to [.NET Core logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="fa95e-108"><xref:Microsoft.Extensions.Logging.ILogger> используется платформами приложений для записи журналов и пользователей для их собственного ведения журнала в приложении.</span><span class="sxs-lookup"><span data-stu-id="fa95e-108"><xref:Microsoft.Extensions.Logging.ILogger> is used by app frameworks to write logs, and by users for their own logging in an app.</span></span>
* <span data-ttu-id="fa95e-109">**Трассировка** — события, связанные с операцией, написанной с помощью `DiaganosticSource` и `Activity`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-109">**Tracing** - Events related to an operation written using `DiaganosticSource` and `Activity`.</span></span> <span data-ttu-id="fa95e-110">Трассировки из источника диагностики обычно используются для сбора данных телеметрии приложений с помощью таких библиотек, как [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) и [опентелеметри](https://github.com/open-telemetry/opentelemetry-dotnet).</span><span class="sxs-lookup"><span data-stu-id="fa95e-110">Traces from diagnostic source are commonly used to collect app telemetry by libraries such as [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) and [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet).</span></span>
* <span data-ttu-id="fa95e-111">**Метрики** — представление мер данных за интервалы времени, например количество запросов в секунду.</span><span class="sxs-lookup"><span data-stu-id="fa95e-111">**Metrics** - Representation of data measures over intervals of time, for example, requests per second.</span></span> <span data-ttu-id="fa95e-112">Метрики создаются с помощью `EventCounter` и могут быть просмотрены с помощью средства командной строки [DotNet-Counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) или с помощью [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).</span><span class="sxs-lookup"><span data-stu-id="fa95e-112">Metrics are emitted using `EventCounter` and can be observed using [dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) command line tool or with [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).</span></span>

## <a name="logging"></a><span data-ttu-id="fa95e-113">Logging</span><span class="sxs-lookup"><span data-stu-id="fa95e-113">Logging</span></span>

<span data-ttu-id="fa95e-114">gRPC Services и клиент gRPC пишут журналы с помощью [ведения журнала .NET Core](xref:fundamentals/logging/index).</span><span class="sxs-lookup"><span data-stu-id="fa95e-114">gRPC services and the gRPC client write logs using [.NET Core logging](xref:fundamentals/logging/index).</span></span> <span data-ttu-id="fa95e-115">Журналы являются хорошим местом для запуска при необходимости отладки непредвиденного поведения в приложениях.</span><span class="sxs-lookup"><span data-stu-id="fa95e-115">Logs are a good place to start when you need to debug unexpected behavior in your apps.</span></span>

### <a name="grpc-services-logging"></a><span data-ttu-id="fa95e-116">ведение журнала gRPC Services</span><span class="sxs-lookup"><span data-stu-id="fa95e-116">gRPC services logging</span></span>

> [!WARNING]
> <span data-ttu-id="fa95e-117">Журналы на стороне сервера могут содержать конфиденциальные сведения из приложения.</span><span class="sxs-lookup"><span data-stu-id="fa95e-117">Server-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="fa95e-118">**Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.</span><span class="sxs-lookup"><span data-stu-id="fa95e-118">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="fa95e-119">Поскольку службы gRPC размещаются на ASP.NET Core, используется система ведения журнала ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="fa95e-119">Since gRPC services are hosted on ASP.NET Core, it uses the ASP.NET Core logging system.</span></span> <span data-ttu-id="fa95e-120">В конфигурации по умолчанию gRPC регистрирует очень мало информации, но это может быть настроено.</span><span class="sxs-lookup"><span data-stu-id="fa95e-120">In the default configuration, gRPC logs very little information, but this can configured.</span></span> <span data-ttu-id="fa95e-121">Дополнительные сведения о настройке ведения журнала ASP.NET Core см. в документации по [ASP.NET Core ведению журнала](xref:fundamentals/logging/index#configuration) .</span><span class="sxs-lookup"><span data-stu-id="fa95e-121">See the documentation on [ASP.NET Core logging](xref:fundamentals/logging/index#configuration) for details on configuring ASP.NET Core logging.</span></span>

<span data-ttu-id="fa95e-122">gRPC добавляет журналы в категорию `Grpc`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-122">gRPC adds logs under the `Grpc` category.</span></span> <span data-ttu-id="fa95e-123">Чтобы включить подробные журналы из gRPC, настройте префиксы `Grpc` на уровне `Debug` в файле *appSettings. JSON* , добавив следующие элементы в подраздел `LogLevel` в `Logging`:</span><span class="sxs-lookup"><span data-stu-id="fa95e-123">To enable detailed logs from gRPC, configure the `Grpc` prefixes to the `Debug` level in your *appsettings.json* file by adding the following items to the `LogLevel` sub-section in `Logging`:</span></span>

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

<span data-ttu-id="fa95e-124">Это также можно настроить в *Startup.CS* с помощью `ConfigureLogging`:</span><span class="sxs-lookup"><span data-stu-id="fa95e-124">You can also configure this in *Startup.cs* with `ConfigureLogging`:</span></span>

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

<span data-ttu-id="fa95e-125">Если конфигурация на основе JSON не используется, установите следующее значение конфигурации в системе конфигурации:</span><span class="sxs-lookup"><span data-stu-id="fa95e-125">If you aren't using JSON-based configuration, set the following configuration value in your configuration system:</span></span>

* `Logging:LogLevel:Grpc` = `Debug`

<span data-ttu-id="fa95e-126">Ознакомьтесь с документацией по системе конфигурации, чтобы определить, как указать вложенные значения конфигурации.</span><span class="sxs-lookup"><span data-stu-id="fa95e-126">Check the documentation for your configuration system to determine how to specify nested configuration values.</span></span> <span data-ttu-id="fa95e-127">Например, при использовании переменных среды вместо `:` используются два `_` символов (например, `Logging__LogLevel__Grpc`).</span><span class="sxs-lookup"><span data-stu-id="fa95e-127">For example, when using environment variables, two `_` characters are used instead of the `:` (for example, `Logging__LogLevel__Grpc`).</span></span>

<span data-ttu-id="fa95e-128">При сборе более подробных диагностических сведений о приложении рекомендуется использовать уровень `Debug`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-128">We recommend using the `Debug` level when gathering more detailed diagnostics for your app.</span></span> <span data-ttu-id="fa95e-129">Уровень `Trace` создает очень низкий уровень диагностики и редко требуется для диагностики проблем в приложении.</span><span class="sxs-lookup"><span data-stu-id="fa95e-129">The `Trace` level produces very low-level diagnostics and is rarely needed to diagnose issues in your app.</span></span>

#### <a name="sample-logging-output"></a><span data-ttu-id="fa95e-130">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="fa95e-130">Sample logging output</span></span>

<span data-ttu-id="fa95e-131">Ниже приведен пример вывода на консоль на уровне `Debug` службы gRPC:</span><span class="sxs-lookup"><span data-stu-id="fa95e-131">Here is an example of console output at the `Debug` level of a gRPC service:</span></span>

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

### <a name="access-server-side-logs"></a><span data-ttu-id="fa95e-132">Доступ к журналам на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="fa95e-132">Access server-side logs</span></span>

<span data-ttu-id="fa95e-133">Доступ к журналам на стороне сервера зависит от среды, в которой выполняется.</span><span class="sxs-lookup"><span data-stu-id="fa95e-133">How you access server-side logs depends on the environment in which you're running.</span></span>

#### <a name="as-a-console-app"></a><span data-ttu-id="fa95e-134">Как консольное приложение</span><span class="sxs-lookup"><span data-stu-id="fa95e-134">As a console app</span></span>

<span data-ttu-id="fa95e-135">Если вы работаете в консольном приложении, [средство ведения журнала консоли](xref:fundamentals/logging/index#console-provider) должно быть включено по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="fa95e-135">If you're running in a console app, the [Console logger](xref:fundamentals/logging/index#console-provider) should be enabled by default.</span></span> <span data-ttu-id="fa95e-136">журналы gRPC будут отображаться в консоли.</span><span class="sxs-lookup"><span data-stu-id="fa95e-136">gRPC logs will appear in the console.</span></span>

#### <a name="other-environments"></a><span data-ttu-id="fa95e-137">Другие среды</span><span class="sxs-lookup"><span data-stu-id="fa95e-137">Other environments</span></span>

<span data-ttu-id="fa95e-138">Если приложение развертывается в другой среде (например, Docker, Kubernetes или службе Windows), см. Дополнительные сведения о настройке регистраторов, подходящих для среды, в <xref:fundamentals/logging/index>.</span><span class="sxs-lookup"><span data-stu-id="fa95e-138">If the app is deployed to another environment (for example, Docker, Kubernetes, or Windows Service), see <xref:fundamentals/logging/index> for more information on how to configure logging providers suitable for the environment.</span></span>

### <a name="grpc-client-logging"></a><span data-ttu-id="fa95e-139">ведение журнала клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="fa95e-139">gRPC client logging</span></span>

> [!WARNING]
> <span data-ttu-id="fa95e-140">Журналы на стороне клиента могут содержать конфиденциальные сведения из приложения.</span><span class="sxs-lookup"><span data-stu-id="fa95e-140">Client-side logs may contain sensitive information from your app.</span></span> <span data-ttu-id="fa95e-141">**Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.</span><span class="sxs-lookup"><span data-stu-id="fa95e-141">**Never** post raw logs from production apps to public forums like GitHub.</span></span>

<span data-ttu-id="fa95e-142">Чтобы получить журналы из клиента .NET, можно задать свойство `GrpcChannelOptions.LoggerFactory` при создании канала клиента.</span><span class="sxs-lookup"><span data-stu-id="fa95e-142">To get logs from the .NET client, you can set the `GrpcChannelOptions.LoggerFactory` property when the client's channel is created.</span></span> <span data-ttu-id="fa95e-143">Если вы вызываете службу gRPC из приложения ASP.NET Core, фабрика журнала может быть разрешена путем внедрения зависимостей (DI):</span><span class="sxs-lookup"><span data-stu-id="fa95e-143">If you are calling a gRPC service from an ASP.NET Core app then the logger factory can be resolved from dependency injection (DI):</span></span>

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

<span data-ttu-id="fa95e-144">Альтернативный способ включения ведения журнала клиента — использование [фабрики клиента gRPC](xref:grpc/clientfactory) для создания клиента.</span><span class="sxs-lookup"><span data-stu-id="fa95e-144">An alternative way to enable client logging is to use the [gRPC client factory](xref:grpc/clientfactory) to create the client.</span></span> <span data-ttu-id="fa95e-145">Клиент gRPC, зарегистрированный в фабрике клиента и разрешенный из DI, автоматически будет использовать настроенное для приложения ведение журнала.</span><span class="sxs-lookup"><span data-stu-id="fa95e-145">A gRPC client registered with the client factory and resolved from DI will automatically use the app's configured logging.</span></span>

<span data-ttu-id="fa95e-146">Если приложение не использует DI, можно создать новый экземпляр `ILoggerFactory` с помощью [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span><span class="sxs-lookup"><span data-stu-id="fa95e-146">If your app isn't using DI then you can create a new `ILoggerFactory` instance with [LoggerFactory.Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*).</span></span> <span data-ttu-id="fa95e-147">Чтобы получить доступ к этому методу, добавьте в приложение пакет [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) .</span><span class="sxs-lookup"><span data-stu-id="fa95e-147">To access this method add the [Microsoft.Extensions.Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) package to your app.</span></span>

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a><span data-ttu-id="fa95e-148">области журнала клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="fa95e-148">gRPC client log scopes</span></span>

<span data-ttu-id="fa95e-149">Клиент gRPC добавляет [область ведения журнала](https://docs.microsoft.com/aspnet/core/fundamentals/logginglog-scopes) в журналы, сделанные во время вызова gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-149">The gRPC client adds a [logging scope](https://docs.microsoft.com/aspnet/core/fundamentals/logginglog-scopes) to logs made during a gRPC call.</span></span> <span data-ttu-id="fa95e-150">Область содержит метаданные, связанные с вызовом gRPC:</span><span class="sxs-lookup"><span data-stu-id="fa95e-150">The scope has metadata related to the gRPC call:</span></span>

* <span data-ttu-id="fa95e-151">**Грпкмесодтипе** — тип метода gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-151">**GrpcMethodType** - The gRPC method type.</span></span> <span data-ttu-id="fa95e-152">Возможными значениями являются имена из перечисления `Grpc.Core.MethodType`, например унарные</span><span class="sxs-lookup"><span data-stu-id="fa95e-152">Possible values are names from `Grpc.Core.MethodType` enum, e.g. Unary</span></span>
* <span data-ttu-id="fa95e-153">**Грпкури** — относительный URI метода gRPC, например/Грит. Гритер/Сайхеллос</span><span class="sxs-lookup"><span data-stu-id="fa95e-153">**GrpcUri** - The relative URI of the gRPC method, e.g. /greet.Greeter/SayHellos</span></span>

#### <a name="sample-logging-output"></a><span data-ttu-id="fa95e-154">Пример выходных данных ведения журнала</span><span class="sxs-lookup"><span data-stu-id="fa95e-154">Sample logging output</span></span>

<span data-ttu-id="fa95e-155">Ниже приведен пример вывода на консоль на уровне `Debug` клиента gRPC:</span><span class="sxs-lookup"><span data-stu-id="fa95e-155">Here is an example of console output at the `Debug` level of a gRPC client:</span></span>

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

## <a name="tracing"></a><span data-ttu-id="fa95e-156">Трассировка</span><span class="sxs-lookup"><span data-stu-id="fa95e-156">Tracing</span></span>

<span data-ttu-id="fa95e-157">gRPC Services и клиент gRPC предоставляют сведения о вызовах gRPC с помощью [DiagnosticSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource) и [действия](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity).</span><span class="sxs-lookup"><span data-stu-id="fa95e-157">gRPC services and the gRPC client provide information about gRPC calls using [DiagnosticSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource) and [Activity](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity).</span></span>

* <span data-ttu-id="fa95e-158">.NET gRPC использует действие для представления вызова gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-158">.NET gRPC uses an activity to represent a gRPC call.</span></span>
* <span data-ttu-id="fa95e-159">События трассировки записываются в источник диагностики при запуске и окончании действия вызова gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-159">Tracing events are written to the diagnostic source at the start and stop of the gRPC call activity.</span></span>
* <span data-ttu-id="fa95e-160">Трассировка не собирает сведения о том, когда сообщения отправляются в течение времени существования вызовов потоковой передачи gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-160">Tracing doesn't capture information about when messages are sent over the lifetime of gRPC streaming calls.</span></span>

### <a name="grpc-service-tracing"></a><span data-ttu-id="fa95e-161">Трассировка службы gRPC</span><span class="sxs-lookup"><span data-stu-id="fa95e-161">gRPC service tracing</span></span>

<span data-ttu-id="fa95e-162">службы gRPC размещаются на ASP.NET Core, которая сообщает о событиях входящих HTTP-запросов.</span><span class="sxs-lookup"><span data-stu-id="fa95e-162">gRPC services are hosted on ASP.NET Core which reports events about incoming HTTP requests.</span></span> <span data-ttu-id="fa95e-163">метаданные gRPC добавляются к существующим средствам диагностики HTTP-запросов, которые ASP.NET Core предоставляет.</span><span class="sxs-lookup"><span data-stu-id="fa95e-163">gRPC specific metadata is added to the existing HTTP request diagnostics that ASP.NET Core provides.</span></span>

* <span data-ttu-id="fa95e-164">Имя источника диагностики — `Microsoft.AspNetCore`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-164">Diagnostic source name is `Microsoft.AspNetCore`.</span></span>
* <span data-ttu-id="fa95e-165">Имя действия — `Microsoft.AspNetCore.Hosting.HttpRequestIn`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-165">Activity name is `Microsoft.AspNetCore.Hosting.HttpRequestIn`.</span></span>
  * <span data-ttu-id="fa95e-166">Имя метода gRPC, вызываемого вызовом gRPC, добавляется в качестве тега с именем `grpc.method`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-166">Name of the gRPC method invoked by the gRPC call is added as a tag with the name `grpc.method`.</span></span>
  * <span data-ttu-id="fa95e-167">Код состояния вызова gRPC после его завершения добавляется в виде тега с именем `grpc.status_code`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-167">Status code of the gRPC call when it is complete is added as a tag with the name `grpc.status_code`.</span></span>

### <a name="grpc-client-tracing"></a><span data-ttu-id="fa95e-168">трассировка клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="fa95e-168">gRPC client tracing</span></span>

<span data-ttu-id="fa95e-169">Клиент .NET gRPC использует `HttpClient` для выполнения вызовов gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-169">The .NET gRPC client uses `HttpClient` to make gRPC calls.</span></span> <span data-ttu-id="fa95e-170">Хотя `HttpClient` записывает события диагностики, клиент .NET gRPC предоставляет пользовательский источник диагностики, действия и события, чтобы можно было собирать полные сведения о вызове gRPC.</span><span class="sxs-lookup"><span data-stu-id="fa95e-170">Although `HttpClient` writes diagnostic events, the .NET gRPC client provides a custom diagnostic source, activity and events so that complete information about a gRPC call can be collected.</span></span>

* <span data-ttu-id="fa95e-171">Имя источника диагностики — `Grpc.Net.Client`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-171">Diagnostic source name is `Grpc.Net.Client`.</span></span>
* <span data-ttu-id="fa95e-172">Имя действия — `Grpc.Net.Client.GrpcOut`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-172">Activity name is `Grpc.Net.Client.GrpcOut`.</span></span>
  * <span data-ttu-id="fa95e-173">Имя метода gRPC, вызываемого вызовом gRPC, добавляется в качестве тега с именем `grpc.method`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-173">Name of the gRPC method invoked by the gRPC call is added as a tag with the name `grpc.method`.</span></span>
  * <span data-ttu-id="fa95e-174">Код состояния вызова gRPC после его завершения добавляется в виде тега с именем `grpc.status_code`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-174">Status code of the gRPC call when it is complete is added as a tag with the name `grpc.status_code`.</span></span>

### <a name="collecting-tracing"></a><span data-ttu-id="fa95e-175">Сбор данных трассировки</span><span class="sxs-lookup"><span data-stu-id="fa95e-175">Collecting tracing</span></span>

<span data-ttu-id="fa95e-176">Самый простой способ использовать `DiagnosticSource` — настроить в приложении библиотеку телеметрии, например [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) или [опентелеметри](https://github.com/open-telemetry/opentelemetry-dotnet) .</span><span class="sxs-lookup"><span data-stu-id="fa95e-176">The easiest way to use `DiagnosticSource` is to configure a telemetry library such as [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) or [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet) in your app.</span></span> <span data-ttu-id="fa95e-177">Библиотека будет обрабатывать сведения о вызовах gRPC на стороне других данных телеметрии приложений.</span><span class="sxs-lookup"><span data-stu-id="fa95e-177">The library will process information about gRPC calls along-side other app telemetry.</span></span>

<span data-ttu-id="fa95e-178">Трассировку можно просмотреть в управляемой службе, например Application Insights, или можно выбрать Запуск собственной распределенной системы трассировки.</span><span class="sxs-lookup"><span data-stu-id="fa95e-178">Tracing can be viewed in a managed service like Application Insights, or you can choose to run your own distributed tracing system.</span></span> <span data-ttu-id="fa95e-179">Опентелеметри поддерживает экспорт данных трассировки в [жаежер](https://www.jaegertracing.io/) и [зипкин](https://zipkin.io/).</span><span class="sxs-lookup"><span data-stu-id="fa95e-179">OpenTelemetry supports exporting tracing data to [Jaeger](https://www.jaegertracing.io/) and [Zipkin](https://zipkin.io/).</span></span>

<span data-ttu-id="fa95e-180">`DiagnosticSource` может использовать трассировку событий в коде с помощью `DiagnosticListener`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-180">`DiagnosticSource` can consume tracing events in code using `DiagnosticListener`.</span></span> <span data-ttu-id="fa95e-181">Сведения о прослушивании источника диагностики с помощью кода см. в разделе [DiagnosticSource пользователя](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener).</span><span class="sxs-lookup"><span data-stu-id="fa95e-181">For information about listening to a diagnostic source with code, see the [DiagnosticSource user's guide](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener).</span></span>

> [!NOTE]
> <span data-ttu-id="fa95e-182">Библиотеки телеметрии не записывают gRPC данные телеметрии для конкретного `Grpc.Net.Client.GrpcOut` в настоящее время.</span><span class="sxs-lookup"><span data-stu-id="fa95e-182">Telemetry libraries do not capture gRPC specific `Grpc.Net.Client.GrpcOut` telemetry currently.</span></span> <span data-ttu-id="fa95e-183">Работа по улучшению библиотек телеметрии, которые фиксируют эту трассировку.</span><span class="sxs-lookup"><span data-stu-id="fa95e-183">Work to improve telemetry libraries capturing this tracing is ongoing.</span></span>

## <a name="metrics"></a><span data-ttu-id="fa95e-184">Метрики</span><span class="sxs-lookup"><span data-stu-id="fa95e-184">Metrics</span></span>

<span data-ttu-id="fa95e-185">Метрики — это представление мер данных за интервалы времени, например количество запросов в секунду.</span><span class="sxs-lookup"><span data-stu-id="fa95e-185">Metrics is a representation of data measures over intervals of time, for example, requests per second.</span></span> <span data-ttu-id="fa95e-186">Данные метрик позволяют выполнять наблюдение за состоянием приложения на высоком уровне.</span><span class="sxs-lookup"><span data-stu-id="fa95e-186">Metrics data allows observation of the state of an app at a high-level.</span></span> <span data-ttu-id="fa95e-187">Метрики .NET gRPC создаются с помощью `EventCounter`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-187">.NET gRPC metrics are emitted using `EventCounter`.</span></span>

### <a name="grpc-service-metrics"></a><span data-ttu-id="fa95e-188">метрики службы gRPC</span><span class="sxs-lookup"><span data-stu-id="fa95e-188">gRPC service metrics</span></span>

<span data-ttu-id="fa95e-189">метрики сервера gRPC выводятся в `Grpc.AspNetCore.Server` источника событий.</span><span class="sxs-lookup"><span data-stu-id="fa95e-189">gRPC server metrics are reported on `Grpc.AspNetCore.Server` event source.</span></span>

| <span data-ttu-id="fa95e-190">Имя</span><span class="sxs-lookup"><span data-stu-id="fa95e-190">Name</span></span>                      | <span data-ttu-id="fa95e-191">Description</span><span class="sxs-lookup"><span data-stu-id="fa95e-191">Description</span></span>                   |
| --------------------------|-------------------------------|
| `total-calls`             | <span data-ttu-id="fa95e-192">Total Calls</span><span class="sxs-lookup"><span data-stu-id="fa95e-192">Total Calls</span></span>                   |
| `current-calls`           | <span data-ttu-id="fa95e-193">Текущие вызовы</span><span class="sxs-lookup"><span data-stu-id="fa95e-193">Current Calls</span></span>                 |
| `calls-failed`            | <span data-ttu-id="fa95e-194">Всего вызовов с ошибками</span><span class="sxs-lookup"><span data-stu-id="fa95e-194">Total Calls Failed</span></span>            |
| `calls-deadline-exceeded` | <span data-ttu-id="fa95e-195">Превышен порог общего числа вызовов</span><span class="sxs-lookup"><span data-stu-id="fa95e-195">Total Calls Deadline Exceeded</span></span> |
| `messages-sent`           | <span data-ttu-id="fa95e-196">Всего отправленных сообщений</span><span class="sxs-lookup"><span data-stu-id="fa95e-196">Total Messages Sent</span></span>           |
| `messages-received`       | <span data-ttu-id="fa95e-197">Всего получено сообщений</span><span class="sxs-lookup"><span data-stu-id="fa95e-197">Total Messages Received</span></span>       |
| `calls-unimplemented`     | <span data-ttu-id="fa95e-198">Всего нереализованных вызовов</span><span class="sxs-lookup"><span data-stu-id="fa95e-198">Total Calls Unimplemented</span></span>     |

<span data-ttu-id="fa95e-199">ASP.NET Core также предоставляет собственные метрики для `Microsoft.AspNetCore.Hosting` источника событий.</span><span class="sxs-lookup"><span data-stu-id="fa95e-199">ASP.NET Core also provides its own metrics on `Microsoft.AspNetCore.Hosting` event source.</span></span>

### <a name="grpc-client-metrics"></a><span data-ttu-id="fa95e-200">метрики клиента gRPC</span><span class="sxs-lookup"><span data-stu-id="fa95e-200">gRPC client metrics</span></span>

<span data-ttu-id="fa95e-201">метрики клиента gRPC передаются в источник событий `Grpc.Net.Client`.</span><span class="sxs-lookup"><span data-stu-id="fa95e-201">gRPC client metrics are reported on `Grpc.Net.Client` event source.</span></span>

| <span data-ttu-id="fa95e-202">Имя</span><span class="sxs-lookup"><span data-stu-id="fa95e-202">Name</span></span>                      | <span data-ttu-id="fa95e-203">Description</span><span class="sxs-lookup"><span data-stu-id="fa95e-203">Description</span></span>                   |
| --------------------------|-------------------------------|
| `total-calls`             | <span data-ttu-id="fa95e-204">Total Calls</span><span class="sxs-lookup"><span data-stu-id="fa95e-204">Total Calls</span></span>                   |
| `current-calls`           | <span data-ttu-id="fa95e-205">Текущие вызовы</span><span class="sxs-lookup"><span data-stu-id="fa95e-205">Current Calls</span></span>                 |
| `calls-failed`            | <span data-ttu-id="fa95e-206">Всего вызовов с ошибками</span><span class="sxs-lookup"><span data-stu-id="fa95e-206">Total Calls Failed</span></span>            |
| `calls-deadline-exceeded` | <span data-ttu-id="fa95e-207">Превышен порог общего числа вызовов</span><span class="sxs-lookup"><span data-stu-id="fa95e-207">Total Calls Deadline Exceeded</span></span> |
| `messages-sent`           | <span data-ttu-id="fa95e-208">Всего отправленных сообщений</span><span class="sxs-lookup"><span data-stu-id="fa95e-208">Total Messages Sent</span></span>           |
| `messages-received`       | <span data-ttu-id="fa95e-209">Всего получено сообщений</span><span class="sxs-lookup"><span data-stu-id="fa95e-209">Total Messages Received</span></span>       |

### <a name="observe-metrics"></a><span data-ttu-id="fa95e-210">Наблюдение за метриками</span><span class="sxs-lookup"><span data-stu-id="fa95e-210">Observe metrics</span></span>

<span data-ttu-id="fa95e-211">[DotNet-Counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) — это средство мониторинга производительности для нерегламентированного мониторинга работоспособности и анализа производительности первого уровня.</span><span class="sxs-lookup"><span data-stu-id="fa95e-211">[dotnet-counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) is a performance monitoring tool for ad-hoc health monitoring and first-level performance investigation.</span></span> <span data-ttu-id="fa95e-212">Отслеживайте приложение .NET, используя `Grpc.AspNetCore.Server` или `Grpc.Net.Client` в качестве имени поставщика.</span><span class="sxs-lookup"><span data-stu-id="fa95e-212">Monitor a .NET app with either `Grpc.AspNetCore.Server` or `Grpc.Net.Client` as the provider name.</span></span>

```console
> dotnet-counters monitor --process-id 1902 Grpc.AspNetCore.Server

Press p to pause, r to resume, q to quit.
    Status: Running
[Grpc.AspNetCore.Server]
    Total Calls                                 300
    Current Calls                               5
    Total Calls Failed                          0
    Total Calls Deadline Exceeded               0
    Total Messages Sent                         295
    Total Messages Received                     300
    Total Calls Unimplemented                   0
```

<span data-ttu-id="fa95e-213">Другим способом наблюдения за gRPC метриками является запись данных счетчиков с помощью [пакета Microsoft. ApplicationInsights. евенткаунтерколлектор](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)Application Insights.</span><span class="sxs-lookup"><span data-stu-id="fa95e-213">Another way to observe gRPC metrics is to capture counter data using Application Insights's [Microsoft.ApplicationInsights.EventCounterCollector package](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).</span></span> <span data-ttu-id="fa95e-214">После установки Application Insights собирает общие счетчики .NET во время выполнения.</span><span class="sxs-lookup"><span data-stu-id="fa95e-214">Once setup, Application Insights collects common .NET counters at runtime.</span></span> <span data-ttu-id="fa95e-215">Счетчики gRPC не собираются по умолчанию, но можно настроить App Insights [для включения дополнительных счетчиков](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).</span><span class="sxs-lookup"><span data-stu-id="fa95e-215">gRPC's counters are not collected by default, but App Insights can be [customized to include additional counters](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).</span></span>

<span data-ttu-id="fa95e-216">Укажите счетчики gRPC для Application Insights, которые должны быть собраны в *Startup.CS*:</span><span class="sxs-lookup"><span data-stu-id="fa95e-216">Specify the gRPC counters for Application Insight to collect in *Startup.cs*:</span></span>

```csharp
    using Microsoft.ApplicationInsights.Extensibility.EventCounterCollector;

    public void ConfigureServices(IServiceCollection services)
    {
        //... other code...

        services.ConfigureTelemetryModule<EventCounterCollectionModule>(
            (module, o) =>
            {
                // Configure App Insights to collect gRPC counters gRPC services hosted in an ASP.NET Core app
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "current-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "total-calls"));
                module.Counters.Add(new EventCounterCollectionRequest("Grpc.AspNetCore.Server", "calls-failed"));
            }
        );
    }
```

## <a name="additional-resources"></a><span data-ttu-id="fa95e-217">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="fa95e-217">Additional resources</span></span>

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
