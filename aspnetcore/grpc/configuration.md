---
title: gRPC для конфигурации ASP.NET Core
author: jamesnk
description: Сведения о настройке gRPC для приложений ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983136"
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

## <a name="configure-kestrel-options"></a>Настроить параметры Kestrel

Сервер kestrel имеет параметры конфигурации, которые влияют на поведение gRPC для ASP.NET.

### <a name="request-body-data-rate-limit"></a>Ограничение частоты запросов текста данных

По умолчанию сервер Kestrel налагает [скорость передачи данных запроса минимальное тело](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>). Для клиента потоковой передачи и потоковой передаче вызовов дуплексный режим это значение не может быть удовлетворен и соединение может быть превышено время ожидания. Текст, ограничения скорость передачи данных должна быть отключена, если служба gRPC включает клиент потоковой передачи и потоковой передаче вызовов дуплексных запроса минимальное:

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
