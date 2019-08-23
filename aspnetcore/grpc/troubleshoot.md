---
title: Устранение неполадок gRPC в .NET Core
author: jamesnk
description: Устранение ошибок при использовании gRPC в .NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 08/17/2019
uid: grpc/troubleshoot
ms.openlocfilehash: 7621266dfe26b7126d1607e195dd5dcaab4efa55
ms.sourcegitcommit: 41f2c1a6b316e6e368a4fd27a8b18d157cef91e1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 08/21/2019
ms.locfileid: "69886493"
---
# <a name="troubleshoot-grpc-on-net-core"></a>Устранение неполадок gRPC в .NET Core

[Джеймс Ньютона-короля](https://twitter.com/jamesnk)

## <a name="mismatch-between-client-and-service-ssltls-configuration"></a>Несоответствие конфигурации SSL/TLS клиента и службы

Шаблон gRPC и примеры используют [протокол TLS](https://tools.ietf.org/html/rfc5246) для защиты служб gRPC по умолчанию. Клиенты gRPC должны использовать безопасное соединение для успешного вызова защищенных служб gRPC.

Вы можете убедиться, что ASP.NET Core служба gRPC использует TLS в журналах, написанных при запуске приложения. Служба будет прослушивать конечную точку HTTPS:

```
info: Microsoft.Hosting.Lifetime[0]
      Now listening on: https://localhost:5001
info: Microsoft.Hosting.Lifetime[0]
      Application started. Press Ctrl+C to shut down.
info: Microsoft.Hosting.Lifetime[0]
      Hosting environment: Development
```

Клиент .NET Core должен использовать `https` в адресе сервера для выполнения вызовов с защищенным соединением:

```csharp
static async Task Main(string[] args)
{
    var httpClient = new HttpClient();
    // The port number(5001) must match the port of the gRPC server.
    httpClient.BaseAddress = new Uri("https://localhost:5001");
    var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
}
```

Все реализации клиента gRPC поддерживают протокол TLS. для клиентов gRPC из других языков обычно требуется канал, настроенный с помощью `SslCredentials`. `SslCredentials`Указывает сертификат, который будет использоваться клиентом, и его необходимо использовать вместо незащищенных учетных данных. Примеры настройки различных реализаций клиента gRPC для использования TLS см. в разделе [Проверка подлинности gRPC](https://www.grpc.io/docs/guides/auth/).

## <a name="call-insecure-grpc-services-with-net-core-client"></a>Вызов незащищенных служб gRPC Services с помощью клиента .NET Core

Для вызова незащищенных служб gRPC с клиентом .NET Core требуется дополнительная настройка. Клиент gRPC должен задать `System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport` для `true` параметра значение и использовать `http` в адресе сервера:

```csharp
// This switch must be set before creating the HttpClient.
AppContext.SetSwitch("System.Net.Http.SocketsHttpHandler.Http2UnencryptedSupport", true);

var httpClient = new HttpClient();
// The port number(5000) must match the port of the gRPC server.
httpClient.BaseAddress = new Uri("http://localhost:5000");
var client = GrpcClient.Create<Greeter.GreeterClient>(httpClient);
```

## <a name="unable-to-start-aspnet-core-grpc-app-on-macos"></a>Не удается запустить ASP.NET Core приложение gRPC на macOS

Kestrel не поддерживает HTTP/2 с TLS в macOS и более ранних версиях Windows, таких как Windows 7. Шаблон ASP.NET Core gRPC и примеры по умолчанию используют протокол TLS. При попытке запустить сервер gRPC появится следующее сообщение об ошибке:

> Не удалось выполнить привязку к https://localhost:5001 интерфейсу замыкания на себя IPv4: "HTTP/2 через TLS не поддерживается в macOS из-за отсутствия поддержки АЛПН".

Чтобы обойти эту ошибку, настройте Kestrel и клиент gRPC для использования HTTP/2 *без* TLS. Это следует делать только во время разработки. Отсутствие использования TLS приведет к тому, что сообщения gRPC будут отправляться без шифрования.

Kestrel должен настроить конечную точку HTTP/2 без TLS в *Program.CS*:

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

Если конечная точка HTTP/2 настроена без TLS, [листеноптионс. Protocols](xref:fundamentals/servers/kestrel#listenoptionsprotocols) должна иметь значение `HttpProtocols.Http2`. `HttpProtocols.Http1AndHttp2`не может использоваться, так как протокол TLS требуется для согласования HTTP/2. Без TLS все соединения с конечной точкой по умолчанию заключаются в HTTP/1.1, а вызовы gRPC завершаются ошибкой.

Кроме того, клиент gRPC должен быть настроен так, чтобы не использовал TLS. Дополнительные сведения см. в статье [вызов незащищенных служб gRPC Services с помощью клиента .NET Core](#call-insecure-grpc-services-with-net-core-client).

> [!WARNING]
> HTTP/2 без TLS следует использовать только во время разработки приложений. В рабочих приложениях всегда следует использовать безопасность транспорта. Дополнительные сведения см. [в разделе вопросы безопасности в gRPC for ASP.NET Core](xref:grpc/security#transport-security).

## <a name="grpc-c-assets-are-not-code-generated-from-proto-files"></a>ресурсы C# gRPC не являются кодом, созданным из файлов с  *\*.*

Создание кода gRPC для конкретных клиентов и базовых классов служб требует наличия в проекте ссылок на файлы и инструментарий protobuf. Необходимо включить:

* *.* укажите файлы, которые необходимо использовать в `<Protobuf>` группе элементов. Проект должен ссылаться на [импортированные файлы ](https://developers.google.com/protocol-buffers/docs/proto3#importing-definitions) с указанными файлами.
* Ссылка на пакет для пакета инструментов gRPC [gRPC. Tools](https://www.nuget.org/packages/Grpc.Tools/).

Дополнительные сведения о создании ресурсов gRPC C# см. в <xref:grpc/basics>разделе.

По умолчанию `<Protobuf>` ссылка создает конкретный клиент и базовый класс службы. `GrpcServices` Атрибут элемента Reference можно использовать для ограничения C# создания ресурсов. Допустимые `GrpcServices` параметры:

* `Both`(по умолчанию, если отсутствует)
* `Server`
* `Client`
* `None`

Для ASP.NET Core веб-приложения, в котором размещены службы gRPC Services, требуется только базовый класс службы:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Server" />
</ItemGroup>
```

Клиентское приложение gRPC, выполняющее вызовы gRPC, требует только создания конкретного клиента:

```xml
<ItemGroup>
  <Protobuf Include="Protos\greet.proto" GrpcServices="Client" />
</ItemGroup>
```