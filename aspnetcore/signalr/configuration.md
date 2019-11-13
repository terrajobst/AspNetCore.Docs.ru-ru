---
title: Конфигурация SignalR ASP.NET Core
author: bradygaster
description: Узнайте, как настроить ASP.NET Core SignalR приложений.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/configuration
ms.openlocfilehash: 682cc36216a4dc9b38c87b4f67100ab565a70e5c
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/12/2019
ms.locfileid: "73963984"
---
# <a name="aspnet-core-opno-locsignalr-configuration"></a>Конфигурация SignalR ASP.NET Core

## <a name="jsonmessagepack-serialization-options"></a>Параметры сериализации JSON/MessagePack

ASP.NET Core SignalR поддерживает два протокола кодирования сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html). Для каждого протокола предусмотрены параметры конфигурации сериализации.

::: moniker range=">= aspnetcore-3.0"

Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) . `AddJsonProtocol` можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в `Startup.ConfigureServices`. Метод `AddJsonProtocol` принимает делегат, который получает объект `options`. Свойство [пайлоадсериализероптионс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializeroptions) этого объекта является объектом `System.Text.Json` <xref:System.Text.Json.JsonSerializerOptions>, который можно использовать для настройки сериализации аргументов и возвращаемых значений. Дополнительные сведения см. в [документации System. Text. JSON](/dotnet/api/system.text.json).

Например, чтобы настроить в сериализаторе не изменять регистр имен свойств, вместо имен по умолчанию используйте следующий код в `Startup.ConfigureServices`:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null
    });
```

В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Для разрешения метода расширения необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerOptions.PropertyNamingPolicy = null;
    })
    .Build();
```

> [!NOTE]
> В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.

### <a name="switch-to-newtonsoftjson"></a>Переключиться на Newtonsoft. JSON

Если вам нужны функции `Newtonsoft.Json`, которые не поддерживаются в `System.Text.Json`, см. раздел [Переключение на Newtonsoft. JSON](xref:migration/22-to-30#switch-to-newtonsoftjson).

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Сериализацию JSON можно настроить на сервере с помощью метода расширения [адджсонпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) , который можно добавить после [аддсигналр](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в методе `Startup.ConfigureServices`. Метод `AddJsonProtocol` принимает делегат, который получает объект `options`. Свойство [пайлоадсериализерсеттингс](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) этого объекта является объектом JSON.NET `JsonSerializerSettings`, который можно использовать для настройки сериализации аргументов и возвращаемых значений. Дополнительные сведения см. в [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm).
 
Например, чтобы настроить сериализатор для использования имен свойств "PascalCase" вместо имен по умолчанию "camelCase", используйте следующий код в `Startup.ConfigureServices`.
 
```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    });
```

В клиенте .NET один и тот же метод расширения `AddJsonProtocol` существует в [хубконнектионбуилдер](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). Для разрешения метода расширения необходимо импортировать пространство имен `Microsoft.Extensions.DependencyInjection`.

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;
 
// When constructing your connection:
var connection = new HubConnectionBuilder()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
            new DefaultContractResolver();
    })
    .Build();
