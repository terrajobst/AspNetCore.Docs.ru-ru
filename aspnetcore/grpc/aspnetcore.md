---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Ознакомьтесь с основными понятиями при написании gRPC Services с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 08/07/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 26f0d7610151460967b97665ed61deab1ef56d68
ms.sourcegitcommit: 2719c70cd15a430479ab4007ff3e197fbf5dfee0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/09/2019
ms.locfileid: "68862931"
---
# <a name="grpc-services-with-aspnet-core"></a>Службы gRPC в ASP.NET Core

В этом документе показано, как приступить к работе с gRPC Services с помощью ASP.NET Core.

## <a name="prerequisites"></a>Предварительные требования

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code.](#tab/visual-studio-code)

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

gRPC включается с помощью `AddGrpc` метода:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Каждая служба gRPC добавляется в конвейер маршрутизации с помощью `MapGrpcService` метода:

[!code-csharp[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

ASP.NET Core по промежуточного слоя и компоненты совместно используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания дополнительных обработчиков запросов. Дополнительные обработчики запросов, такие как контроллеры MVC, работают параллельно с настроенными службами gRPC.

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

## <a name="grpc-and-aspnet-core-on-macos"></a>gRPC и ASP.NET Core на macOS

Kestrel не поддерживает HTTP/2 с протоколом [TLS](https://tools.ietf.org/html/rfc5246) в macOS. Шаблон ASP.NET Core gRPC и примеры по умолчанию используют протокол TLS. При попытке запустить сервер gRPC появится следующее сообщение об ошибке:

> Не удалось выполнить привязку к https://localhost:5001 интерфейсу замыкания на себя IPv4: "HTTP/2 через TLS не поддерживается в macOS из-за отсутствия поддержки АЛПН".

Чтобы обойти эту ошибку, настройте Kestrel и клиент gRPC для использования HTTP/2 **без** TLS. Это следует делать только во время разработки. Отсутствие использования TLS приведет к тому, что сообщения gRPC будут отправляться без шифрования.

Kestrel должен настроить конечную точку HTTP/2 без TLS `Program.cs`в:

```csharp
public static IHostBuilder CreateHostBuilder(string[] args) =>
    Host.CreateDefaultBuilder(args)
        .ConfigureWebHostDefaults(webBuilder =>
        {
            webBuilder.ConfigureKestrel(options =>
            {
                // Setup a HTTP/2 endpoint without TLS.
                options.ListenLocalhost(5000, o => o.Protocols = HttpProtocols.Http2);
            });
            webBuilder.UseStartup<Startup>();
        });
```

Клиент gRPC должен задать `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` для `true` параметра значение и использовать `http` в адресе сервера:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

> [!WARNING]
> HTTP/2 без TLS следует использовать только во время разработки приложений. В рабочих приложениях всегда следует использовать безопасность транспорта. Дополнительные сведения см. [в разделе вопросы безопасности в gRPC for ASP.NET Core](xref:grpc/security#transport-security).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
