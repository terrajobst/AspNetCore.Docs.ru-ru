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
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Ведение журналов и диагностика в gRPC на .NET

[Джеймс Ньютона-короля](https://twitter.com/jamesnk)

В этой статье приводятся рекомендации по сбору диагностических данных из приложения gRPC для устранения проблем. Здесь рассматриваются такие темы:

* Журналы, структурированные **с ведением журнала** , записываются в [журнал .NET Core](xref:fundamentals/logging/index). <xref:Microsoft.Extensions.Logging.ILogger> используется платформами приложений для записи журналов и пользователей для их собственного ведения журнала в приложении.
* **Трассировка** — события, связанные с операцией, написанной с помощью `DiaganosticSource` и `Activity`. Трассировки из источника диагностики обычно используются для сбора данных телеметрии приложений с помощью таких библиотек, как [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) и [опентелеметри](https://github.com/open-telemetry/opentelemetry-dotnet).
* **Метрики** — представление мер данных за интервалы времени, например количество запросов в секунду. Метрики создаются с помощью `EventCounter` и могут быть просмотрены с помощью средства командной строки [DotNet-Counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) или с помощью [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters).

## <a name="logging"></a>Logging

gRPC Services и клиент gRPC пишут журналы с помощью [ведения журнала .NET Core](xref:fundamentals/logging/index). Журналы являются хорошим местом для запуска при необходимости отладки непредвиденного поведения в приложениях.

### <a name="grpc-services-logging"></a>ведение журнала gRPC Services

> [!WARNING]
> Журналы на стороне сервера могут содержать конфиденциальные сведения из приложения. **Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.

Поскольку службы gRPC размещаются на ASP.NET Core, используется система ведения журнала ASP.NET Core. В конфигурации по умолчанию gRPC регистрирует очень мало информации, но это может быть настроено. Дополнительные сведения о настройке ведения журнала ASP.NET Core см. в документации по [ASP.NET Core ведению журнала](xref:fundamentals/logging/index#configuration) .

gRPC добавляет журналы в категорию `Grpc`. Чтобы включить подробные журналы из gRPC, настройте префиксы `Grpc` на уровне `Debug` в файле *appSettings. JSON* , добавив следующие элементы в подраздел `LogLevel` в `Logging`:

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

Это также можно настроить в *Startup.CS* с помощью `ConfigureLogging`:

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

Если конфигурация на основе JSON не используется, установите следующее значение конфигурации в системе конфигурации:

* `Logging:LogLevel:Grpc` = `Debug`

Ознакомьтесь с документацией по системе конфигурации, чтобы определить, как указать вложенные значения конфигурации. Например, при использовании переменных среды вместо `:` используются два `_` символов (например, `Logging__LogLevel__Grpc`).

При сборе более подробных диагностических сведений о приложении рекомендуется использовать уровень `Debug`. Уровень `Trace` создает очень низкий уровень диагностики и редко требуется для диагностики проблем в приложении.

#### <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

Ниже приведен пример вывода на консоль на уровне `Debug` службы gRPC:

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

### <a name="access-server-side-logs"></a>Доступ к журналам на стороне сервера

Доступ к журналам на стороне сервера зависит от среды, в которой выполняется.

#### <a name="as-a-console-app"></a>Как консольное приложение

Если вы работаете в консольном приложении, [средство ведения журнала консоли](xref:fundamentals/logging/index#console-provider) должно быть включено по умолчанию. журналы gRPC будут отображаться в консоли.

#### <a name="other-environments"></a>Другие среды

Если приложение развертывается в другой среде (например, Docker, Kubernetes или службе Windows), см. Дополнительные сведения о настройке регистраторов, подходящих для среды, в <xref:fundamentals/logging/index>.

### <a name="grpc-client-logging"></a>ведение журнала клиента gRPC

> [!WARNING]
> Журналы на стороне клиента могут содержать конфиденциальные сведения из приложения. **Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.

Чтобы получить журналы из клиента .NET, можно задать свойство `GrpcChannelOptions.LoggerFactory` при создании канала клиента. Если вы вызываете службу gRPC из приложения ASP.NET Core, фабрика журнала может быть разрешена путем внедрения зависимостей (DI):

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

Альтернативный способ включения ведения журнала клиента — использование [фабрики клиента gRPC](xref:grpc/clientfactory) для создания клиента. Клиент gRPC, зарегистрированный в фабрике клиента и разрешенный из DI, автоматически будет использовать настроенное для приложения ведение журнала.

Если приложение не использует DI, можно создать новый экземпляр `ILoggerFactory` с помощью [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Чтобы получить доступ к этому методу, добавьте в приложение пакет [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) .

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

#### <a name="grpc-client-log-scopes"></a>области журнала клиента gRPC

Клиент gRPC добавляет [область ведения журнала](https://docs.microsoft.com/aspnet/core/fundamentals/logginglog-scopes) в журналы, сделанные во время вызова gRPC. Область содержит метаданные, связанные с вызовом gRPC:

* **Грпкмесодтипе** — тип метода gRPC. Возможными значениями являются имена из перечисления `Grpc.Core.MethodType`, например унарные
* **Грпкури** — относительный URI метода gRPC, например/Грит. Гритер/Сайхеллос

#### <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

Ниже приведен пример вывода на консоль на уровне `Debug` клиента gRPC:

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

## <a name="tracing"></a>Трассировка

gRPC Services и клиент gRPC предоставляют сведения о вызовах gRPC с помощью [DiagnosticSource](https://docs.microsoft.com/dotnet/api/system.diagnostics.diagnosticsource) и [действия](https://docs.microsoft.com/dotnet/api/system.diagnostics.activity).

* .NET gRPC использует действие для представления вызова gRPC.
* События трассировки записываются в источник диагностики при запуске и окончании действия вызова gRPC.
* Трассировка не собирает сведения о том, когда сообщения отправляются в течение времени существования вызовов потоковой передачи gRPC.

### <a name="grpc-service-tracing"></a>Трассировка службы gRPC

службы gRPC размещаются на ASP.NET Core, которая сообщает о событиях входящих HTTP-запросов. метаданные gRPC добавляются к существующим средствам диагностики HTTP-запросов, которые ASP.NET Core предоставляет.

* Имя источника диагностики — `Microsoft.AspNetCore`.
* Имя действия — `Microsoft.AspNetCore.Hosting.HttpRequestIn`.
  * Имя метода gRPC, вызываемого вызовом gRPC, добавляется в качестве тега с именем `grpc.method`.
  * Код состояния вызова gRPC после его завершения добавляется в виде тега с именем `grpc.status_code`.

### <a name="grpc-client-tracing"></a>трассировка клиента gRPC

Клиент .NET gRPC использует `HttpClient` для выполнения вызовов gRPC. Хотя `HttpClient` записывает события диагностики, клиент .NET gRPC предоставляет пользовательский источник диагностики, действия и события, чтобы можно было собирать полные сведения о вызове gRPC.

* Имя источника диагностики — `Grpc.Net.Client`.
* Имя действия — `Grpc.Net.Client.GrpcOut`.
  * Имя метода gRPC, вызываемого вызовом gRPC, добавляется в качестве тега с именем `grpc.method`.
  * Код состояния вызова gRPC после его завершения добавляется в виде тега с именем `grpc.status_code`.

### <a name="collecting-tracing"></a>Сбор данных трассировки

Самый простой способ использовать `DiagnosticSource` — настроить в приложении библиотеку телеметрии, например [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/asp-net-core) или [опентелеметри](https://github.com/open-telemetry/opentelemetry-dotnet) . Библиотека будет обрабатывать сведения о вызовах gRPC на стороне других данных телеметрии приложений.

Трассировку можно просмотреть в управляемой службе, например Application Insights, или можно выбрать Запуск собственной распределенной системы трассировки. Опентелеметри поддерживает экспорт данных трассировки в [жаежер](https://www.jaegertracing.io/) и [зипкин](https://zipkin.io/).

`DiagnosticSource` может использовать трассировку событий в коде с помощью `DiagnosticListener`. Сведения о прослушивании источника диагностики с помощью кода см. в разделе [DiagnosticSource пользователя](https://github.com/dotnet/corefx/blob/d3942d4671919edb0cca6ddc1840190f524a809d/src/System.Diagnostics.DiagnosticSource/src/DiagnosticSourceUsersGuide.md#consuming-data-with-diagnosticlistener).

> [!NOTE]
> Библиотеки телеметрии не записывают gRPC данные телеметрии для конкретного `Grpc.Net.Client.GrpcOut` в настоящее время. Работа по улучшению библиотек телеметрии, которые фиксируют эту трассировку.

## <a name="metrics"></a>Метрики

Метрики — это представление мер данных за интервалы времени, например количество запросов в секунду. Данные метрик позволяют выполнять наблюдение за состоянием приложения на высоком уровне. Метрики .NET gRPC создаются с помощью `EventCounter`.

### <a name="grpc-service-metrics"></a>метрики службы gRPC

метрики сервера gRPC выводятся в `Grpc.AspNetCore.Server` источника событий.

| Имя                      | Description                   |
| --------------------------|-------------------------------|
| `total-calls`             | Total Calls                   |
| `current-calls`           | Текущие вызовы                 |
| `calls-failed`            | Всего вызовов с ошибками            |
| `calls-deadline-exceeded` | Превышен порог общего числа вызовов |
| `messages-sent`           | Всего отправленных сообщений           |
| `messages-received`       | Всего получено сообщений       |
| `calls-unimplemented`     | Всего нереализованных вызовов     |

ASP.NET Core также предоставляет собственные метрики для `Microsoft.AspNetCore.Hosting` источника событий.

### <a name="grpc-client-metrics"></a>метрики клиента gRPC

метрики клиента gRPC передаются в источник событий `Grpc.Net.Client`.

| Имя                      | Description                   |
| --------------------------|-------------------------------|
| `total-calls`             | Total Calls                   |
| `current-calls`           | Текущие вызовы                 |
| `calls-failed`            | Всего вызовов с ошибками            |
| `calls-deadline-exceeded` | Превышен порог общего числа вызовов |
| `messages-sent`           | Всего отправленных сообщений           |
| `messages-received`       | Всего получено сообщений       |

### <a name="observe-metrics"></a>Наблюдение за метриками

[DotNet-Counters](https://docs.microsoft.com/dotnet/core/diagnostics/dotnet-counters) — это средство мониторинга производительности для нерегламентированного мониторинга работоспособности и анализа производительности первого уровня. Отслеживайте приложение .NET, используя `Grpc.AspNetCore.Server` или `Grpc.Net.Client` в качестве имени поставщика.

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

Другим способом наблюдения за gRPC метриками является запись данных счетчиков с помощью [пакета Microsoft. ApplicationInsights. евенткаунтерколлектор](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters)Application Insights. После установки Application Insights собирает общие счетчики .NET во время выполнения. Счетчики gRPC не собираются по умолчанию, но можно настроить App Insights [для включения дополнительных счетчиков](https://docs.microsoft.com/azure/azure-monitor/app/eventcounters#customizing-counters-to-be-collected).

Укажите счетчики gRPC для Application Insights, которые должны быть собраны в *Startup.CS*:

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
