---
title: gRPC для конфигурации ASP.NET Core
author: jamesnk
description: Сведения о настройке gRPC для приложений ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 5/30/2019
uid: grpc/configuration
ms.openlocfilehash: 1f8250dc9aa8b82da384ee28287011baa19dc11f
ms.sourcegitcommit: a1364109d11d414121a6337b611bee61d6e489e9
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/04/2019
ms.locfileid: "66491244"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC для конфигурации ASP.NET Core

## <a name="configure-services-options"></a>Настройка параметров службы

В следующей таблице описаны параметры для настройки службы gRPC:

| Параметр | Значение по умолчанию | Описание |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | Максимальный размер сообщения в байтах, которые могут быть отправлены с сервера. Попытка отправить сообщение, которое превышает размер настроенного сообщения приводят к возникновению исключения. |
| `ReceiveMaxMessageSize` | 4 МБ | Максимальный размер сообщения в байтах, которые могут быть получены сервером. Если сервер получает сообщение, которое превысит это ограничение, возникает исключение. Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти. |
| `EnableDetailedErrors` | `false` | Если `true`подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе службы. Значение по умолчанию — `false`. Установка `EnableDetailedErrors` для `true` может привести к утечке конфиденциальной информации. |
| `CompressionProviders` | Gzip | Коллекция поставщиков сжатия, используемый для сжатия и распаковки сообщения. Сжатие пользовательских поставщиков можно создать и добавить в коллекцию. Значение по умолчанию настроен поставщик поддерживает **gzip** сжатия. |
| `ResponseCompressionAlgorithm` | `null` | Алгоритм сжатия, используемый для сжатия сообщений, отправленных сервером. Алгоритм должен соответствовать поставщика сжатия в `CompressionProviders`. Алгоритм для сжатия ответа, клиент должен указать, он поддерживает алгоритм, отправляя ему **grpc приемлемой кодировкой** заголовка. |
| `ResponseCompressionLevel` | `null` | Уровень сжатия, используемый для сжатия сообщений, отправленных сервером. |

Можно настроить параметры для всех служб, предоставляя делегат параметры для `AddGrpc` вызов в `Startup.ConfigureServices`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup.cs?name=snippet)]

Параметры для одной службы переопределяют глобальные параметры, указанные на `AddGrpc` и могут настраиваться с помощью `AddServiceOptions<TService>`:

[!code-csharp[](~/grpc/configuration/sample/GrcpService/Startup2.cs?name=snippet)]

## <a name="configure-client-options"></a>Настройка параметров клиента

Следующий код задает максимальное число отправляемых клиента и получения размер сообщения:

[!code-csharp[](~/grpc/configuration/sample/Program.cs?name=snippet&highlight=3-6)]

## <a name="additional-resources"></a>Дополнительные ресурсы

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