```

> [!NOTE]
> В настоящее время невозможно настроить сериализацию JSON в клиенте JavaScript.

::: moniker-end

### <a name="messagepack-serialization-options"></a>Параметры сериализации MessagePack

MessagePack сериализацию можно настроить, предоставив делегат вызову [аддмессажепаккпротокол](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) . Дополнительные сведения см. [в разделе MessagePack in SignalR](xref:signalr/messagepackhubprotocol) .

> [!NOTE]
> В настоящее время невозможно настроить сериализацию MessagePack в клиенте JavaScript.

## <a name="configure-server-options"></a>Настройка параметров сервера

В следующей таблице описаны параметры для настройки центров SignalR.

::: moniker range=">= aspnetcore-3.0"

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 секунд | Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности"). Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано. Рекомендуемое значение — это значение типа Double, равное `KeepAliveInterval`.|
| `HandshakeTimeout` | 15 секунд | Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается. Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 секунд | Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось. При изменении `KeepAliveInterval`измените параметр `ServerTimeout`/`serverTimeoutInMilliseconds` на клиенте. Рекомендуемое `ServerTimeout`/`serverTimeoutInMilliseconds` является двойным значением `KeepAliveInterval`.  |
| `SupportedProtocols` | Все установленные протоколы | Протоколы, поддерживаемые этим концентратором. По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов. |
| `EnableDetailedErrors` | `false` | Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора. Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения. |
| `StreamBufferCapacity` | `10` | Максимальное число элементов, которые могут быть помещены в буфер для потоков загрузки клиента. При достижении этого предела обработка вызовов блокируется до тех пор, пока сервер не обработает потоковые элементы.|
| `MaximumReceiveMessageSize` | 32 КБ | Максимальный размер одного входящего сообщения концентратора. |

::: moniker-end

::: moniker range="= aspnetcore-2.2"

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 секунд | Сервер будет считать, что Клиент отключен, если в этот интервал не было получено сообщение (включая "срок поддержания активности"). Чтобы клиент фактически был помечен как отключенный, он может занять больше времени, чем это время ожидания, из-за того, как это реализовано. Рекомендуемое значение — это значение типа Double, равное `KeepAliveInterval`.|
| `HandshakeTimeout` | 15 секунд | Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается. Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 секунд | Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось. При изменении `KeepAliveInterval`измените параметр `ServerTimeout`/`serverTimeoutInMilliseconds` на клиенте. Рекомендуемое `ServerTimeout`/`serverTimeoutInMilliseconds` является двойным значением `KeepAliveInterval`.  |
| `SupportedProtocols` | Все установленные протоколы | Протоколы, поддерживаемые этим концентратором. По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов. |
| `EnableDetailedErrors` | `false` | Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора. Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения. |

::: moniker-end

::: moniker range="= aspnetcore-2.1"

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `HandshakeTimeout` | 15 секунд | Если клиент не отправляет исходное сообщение подтверждения в течение этого интервала времени, соединение закрывается. Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 секунд | Если сервер не отправил сообщение в течение этого интервала, сообщение проверки связи отправляется автоматически, чтобы подключение не открывалось. При изменении `KeepAliveInterval`измените параметр `ServerTimeout`/`serverTimeoutInMilliseconds` на клиенте. Рекомендуемое `ServerTimeout`/`serverTimeoutInMilliseconds` является двойным значением `KeepAliveInterval`.  |
| `SupportedProtocols` | Все установленные протоколы | Протоколы, поддерживаемые этим концентратором. По умолчанию все зарегистрированные на сервере протоколы разрешены, но в этом списке можно удалить протоколы, чтобы отключить определенные протоколы для отдельных концентраторов. |
| `EnableDetailedErrors` | `false` | Если `true`, подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора. Значение по умолчанию — `false`, так как эти сообщения об исключениях могут содержать конфиденциальные сведения. |

::: moniker-end

Параметры можно настроить для всех концентраторов, предоставив параметры делегата для вызова `AddSignalR` в `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    });
}
```

Параметры одного концентратора переопределяют глобальные параметры, предоставленные в `AddSignalR` и могут быть настроены с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Дополнительные параметры конфигурации HTTP

::: moniker range=">= aspnetcore-3.0"

Используйте `HttpConnectionDispatcherOptions` для настройки дополнительных параметров, относящихся к транспортам и управлению буферами памяти. Эти параметры настраиваются путем передачи делегата в [мафуб\<t >](/dotnet/api/microsoft.aspnetcore.builder.hubendpointroutebuilderextensions.maphub) в `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseRouting();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapHub<MyHub>("/myhub", options =>
        {
            options.Transports =
                HttpTransportType.WebSockets |
                HttpTransportType.LongPolling;
        });
    });
}
```
::: moniker-end

::: moniker range="<= aspnetcore-2.2"

Используйте `HttpConnectionDispatcherOptions` для настройки дополнительных параметров, относящихся к транспортам и управлению буферами памяти. Эти параметры настраиваются путем передачи делегата в [мафуб\<t >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseSignalR((configure) =>
    {
        var desiredTransports =
            HttpTransportType.WebSockets |
            HttpTransportType.LongPolling;

        configure.MapHub<MyHub>("/myhub", (options) =>
        {
            options.Transports = desiredTransports;
        });
    });
}
```

