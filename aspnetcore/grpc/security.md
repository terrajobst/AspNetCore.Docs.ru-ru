---
title: Вопросы безопасности в gRPC для ASP.NET Core
author: jamesnk
description: Сведения о вопросах безопасности для gRPC для ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 07/07/2019
uid: grpc/security
ms.openlocfilehash: f84bec0ef485b701b2be36384a2e49b9b28e473d
ms.sourcegitcommit: 8b36f75b8931ae3f656e2a8e63572080adc78513
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/05/2019
ms.locfileid: "70310379"
---
# <a name="security-considerations-in-grpc-for-aspnet-core"></a>Вопросы безопасности в gRPC для ASP.NET Core

[Джеймс Ньютона-короля](https://twitter.com/jamesnk)

Эта статья содержит сведения о защите gRPC с помощью .NET Core.

## <a name="transport-security"></a>Безопасность транспорта

сообщения gRPC отправляются и принимаются с помощью HTTP/2. Мы рекомендуем:

* [Протокол TLS](https://tools.ietf.org/html/rfc5246) используется для защиты сообщений в производственных gRPC приложениях.
* службы gRPC Services должны только прослушивать защищенные порты и отвечать на них.

Протокол TLS настраивается в Kestrel. Дополнительные сведения о настройке конечных точек Kestrel см. в разделе [Конфигурация конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).

## <a name="exceptions"></a>Исключения

Сообщения об исключениях обычно считаются конфиденциальными данными, которые не должны быть раскрыты для клиента. По умолчанию gRPC не отправляет клиенту сведения об исключении, созданном службой gRPC. Вместо этого клиент получает универсальное сообщение, указывающее на возникновение ошибки. Доставка сообщений об исключениях клиенту может быть переопределена (например, в процессе разработки или тестирования) с помощью [енабледетаиледеррорс](xref:grpc/configuration#configure-services-options). Сообщения об исключениях не должны предоставляться клиенту в рабочих приложениях.

## <a name="message-size-limits"></a>Ограничения размера сообщений

Входящие сообщения для клиентов и служб gRPC загружаются в память. Ограничения размера сообщений — это механизм, позволяющий предотвратить чрезмерное использование ресурсов gRPC.

gRPC использует ограничения на размер каждого сообщения для управления входящими и исходящими сообщениями. По умолчанию gRPC ограничивает входящие сообщения 4 МБ. Ограничения на исходящие сообщения отсутствуют.

На сервере ограничения для сообщений gRPC можно настроить для всех служб в приложении с помощью `AddGrpc`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.MaxReceiveMessageSize = 1 * 1024 * 1024; // 1 MB
        options.MaxSendMessageSize = 1 * 1024 * 1024; // 1 MB
    });
}
```

Ограничения также можно настроить для отдельной службы с помощью `AddServiceOptions<TService>`. Дополнительные сведения о настройке ограничений на размер сообщений см. в разделе [gRPC Configuration](xref:grpc/configuration).

## <a name="client-certificate-validation"></a>Проверка сертификата клиента

[Сертификаты клиента](https://tools.ietf.org/html/rfc5246#section-7.4.4) изначально проверяются при установке соединения. По умолчанию Kestrel не выполняет дополнительную проверку сертификата клиента подключения.

Рекомендуется, чтобы gRPC службы, защищенные сертификатами клиента, использовали пакет [Microsoft. AspNetCore. Authentication. Certificate](xref:security/authentication/certauth) . ASP.NET Core проверка подлинности с помощью сертификации выполняет дополнительную проверку сертификата клиента, в том числе:

* Сертификат имеет допустимое расширенное использование ключа (EKU)
* Находится в пределах срока действия
* Проверка отзыва сертификата
