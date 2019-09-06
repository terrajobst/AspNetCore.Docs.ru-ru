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
# <a name="security-considerations-in-grpc-for-aspnet-core"></a><span data-ttu-id="7a7e1-103">Вопросы безопасности в gRPC для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="7a7e1-103">Security considerations in gRPC for ASP.NET Core</span></span>

<span data-ttu-id="7a7e1-104">[Джеймс Ньютона-короля](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="7a7e1-104">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="7a7e1-105">Эта статья содержит сведения о защите gRPC с помощью .NET Core.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-105">This article provides information on securing gRPC with .NET Core.</span></span>

## <a name="transport-security"></a><span data-ttu-id="7a7e1-106">Безопасность транспорта</span><span class="sxs-lookup"><span data-stu-id="7a7e1-106">Transport security</span></span>

<span data-ttu-id="7a7e1-107">сообщения gRPC отправляются и принимаются с помощью HTTP/2.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-107">gRPC messages are sent and received using HTTP/2.</span></span> <span data-ttu-id="7a7e1-108">Мы рекомендуем:</span><span class="sxs-lookup"><span data-stu-id="7a7e1-108">We recommend:</span></span>

* <span data-ttu-id="7a7e1-109">[Протокол TLS](https://tools.ietf.org/html/rfc5246) используется для защиты сообщений в производственных gRPC приложениях.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-109">[Transport Layer Security (TLS)](https://tools.ietf.org/html/rfc5246) be used to secure messages in production gRPC apps.</span></span>
* <span data-ttu-id="7a7e1-110">службы gRPC Services должны только прослушивать защищенные порты и отвечать на них.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-110">gRPC services should only listen and respond over secured ports.</span></span>

<span data-ttu-id="7a7e1-111">Протокол TLS настраивается в Kestrel.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-111">TLS is configured in Kestrel.</span></span> <span data-ttu-id="7a7e1-112">Дополнительные сведения о настройке конечных точек Kestrel см. в разделе [Конфигурация конечной точки Kestrel](xref:fundamentals/servers/kestrel#endpoint-configuration).</span><span class="sxs-lookup"><span data-stu-id="7a7e1-112">For more information on configuring Kestrel endpoints, see [Kestrel endpoint configuration](xref:fundamentals/servers/kestrel#endpoint-configuration).</span></span>

## <a name="exceptions"></a><span data-ttu-id="7a7e1-113">Исключения</span><span class="sxs-lookup"><span data-stu-id="7a7e1-113">Exceptions</span></span>

<span data-ttu-id="7a7e1-114">Сообщения об исключениях обычно считаются конфиденциальными данными, которые не должны быть раскрыты для клиента.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-114">Exception messages are generally considered sensitive data that shouldn't be revealed to a client.</span></span> <span data-ttu-id="7a7e1-115">По умолчанию gRPC не отправляет клиенту сведения об исключении, созданном службой gRPC.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-115">By default, gRPC doesn't send the details of an exception thrown by a gRPC service to the client.</span></span> <span data-ttu-id="7a7e1-116">Вместо этого клиент получает универсальное сообщение, указывающее на возникновение ошибки.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-116">Instead, the client receives a generic message indicating an error occurred.</span></span> <span data-ttu-id="7a7e1-117">Доставка сообщений об исключениях клиенту может быть переопределена (например, в процессе разработки или тестирования) с помощью [енабледетаиледеррорс](xref:grpc/configuration#configure-services-options).</span><span class="sxs-lookup"><span data-stu-id="7a7e1-117">Exception message delivery to the client can be overridden (for example, in development or test) with [EnableDetailedErrors](xref:grpc/configuration#configure-services-options).</span></span> <span data-ttu-id="7a7e1-118">Сообщения об исключениях не должны предоставляться клиенту в рабочих приложениях.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-118">Exception messages shouldn't be exposed to the client in production apps.</span></span>

## <a name="message-size-limits"></a><span data-ttu-id="7a7e1-119">Ограничения размера сообщений</span><span class="sxs-lookup"><span data-stu-id="7a7e1-119">Message size limits</span></span>

<span data-ttu-id="7a7e1-120">Входящие сообщения для клиентов и служб gRPC загружаются в память.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-120">Incoming messages to gRPC clients and services are loaded into memory.</span></span> <span data-ttu-id="7a7e1-121">Ограничения размера сообщений — это механизм, позволяющий предотвратить чрезмерное использование ресурсов gRPC.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-121">Message size limits are a mechanism to help prevent gRPC from consuming excessive resources.</span></span>

<span data-ttu-id="7a7e1-122">gRPC использует ограничения на размер каждого сообщения для управления входящими и исходящими сообщениями.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-122">gRPC uses per-message size limits to manage incoming and outgoing messages.</span></span> <span data-ttu-id="7a7e1-123">По умолчанию gRPC ограничивает входящие сообщения 4 МБ.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-123">By default, gRPC limits incoming messages to 4 MB.</span></span> <span data-ttu-id="7a7e1-124">Ограничения на исходящие сообщения отсутствуют.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-124">There is no limit on outgoing messages.</span></span>

<span data-ttu-id="7a7e1-125">На сервере ограничения для сообщений gRPC можно настроить для всех служб в приложении с помощью `AddGrpc`:</span><span class="sxs-lookup"><span data-stu-id="7a7e1-125">On the server, gRPC message limits can be configured for all services in an app with `AddGrpc`:</span></span>

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

<span data-ttu-id="7a7e1-126">Ограничения также можно настроить для отдельной службы с помощью `AddServiceOptions<TService>`.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-126">Limits can also be configured for an individual service using `AddServiceOptions<TService>`.</span></span> <span data-ttu-id="7a7e1-127">Дополнительные сведения о настройке ограничений на размер сообщений см. в разделе [gRPC Configuration](xref:grpc/configuration).</span><span class="sxs-lookup"><span data-stu-id="7a7e1-127">For more information on configuring message size limits, see [gRPC configuration](xref:grpc/configuration).</span></span>

## <a name="client-certificate-validation"></a><span data-ttu-id="7a7e1-128">Проверка сертификата клиента</span><span class="sxs-lookup"><span data-stu-id="7a7e1-128">Client certificate validation</span></span>

<span data-ttu-id="7a7e1-129">[Сертификаты клиента](https://tools.ietf.org/html/rfc5246#section-7.4.4) изначально проверяются при установке соединения.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-129">[Client certificates](https://tools.ietf.org/html/rfc5246#section-7.4.4) are initially validated when the connection is established.</span></span> <span data-ttu-id="7a7e1-130">По умолчанию Kestrel не выполняет дополнительную проверку сертификата клиента подключения.</span><span class="sxs-lookup"><span data-stu-id="7a7e1-130">By default, Kestrel doesn't perform additional validation of a connection's client certificate.</span></span>

<span data-ttu-id="7a7e1-131">Рекомендуется, чтобы gRPC службы, защищенные сертификатами клиента, использовали пакет [Microsoft. AspNetCore. Authentication. Certificate](xref:security/authentication/certauth) .</span><span class="sxs-lookup"><span data-stu-id="7a7e1-131">We recommend that gRPC services secured by client certificates use the [Microsoft.AspNetCore.Authentication.Certificate](xref:security/authentication/certauth) package.</span></span> <span data-ttu-id="7a7e1-132">ASP.NET Core проверка подлинности с помощью сертификации выполняет дополнительную проверку сертификата клиента, в том числе:</span><span class="sxs-lookup"><span data-stu-id="7a7e1-132">ASP.NET Core certification authentication will perform additional validation on a client certificate, including:</span></span>

* <span data-ttu-id="7a7e1-133">Сертификат имеет допустимое расширенное использование ключа (EKU)</span><span class="sxs-lookup"><span data-stu-id="7a7e1-133">Certificate has a valid extended key use (EKU)</span></span>
* <span data-ttu-id="7a7e1-134">Находится в пределах срока действия</span><span class="sxs-lookup"><span data-stu-id="7a7e1-134">Is within its validity period</span></span>
* <span data-ttu-id="7a7e1-135">Проверка отзыва сертификата</span><span class="sxs-lookup"><span data-stu-id="7a7e1-135">Check certificate revocation</span></span>