::: moniker-end

В следующей таблице описаны параметры для настройки расширенных HTTP-параметров ASP.NET Core SignalR.

::: moniker range=">= aspnetcore-3.0"

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 КБ | Максимальное число байтов, полученных от клиента, который буфер сервера перед применением обратной нагрузки. Увеличение этого значения позволяет серверу быстрее принимать сообщения большего размера без применения недостаточной нагрузки, но может увеличить потребление памяти. |
| `AuthorizationData` | Данные, автоматически собираемые из `Authorize` атрибутов, применяемых к классу Hub. | Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору. |
| `TransportMaxBufferSize` | 32 КБ | Максимальное число байтов, отправленных приложением, которые были буфером сервера, прежде чем наблюдать за нехваткой. Увеличение этого значения позволяет серверу быстрее занимать больше сообщений, не ожидая перехода на резервную нагрузку, но может увеличить потребление памяти. |
| `Transports` | Все транспорты включены. | Битовая перечисление битовых флагов значений `HttpTransportType`, которые могут ограничивать возможности транспорта, которые клиент может использовать для подключения. |
| `LongPolling` | См. ниже. | Дополнительные параметры, относящиеся к длительному транспортному потоку. |
| `WebSockets` | См. ниже. | Дополнительные параметры, относящиеся к транспортному протоколу WebSocket. |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 КБ | Максимальное число байтов, полученных от клиента, который является буфером сервера. Увеличение этого значения позволяет серверу принимать большие сообщения, но может негативно сказаться на потреблении памяти. |
| `AuthorizationData` | Данные, автоматически собираемые из `Authorize` атрибутов, применяемых к классу Hub. | Список объектов [иаусоризедата](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) , которые позволяют определить, разрешено ли клиенту подключаться к концентратору. |
| `TransportMaxBufferSize` | 32 КБ | Максимальное число байтов, отправленных приложением, которые используются в буфере сервера. Увеличение этого значения позволяет серверу передавать большие сообщения, но может негативно сказаться на потреблении памяти. |
| `Transports` | Все транспорты включены. | Битовая перечисление битовых флагов значений `HttpTransportType`, которые могут ограничивать возможности транспорта, которые клиент может использовать для подключения. |
| `LongPolling` | См. ниже. | Дополнительные параметры, относящиеся к длительному транспортному потоку. |
| `WebSockets` | См. ниже. | Дополнительные параметры, относящиеся к транспортному протоколу WebSocket. |

::: moniker-end

У транспорта с длинным опросом есть дополнительные параметры, которые можно настроить с помощью свойства `LongPolling`:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 секунд | Максимальное количество времени, в течение которого сервер ожидает отправки сообщения клиенту перед завершением одного запроса опроса. Уменьшение этого значения приводит к тому, что клиент будет чаще выдавать новые запросы на опрос. |

Транспорт WebSocket имеет дополнительные параметры, которые можно настроить с помощью свойства `WebSockets`:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 секунд | Когда сервер закрывается, если не удается закрыть клиент в течение этого интервала времени, соединение будет разорвано. |
| `SubProtocolSelector` | `null` | Делегат, который можно использовать для задания заголовка `Sec-WebSocket-Protocol` в пользовательском значении. Делегат получает значения, запрошенные клиентом в качестве входных данных, и ожидается, что он возвращает нужное значение. |

## <a name="configure-client-options"></a>Настройка параметров клиента

