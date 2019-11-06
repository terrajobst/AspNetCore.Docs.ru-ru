---
title: Миграция gRPC Services с C-Core на ASP.NET Core
author: juntaoluo
description: Узнайте, как переместить существующее приложение gRPC на основе C-Core для выполнения на вершине стека ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 09/25/2019
uid: grpc/migration
ms.openlocfilehash: c4c07808540c9af370bfa253e8154a8a19f0f3de
ms.sourcegitcommit: 897d4abff58505dae86b2947c5fe3d1b80d927f3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/06/2019
ms.locfileid: "73634070"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>Миграция gRPC Services с C-Core на ASP.NET Core

Автор: [John Luo](https://github.com/juntaoluo) (Джон Луо)

Из-за реализации базового стека не все функции работают таким же образом, как и приложения [gRPC на основе C-Core](https://grpc.io/blog/grpc-stacks) и приложения на основе ASP.NET Core. В этом документе описываются основные различия между двумя стеками.

## <a name="grpc-service-implementation-lifetime"></a>время существования реализации службы gRPC

В стеке ASP.NET Core службы gRPC по умолчанию создаются с [временем существования с областью действия](xref:fundamentals/dependency-injection#service-lifetimes). В отличие от этого, gRPC C-Core по умолчанию привязывается к службе с [одноэлементным жизненным циклом](xref:fundamentals/dependency-injection#service-lifetimes).

Время существования с заданной областью позволяет реализации службы разрешать другие службы с ограниченным временем существования. Например, время существования в области может также разрешать `DbContext` из контейнера DI посредством внедрения конструктора. Использование ограниченного времени существования:

* Новый экземпляр реализации службы создается для каждого запроса.
* Невозможно совместно использовать состояние между запросами через члены экземпляра в типе реализации.
* Ожидание заключается в хранении общих состояний в одноэлементной службе в контейнере внедрения. Хранимые общие состояния разрешаются в конструкторе реализации службы gRPC.

Дополнительные сведения о времени существования службы см. в разделе <xref:fundamentals/dependency-injection#service-lifetimes>.

### <a name="add-a-singleton-service"></a>Добавление одноэлементной службы

Чтобы упростить переход от реализации gRPC C-Core к ASP.NET Core, можно изменить время существования службы реализации службы с ограниченного на Singleton. Это включает добавление экземпляра реализации службы в контейнер DI:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

Однако реализация службы с одноэлементным жизненным циклом больше не может разрешать службы с заданной областью посредством внедрения конструктора.

## <a name="configure-grpc-services-options"></a>Настройка параметров служб gRPC Services

В приложениях на основе C-Core такие параметры, как `grpc.max_receive_message_length` и `grpc.max_send_message_length`, настраиваются с помощью `ChannelOption` при [создании экземпляра сервера](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__).

В ASP.NET Core gRPC предоставляет конфигурацию с помощью `GrpcServiceOptions` типа. Например, максимальный размер входящего сообщения службы gRPC можно настроить с помощью `AddGrpc`. В следующем примере `MaxReceiveMessageSize` по умолчанию для 4 МБ изменяется на 16 МБ:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 16 * 1024 * 1024; // 16 MB
    });
}
```

Дополнительные сведения о конфигурации см. в разделе <xref:grpc/configuration>.

## <a name="logging"></a>Ведение журнала

Приложения на основе C-Core используют `GrpcEnvironment` для [настройки средства ведения журнала](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_) в целях отладки. Стек ASP.NET Core предоставляет эту функцию через [API ведения журнала](xref:fundamentals/logging/index). Например, средство ведения журнала можно добавить в службу gRPC посредством внедрения конструктора:

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

Приложения на основе C-Core настраивают HTTPS через [свойство Server. Ports](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports). Аналогичная концепция используется для настройки серверов в ASP.NET Core. Например, Kestrel использует [конфигурацию конечной точки](xref:fundamentals/servers/kestrel#endpoint-configuration) для этой функции.

## <a name="grpc-interceptors-vs-middleware"></a>gRPC перехватчики VS по промежуточного слоя

ASP.NET Core по [промежуточного слоя](xref:fundamentals/middleware/index) предлагает аналогичные функциональные возможности по сравнению с перехватчиками в приложениях gRPC на основе C-Core. ASP.NET Core по промежуточного слоя и перехватчики концептуально похожи. Как

* Используются для создания конвейера, обрабатывающего запрос gRPC.
* Разрешить выполнение работы до или после следующего компонента в конвейере.
* Предоставление доступа к `HttpContext`:
  * В промежуточном слое `HttpContext` является параметром.
  * В перехватчиках доступ к `HttpContext` можно получить с помощью параметра `ServerCallContext` с помощью метода расширения `ServerCallContext.GetHttpContext`. Обратите внимание, что эта функция относится к перехватчикам, выполняемым в ASP.NET Core.

отличия gRPC от по промежуточного слоя ASP.NET Core:

* Перехватчики
  * Работа с уровнем абстракции gRPC с помощью [серверкаллконтекст](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html).
  * Предоставить доступ к:
    * Десериализованное сообщение, отправленное в вызов.
    * Сообщение, возвращаемое из вызова перед его сериализацией.
* По промежуточного слоя
  * Выполняется перед перехватчиками gRPC.
  * Работает с базовыми сообщениями HTTP/2.
  * Может получать доступ только к байтам из потоков запросов и ответов.

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
