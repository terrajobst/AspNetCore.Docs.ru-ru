---
title: "Открытый веб-интерфейс для .NET (OWIN)"
author: ardalis
description: "Узнайте, как ASP.NET Core поддерживает открытие веб-интерфейса для .NET (OWIN), который позволяет веб-приложений, связано с веб-серверов."
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 42ffa01745b7a492b3b8cb2778805f254863b890
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a>Общие сведения, чтобы открыть веб-интерфейс для .NET (OWIN)

По [Стив Смит](https://ardalis.com/) и [Рик Андерсон](https://twitter.com/RickAndMSFT)

ASP.NET Core поддерживает открытый веб-интерфейс для .NET (OWIN). OWIN позволяет ослабить зависимость веб-приложений от веб-сервера. Он определяет стандартный способ для промежуточного по для использования в конвейере для обработки запросов и связанных ответов. Приложения ASP.NET Core и по промежуточного слоя могут взаимодействовать с по промежуточного слоя, серверов и приложений на основе OWIN.

OWIN обеспечивает разделение уровень, позволяющий две платформы с разрозненными объектные модели для совместного использования. `Microsoft.AspNetCore.Owin` Пакет содержит две реализации адаптера:
- ASP.NET Core для OWIN 
- OWIN для ASP.NET Core

Это позволяет ASP.NET Core для размещения на основе OWIN server совместимую или для других совместимых компонентов OWIN для выполнения на основе ASP.NET Core.

Примечание: С помощью этих адаптеров поставляется с затратами по производительности. Приложения, использующие только компоненты ASP.NET Core не следует использовать пакет Owin или адаптеров.

[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a>Под управлением OWIN по промежуточного слоя в конвейере ASP.NET

Поддержка OWIN ASP.NET Core развертывается как часть `Microsoft.AspNetCore.Owin` пакета. Поддержка OWIN можно импортировать в проект при установке этого пакета.

По промежуточного слоя OWIN соответствует [спецификации OWIN](http://owin.org/spec/spec/owin-1.0.0.html), что требует `Func<IDictionary<string, object>, Task>` задать интерфейс и определенные ключи (такие как `owin.ResponseBody`). Следующий простой OWIN по промежуточного слоя отображается «Hello, World!»:

```csharp
public Task OwinHello(IDictionary<string, object> environment)
{
    string responseText = "Hello World via OWIN";
    byte[] responseBytes = Encoding.UTF8.GetBytes(responseText);

    // OWIN Environment Keys: http://owin.org/spec/spec/owin-1.0.0.html
    var responseStream = (Stream)environment["owin.ResponseBody"];
    var responseHeaders = (IDictionary<string, string[]>)environment["owin.ResponseHeaders"];

    responseHeaders["Content-Length"] = new string[] { responseBytes.Length.ToString(CultureInfo.InvariantCulture) };
    responseHeaders["Content-Type"] = new string[] { "text/plain" };

    return responseStream.WriteAsync(responseBytes, 0, responseBytes.Length);
}
```

Возвращает образец подписи `Task` и принимает `IDictionary<string, object>` согласно требованиям OWIN.

Следующий код демонстрирует добавление `OwinHello` (показано выше) в конвейере ASP.NET с по промежуточного слоя `UseOwin` метода расширения.

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}
```

Можно настроить другие действия для выполнения в конвейер OWIN.

> [!NOTE]
> Заголовки ответа может изменяться только до первой записи в поток ответа.

> [!NOTE]
> Несколько вызовов `UseOwin` не рекомендуется по соображениям производительности. Компоненты OWIN будет работать наилучшим образом, если группируются вместе.

```csharp
app.UseOwin(pipeline =>
{
    pipeline(next =>
    {
        // do something before
        return OwinHello;
        // do something after
    });
});
```

<a name="hosting-on-owin"></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a>С помощью ASP.NET размещения на сервере под управлением OWIN

Серверы на основе OWIN можно размещать приложения ASP.NET. Один сервер является [Nowin](https://github.com/Bobris/Nowin), веб-сервера .NET OWIN. В этом образце для данной статьи я включил проект, который ссылается на Nowin и использует ее для создания `IServer` для самостоятельного размещения ASP.NET Core.

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

`IServer`— Это интерфейс, который требует `Features` свойство и `Start` метод.

`Start`отвечает за настройки и запуска на сервер, который в этом случае выполняется с помощью ряда fluent вызовы API, которые установить синтаксический анализ адреса из IServerAddressesFeature. Обратите внимание, что конфигурация fluent `_builder` переменная указывает, что запросы будут обрабатываться `appFunc` определенные ранее в метод. Это `Func` вызывается при каждом запросе для обработки входящих запросов.

Также мы добавим `IWebHostBuilder` расширения облегчает добавление и настройка сервера Nowin.

```csharp
using System;
using Microsoft.AspNetCore.Hosting.Server;
using Microsoft.Extensions.DependencyInjection;
using Nowin;
using NowinSample;

namespace Microsoft.AspNetCore.Hosting
{
    public static class NowinWebHostBuilderExtensions
    {
        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder)
        {
            return builder.ConfigureServices(services =>
            {
                services.AddSingleton<IServer, NowinServer>();
            });
        }

        public static IWebHostBuilder UseNowin(this IWebHostBuilder builder, Action<ServerBuilder> configure)
        {
            builder.ConfigureServices(services =>
            {
                services.Configure(configure);
            });
            return builder.UseNowin();
        }
    }
}
```

Это место, все, что необходимые для запуска приложения ASP.NET с помощью этого пользовательского сервера для вызова модуля *Program.cs*:

```csharp

using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Microsoft.AspNetCore.Hosting;

namespace NowinSample
{
    public class Program
    {
        public static void Main(string[] args)
        {
            var host = new WebHostBuilder()
                .UseNowin()
                .UseContentRoot(Directory.GetCurrentDirectory())
                .UseIISIntegration()
                .UseStartup<Startup>()
                .Build();

            host.Run();
        }
    }
}

```

Дополнительные сведения о ASP.NET [серверов](servers/index.md).

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a>Запустить ASP.NET Core на сервере под управлением OWIN и использовать его поддержка WebSockets

Будет доступен другой пример серверы на базе OWIN функций ASP.NET Core имеет доступ к функциям, например WebSockets. Веб-сервера .NET OWIN, используемый в предыдущем примере реализована поддержка веб-сокеты встроенным, который может использоваться приложением ASP.NET Core. В приведенном ниже примере показан простой веб-приложения, который поддерживает веб-сокеты и возвращает обратно все данные отправляются на сервер через WebSockets.

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            if (context.WebSockets.IsWebSocketRequest)
            {
                WebSocket webSocket = await context.WebSockets.AcceptWebSocketAsync();
                await EchoWebSocket(webSocket);
            }
            else
            {
                await next();
            }
        });

        app.Run(context =>
        {
            return context.Response.WriteAsync("Hello World");
        });
    }

    private async Task EchoWebSocket(WebSocket webSocket)
    {
        byte[] buffer = new byte[1024];
        WebSocketReceiveResult received = await webSocket.ReceiveAsync(
            new ArraySegment<byte>(buffer), CancellationToken.None);

        while (!webSocket.CloseStatus.HasValue)
        {
            // Echo anything we receive
            await webSocket.SendAsync(new ArraySegment<byte>(buffer, 0, received.Count), 
                received.MessageType, received.EndOfMessage, CancellationToken.None);

            received = await webSocket.ReceiveAsync(new ArraySegment<byte>(buffer), 
                CancellationToken.None);
        }

        await webSocket.CloseAsync(webSocket.CloseStatus.Value, 
            webSocket.CloseStatusDescription, CancellationToken.None);
    }
}
```

Это [пример](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) настраивается с помощью одного `NowinServer` как предыдущего - единственное различие заключается в том, как приложение настроено в его `Configure` метод. Тест, с помощью [клиент простой websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) демонстрирует приложение:

![Веб-сокета тестового клиента](owin/_static/websocket-test.png)

## <a name="owin-environment"></a>Среда OWIN

Можно создать среду OWIN с помощью `HttpContext`.

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a>Ключи OWIN

Зависит от OWIN `IDictionary<string,object>` объекта для передачи сведений в HTTP-запроса или ответа exchange. ASP.NET Core реализует перечисленные ниже ключи. В разделе [основной спецификации, расширения](http://owin.org/#spec), и [OWIN ключ рекомендации и общие ключи](http://owin.org/spec/spec/CommonKeys.html).

### <a name="request-data-owin-v100"></a>Данные запроса (версии 1.0.0 OWIN)

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| owin.RequestScheme | `String` |  |
| owin. RequestMethod  | `String` | |    
| owin.RequestPathBase  | `String` | |    
| owin.RequestPath | `String` | |     
| owin.RequestQueryString  | `String` | |    
| owin.RequestProtocol  | `String` | |    
| owin. RequestHeaders | `IDictionary<string,string[]>`  | |
| owin.RequestBody | `Stream`  | |

### <a name="request-data-owin-v110"></a>Данные запроса (OWIN v1.1.0)

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| owin. ИД запроса | `String` | Optional |

### <a name="response-data-owin-v100"></a>Данные ответа (версии 1.0.0 OWIN)

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| owin.ResponseStatusCode | `int` | Optional |
| owin.ResponseReasonPhrase | `String` | Optional |
| owin. ResponseHeaders | `IDictionary<string,string[]>`  | |
| owin.ResponseBody | `Stream`  | |


### <a name="other-data-owin-v100"></a>Другие данные (версии 1.0.0 OWIN)

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| owin. CallCancelled | `CancellationToken` |  |
| owin. Версия  | `String` | |   


### <a name="common-keys"></a>Общие ключи

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| ssl.ClientCertificate | `X509Certificate` |  |
| ssl.LoadClientCertAsync  | `Func<Task>` | |    
| server.RemoteIpAddress  | `String` | |    
| server.RemotePort | `String` | |     
| server.LocalIpAddress  | `String` | |    
| server.LocalPort  | `String` | |    
| сервер. IsLocal  | `bool` | |    
| сервер. Псевдособытие  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a>SendFiles v0.3.0

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| sendfile.SendAsync | В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) | По запросу |


### <a name="opaque-v030"></a>Непрозрачный v0.3.0

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| opaque.Version | `String` |  |
| непрозрачный. Обновление | `OpaqueUpgrade` | В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| opaque.Stream | `Stream` |  |
| непрозрачный. CallCancelled | `CancellationToken` |  |


### <a name="websocket-v030"></a>WebSocket v0.3.0

| Ключ               | Значение (тип) | Описание: |
| ----------------- | ------------ | ----------- |
| WebSocket. Версия | `String` |  |
| websocket.Accept | `WebSocketAccept` | В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm) |
| websocket.AcceptAlt |  | Non-spec |
| WebSocket. Подпротокол | `String` | В разделе [RFC6455 раздел 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) шаг 5.5 |
| websocket.SendAsync | `WebSocketSendAsync` | В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.ReceiveAsync | `WebSocketReceiveAsync` | В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| websocket.CloseAsync | `WebSocketCloseAsync` | В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)  |
| WebSocket. CallCancelled | `CancellationToken` |  |
| websocket.ClientCloseStatus | `int` | Optional |
| websocket.ClientCloseDescription | `String` | Optional |


## <a name="additional-resources"></a>Дополнительные ресурсы

* [ПО промежуточного слоя](middleware.md)

* [Серверы](servers/index.md)
