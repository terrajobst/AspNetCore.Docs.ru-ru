---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC Services с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/03/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 28e6b8589bbe0b6a3723b64736c723c883302571
ms.sourcegitcommit: e6bd2bbe5683e9a7dbbc2f2eab644986e6dc8a87
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/03/2019
ms.locfileid: "70238160"
---
# <a name="grpc-services-with-aspnet-core"></a>Службы gRPC в ASP.NET Core

В этом документе показано, как приступить к работе с gRPC Services с помощью ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio для Mac](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>Начало работы со службой gRPC в ASP.NET Core

[Просмотреть или скачать пример кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/grpc/grpc-start/sample) ([описание скачивания](xref:index#how-to-download-a-sample)).

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

Подробные инструкции по созданию проекта gRPC см. в статье Начало [работы с gRPC Services](xref:tutorials/grpc/grpc-start) .

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Добавление gRPC Services в приложение ASP.NET Core

для gRPC требуется пакет [gRPC. AspNetCore](https://www.nuget.org/packages/Grpc.AspNetCore) .

### <a name="configure-grpc"></a>Настройка gRPC

В файле *Startup.cs*:

* gRPC включается с помощью `AddGrpc` метода.
* Каждая служба gRPC добавляется в конвейер маршрутизации с помощью `MapGrpcService` метода.

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7,24)]

ASP.NET Core по промежуточного слоя и компоненты совместно используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания дополнительных обработчиков запросов. Дополнительные обработчики запросов, такие как контроллеры MVC, работают параллельно с настроенными службами gRPC.

### <a name="configure-kestrel"></a>Настройка Kestrel

Конечные точки gRPC Kestrel:

* Требовать HTTP/2.
* Должен быть защищен с помощью протокола HTTPS.

#### <a name="http2"></a>HTTP/2

для gRPC требуется HTTP/2. gRPC для ASP.NET Core проверяет [HttpRequest. Protocol](xref:Microsoft.AspNetCore.Http.HttpRequest.Protocol*) — `HTTP/2`.

Kestrel [поддерживает HTTP/2](xref:fundamentals/servers/kestrel#http2-support) в большинстве современных операционных систем. Конечные точки Kestrel настроены для поддержки подключений HTTP/1.1 и HTTP/2 по умолчанию.

#### <a name="https"></a>HTTPS

Конечные точки Kestrel, используемые для gRPC, должны быть защищены с помощью протокола HTTPS. В процессе разработки конечная точка HTTPS создается `https://localhost:5001` автоматически при наличии сертификата разработки ASP.NET Core. Настройка не требуется.

В рабочей среде необходимо явно настроить HTTPS. В следующем примере *appSettings. JSON* предоставляется точка HTTP/2, защищенная с помощью HTTPS:

```json
{
  "Kestrel": {
    "Endpoints": {
      "HttpsDefaultCert": {
        "Url": "https://localhost:5001",
        "Protocols": "Http2"
      }
    },
    "Certificates": {
      "Default": {
        "Path": "<path to .pfx file>",
        "Password": "<certificate password>"
      }
    }
  }
}
```

Кроме того, конечные точки Kestrel можно настроить в *Program.CS*:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // This endpoint will use HTTP/2 and HTTPS on port 5001.
                options.Listen(IPAddress.Any, 5001, listenOptions =>
                {
                    listenOptions.Protocols = HttpProtocols.Http2;
                    listenOptions.UseHttps("<path to .pfx file>", 
                        "<certificate password>");
                });
            });
            webBuilder.UseStartup<Startup>();
        });
```

Если конечная точка HTTP/2 настроена без HTTPS, для `HttpProtocols.Http2` [листеноптионс. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) должно быть установлено значение. `HttpProtocols.Http1AndHttp2`не может использоваться, так как для согласования HTTP/2 требуется протокол HTTPS. Без протокола HTTPS все соединения с конечной точкой по умолчанию HTTP/1.1, а вызовы gRPC завершаются ошибкой.

Дополнительные сведения о включении HTTP/2 и HTTPS с помощью Kestrel см. в разделе [Конфигурация конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

> [!NOTE]
> macOS не поддерживает ASP.NET Core gRPC с [протоколом TLS](https://tools.ietf.org/html/rfc5246). Для успешного запуска служб gRPC в macOS требуется дополнительная настройка. Дополнительные сведения см. в статье [Не удается запустить приложение ASP.NET Core gRPC в macOS](xref:grpc/troubleshoot#unable-to-start-aspnet-core-grpc-app-on-macos).

## <a name="integration-with-aspnet-core-apis"></a>Интеграция с ASP.NET Core API

gRPC Services имеют полный доступ к ASP.NET Coreным функциям, таким как [внедрение зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index). Например, реализация службы может разрешить службу ведения журнала из контейнера DI с помощью конструктора:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

По умолчанию реализация службы gRPC может разрешать другие службы DI с любым временем существования (singleton, Scope или временно).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Разрешение HttpContext в методах gRPC

API gRPC предоставляет доступ к некоторым данным сообщений HTTP/2, таким как метод, узел, заголовок и трейлер. Доступ осуществляется посредством `ServerCallContext` аргумента, передаваемого в каждый метод gRPC:

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext`не предоставляет полный доступ ко `HttpContext` всем API-интерфейсам ASP.NET. Метод расширения предоставляет полный доступ к элементу `HttpContext` , представляющему базовое сообщение HTTP/2 в API-интерфейсах ASP.NET: `GetHttpContext`

[!code-csharp[](~/grpc/aspnetcore/sample/GrcpService/GreeterService2.cs?highlight=6-7&name=snippet)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:fundamentals/servers/kestrel>