Параметры клиента можно настроить для типа `HubConnectionBuilder` (доступного в клиентах .NET и JavaScript). Он также доступен в клиенте Java, но `HttpHubConnectionBuilder` подкласс содержит параметры конфигурации построителя, а также сам `HubConnection`.

### <a name="configure-logging"></a>Настройка ведения журнала

Ведение журнала настраивается в клиенте .NET с помощью метода `ConfigureLogging`. Регистраторы и фильтры могут быть зарегистрированы так же, как и на сервере. Дополнительные сведения см. в документации по [ведению журнала ASP.NET Core](xref:fundamentals/logging/index) .

> [!NOTE]
> Чтобы зарегистрировать регистраторы, необходимо установить необходимые пакеты. Полный список см. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) документации.

Например, чтобы включить ведение журнала консоли, установите `Microsoft.Extensions.Logging.Console` пакет NuGet. Вызовите метод расширения `AddConsole`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

В клиенте JavaScript аналогичный метод `configureLogging` существует. Укажите `LogLevel` значение, указывающее минимальный уровень создаваемых сообщений журнала. Журналы записываются в окно консоли браузера.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

::: moniker range=">= aspnetcore-3.0"

Вместо `LogLevel` значения можно также указать `string` значение, представляющее имя уровня журнала. Это полезно при настройке SignalR ведения журнала в средах, где у вас нет доступа к `LogLevel`ным константам.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging("warn")
    .build();
```

В следующей таблице перечислены доступные уровни ведения журнала. Значение, которое вы задаете для `configureLogging`, задает **Минимальный** уровень ведения журнала, который будет регистрироваться в журнале. Сообщения, зарегистрированные на этом уровне **или перечисленные в таблице уровни**, будут занесены в журнал.

| Строковое                      | LogLevel               |
| --------------------------- | ---------------------- |
| `trace`                     | `LogLevel.Trace`       |
| `debug`                     | `LogLevel.Debug`       |
| `info` **или** `information` | `LogLevel.Information` |
| `warn` **или** `warning`     | `LogLevel.Warning`     |
| `error`                     | `LogLevel.Error`       |
| `critical`                  | `LogLevel.Critical`    |
| `none`                      | `LogLevel.None`        |

::: moniker-end

> [!NOTE]
> Чтобы полностью отключить ведение журнала, укажите `signalR.LogLevel.None` в методе `configureLogging`.

Дополнительные сведения о ведении журналов см. в [документации поSignalR диагностике](xref:signalr/diagnostics).

Клиент SignalR Java использует библиотеку [SLF4J](https://www.slf4j.org/) для ведения журнала. Это высокоуровневый API ведения журнала, который позволяет пользователям библиотеки выбирать собственную реализацию ведения журнала, применяя определенную зависимость от ведения журнала. В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом Java SignalR.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Если вы не настраиваете ведение журнала в зависимостях, SLF4J загружает средство ведения журнала без операций по умолчанию со следующим предупреждающим сообщением:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Это можно игнорировать.

### <a name="configure-allowed-transports"></a>Настройка разрешенных транспортов

Транспорты, используемые SignalR, можно настроить в вызове `WithUrl` (`withUrl` в JavaScript). Побитовое или для значений `HttpTransportType` можно использовать, чтобы ограничить клиент использованием только указанных транспортов. По умолчанию все транспорты включены.

Например, чтобы отключить транспорт событий, отправленных сервером, но разрешить соединения WebSockets и long опрашиваете:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

В клиенте JavaScript транспорты настраиваются путем установки поля `transport` в объекте Options, предоставленном для `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

В этой версии клиентские WebSocket-сокеты Java являются единственным доступным транспортом.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

В клиенте Java транспорт выбирается с помощью метода `withTransport` на `HttpHubConnectionBuilder`. Клиент Java по умолчанию использует транспорт WebSockets.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> Клиент SignalR Java еще не поддерживает откат транспорта.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Настройка проверки подлинности носителя

