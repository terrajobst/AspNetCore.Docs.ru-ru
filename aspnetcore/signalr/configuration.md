---
title: Конфигурация ASP.NET Core SignalR
author: tdykstra
description: Сведения о настройке приложения ASP.NET Core SignalR.
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/06/2018
uid: signalr/configuration
ms.openlocfilehash: 72fc53cad7caf55e85d0668c9dbea1a70fc8674b
ms.sourcegitcommit: c684eb6c0999d11d19e15e65939e5c7f99ba47df
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/19/2018
ms.locfileid: "46292340"
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

Параметры для одного центра переопределить глобальные параметры, указанные на `AddSignalR` и могут настраиваться с помощью [AddHubOptions\<T >](/dotnet/api/microsoft.extensions.dependencyinjection.huboptionsdependencyinjectionextensions.addhuboptions):

```csharp
services.AddSignalR().AddHubOptions<MyHub>(options =>
{
    options.EnableDetailedErrors = true;
});
```

Используйте `HttpConnectionDispatcherOptions` для расширенной настройки, относящиеся к транспортов и управление буфером памяти. Эти параметры настраиваются путем передачи делегата для [MapHub\<T >](/dotnet/api/microsoft.aspnetcore.signalr.hubroutebuilder.maphub).

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `ApplicationMaxBufferSize` | 32 КБ | Максимальное число байтов, полученных от клиента, буферы сервера. Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти. |
| `AuthorizationData` | Данные, собранные из автоматически `Authorize` атрибутов, примененных к классу Hub. | Список [IAuthorizeData](/dotnet/api/microsoft.aspnetcore.authorization.iauthorizedata) объекты, используемые для определения того, если клиенту разрешено подключаться к концентратору. |
| `TransportMaxBufferSize` | 32 КБ | Максимальное число байтов, отправленных в приложение, буферы сервера. Увеличение этого значения позволяет серверу для отправки сообщения большего размера, но может отрицательно повлиять на потребление памяти. |
| `Transports` | Включены все транспорты. | Битовая маска `HttpTransportType` значения, которые можно ограничить транспорты, которые клиент может использовать для подключения. |
| `LongPolling` | См. ниже. | Дополнительные параметры, характерные для транспорта длинный опрос. |
| `WebSockets` | См. ниже. | Дополнительные параметры, характерные для транспорта WebSockets. |

Транспорт длинный опрос имеет дополнительные параметры, которые могут быть настроены с помощью `LongPolling` свойство:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `PollTimeout` | 90 секунд | Максимальное время ожидания сервером ответа сообщение, отправляемое клиенту перед завершением работы запрос на единый опроса. Уменьшение этого значения приводит к клиентам выполнять чаще, новые запросы опроса. |

Транспорт WebSocket имеет дополнительные параметры, которые могут быть настроены с помощью `WebSockets` свойство:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `CloseTimeout` | 5 секунд | После завершения работы сервера, если клиенту не удается закрыть в течение этого времени интервала, соединение будет разорвано. |
| `SubProtocolSelector` | `null` | Делегат, который может использоваться для задания `Sec-WebSocket-Protocol` заголовок пользовательское значение. Делегат получает значения, запрошенной клиентом в качестве входных данных и должен возвращать необходимое значение. |

## <a name="configure-client-options"></a>Настройка параметров клиента

Можно настроить параметры клиента на `HubConnectionBuilder` тип (доступна в клиентах .NET и JavaScript), а также в `HubConnection` сам.

### <a name="configure-logging"></a>Настройка ведения журнала

