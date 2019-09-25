---
title: Ведение журналов и диагностика в gRPC на .NET
author: jamesnk
description: Узнайте, как собирать диагностические сведения из приложения gRPC на платформе .NET.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.date: 09/23/2019
uid: grpc/diagnostics
ms.openlocfilehash: 7194e91b40a08c4a7ee619b8f207900af2683aa1
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/25/2019
ms.locfileid: "71250740"
---
# <a name="logging-and-diagnostics-in-grpc-on-net"></a>Ведение журналов и диагностика в gRPC на .NET

[Джеймс Ньютона-короля](https://twitter.com/jamesnk)

В этой статье приводятся рекомендации по сбору диагностических данных из приложения gRPC для устранения проблем.

## <a name="grpc-services-logging"></a>ведение журнала gRPC Services

> [!WARNING]
> Журналы на стороне сервера могут содержать конфиденциальные сведения из приложения. **Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.

Поскольку службы gRPC размещаются на ASP.NET Core, используется система ведения журнала ASP.NET Core. В конфигурации по умолчанию gRPC регистрирует очень мало информации, но это может быть настроено. Дополнительные сведения о настройке ведения журнала ASP.NET Core см. в документации по [ASP.NET Core ведению журнала](xref:fundamentals/logging/index#configuration) .

gRPC добавляет журналы в `Grpc` категорию. Чтобы включить `Grpc` подробные журналы из gRPC, настройте префиксы `Debug` для уровня в файле *appSettings. JSON* `LogLevel` , добавив следующие элементы в подраздел в `Logging`:

[!code-json[](diagnostics/sample/logging-config.json?highlight=7)]

Это также можно настроить в *Startup.CS* с помощью `ConfigureLogging`:

[!code-csharp[](diagnostics/sample/logging-config-code.cs?highlight=5)]

Если конфигурация на основе JSON не используется, установите следующее значение конфигурации в системе конфигурации:

* `Logging:LogLevel:Grpc` = `Debug`

Ознакомьтесь с документацией по системе конфигурации, чтобы определить, как указать вложенные значения конфигурации. Например, при использовании переменных среды вместо параметра `_` `:` (например, `Logging__LogLevel__Grpc`) используются два символа.

Рекомендуется использовать `Debug` уровень при сборе более подробных диагностических сведений о приложении. `Trace` Уровень обеспечивает очень низкую диагностику и редко требуется для диагностики проблем в приложении.

### <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

Ниже приведен пример вывода на консоль на `Debug` уровне службы gRPC:

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

## <a name="access-server-side-logs"></a>Доступ к журналам на стороне сервера

Доступ к журналам на стороне сервера зависит от среды, в которой выполняется.

### <a name="as-a-console-app"></a>Как консольное приложение

Если вы работаете в консольном приложении, [средство ведения журнала консоли](xref:fundamentals/logging/index#console-provider) должно быть включено по умолчанию. журналы gRPC будут отображаться в консоли.

### <a name="other-environments"></a>Другие среды

Если приложение развертывается в другой среде (например, Docker, Kubernetes или службе Windows), см <xref:fundamentals/logging/index> . Дополнительные сведения о настройке регистраторов, подходящих для среды.

## <a name="grpc-client-logging"></a>ведение журнала клиента gRPC

> [!WARNING]
> Журналы на стороне клиента могут содержать конфиденциальные сведения из приложения. **Никогда не** размещайте необработанные журналы из рабочих приложений на общедоступные форумы, такие как GitHub.

Чтобы получить журналы из клиента .NET, можно задать `GrpcChannelOptions.LoggerFactory` свойство при создании канала клиента. Если вы вызываете службу gRPC из приложения ASP.NET Core, фабрика журнала может быть разрешена путем внедрения зависимостей (DI):

[!code-csharp[](diagnostics/sample/net-client-dependency-injection.cs?highlight=7,16)]

Альтернативный способ включения ведения журнала клиента — использование [фабрики клиента gRPC](xref:grpc/clientfactory) для создания клиента. Клиент gRPC, зарегистрированный в фабрике клиента и разрешенный из DI, автоматически будет использовать настроенное для приложения ведение журнала.

Если приложение не использует di, можно создать новый `ILoggerFactory` экземпляр с помощью [LoggerFactory. Create](xref:Microsoft.Extensions.Logging.LoggerFactory.Create*). Чтобы получить доступ к этому методу, добавьте в приложение пакет [Microsoft. Extensions. Logging](https://www.nuget.org/packages/microsoft.extensions.logging/) .

[!code-csharp[](diagnostics/sample/net-client-loggerfactory-create.cs?highlight=1,8)]

### <a name="sample-logging-output"></a>Пример выходных данных ведения журнала

Ниже приведен пример вывода на консоль на `Debug` уровне клиента gRPC.

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

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:fundamentals/logging/index>
* <xref:grpc/configuration>
* <xref:grpc/clientfactory>