Чтобы предоставить данные проверки подлинности вместе с запросами SignalR, используйте параметр `AccessTokenProvider` (`accessTokenFactory` в JavaScript), чтобы указать функцию, которая возвращает нужный маркер доступа. В клиенте .NET этот маркер доступа передается в качестве токена "Проверка подлинности носителя" HTTP (с использованием заголовка `Authorization` с типом `Bearer`). В клиенте JavaScript маркер доступа используется в качестве токена носителя, **за исключением** случаев, когда интерфейсы API браузера ограничивают возможность применения заголовков (в частности, при отправке сервером событий и запросах WebSocket). В таких случаях маркер доступа предоставляется как значение строки запроса `access_token`.

В клиенте .NET параметр `AccessTokenProvider` можно указать с помощью делегата Options в `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

В клиенте JavaScript маркер доступа настраивается путем установки поля `accessTokenFactory` в объекте Options в `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        accessTokenFactory: () => {
            // Get and return the access token.
            // This function can return a JavaScript Promise if asynchronous
            // logic is required to retrieve the access token.
        }
    })
    .build();
```

В клиенте SignalR Java можно настроить токен носителя, который будет использоваться для проверки подлинности, предоставив фабрику маркеров доступа к [хттфубконнектионбуилдер](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Используйте [висакцесстокенфактори](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [Рксжава](https://github.com/ReactiveX/RxJava) [одно\<String >](https://reactivex.io/documentation/single.html). При вызове [Single. отсрочки](https://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-)можно написать логику для создания маркеров доступа для клиента.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Настройка времени ожидания и параметров поддержания активности

Дополнительные параметры для настройки времени ожидания и проверки активности доступны для самого `HubConnection` объекта:


::: moniker range=">= aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript). Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания. Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи. |
| `HandshakeTimeout` | 15 секунд | Время ожидания для первоначального подтверждения сервера. Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript). Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 секунд | Определяет интервал, с которым клиент отправляет сообщения проверки связи. Отправка любого сообщения от клиента сбрасывает таймер до начала интервала. Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен. |

В клиенте .NET значения времени ожидания указываются как `TimeSpan` значения.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`. Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания. Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи. |
| `keepAliveIntervalInMilliseconds` | 15 секунд (15 000 миллисекундах) | Определяет интервал, с которым клиент отправляет сообщения проверки связи. Отправка любого сообщения от клиента сбрасывает таймер до начала интервала. Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`. Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания. Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи. |
| `withHandshakeResponseTimeout` | 15 секунд | Время ожидания для первоначального подтверждения сервера. Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`. Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `getKeepAliveInterval` / `setKeepAliveInterval` | 15 секунд (15 000 миллисекунд) | Определяет интервал, с которым клиент отправляет сообщения проверки связи. Отправка любого сообщения от клиента сбрасывает таймер до начала интервала. Если клиент не отправил сообщение в `ClientTimeoutInterval`, установленном на сервере, сервер считает, что Клиент отключен. |

::: moniker-end

::: moniker range="< aspnetcore-2.2"

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `Closed` (`onclose` в JavaScript). Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания. Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи. |
| `HandshakeTimeout` | 15 секунд | Время ожидания для первоначального подтверждения сервера. Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `Closed` (`onclose` в JavaScript). Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

