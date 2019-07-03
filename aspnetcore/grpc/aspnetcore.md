---
title: Службы gRPC в ASP.NET Core
author: juntaoluo
description: Изучите основные принципы, при создании gRPC служб с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 5937ca9f2a783c4dabe324dae828b97953782938
ms.sourcegitcommit: d6e51c60439f03a8992bda70cc982ddb15d3f100
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 07/03/2019
ms.locfileid: "67555866"
---
# <a name="grpc-services-with-aspnet-core"></a>Службы gRPC в ASP.NET Core

В этом документе показано, как приступить к работе со службами gRPC, с помощью ASP.NET Core.

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

См. в разделе [приступить к работе со службами gRPC](xref:tutorials/grpc/grpc-start) подробные инструкции о том, как создать проект gRPC.

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code/Visual Studio для Mac](#tab/visual-studio-code+visual-studio-mac)

Из командной строки выполните команду `dotnet new grpc -o GrpcGreeter`.

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>Добавление служб gRPC в приложения ASP.NET Core

gRPC требует наличия следующих пакетов:

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) для protobuf сообщений API-интерфейсы.
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>Настройка gRPC

gRPC включен с `AddGrpc` метод:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=7)]

Каждая служба gRPC добавляется в конвейер маршрутизации через `MapGrpcService` метод:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Startup.cs?name=snippet&highlight=24)]

По промежуточного слоя ASP.NET Core и компоненты используют конвейер маршрутизации, поэтому приложение можно настроить для обслуживания запроса дополнительные обработчики. Обработчики дополнительный запрос, например, контроллеры MVC работать параллельно со службами настроенных gRPC.

## <a name="integration-with-aspnet-core-apis"></a>Интеграция с ASP.NET Core API-интерфейсов

gRPC службы имеют полный доступ к возможностям ASP.NET Core, такие как [внедрения зависимостей](xref:fundamentals/dependency-injection) (DI) и [ведение журнала](xref:fundamentals/logging/index). Например реализация службы можно разрешить службу средства ведения журнала из контейнера внедрения Зависимостей через конструктор:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

По умолчанию реализация службы gRPC можно разрешить другим службам внедрения Зависимостей с помощью любого времени существования (одноэлементный Scoped и временные).

### <a name="resolve-httpcontext-in-grpc-methods"></a>Разрешить HttpContext в методах gRPC

GRPC API предоставляет доступ к некоторым данным сообщения HTTP/2, например метод, узла, заголовок и прицепов. Доступ осуществляется через `ServerCallContext` аргумент, переданный к каждому методу gRPC:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` не предоставляет полный доступ к `HttpContext` в интерфейсах для ASP.NET. `GetHttpContext` Метод расширения предоставляет полный доступ к `HttpContext` представляет базовое сообщение HTTP/2 в интерфейсах API ASP.NET:

[!code-cs[](~/tutorials/grpc/grpc-start/sample/GrpcGreeter/Services/GreeterService.cs?name=snippet)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
