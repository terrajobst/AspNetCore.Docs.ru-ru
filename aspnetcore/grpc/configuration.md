---
title: gRPC для конфигурации ASP.NET Core
author: jamesnk
description: Сведения о настройке gRPC для приложений ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041893"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC для конфигурации ASP.NET Core

## <a name="configure-services-options"></a>Настройка параметров службы

В следующей таблице описаны параметры для настройки службы gRPC:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | Максимальный размер сообщения в байтах, которые могут быть отправлены с сервера. Попытка отправить сообщение, которое превышает размер настроенного сообщения приводят к возникновению исключения. |
| `ReceiveMaxMessageSize` | 4 МБ | Максимальный размер сообщения в байтах, которые могут быть получены сервером. Если сервер получает сообщение, которое превысит это ограничение, возникает исключение. Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти. |
| `EnableDetailedErrors` | `false` | Если `true`подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе службы. Значение по умолчанию — `false`. Этому параметру `true` может привести к утечке конфиденциальной информации. |
| `CompressionProviders` | Gzip | Коллекция поставщиков сжатия, используемый для сжатия и распаковки сообщения. Сжатие пользовательских поставщиков можно создать и добавить в коллекцию. Значение по умолчанию настроен поставщик поддерживает **gzip** сжатия. |
| `ResponseCompressionAlgorithm` | `null` | Алгоритм сжатия, используемый для сжатия сообщений, отправленных сервером. Алгоритм должен соответствовать поставщика сжатия в `CompressionProviders`. Algorthm для сжатия ответа клиент должен указать, он поддерживает алгоритм, отправляя ему **grpc приемлемой кодировкой** заголовка. |
| `ResponseCompressionLevel` | `null` | Уровень сжатия, используемый для сжатия сообщений, отправленных сервером. |

Можно настроить параметры для всех служб, предоставляя делегат параметры для `AddGrpc` вызов в `Startup.ConfigureServices`.

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

Параметры для одной службы переопределяют глобальные параметры, указанные на `AddGrpc` и могут настраиваться с помощью `AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
