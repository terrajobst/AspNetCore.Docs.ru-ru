---
title: Конфигурация ASP.NET Core SignalR
author: rachelappel
description: Подробные сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 06/30/2018
uid: signalr/configuration
ms.openlocfilehash: 167b8828efd093d755aeb1c91e080dbd22b5d485
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2018
ms.locfileid: "36961990"
---
# <a name="aspnet-core-signalr-configuration"></a>Конфигурация ASP.NET Core SignalR

## <a name="jsonmessagepack-serialization-options"></a>Параметры сериализации JSON/MessagePack

ASP.NET Core SignalR поддерживает два протокола для кодировки сообщений: [JSON](https://www.json.org/) и [MessagePack](https://msgpack.org/index.html). Каждый протокол имеет параметры конфигурации сериализации.

Сериализация JSON можно настроить на сервере с помощью [ `AddJsonProtocol` ](/dotnet/api/microsoft.extensions.dependencyinjection.jsonprotocoldependencyinjectionextensions.addjsonprotocol) метод расширения, которые могут быть установлены после [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr) в ваш `Startup.ConfigureServices` метод. `AddJsonProtocol` Метод принимает делегат, принимающий `options` объекта. [ `PayloadSerializerSettings` ](/dotnet/api/microsoft.aspnetcore.signalr.jsonhubprotocoloptions.payloadserializersettings) Свойство на этот объект является JSON.NET `JsonSerializerSettings` объект, который можно использовать для настройки сериализации аргументов и возвращаемых значений. В разделе [JSON.NET документации](https://www.newtonsoft.com/json/help/html/Introduction.htm) для получения дополнительных сведений.

Например чтобы настроить сериализатор, используемый вместо имен по умолчанию «camelCase», «PascalCase» имена свойств, используйте следующий код:

```csharp
services.AddSignalR()
    .AddJsonHubProtocol(options => {
        options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
    });
```

В клиента .NET, так же `AddJsonHubProtocol` метод расширения существует на [HubConnectionBuilder](/dotnet/api/microsoft.aspnetcore.signalr.client.hubconnectionbuilder). `Microsoft.Extensions.DependencyInjection` Пространства имен должны импортироваться в разрешении метода расширения:

```csharp
// At the top of the file:
using Microsoft.Extensions.DependencyInjection;

// When constructing your connection:
var connection = new HubConnectionBuilder()
.AddJsonHubProtocol(options => {
    options.PayloadSerializerSettings.ContractResolver = 
        new DefaultContractResolver();
});
```

> [!NOTE]
> Не поддерживается для настройки сериализации JSON в клиенте JavaScript в данный момент.

### <a name="messagepack-serialization-options"></a>Параметры сериализации MessagePack

Можно настроить сериализацию MessagePack, предоставляя делегат для [AddMessagePackProtocol](/dotnet/api/microsoft.extensions.dependencyinjection.msgpackprotocoldependencyinjectionextensions.addmessagepackprotocol) вызова. В разделе [MessagePack в SignalR](xref:signalr/messagepackhubprotocol) для получения дополнительных сведений.

> [!NOTE]
> Настройка MessagePack сериализации в клиента JavaScript в данный момент невозможна.

## <a name="configure-server-options"></a>Настройка параметров сервера

В следующей таблице описаны параметры для настройки концентраторы SignalR:

| Параметр | Описание: |
| ------ | ----------- |
| `HandshakeTimeout` | Если клиент не отправляет сообщение первоначальное подтверждение установления связи в течение этого интервала времени, соединение закрывается. |
| `KeepAliveInterval` | Если сервер еще не отправил сообщение в течение этого интервала, автоматически отправляется сообщение ping необходимо сохранить открытым подключение. |
| `SupportedProtocols` | Протоколы, поддерживаемые этого концентратора. По умолчанию разрешены все протоколы, которые зарегистрированы на сервере, но протоколы можно удалить из этого списка, чтобы отключить протоколы, определенные для отдельных узлов. |
| `EnableDetailedErrors` | Если `true`подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе концентратора. Значение по умолчанию — `false`, как эти сообщения об исключениях могут содержать конфиденциальные сведения. |

Можно настроить параметры для всех концентраторов, предоставляя делегата параметры для `AddSignalR` вызов в `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddSignalR(hubOptions =>
    {
        hubOptions.EnableDetailedErrors = true;
        hubOptions.KeepAliveInterval = TimeSpan.FromMinutes(1);
    })
}
```

Параметры для одного концентратора переопределить глобальные параметры, представленные в `AddSignalR` и могут быть настроены с помощью [ `AddHubOptions<T>` ](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
}
```

Используйте `HttpConnectionDispatcherOptions` настроить дополнительные параметры, относящиеся к транспортов и управление буфером памяти. Эти параметры настраиваются путем передачи делегата для [ `MapHub<T>` ](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Параметр | Описание: |
| ------ | ----------- |
| `ApplicationMaxBufferSize` | Максимальное число байтов, полученных от клиента, буферы сервера. Увеличение этого значения позволяет серверу для получения больших сообщений, но может отрицательно повлиять на потребление памяти. Значение по умолчанию составляет 32 КБ. |
| `AuthorizationData` | Список [ `IAuthorizeData` ](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору. По умолчанию заполняется значениями из `Authorize` атрибуты, примененные к классу Hub. |
| `TransportMaxBufferSize` | Максимальное число байтов, отправляемых приложением, буферы сервера. Увеличение этого значения позволяет серверу отправлять сообщения большего размера, однако может отрицательно повлиять на потребление памяти. Значение по умолчанию составляет 32 КБ. |
| `Transports` | Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения. Все транспорты включены по умолчанию. |
| `LongPolling` | Дополнительные параметры, характерные для транспорта Long опроса. |
| `WebSockets` | Дополнительные параметры, характерные для транспорта WebSockets. |

Транспорт Long опроса имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:

| Параметр | Описание: |
| ------ | ----------- |
| `PollTimeout` | Максимальное время ожидания сервером ответа сообщения для отправки клиенту перед завершением работы опроса один запрос. Уменьшение этого значения приводит к тому, клиент может новые запросы опрос чаще. Значение по умолчанию — 90 секунд. |

Транспорт WebSocket отображаются дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:

| Параметр | Описание: |
| ------ | ----------- |
| `CloseTimeout` | После закрытия сервера, если клиент не сможет закрыть в течение этого интервала, соединение будет разорвано. |
| `SubProtocolSelector` | Делегат, который можно использовать для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение. Делегат получает запроса от клиента в качестве входного значения и должен возвращать нужное значение. |

## <a name="configure-client-options"></a>Настройка параметров клиента

Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступный в клиенты .NET и JavaScript), а также в `HubConnection` сам.

### <a name="configure-logging"></a>Настройка ведения журнала

Запись в журнал настраивается с помощью клиента .NET `ConfigureLogging` метод. Ведение журнала поставщики и фильтры могут быть зарегистрированы так же как при работе на сервере. В разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Дополнительные сведения см.

> [!NOTE]
> Чтобы зарегистрировать регистраторов, необходимо установить необходимые пакеты. В разделе [встроенных регистраторов](xref:fundamentals/logging/index#built-in-logging-providers) раздел документации для получения полного списка.

Например, чтобы включить ведение журнала консоли, установить `Microsoft.Extensions.Logging.Console` пакет NuGet. Вызовите `AddConsole` метода расширения:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub")
    .ConfigureLogging(logging => {
        logging.SetMinimumLevel(LogLevel.Information);
        logging.AddConsole();
    })
    .Build();
```

В клиенте JavaScript, аналогичное `configureLogging` метод существует. Укажите `LogLevel` значение, указывающее, минимальный уровень сообщений журнала для создания. Журналы записываются в окно консоли браузера.

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub")
    .configureLogging(signalR.LogLevel.Information)
    .build();
```

> [!NOTE]
> Чтобы отключить регистрацию полностью, укажите `signalR.LogLevel.None` в `configureLogging` метод.

Ниже перечислены уровни журнала, доступные для клиента JavaScript. Установка уровня журнала в одно из следующих значений ведение журнала сообщений на **или более поздней версии** этого уровня.

| Уровень | Описание: |
| ----- | ----------- |
| `None` | Сообщения не регистрируются. |
| `Critical` | Сообщения, которые указывают на сбой в всего приложения. |
| `Error` | Сообщения, которые указывают на сбой во время выполнения текущей операции. |
| `Warning` | Сообщения, которые указывают на проблему, не являющиеся неустранимыми. |
| `Information` | Информационные сообщения. |
| `Debug` | Диагностические сообщения полезны для отладки. |
| `Trace` | Подробные диагностические сообщения, предназначены для диагностики определенных проблем. |

### <a name="configure-allowed-transports"></a>Настройка разрешенных транспортов

Транспорт, используемый SignalR можно настроить в `WithUrl` вызова (`withUrl` на языке JavaScript). Побитовое или значений `HttpTransportType` может использоваться для ограничения клиента на использование только для указанного транспорта. Все транспорты включены по умолчанию.

Например чтобы отключить события Server-Sent транспорта, но разрешить подключения WebSockets и Long опроса:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", HttpTransportType.WebSockets | HttpTransportType.LongPolling)
    .Build();
```

В клиенте JavaScript транспортов настроены, задав `transport` на объект параметров, передаваемых `withUrl`:

```javascript
let connection = new signalR.HubConnectionBuilder()
    .withUrl("/myhub", { transport: signalR.HttpTransportType.WebSockets | signalR.HttpTransportType.LongPolling })
    .build();
```

### <a name="configure-bearer-authentication"></a>Настройка проверки подлинности носителя

Чтобы предоставить данные проверки подлинности вместе с SignalR запросов, используйте `AccessTokenProvider` параметр (`accessTokenFactory` на языке JavaScript) для указания функция, которая возвращает токен требуемых прав доступа. В клиенте .NET, этот токен доступа передается как HTTP «Проверки подлинности носителя» токена (с помощью `Authorization` заголовок с типом `Bearer`). В клиенте JavaScript используется маркер доступа в качестве токен носителя, **за исключением** в некоторых случаях, когда обозреватель API-интерфейсы ограничить возможность применить заголовки (в частности, в события Server-Sent и WebSockets запросов). В этих случаях токен доступа предоставляется как значение строки запроса `access_token`.

В клиенте .NET `AccessTokenProvider` параметр можно указать с помощью параметров делегата в `WithUrl`:

```csharp
var connection = new HubConnectionBuilder()
    .WithUrl("https://example.com/myhub", options => {
        options.AccessTokenProvider = async () => {
            // Get and return the access token.
        }
    })
    .Build();
```

В клиенте JavaScript маркер доступа настроена, задав `accessTokenFactory` на объект параметров в `withUrl`:

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

### <a name="configure-timeout-and-keep-alive-options"></a>Настройка времени ожидания и сохранения параметров

Доступны дополнительные параметры для настройки времени ожидания и активности поведение на `HubConnection` сам объект:

| Параметр .NET | Параметр JavaScript | Описание: |
| ----------- | ----------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | Время ожидания активности сервера. Если сервер еще не отправил все сообщения в этот промежуток времени, клиент считает, что сервер отключен и триггер `Closed` событий (`onclose` на языке JavaScript). |
| `HandshakeTimeout` | Не настраивается | Время ожидания подтверждения исходным сервером. Если сервер не отправляет ответ подтверждения в этот промежуток времени, клиент не отменит подтверждения и триггер `Closed` событий (`onclose` на языке JavaScript). |

В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения. В клиенте JavaScript значения времени ожидания задаются как числа. Цифры представляют значения времени в миллисекундах.

### <a name="configure-additional-options"></a>Настройка дополнительных параметров

Можно настроить дополнительные параметры в `WithUrl` (`withUrl` на языке JavaScript) метод `HubConnectionBuilder`:

| Параметр .NET | Параметр JavaScript | Описание: |
| ----------- | ----------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | Функция, возвращающая строка, которая предоставляется в качестве маркера проверки подлинности с носителя в HTTP-запросов. |
| `SkipNegotiation` | `skipNegotiation` | Задайте значение `true` для пропуска шага согласования. **Поддерживается только при передаче по WebSockets только включенные транспорта**. Этот параметр не включен, при использовании службы Azure SignalR. |
| `ClientCertificates` | Не настраивается * | Коллекция сертификатов TLS для отправки для проверки подлинности запросов. |
| `Cookies` | Не настраивается * | Коллекция файлов cookie HTTP для отправки в каждом HTTP-запросе. |
| `Credentials` | Не настраивается * | Учетные данные для отправки в каждом HTTP-запросе. |
| `CloseTimeout` | Не настраивается * | WebSockets. Максимальное время ожидания клиентом после закрывающего тега для сервера подтвердить запрос на закрытие. Если сервер не подтвердил закрытия в течение этого времени, отключении клиента. |
| `Headers` | Не настраивается * | Словарь дополнительных заголовков HTTP для отправки в каждом HTTP-запросе. |
| `HttpMessageHandlerFactory` | Не настраивается * | Делегат, который можно использовать для настройки или замените `HttpMessageHandler` для отправки запросов HTTP. Не используется для соединения WebSocket. Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра. Измените параметры в значения по умолчанию и вернуть его или вернуть совершенно новую `HttpMessageHandler` экземпляра. |
| `Proxy` | Не настраивается * | Прокси-сервер HTTP при отправке запросов HTTP. |
| `UseDefaultCredentials` | Не настраивается * | Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets. Это позволяет использовать проверку подлинности Windows. |
| `WebSocketConfiguration` | Не настраивается * | Делегат, который можно использовать для настройки дополнительных параметров WebSocket. Получает экземпляр [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров. |

Параметры, помеченные звездочкой (*) не могут быть изменены в клиент JavaScript, из-за ограничений в браузере API-интерфейсы.

В клиенте .NET эти параметры могут быть изменены параметры делегат, передаваемый `WithUrl`:

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
    });
    .build();
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
