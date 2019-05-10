---
title: Миграция служб gRPC из C-core на ASP.NET Core
author: juntaoluo
description: Узнайте, как переместить существующее приложение на основе gRPC C core для запуска на вершине стека ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 47d74edd821124f0c8390d704ca7931b7eb6c4cd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895241"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Миграция служб gRPC из C-core на ASP.NET Core

Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)

Из-за реализации нижележащего стека, не все функции работают одинаково между [C-ядро на основе gRPC](https://grpc.io/blog/grpc-stacks) приложения и приложения на основе ASP.NET Core. В этом документе описываются ключевые различия по миграции между двумя стеками.

## <a name="grpc-service-implementation-lifetime"></a>время существования реализации службы gRPC

В стеке ASP.NET Core gRPC службы, по умолчанию создаются с помощью [области времени существования](xref:fundamentals/dependency-injection#service-lifetimes). Напротив, gRPC C-core по умолчанию привязывается к службе с помощью [времени существования singleton](xref:fundamentals/dependency-injection#service-lifetimes).

Время существования областью действия позволяет реализации службы, чтобы разрешить другим службам с помощью области времени существования. Например, время существования областью действия для устранения `DBContext` из контейнера внедрения Зависимостей через конструктор injection. С помощью области времени существования:

* Для каждого запроса создается новый экземпляр реализации службы.
* Невозможно получить совместного использования состояния между запросами через члены экземпляра в реализации типа.
* Ожидается, для хранения общих состояний в это отдельная служба в контейнере внедрения Зависимостей. В конструкторе класса реализации службы gRPC разрешаются хранимые общих состояний.

Дополнительные сведения о времени существования службы, см. в разделе <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Добавьте это отдельная служба

Для облегчения перехода от реализации C-core gRPC на ASP.NET Core, его можно изменить время существования службы из реализации службы за один элемент с заданной областью. Для этого необходимо добавить экземпляр реализации службы в контейнер внедрения Зависимостей.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Тем не менее реализация службы со временем существования singleton больше не может разрешить ограниченных служб через внедрение через конструктор.

## <a name="configure-grpc-services-options"></a>Настройка параметров служб gRPC

В приложениях на основе C-core, параметры, такие как `grpc.max_receive_message_length` и `grpc.max_send_message_length` должны быть настроены `ChannelOption` при [конструирование экземпляра сервера](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

В ASP.NET Core, gRPC предоставляет конфигурацию с помощью `GrpcServiceOptions` типа. Например, gRPC службы максимальный размер входящего сообщения, которые могут быть настроены с помощью `AddGrpc`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.ReceiveMaxMessageSize = 16384; // 16 MB
    });
}
```

Дополнительные сведения о конфигурации, см. в разделе <xref:grpc/configuration>.

## <a name="logging"></a>Ведение журнала

C-ядро на основе приложения зависят от `GrpcEnvironment` для [настроить средство ведения журнала](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) для целей отладки. Стек ASP.NET Core предоставляет следующие функциональные возможности через [API ведения журналов](xref:fundamentals/logging/index). Например средство ведения журнала можно добавить к службе gRPC через внедрение через конструктор:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Приложения на основе C-core Настройка протокола HTTPS через [Server.Ports свойство](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Настройка серверов в ASP.NET Core используется похожая концепция. Например, использует Kestrel [конфигурации конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration) для использования этой функции.

## <a name="interceptors-and-middleware"></a>Перехватчики и по промежуточного слоя

ASP.NET Core [по промежуточного слоя](xref:fundamentals/middleware/index) предлагает перехватчики в приложениях C-ядро на основе gRPC по сравнению с аналогичными функциональными возможностями. По промежуточного слоя и перехватчики аналогичны концептуально оба используются для создания конвейера, где обрабатывается запрос gRPC. Они оба допускают работы до или после следующему компоненту в конвейере. Тем не менее, по промежуточного слоя ASP.NET Core работает в соответствующих сообщениях HTTP/2, пока перехватчики оперируют gRPC уровень абстракции с помощью [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