Настроить ведение журнала с помощью клиента .NET `ConfigureLogging` метод. Ведение журнала, поставщики и фильтры могут быть зарегистрированы в так же как при работе на сервере. См. в разделе [ведения журналов в ASP.NET Core](xref:fundamentals/logging/index#how-to-add-providers) Дополнительные сведения см.

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

Ниже перечислены уровни ведения журнала, доступные для клиента JavaScript. Установка уровня журнала в одно из следующих значений включает ведение журнала сообщений в **или более поздней версии** этого уровня.

| Уровень | Описание |
| ----- | ----------- |
| `None` | Сообщения не регистрируются. |
| `Critical` | Сообщения, которые означают сбой всего приложения. |
| `Error` | Сообщения, которые указывают на сбой в текущей операции. |
| `Warning` | Сообщения, которые указывают на проблему, не являющиеся неустранимыми. |
| `Information` | Информационные сообщения. |
| `Debug` | Диагностические сообщения, полезны для отладки. |
| `Trace` | Очень подробные диагностические сообщения, предназначены для диагностики конкретных проблем. |

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

### <a name="configure-timeout-and-keep-alive-options"></a>Настройка времени ожидания и параметры проверки активности

Доступны дополнительные параметры для настройки времени ожидания и поведение активности на `HubConnection` сам объект:

| Параметр .NET | Параметр JavaScript | Значение по умолчанию | Описание |
| ----------- | ----------------- | ------------- | ----------- |
| `ServerTimeout` | `serverTimeoutInMilliseconds` | 30 секунд (30 000 миллисекунд) | Время ожидания активности сервера. Если сервер не отправил сообщение в этом интервале, клиент считает, что сервер отключен и триггеры `Closed` событий (`onclose` в JavaScript). Это значение должно быть достаточно большим для получения сообщения ping, отправляемых с сервера **и** полученные клиентом в течение времени ожидания. Рекомендуемое значение — номера по крайней мере двойное server `KeepAliveInterval` значение, чтобы выделить время для проверки связи для получения. |
| `HandshakeTimeout` | Недоступно для настройки | 15 секунд | Время ожидания подтверждения исходным сервером. Если сервер не отправляет ответ подтверждения в этом интервале, клиент отменяет подтверждения и триггеры `Closed` событий (`onclose` в JavaScript). Это расширенный параметр, который может изменяться только если из-за серьезных сетевой задержки происходят ошибки времени ожидания подтверждения. Подробности, касающиеся процесса подтверждения, см. в разделе [спецификации протокола концентратора SignalR](https://github.com/aspnet/SignalR/blob/master/specs/HubProtocol.md). |

В клиенте .NET, значения времени ожидания задаются в виде `TimeSpan` значения. В клиенте JavaScript значения времени ожидания задаются как число, указывающее длительность в миллисекундах.

### <a name="configure-additional-options"></a>Настройка дополнительных параметров

Дополнительные параметры можно настроить в `WithUrl` (`withUrl` в JavaScript) метод `HubConnectionBuilder`:

| Параметр .NET | Параметр JavaScript | Значение по умолчанию | Описание |
| ----------- | ----------------- | ------------- | ----------- |
| `AccessTokenProvider` | `accessTokenFactory` | `null` | Функция, возвращающая строку, которая предоставляется как токен проверки подлинности носителя в HTTP-запросов. |
| `SkipNegotiation` | `skipNegotiation` | `false` | Задайте значение `true` пропустить шаг согласования. **Поддерживается только при передаче WebSockets только транспорта включена**. Этот параметр не может быть включен, при использовании служба Azure SignalR. |
| `ClientCertificates` | Недоступно для настройки * | Empty | Коллекция TLS-сертификатов, отправляемых для проверки подлинности запросов. |
| `Cookies` | Недоступно для настройки * | Empty | Коллекция файлов cookie HTTP для отправки с каждым запросом HTTP. |
| `Credentials` | Недоступно для настройки * | Empty | Учетные данные для отправки с каждым запросом HTTP. |
| `CloseTimeout` | Недоступно для настройки * | 5 секунд | WebSockets. Максимальное количество времени, клиент ожидает после закрывающего тега для сервера подтвердить запрос на закрытие. Если сервер не подтвердил закрытия в течение этого времени, клиент отключается. |
| `Headers` | Недоступно для настройки * | Empty | Словарь дополнительных HTTP-заголовков для отправки с каждым запросом HTTP. |
| `HttpMessageHandlerFactory` | Недоступно для настройки * | `null` | Делегат, который может использоваться для настройки или замените `HttpMessageHandler` используется для отправки запросов HTTP. Не используется для соединений WebSocket. Этот делегат должен возвращать значение отличное от null, и он получает значение по умолчанию в качестве параметра. Измените параметры на это значение по умолчанию и вернуть его или возвращают новый `HttpMessageHandler` экземпляра. **Если заменить обработчик обязательно скопируйте параметры, которые вы хотите сохранить из обработчика, в противном случае настроенные параметры (например, файлы cookie и заголовки) нельзя применить новый обработчик.** |
| `Proxy` | Недоступно для настройки * | `null` | HTTP-прокси для использования при отправке HTTP-запросов. |
| `UseDefaultCredentials` | Недоступно для настройки * | `false` | Задает это значение типа boolean для отправки учетных данных по умолчанию для запросов HTTP и WebSockets. Это позволяет использовать проверку подлинности Windows. |
| `WebSocketConfiguration` | Недоступно для настройки * | `null` | Делегат, который может использоваться для настройки дополнительных параметров WebSocket. Получает экземпляр класса [ClientWebSocketOptions](/dotnet/api/system.net.websockets.clientwebsocketoptions) , можно использовать для настройки параметров. |

Параметры, отмеченные звездочкой (*) не может быть настроено в клиенте JavaScript из-за ограничений в браузере API-интерфейсы.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/signalr>
* <xref:signalr/hubs>
* <xref:signalr/javascript-client>
* <xref:signalr/dotnet-client>
* <xref:signalr/messagepackhubprotocol>
* <xref:signalr/supported-platforms>
