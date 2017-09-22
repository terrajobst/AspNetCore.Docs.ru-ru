---
title: "Открыть веб-интерфейс для .NET (OWIN)"
author: ardalis
description: "Общие сведения, чтобы открыть веб-интерфейс для .NET (OWIN)."
keywords: "ASP.NET Core, откройте веб-интерфейс .NET, OWIN"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 70c4e6bc-a773-4039-96ec-6fe557c9369d
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/owin
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9edacb494c38d7812f9e3826ab9277cd1dffd675
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-open-web-interface-for-net-owin"></a><span data-ttu-id="ea527-104">Общие сведения, чтобы открыть веб-интерфейс для .NET (OWIN)</span><span class="sxs-lookup"><span data-stu-id="ea527-104">Introduction to Open Web Interface for .NET (OWIN)</span></span>

<span data-ttu-id="ea527-105">По [Стив Смит](https://ardalis.com/) и [Рик Андерсон](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="ea527-105">By [Steve Smith](https://ardalis.com/) and  [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="ea527-106">ASP.NET Core поддерживает открытие веб-интерфейс .NET (OWIN).</span><span class="sxs-lookup"><span data-stu-id="ea527-106">ASP.NET Core supports the Open Web Interface for .NET (OWIN).</span></span> <span data-ttu-id="ea527-107">OWIN позволяет веб-приложений, связано с веб-серверов.</span><span class="sxs-lookup"><span data-stu-id="ea527-107">OWIN allows web apps to be decoupled from web servers.</span></span> <span data-ttu-id="ea527-108">Он определяет стандартный способ для промежуточного по для использования в конвейере для обработки запросов и связанных ответов.</span><span class="sxs-lookup"><span data-stu-id="ea527-108">It defines a standard way for middleware to be used in a pipeline to handle requests and associated responses.</span></span> <span data-ttu-id="ea527-109">Приложения ASP.NET Core и по промежуточного слоя могут взаимодействовать с по промежуточного слоя, серверов и приложений на основе OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea527-109">ASP.NET Core applications and middleware can interoperate with OWIN-based applications, servers, and middleware.</span></span>

<span data-ttu-id="ea527-110">OWIN обеспечивает разделение уровень, позволяющий две платформы с разрозненными объектные модели для совместного использования.</span><span class="sxs-lookup"><span data-stu-id="ea527-110">OWIN provides a decoupling layer that allows two frameworks with disparate object models to be used together.</span></span> <span data-ttu-id="ea527-111">`Microsoft.AspNetCore.Owin` Пакет содержит две реализации адаптера:</span><span class="sxs-lookup"><span data-stu-id="ea527-111">The `Microsoft.AspNetCore.Owin` package provides two adapter implementations:</span></span>
- <span data-ttu-id="ea527-112">ASP.NET Core для OWIN</span><span class="sxs-lookup"><span data-stu-id="ea527-112">ASP.NET Core to OWIN</span></span> 
- <span data-ttu-id="ea527-113">OWIN для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="ea527-113">OWIN to ASP.NET Core</span></span>

<span data-ttu-id="ea527-114">Это позволяет ASP.NET Core для размещения на основе OWIN server совместимую или для других совместимых компонентов OWIN для выполнения на основе ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea527-114">This allows ASP.NET Core to be hosted on top of an OWIN compatible server/host, or for other OWIN compatible components to be run on top of ASP.NET Core.</span></span>

<span data-ttu-id="ea527-115">Примечание: С помощью этих адаптеров поставляется с затратами по производительности.</span><span class="sxs-lookup"><span data-stu-id="ea527-115">Note: Using these adapters comes with a performance cost.</span></span> <span data-ttu-id="ea527-116">Приложения, использующие только компоненты ASP.NET Core не следует использовать пакет Owin или адаптеров.</span><span class="sxs-lookup"><span data-stu-id="ea527-116">Applications using only ASP.NET Core components should not use the Owin package or adapters.</span></span>

[<span data-ttu-id="ea527-117">Просмотреть или скачать образец кода</span><span class="sxs-lookup"><span data-stu-id="ea527-117">View or download sample code</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample)

## <a name="running-owin-middleware-in-the-aspnet-pipeline"></a><span data-ttu-id="ea527-118">Под управлением OWIN по промежуточного слоя в конвейере ASP.NET</span><span class="sxs-lookup"><span data-stu-id="ea527-118">Running OWIN middleware in the ASP.NET pipeline</span></span>

<span data-ttu-id="ea527-119">Поддержка OWIN ASP.NET Core развертывается как часть `Microsoft.AspNetCore.Owin` пакета.</span><span class="sxs-lookup"><span data-stu-id="ea527-119">ASP.NET Core's OWIN support is deployed as part of the `Microsoft.AspNetCore.Owin` package.</span></span> <span data-ttu-id="ea527-120">Поддержка OWIN можно импортировать в проект при установке этого пакета.</span><span class="sxs-lookup"><span data-stu-id="ea527-120">You can import OWIN support into your project by installing this package.</span></span>

<span data-ttu-id="ea527-121">По промежуточного слоя OWIN соответствует [спецификации OWIN](http://owin.org/spec/spec/owin-1.0.0.html), что требует `Func<IDictionary<string, object>, Task>` задать интерфейс и определенные ключи (такие как `owin.ResponseBody`).</span><span class="sxs-lookup"><span data-stu-id="ea527-121">OWIN middleware conforms to the [OWIN specification](http://owin.org/spec/spec/owin-1.0.0.html), which requires a `Func<IDictionary<string, object>, Task>` interface, and specific keys be set (such as `owin.ResponseBody`).</span></span> <span data-ttu-id="ea527-122">Следующий простой OWIN по промежуточного слоя отображается «Hello, World!»:</span><span class="sxs-lookup"><span data-stu-id="ea527-122">The following simple OWIN middleware displays "Hello World":</span></span>

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

<span data-ttu-id="ea527-123">Возвращает образец подписи `Task` и принимает `IDictionary<string, object>` согласно требованиям OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea527-123">The sample signature returns a `Task` and accepts an `IDictionary<string, object>` as required by OWIN.</span></span>

<span data-ttu-id="ea527-124">Следующий код демонстрирует добавление `OwinHello` (показано выше) в конвейере ASP.NET с по промежуточного слоя `UseOwin` метода расширения.</span><span class="sxs-lookup"><span data-stu-id="ea527-124">The following code shows how to add the `OwinHello` middleware (shown above) to the ASP.NET pipeline with the `UseOwin` extension method.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/OwinSample/Startup.cs"} -->

```csharp
public void Configure(IApplicationBuilder app)
{
    app.UseOwin(pipeline =>
    {
        pipeline(next => OwinHello);
    });
}


```

<span data-ttu-id="ea527-125">Можно настроить другие действия для выполнения в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea527-125">You can configure other actions to take place within the OWIN pipeline.</span></span>

> [!NOTE]
> <span data-ttu-id="ea527-126">Заголовки ответа может изменяться только до первой записи в поток ответа.</span><span class="sxs-lookup"><span data-stu-id="ea527-126">Response headers should only be modified prior to the first write to the response stream.</span></span>

> [!NOTE]
> <span data-ttu-id="ea527-127">Несколько вызовов `UseOwin` не рекомендуется по соображениям производительности.</span><span class="sxs-lookup"><span data-stu-id="ea527-127">Multiple calls to `UseOwin` is discouraged for performance reasons.</span></span> <span data-ttu-id="ea527-128">Компоненты OWIN будет работать наилучшим образом, если группируются вместе.</span><span class="sxs-lookup"><span data-stu-id="ea527-128">OWIN components will operate best if grouped together.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

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

<a name=hosting-on-owin></a>

## <a name="using-aspnet-hosting-on-an-owin-based-server"></a><span data-ttu-id="ea527-129">С помощью ASP.NET размещения на сервере под управлением OWIN</span><span class="sxs-lookup"><span data-stu-id="ea527-129">Using ASP.NET Hosting on an OWIN-based server</span></span>

<span data-ttu-id="ea527-130">Серверы на основе OWIN можно размещать приложения ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="ea527-130">OWIN-based servers can host ASP.NET applications.</span></span> <span data-ttu-id="ea527-131">Один сервер является [Nowin](https://github.com/Bobris/Nowin), веб-сервера .NET OWIN.</span><span class="sxs-lookup"><span data-stu-id="ea527-131">One such server is [Nowin](https://github.com/Bobris/Nowin), a .NET OWIN web server.</span></span> <span data-ttu-id="ea527-132">В этом образце для данной статьи я включил проект, который ссылается на Nowin и использует ее для создания `IServer` для самостоятельного размещения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea527-132">In the sample for this article, I've included a project that references Nowin and uses it to create an `IServer` capable of self-hosting ASP.NET Core.</span></span>

[!code-csharp[Main](owin/sample/src/NowinSample/Program.cs?highlight=15)]

<span data-ttu-id="ea527-133">`IServer`— Это интерфейс, который требует `Features` свойство и `Start` метод.</span><span class="sxs-lookup"><span data-stu-id="ea527-133">`IServer` is an interface that requires an `Features` property and a `Start` method.</span></span>

<span data-ttu-id="ea527-134">`Start`отвечает за настройки и запуска на сервер, который в этом случае выполняется с помощью ряда fluent вызовы API, которые установить синтаксический анализ адреса из IServerAddressesFeature.</span><span class="sxs-lookup"><span data-stu-id="ea527-134">`Start` is responsible for configuring and starting the server, which in this case is done through a series of fluent API calls that set addresses parsed from the IServerAddressesFeature.</span></span> <span data-ttu-id="ea527-135">Обратите внимание, что конфигурация fluent `_builder` переменная указывает, что запросы будут обрабатываться `appFunc` определенные ранее в метод.</span><span class="sxs-lookup"><span data-stu-id="ea527-135">Note that the fluent configuration of the `_builder` variable specifies that requests will be handled by the `appFunc` defined earlier in the method.</span></span> <span data-ttu-id="ea527-136">Это `Func` вызывается при каждом запросе для обработки входящих запросов.</span><span class="sxs-lookup"><span data-stu-id="ea527-136">This `Func` is called on each request to process incoming requests.</span></span>

<span data-ttu-id="ea527-137">Также мы добавим `IWebHostBuilder` расширения облегчает добавление и настройка сервера Nowin.</span><span class="sxs-lookup"><span data-stu-id="ea527-137">We'll also add an `IWebHostBuilder` extension to make it easy to add and configure the Nowin server.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [11], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/NowinWebHostBuilderExtensions.cs"} -->

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

<span data-ttu-id="ea527-138">Это место, все, что необходимые для запуска приложения ASP.NET с помощью этого пользовательского сервера для вызова модуля *Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="ea527-138">With this in place, all that's required to run an ASP.NET application using this custom server to call the extension in *Program.cs*:</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [15], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinSample/Program.cs"} -->

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

<span data-ttu-id="ea527-139">Дополнительные сведения о ASP.NET [серверов](servers/index.md).</span><span class="sxs-lookup"><span data-stu-id="ea527-139">Learn more about ASP.NET [Servers](servers/index.md).</span></span>

## <a name="run-aspnet-core-on-an-owin-based-server-and-use-its-websockets-support"></a><span data-ttu-id="ea527-140">Запустить ASP.NET Core на сервере под управлением OWIN и использовать его поддержка WebSockets</span><span class="sxs-lookup"><span data-stu-id="ea527-140">Run ASP.NET Core on an OWIN-based server and use its WebSockets support</span></span>

<span data-ttu-id="ea527-141">Будет доступен другой пример серверы на базе OWIN функций ASP.NET Core имеет доступ к функциям, например WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ea527-141">Another example of how OWIN-based servers' features can be leveraged by ASP.NET Core is access to features like WebSockets.</span></span> <span data-ttu-id="ea527-142">Веб-сервера .NET OWIN, используемый в предыдущем примере реализована поддержка веб-сокеты встроенным, который может использоваться приложением ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="ea527-142">The .NET OWIN web server used in the previous example has support for Web Sockets built in, which can be leveraged by an ASP.NET Core application.</span></span> <span data-ttu-id="ea527-143">В приведенном ниже примере показан простой веб-приложения, который поддерживает веб-сокеты и возвращает обратно все данные отправляются на сервер через WebSockets.</span><span class="sxs-lookup"><span data-stu-id="ea527-143">The example below shows a simple web app that supports Web Sockets and echoes back everything sent to the server through WebSockets.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {"hl_lines": [7, 9, 10], "linenostart": 1}, "backrefs": [], "dupnames": [], "linenos": true, "classes": [], "xml:space": "preserve", "language": "c#", "source": "/Users/shirhatti/src/Docs/aspnet/fundamentals/owin/sample/src/NowinWebSockets/Startup.cs"} -->

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

<span data-ttu-id="ea527-144">Это [пример](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) настраивается с помощью одного `NowinServer` как предыдущего - единственное различие заключается в том, как приложение настроено в его `Configure` метод.</span><span class="sxs-lookup"><span data-stu-id="ea527-144">This [sample](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/owin/sample) is configured using the same `NowinServer` as the previous one - the only difference is in how the application is configured in its `Configure` method.</span></span> <span data-ttu-id="ea527-145">Тест, с помощью [клиент простой websocket](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) демонстрирует приложение:</span><span class="sxs-lookup"><span data-stu-id="ea527-145">A test using [a simple websocket client](https://chrome.google.com/webstore/detail/simple-websocket-client/pfdhoblngboilpfeibdedpjgfnlcodoo?hl=en) demonstrates  the application:</span></span>

![Веб-сокета тестового клиента](owin/_static/websocket-test.png)

## <a name="owin-environment"></a><span data-ttu-id="ea527-147">Среда OWIN</span><span class="sxs-lookup"><span data-stu-id="ea527-147">OWIN environment</span></span>

<span data-ttu-id="ea527-148">Можно создать среду OWIN с помощью `HttpContext`.</span><span class="sxs-lookup"><span data-stu-id="ea527-148">You can construct a OWIN environment using the `HttpContext`.</span></span>

<!-- literal_block {"ids": [], "names": [], "highlight_args": {}, "backrefs": [], "dupnames": [], "linenos": false, "classes": [], "xml:space": "preserve", "language": "c#"} -->

```csharp

   var environment = new OwinEnvironment(HttpContext);
   var features = new OwinFeatureCollection(environment);
   ```

## <a name="owin-keys"></a><span data-ttu-id="ea527-149">Ключи OWIN</span><span class="sxs-lookup"><span data-stu-id="ea527-149">OWIN keys</span></span>

<span data-ttu-id="ea527-150">Зависит от OWIN `IDictionary<string,object>` объекта для передачи сведений в HTTP-запроса или ответа exchange.</span><span class="sxs-lookup"><span data-stu-id="ea527-150">OWIN depends on an `IDictionary<string,object>` object to communicate information throughout an HTTP Request/Response exchange.</span></span> <span data-ttu-id="ea527-151">ASP.NET Core реализует перечисленные ниже ключи.</span><span class="sxs-lookup"><span data-stu-id="ea527-151">ASP.NET Core implements the keys listed below.</span></span> <span data-ttu-id="ea527-152">В разделе [основной спецификации, расширения](http://owin.org/#spec), и [OWIN ключ рекомендации и общие ключи](http://owin.org/spec/spec/CommonKeys.html).</span><span class="sxs-lookup"><span data-stu-id="ea527-152">See the [primary specification, extensions](http://owin.org/#spec), and [OWIN Key Guidelines and Common Keys](http://owin.org/spec/spec/CommonKeys.html).</span></span>

### <a name="request-data-owin-v100"></a><span data-ttu-id="ea527-153">Данные запроса (версии 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="ea527-153">Request Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="ea527-154">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-154">Key</span></span>               | <span data-ttu-id="ea527-155">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-155">Value (type)</span></span> | <span data-ttu-id="ea527-156">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-156">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-157">owin. RequestScheme</span><span class="sxs-lookup"><span data-stu-id="ea527-157">owin.RequestScheme</span></span> | `String` |  |
| <span data-ttu-id="ea527-158">owin. RequestMethod</span><span class="sxs-lookup"><span data-stu-id="ea527-158">owin.RequestMethod</span></span>  | `String` | |    
| <span data-ttu-id="ea527-159">owin. RequestPathBase</span><span class="sxs-lookup"><span data-stu-id="ea527-159">owin.RequestPathBase</span></span>  | `String` | |    
| <span data-ttu-id="ea527-160">owin. RequestPath</span><span class="sxs-lookup"><span data-stu-id="ea527-160">owin.RequestPath</span></span> | `String` | |     
| <span data-ttu-id="ea527-161">owin. RequestQueryString</span><span class="sxs-lookup"><span data-stu-id="ea527-161">owin.RequestQueryString</span></span>  | `String` | |    
| <span data-ttu-id="ea527-162">owin. RequestProtocol</span><span class="sxs-lookup"><span data-stu-id="ea527-162">owin.RequestProtocol</span></span>  | `String` | |    
| <span data-ttu-id="ea527-163">owin. RequestHeaders</span><span class="sxs-lookup"><span data-stu-id="ea527-163">owin.RequestHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="ea527-164">owin. RequestBody</span><span class="sxs-lookup"><span data-stu-id="ea527-164">owin.RequestBody</span></span> | `Stream`  | |

### <a name="request-data-owin-v110"></a><span data-ttu-id="ea527-165">Данные запроса (OWIN v1.1.0)</span><span class="sxs-lookup"><span data-stu-id="ea527-165">Request Data (OWIN v1.1.0)</span></span>

| <span data-ttu-id="ea527-166">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-166">Key</span></span>               | <span data-ttu-id="ea527-167">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-167">Value (type)</span></span> | <span data-ttu-id="ea527-168">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-168">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-169">owin. ИД запроса</span><span class="sxs-lookup"><span data-stu-id="ea527-169">owin.RequestId</span></span> | `String` | <span data-ttu-id="ea527-170">Optional</span><span class="sxs-lookup"><span data-stu-id="ea527-170">Optional</span></span> |

### <a name="response-data-owin-v100"></a><span data-ttu-id="ea527-171">Данные ответа (версии 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="ea527-171">Response Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="ea527-172">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-172">Key</span></span>               | <span data-ttu-id="ea527-173">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-173">Value (type)</span></span> | <span data-ttu-id="ea527-174">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-174">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-175">owin. ResponseStatusCode</span><span class="sxs-lookup"><span data-stu-id="ea527-175">owin.ResponseStatusCode</span></span> | `int` | <span data-ttu-id="ea527-176">Optional</span><span class="sxs-lookup"><span data-stu-id="ea527-176">Optional</span></span> |
| <span data-ttu-id="ea527-177">owin. ResponseReasonPhrase</span><span class="sxs-lookup"><span data-stu-id="ea527-177">owin.ResponseReasonPhrase</span></span> | `String` | <span data-ttu-id="ea527-178">Optional</span><span class="sxs-lookup"><span data-stu-id="ea527-178">Optional</span></span> |
| <span data-ttu-id="ea527-179">owin. ResponseHeaders</span><span class="sxs-lookup"><span data-stu-id="ea527-179">owin.ResponseHeaders</span></span> | `IDictionary<string,string[]>`  | |
| <span data-ttu-id="ea527-180">owin. ResponseBody</span><span class="sxs-lookup"><span data-stu-id="ea527-180">owin.ResponseBody</span></span> | `Stream`  | |


### <a name="other-data-owin-v100"></a><span data-ttu-id="ea527-181">Другие данные (версии 1.0.0 OWIN)</span><span class="sxs-lookup"><span data-stu-id="ea527-181">Other Data (OWIN v1.0.0)</span></span>

| <span data-ttu-id="ea527-182">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-182">Key</span></span>               | <span data-ttu-id="ea527-183">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-183">Value (type)</span></span> | <span data-ttu-id="ea527-184">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-184">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-185">owin. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="ea527-185">owin.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="ea527-186">owin. Версия</span><span class="sxs-lookup"><span data-stu-id="ea527-186">owin.Version</span></span>  | `String` | |   


### <a name="common-keys"></a><span data-ttu-id="ea527-187">Общие ключи</span><span class="sxs-lookup"><span data-stu-id="ea527-187">Common Keys</span></span>

| <span data-ttu-id="ea527-188">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-188">Key</span></span>               | <span data-ttu-id="ea527-189">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-189">Value (type)</span></span> | <span data-ttu-id="ea527-190">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-190">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-191">протокол SSL. ClientCertificate</span><span class="sxs-lookup"><span data-stu-id="ea527-191">ssl.ClientCertificate</span></span> | `X509Certificate` |  |
| <span data-ttu-id="ea527-192">протокол SSL. LoadClientCertAsync</span><span class="sxs-lookup"><span data-stu-id="ea527-192">ssl.LoadClientCertAsync</span></span>  | `Func<Task>` | |    
| <span data-ttu-id="ea527-193">сервер. RemoteIpAddress</span><span class="sxs-lookup"><span data-stu-id="ea527-193">server.RemoteIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="ea527-194">сервер. Удаленный порт</span><span class="sxs-lookup"><span data-stu-id="ea527-194">server.RemotePort</span></span> | `String` | |     
| <span data-ttu-id="ea527-195">сервер. LocalIpAddress</span><span class="sxs-lookup"><span data-stu-id="ea527-195">server.LocalIpAddress</span></span>  | `String` | |    
| <span data-ttu-id="ea527-196">сервер. LocalPort</span><span class="sxs-lookup"><span data-stu-id="ea527-196">server.LocalPort</span></span>  | `String` | |    
| <span data-ttu-id="ea527-197">сервер. IsLocal</span><span class="sxs-lookup"><span data-stu-id="ea527-197">server.IsLocal</span></span>  | `bool` | |    
| <span data-ttu-id="ea527-198">сервер. Псевдособытие</span><span class="sxs-lookup"><span data-stu-id="ea527-198">server.OnSendingHeaders</span></span>  | `Action<Action<object>,object>` | |


### <a name="sendfiles-v030"></a><span data-ttu-id="ea527-199">SendFiles v0.3.0</span><span class="sxs-lookup"><span data-stu-id="ea527-199">SendFiles v0.3.0</span></span>

| <span data-ttu-id="ea527-200">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-200">Key</span></span>               | <span data-ttu-id="ea527-201">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-201">Value (type)</span></span> | <span data-ttu-id="ea527-202">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-202">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-203">sendfile. SendAsync</span><span class="sxs-lookup"><span data-stu-id="ea527-203">sendfile.SendAsync</span></span> | <span data-ttu-id="ea527-204">В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea527-204">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> | <span data-ttu-id="ea527-205">По запросу</span><span class="sxs-lookup"><span data-stu-id="ea527-205">Per Request</span></span> |


### <a name="opaque-v030"></a><span data-ttu-id="ea527-206">Непрозрачный v0.3.0</span><span class="sxs-lookup"><span data-stu-id="ea527-206">Opaque v0.3.0</span></span>

| <span data-ttu-id="ea527-207">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-207">Key</span></span>               | <span data-ttu-id="ea527-208">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-208">Value (type)</span></span> | <span data-ttu-id="ea527-209">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-209">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-210">непрозрачный. Версия</span><span class="sxs-lookup"><span data-stu-id="ea527-210">opaque.Version</span></span> | `String` |  |
| <span data-ttu-id="ea527-211">непрозрачный. Обновление</span><span class="sxs-lookup"><span data-stu-id="ea527-211">opaque.Upgrade</span></span> | `OpaqueUpgrade` | <span data-ttu-id="ea527-212">В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea527-212">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="ea527-213">непрозрачный. Поток</span><span class="sxs-lookup"><span data-stu-id="ea527-213">opaque.Stream</span></span> | `Stream` |  |
| <span data-ttu-id="ea527-214">непрозрачный. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="ea527-214">opaque.CallCancelled</span></span> | `CancellationToken` |  |


### <a name="websocket-v030"></a><span data-ttu-id="ea527-215">WebSocket v0.3.0</span><span class="sxs-lookup"><span data-stu-id="ea527-215">WebSocket v0.3.0</span></span>

| <span data-ttu-id="ea527-216">Ключ</span><span class="sxs-lookup"><span data-stu-id="ea527-216">Key</span></span>               | <span data-ttu-id="ea527-217">Значение (тип)</span><span class="sxs-lookup"><span data-stu-id="ea527-217">Value (type)</span></span> | <span data-ttu-id="ea527-218">Описание</span><span class="sxs-lookup"><span data-stu-id="ea527-218">Description</span></span> |
| ----------------- | ------------ | ----------- |
| <span data-ttu-id="ea527-219">WebSocket. Версия</span><span class="sxs-lookup"><span data-stu-id="ea527-219">websocket.Version</span></span> | `String` |  |
| <span data-ttu-id="ea527-220">WebSocket. Принять</span><span class="sxs-lookup"><span data-stu-id="ea527-220">websocket.Accept</span></span> | `WebSocketAccept` | <span data-ttu-id="ea527-221">В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea527-221">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span> |
| <span data-ttu-id="ea527-222">WebSocket. AcceptAlt</span><span class="sxs-lookup"><span data-stu-id="ea527-222">websocket.AcceptAlt</span></span> |  | <span data-ttu-id="ea527-223">Non-spec</span><span class="sxs-lookup"><span data-stu-id="ea527-223">Non-spec</span></span> |
| <span data-ttu-id="ea527-224">WebSocket. Подпротокол</span><span class="sxs-lookup"><span data-stu-id="ea527-224">websocket.SubProtocol</span></span> | `String` | <span data-ttu-id="ea527-225">В разделе [RFC6455 раздел 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) шаг 5.5</span><span class="sxs-lookup"><span data-stu-id="ea527-225">See [RFC6455 Section 4.2.2](https://tools.ietf.org/html/rfc6455#section-4.2.2) Step 5.5</span></span> |
| <span data-ttu-id="ea527-226">WebSocket. SendAsync</span><span class="sxs-lookup"><span data-stu-id="ea527-226">websocket.SendAsync</span></span> | `WebSocketSendAsync` | <span data-ttu-id="ea527-227">В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea527-227">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="ea527-228">WebSocket. ReceiveAsync</span><span class="sxs-lookup"><span data-stu-id="ea527-228">websocket.ReceiveAsync</span></span> | `WebSocketReceiveAsync` | <span data-ttu-id="ea527-229">В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea527-229">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="ea527-230">WebSocket. CloseAsync</span><span class="sxs-lookup"><span data-stu-id="ea527-230">websocket.CloseAsync</span></span> | `WebSocketCloseAsync` | <span data-ttu-id="ea527-231">В разделе [сигнатура делегата](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span><span class="sxs-lookup"><span data-stu-id="ea527-231">See [delegate signature](http://owin.org/spec/extensions/owin-SendFile-Extension-v0.3.0.htm)</span></span>  |
| <span data-ttu-id="ea527-232">WebSocket. CallCancelled</span><span class="sxs-lookup"><span data-stu-id="ea527-232">websocket.CallCancelled</span></span> | `CancellationToken` |  |
| <span data-ttu-id="ea527-233">WebSocket. ClientCloseStatus</span><span class="sxs-lookup"><span data-stu-id="ea527-233">websocket.ClientCloseStatus</span></span> | `int` | <span data-ttu-id="ea527-234">Optional</span><span class="sxs-lookup"><span data-stu-id="ea527-234">Optional</span></span> |
| <span data-ttu-id="ea527-235">WebSocket. ClientCloseDescription</span><span class="sxs-lookup"><span data-stu-id="ea527-235">websocket.ClientCloseDescription</span></span> | `String` | <span data-ttu-id="ea527-236">Optional</span><span class="sxs-lookup"><span data-stu-id="ea527-236">Optional</span></span> |


## <a name="additional-resources"></a><span data-ttu-id="ea527-237">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="ea527-237">Additional Resources</span></span>

* [<span data-ttu-id="ea527-238">ПО промежуточного слоя</span><span class="sxs-lookup"><span data-stu-id="ea527-238">Middleware</span></span>](middleware.md)

* [<span data-ttu-id="ea527-239">Серверы</span><span class="sxs-lookup"><span data-stu-id="ea527-239">Servers</span></span>](servers/index.md)