В клиенте .NET значения времени ожидания указываются как `TimeSpan` значения.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onclose`. Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания. Рекомендуемое значение — это число, по крайней мере, двойное значение `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `getServerTimeout` / `setServerTimeout` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этот интервал, клиент считает, что сервер отключен, и активирует событие `onClose`. Это значение должно быть достаточно большим для отправки сообщения проверки связи с сервера **и** получения клиентом в течение интервала времени ожидания. Рекомендуемое значение — это число по крайней мере удвоенного значения `KeepAliveInterval` сервера, чтобы разрешить получение проверки связи. |
| `withHandshakeResponseTimeout` | 15 секунд | Время ожидания для первоначального подтверждения сервера. Если сервер не отправляет ответ подтверждения в течение этого интервала, клиент отменяет подтверждение и активирует событие `onClose`. Это дополнительный параметр, который следует изменять, только если возникают ошибки времени ожидания подтверждения из-за серьезной задержки сети. Дополнительные сведения о процессе подтверждения связи см. в разделе [Спецификация протокола концентратораSignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

::: moniker-end

---

### <a name="configure-additional-options"></a>Настройка дополнительных параметров

Дополнительные параметры можно настроить в методе `WithUrl` (`withUrl` в JavaScript) на `HubConnectionBuilder` или в различных API-интерфейсах конфигурации `HttpHubConnectionBuilder` в клиенте Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Параметр .NET |  Значение по умолчанию | Описание |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах. |
| `SkipNegotiation` | `false` | Задайте значение `true`, чтобы пропустить шаг согласования. **Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**. Этот параметр нельзя включить при использовании службы SignalR Azure. |
| `ClientCertificates` | Empty | Коллекция сертификатов TLS для отправки в запросы проверки подлинности. |
| `Cookies` | Empty | Коллекция файлов cookie HTTP для отправки с каждым HTTP-запросом. |
| `Credentials` | Empty | Учетные данные для отправки с каждым HTTP-запросом. |
| `CloseTimeout` | 5 секунд | Только WebSockets. Максимальное время ожидания клиента после закрытия сервера для подтверждения запроса на закрытие. Если сервер не подтверждает закрытие в течение этого времени, клиент отключается. |
| `Headers` | Empty | Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом. |
| `HttpMessageHandlerFactory` | `null` | Делегат, который можно использовать для настройки или замены `HttpMessageHandler`, используемой для отправки HTTP-запросов. Не используется для соединений WebSocket. Этот делегат должен возвращать значение, отличное от NULL, и получает значение по умолчанию в качестве параметра. Либо измените параметры для этого значения по умолчанию и верните его, либо возвратите новый экземпляр `HttpMessageHandler`. **При замене обработчика обязательно скопируйте параметры, которые необходимо сохранить из предоставленного обработчика. в противном случае настроенные параметры (такие как файлы cookie и заголовки) не будут применяться к новому обработчику.** |
| `Proxy` | `null` | HTTP-прокси, используемый при отправке HTTP-запросов. |
| `UseDefaultCredentials` | `false` | Установите это логическое значение, чтобы отправить учетные данные по умолчанию для запросов HTTP и WebSockets. Это позволяет использовать проверку подлинности Windows. |
| `WebSocketConfiguration` | `null` | Делегат, который можно использовать для настройки дополнительных параметров WebSocket. Получает экземпляр [клиентвебсоккетоптионс](/dotnet/api/system.net.websockets.clientwebsocketoptions) , который можно использовать для настройки параметров. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Параметр JavaScript | Значение по умолчанию | Описание |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах. |
| `skipNegotiation` | `false` | Задайте значение `true`, чтобы пропустить шаг согласования. **Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**. Этот параметр нельзя включить при использовании службы SignalR Azure. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Параметр Java | Значение по умолчанию | Описание |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Функция, возвращающая строку, которая предоставляется в качестве маркера проверки подлинности носителя в HTTP-запросах. |
| `shouldSkipNegotiate` | `false` | Задайте значение `true`, чтобы пропустить шаг согласования. **Поддерживается только в том случае, если транспорт WebSocket является единственным включенным транспортом**. Этот параметр нельзя включить при использовании службы SignalR Azure. |
| `withHeader` `withHeaders` | Empty | Таблица дополнительных заголовков HTTP для отправки с каждым HTTP-запросом. |

---

В клиенте .NET эти параметры можно изменить с помощью делегата Options, предоставленного для `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

В клиенте JavaScript эти параметры можно указать в объекте JavaScript, предоставленном для `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

В клиенте Java эти параметры можно настроить с помощью методов в `HttpHubConnectionBuilder`, возвращаемых из `HubConnectionBuilder.create("HUB URL")`

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
        .withHeader("Foo", "Bar")
        .shouldSkipNegotiate(true)
        .withHandshakeResponseTimeout(30*1000)
        .build();
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
