---
title: Конфигурация ASP.NET Core SignalR
author: bradygaster
description: Сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/07/2019
uid: signalr/configuration
ms.openlocfilehash: 2c1bb8d5e317813d1fdb8d474b7d7d892e6f67ec
ms.sourcegitcommit: 57792e5f594db1574742588017c708350958bdf0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2019
ms.locfileid: "58264576"
---
# <a name="aspnet-core-signalr-configuration"></a>Конфигурация ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Параметры сериализации JSON/MessagePack

ASP.NET Core SignalR поддерживает два протокола для кодировки сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html). Каждый протокол имеет параметры конфигурации для сериализации.

Сериализация JSON можно настроить на сервере, используя [AddJsonProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) метод расширения, которую можно добавить после [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в вашей `Startup.ConfigureServices` метод. `AddJsonProtocol` Метод принимает делегат, который получает `options` объекта. [PayloadSerializerSettings](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) свойство на этот объект является JSON.NET `JsonSerializerSettings` объект, который может использоваться для настройки сериализации аргументов и возвращаемых значений. См. в разделе [документации по JSON.NET](https://www.newtonsoft.com/json/help/html/Introduction.htm) для получения дополнительных сведений.

Например чтобы настроить сериализатор, используемый имена свойств «PascalCase», а не имена «camelCase» по умолчанию, используйте следующий код:

```csharp
services.AddSignalR()
    .AddJsonProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver =
        new DefaultContractResolver();
    });
```

В клиента .NET, так же `AddJsonProtocol` метод расширения существует на [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Пространства имен должны импортироваться разрешить метод расширения:

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
> Не поддерживается для настройки сериализации JSON в клиенте JavaScript в данный момент.

### <a name="messagepack-serialization-options"></a>Параметры сериализации MessagePack

MessagePack сериализации можно настроить, предоставьте делегат для [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) вызова. См. в разделе [MessagePack в SignalR](xref:signalr/messagepackhubprotocol) для получения дополнительных сведений.

> [!NOTE]
> Не поддерживается для настройки сериализации MessagePack в клиенте JavaScript в данный момент.

## <a name="configure-server-options"></a>Настройка параметров сервера

В следующей таблице описаны параметры для настройки концентраторов SignalR:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ClientTimeoutInterval` | 30 секунд | Сервер будет рассматривать клиента отключен, если он еще не получил сообщение (включая keep-alive) в этом интервале. Может занять больше времени, чем этот интервал времени ожидания для клиента, фактически помечается как отключенный, из-за, как это реализовано. Рекомендуемое значение — double `KeepAliveInterval` значение.|
| `HandshakeTimeout` | 15 секунд | Если клиент не отправляет сообщение первоначального подтверждения в течение этого времени интервала, соединение закрывается. Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения. Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |
| `KeepAliveInterval` | 15 секунд | Если сервер не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение. При изменении `KeepAliveInterval`, изменить `ServerTimeout` / `serverTimeoutInMilliseconds` параметр на клиенте. Минимальная рекомендуемая `ServerTimeout` / `serverTimeoutInMilliseconds` значение double `KeepAliveInterval` значение.  |
| `SupportedProtocols` | Все установленные протоколы | Протоколы, поддерживаемые этого концентратора. По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из списка, чтобы отключить специальные протоколы для отдельных концентраторов. |
| `EnableDetailedErrors` | `false` | Если `true`подробные сообщения об исключениях возвращаются клиентам, когда исключение в методе концентратора. По умолчанию используется `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения. |

Можно настроить параметры для всех концентраторов, предоставляя делегат параметры для `AddSignalR` вызов в `Startup.ConfigureServices`.

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

Параметры для одного центра переопределить глобальные параметры, указанные на `AddSignalR` и могут настраиваться с помощью <xref:Microsoft.Extensions.DependencyInjection.SignalRDependencyInjectionExtensions.AddHubOptions*>:

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

### <a name="advanced-http-configuration-options"></a>Дополнительные параметры конфигурации HTTP

Используйте `HttpConnectionDispatcherOptions` для расширенной настройки, относящиеся к транспортов и управление буфером памяти. Эти параметры настраиваются путем передачи делегата для [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub) в `Startup.Configure`.

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

В следующей таблице описаны параметры для настройки дополнительных параметров HTTP ASP.NET Core SignalR:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 КБ | Максимальное число байтов, полученных от клиента, буферы сервера. Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти. |
| `AuthorizationData` | Данные, собранные из автоматически `Authorize` атрибутов, примененных к классу Hub. | Список [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору. |
| `TransportMaxBufferSize` | 32 КБ | Максимальное число байтов, отправленных в приложение, буферы сервера. Увеличение этого значения позволяет серверу для отправки сообщения большего размера, но может отрицательно повлиять на потребление памяти. |
| `Transports` | Включены все транспорты. | Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения. |
| `LongPolling` | См. ниже. | Дополнительные параметры, характерные для транспорта длинный опрос. |
| `WebSockets` | См. ниже. | Дополнительные параметры, характерные для транспорта WebSockets. |

Транспорт длинный опрос имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:

| Параметр | Значение по умолчанию | Описание: |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 секунд | Максимальное время ожидания сервером ответа сообщение, отправляемое клиенту перед завершением работы запрос на единый опроса. Уменьшение этого значения приводит к клиентам выполнять чаще, новые запросы опроса. |

Транспорт WebSocket имеет дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:

| Параметр | Значение по умолчанию | Описание: |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 секунд | После завершения работы сервера, если клиенту не удается закрыть в течение этого времени интервала, соединение будет разорвано. |
| `SubProtocolSelector` | `null` | Делегат, который может использоваться для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение. Делегат получает значения, запрошенной клиентом в качестве входных данных и должен возвращать необходимое значение. |

## <a name="configure-client-options"></a>Настройка параметров клиента

Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступна в клиентах .NET и JavaScript). Она также доступна в клиенте Java, но `HttpHubConnectionBuilder` производным классом является то, что содержит построитель параметры конфигурации, а также на `HubConnection` сам.

### <a name="configure-logging"></a>Настройка ведения журнала

Настроить ведение журнала с помощью клиента .NET `ConfigureLogging` метод. Ведение журнала, поставщики и фильтры могут быть зарегистрированы в так же как при работе на сервере. См. в разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index) Дополнительные сведения см.

> [!NOTE]
> Чтобы зарегистрировать поставщики ведения журналов, необходимо установить необходимые пакеты. См. в разделе [встроенные поставщики ведения журналов](xref:fundamentals/logging/index#built-in-logging-providers) разделе документы с полным списком.

Например, чтобы включить ведение журнала консоли, установить `Microsoft.Extensions.Logging.Console` пакет NuGet. Вызовите `AddConsole` метод расширения:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

В клиенте JavaScript, аналогичное `configureLogging` метод существует. Укажите `LogLevel` значение, указывающее минимальный уровень журнала сообщений для получения. Журналы записываются в окно консоли браузера.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Чтобы отключить ведение журнала полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.

Дополнительные сведения о ведении журнала см. в разделе [документации диагностики SignalR](xref:signalr/diagnostics).

Клиент SignalR Java использует [SLF4J](https://www.slf4j.org/) библиотеки для ведения журнала. Это API высокого уровня ведения журнала, который позволяет пользователям библиотеки выбрал свою собственную реализацию определенного ведения журнала, Собрав воедино в зависимость от конкретных ведения журнала. В следующем фрагменте кода показано, как использовать `java.util.logging` с клиентом SignalR Java.

```gradle
implementation 'org.slf4j:slf4j-jdk14:1.7.25'
```

Если не настроить ведение журнала в зависимости, SLF4J загружает средство ведения журнала по умолчанию без операции с следующее предупреждающее сообщение:

```
SLF4J: Failed to load class "org.slf4j.impl.StaticLoggerBinder".
SLF4J: Defaulting to no-operation (NOP) logger implementation
SLF4J: See http://www.slf4j.org/codes.html#StaticLoggerBinder for further details.
```

Это можно игнорировать.

### <a name="configure-allowed-transports"></a>Настройка разрешенных транспортов

Можно настроить транспорт, используемый с SignalR в `WithUrl` вызова (`withUrl` в JavaScript). Побитовое или-значения `HttpTransportType` может использоваться для ограничения клиента на использование только указанного транспорта. Все транспорты включены по умолчанию.

Например чтобы отключить события Server-Sent транспорта, но подключений WebSockets и длинный опрос:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

В клиенте JavaScript транспортов настраиваются, задав `transport` на объект параметры, передаваемые `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

::: moniker range=">= aspnetcore-2.2"

В этой версии Java клиента websockets-это доступно только транспорт.

::: moniker-end

::: moniker range="= aspnetcore-3.0"

В клиенте Java, следует выбрать транспорт `withTransport` метод `HttpHubConnectionBuilder`. В клиенте Java по умолчанию с помощью транспорта WebSockets.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withTransport(TransportEnum.WEBSOCKETS)
    .build();
```

> [!NOTE]
> Клиент SignalR Java не поддерживает резервный транспорт.

::: moniker-end

### <a name="configure-bearer-authentication"></a>Настройка проверки подлинности носителя

Чтобы указать данные проверки подлинности, а также SignalR запросов, используйте `AccessTokenProvider` параметр (`accessTokenFactory` в JavaScript) для указания функция, которая возвращает маркер необходимый тип доступа. В клиенте .NET, этот маркер передается как HTTP «Проверки подлинности носителя» токена (с помощью `Authorization` заголовка с типом `Bearer`). В клиенте JavaScript используется маркер доступа в качестве токена носителя, **за исключением** в некоторых случаях, где обозреватель API-интерфейсы ограничить возможность применения заголовки (в частности, в запросы событий Server-Sent и WebSockets). В этих случаях маркер доступа предоставляется как значение строки запроса `access_token`.

В клиенте .NET `AccessTokenProvider` параметр может быть указан с помощью делегата параметры в `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        };
    })
    .Build();
```

В клиенте JavaScript настроен маркер доступа, задав `accessTokenFactory` на объект параметров в `withUrl`:

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

В клиенте SignalR Java, можно настроить токен носителя для проверки подлинности, предоставляя фабрику токена доступа для [HttpHubConnectionBuilder](/java/api/com.microsoft.signalr._http_hub_connection_builder?view=aspnet-signalr-java). Используйте [withAccessTokenFactory](/java/api/com.microsoft.signalr._http_hub_connection_builder.withaccesstokenprovider?view=aspnet-signalr-java#com_microsoft_signalr__http_hub_connection_builder_withAccessTokenProvider_Single_String__) для предоставления [RxJava](https://github.com/ReactiveX/RxJava) [единый\<строка >](http://reactivex.io/documentation/single.html). С помощью вызова [Single.defer](http://reactivex.io/RxJava/javadoc/io/reactivex/Single.html#defer-java.util.concurrent.Callable-), можно написать логику для получения маркеров доступа для вашего клиента.

```java
HubConnection hubConnection = HubConnectionBuilder.create("https://example.com/myhub")
    .withAccessTokenProvider(Single.defer(() -> {
        // Your logic here.
        return Single.just("An Access Token");
    })).build();
```

### <a name="configure-timeout-and-keep-alive-options"></a>Настройка времени ожидания и параметры проверки активности

Доступны дополнительные параметры для настройки времени ожидания и поведение активности на `HubConnection` сам объект:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Параметр | Значение по умолчанию | Описание: |
| ------ | ------------- | ----------- |
| `ServerTimeout` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `Closed` событий (`onclose` в JavaScript). Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания. Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения. |
| `HandshakeTimeout` | 15 секунд | Время ожидания подтверждения исходным сервером. Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `Closed` событий (`onclose` в JavaScript). Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения. Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения.

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Параметр | Значение по умолчанию | Описание: |
| ------ | ------------- | ----------- |
| `serverTimeoutInMilliseconds` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `onclose` событий. Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания. Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Параметр | Значение по умолчанию | Описание: |
| ----------- | ------------- | ----------- |
|`getServerTimeout` `setServerTimeout` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `onClose` событий. Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания. Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения. |
| `withHandshakeResponseTimeout` | 15 секунд | Время ожидания подтверждения исходным сервером. Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `onClose` событий. Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения. Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

---

### <a name="configure-additional-options"></a>Настройка дополнительных параметров

Дополнительные параметры можно настроить в `WithUrl` (`withUrl` в JavaScript) метод `HubConnectionBuilder` или на различных интерфейсов API настройки на `HttpHubConnectionBuilder` в клиенте Java:

# <a name="nettabdotnet"></a>[.NET](#tab/dotnet)

| Параметр .NET |  Значение по умолчанию | Описание: |
| ----------- | -------------- | ----------- |
| `AccessTokenProvider` | `null` | Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов. |
| `SkipNegotiation` | `false` | Задайте значение `true` пропустить шаг согласования. **Поддерживается только при передаче WebSockets только транспорта включена**. Этот параметр не может быть включен, при использовании служба Azure SignalR. |
| `ClientCertificates` | Empty | Коллекция TLS-сертификатов, отправляемых для проверки подлинности запросов. |
| `Cookies` | Empty | Коллекция файлов cookie HTTP для отправки с каждым запросом HTTP. |
| `Credentials` | Empty | Учетные данные для отправки с каждым запросом HTTP. |
| `CloseTimeout` | 5 секунд | WebSockets. Максимальное количество времени, клиент ожидает после закрывающего тега для сервера подтвердить запрос на закрытие. Если сервер не подтвердил закрытия в течение этого времени, клиент отключается. |
| `Headers` | Empty | Сопоставление дополнительных HTTP-заголовков для отправки с каждым запросом HTTP. |
| `HttpMessageHandlerFactory` | `null` | Делегат, который может использоваться для настройки или замените `HttpMessageHandler` используется для отправки запросов HTTP. Не используется для соединений WebSocket. Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра. Измените параметры на это значение по умолчанию и вернуть его или возвращают новый `HttpMessageHandler` экземпляра. **Если заменить обработчик обязательно скопируйте параметры, которые вы хотите сохранить из обработчика, в противном случае настроенные параметры (например, файлы cookie и заголовки) нельзя применить новый обработчик.** |
| `Proxy` | `null` | HTTP-прокси для использования при отправке HTTP-запросов. |
| `UseDefaultCredentials` | `false` | Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets. Это позволяет использовать проверку подлинности Windows. |
| `WebSocketConfiguration` | `null` | Делегат, который может использоваться для настройки дополнительных параметров WebSocket. Получает экземпляр класса [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров. |

# <a name="javascripttabjavascript"></a>[JavaScript](#tab/javascript)

| Параметр JavaScript | Значение по умолчанию | Описание: |
| ----------------- | ------------- | ----------- |
| `accessTokenFactory` | `null` | Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов. |
| `skipNegotiation` | `false` | Задайте значение `true` пропустить шаг согласования. **Поддерживается только при передаче WebSockets только транспорта включена**. Этот параметр не может быть включен, при использовании служба Azure SignalR. |

# <a name="javatabjava"></a>[Java](#tab/java)

| Параметр Java | Значение по умолчанию | Описание: |
| ----------- | ------------- | ----------- |
| `withAccessTokenProvider` | `null` | Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов. |
| `shouldSkipNegotiate` | `false` | Задайте значение `true` пропустить шаг согласования. **Поддерживается только при передаче WebSockets только транспорта включена**. Этот параметр не может быть включен, при использовании служба Azure SignalR. |
| `withHeader` `withHeaders` | Empty | Сопоставление дополнительных HTTP-заголовков для отправки с каждым запросом HTTP. |

---

В клиенте .NET эти параметры можно изменить параметры делегат для `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.Headers["Foo"] = "Bar";
        options.Cookies.Add(new Cookie(/* ... */);
        options.ClientCertificates.Add(/* ... */);
    })
    .Build();
```

В клиенте JavaScript, эти параметры могут быть предоставлены в JavaScript объект, предоставляемый для `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", {
        skipNegotiation: true,
        transport: signalR.HttpTransportType.WebSockets
    })
    .build();
```

В клиенте Java, эти параметры можно настроить на с методами `HttpHubConnectionBuilder` возвращаемые `HubConnectionBuilder.create("HUB URL")`

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
