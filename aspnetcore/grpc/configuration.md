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
# <a name="grpc-for-aspnet-core-configuration"></a><span data-ttu-id="50ed8-103">gRPC для конфигурации ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="50ed8-103">gRPC for ASP.NET Core configuration</span></span>

## <a name="configure-services-options"></a><span data-ttu-id="50ed8-104">Настройка параметров службы</span><span class="sxs-lookup"><span data-stu-id="50ed8-104">Configure services options</span></span>

<span data-ttu-id="50ed8-105">В следующей таблице описаны параметры для настройки службы gRPC:</span><span class="sxs-lookup"><span data-stu-id="50ed8-105">The following table describes options for configuring gRPC services:</span></span>

| <span data-ttu-id="50ed8-106">Параметр</span><span class="sxs-lookup"><span data-stu-id="50ed8-106">Option</span></span> | <span data-ttu-id="50ed8-107">Значение по умолчанию</span><span class="sxs-lookup"><span data-stu-id="50ed8-107">Default Value</span></span> | <span data-ttu-id="50ed8-108">Описание</span><span class="sxs-lookup"><span data-stu-id="50ed8-108">Description</span></span> |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | <span data-ttu-id="50ed8-109">Максимальный размер сообщения в байтах, которые могут быть отправлены с сервера.</span><span class="sxs-lookup"><span data-stu-id="50ed8-109">The maximum message size in bytes that can be sent from the server.</span></span> <span data-ttu-id="50ed8-110">Попытка отправить сообщение, которое превышает размер настроенного сообщения приводят к возникновению исключения.</span><span class="sxs-lookup"><span data-stu-id="50ed8-110">Attempting to send a message that exceeds the configured maximum message size results in an exception.</span></span> |
| `ReceiveMaxMessageSize` | <span data-ttu-id="50ed8-111">4 МБ</span><span class="sxs-lookup"><span data-stu-id="50ed8-111">4 MB</span></span> | <span data-ttu-id="50ed8-112">Максимальный размер сообщения в байтах, которые могут быть получены сервером.</span><span class="sxs-lookup"><span data-stu-id="50ed8-112">The maximum message size in bytes that can be received by the server.</span></span> <span data-ttu-id="50ed8-113">Если сервер получает сообщение, которое превысит это ограничение, возникает исключение.</span><span class="sxs-lookup"><span data-stu-id="50ed8-113">If the server receives a message that exceeds this limit, it throws an exception.</span></span> <span data-ttu-id="50ed8-114">Увеличение этого значения позволяет серверу для получения сообщения большего размера, но может отрицательно повлиять на потребление памяти.</span><span class="sxs-lookup"><span data-stu-id="50ed8-114">Increasing this value allows the server to receive larger messages, but can negatively impact memory consumption.</span></span> |
| `EnableDetailedErrors` | `false` | <span data-ttu-id="50ed8-115">Если `true`подробные сообщения об исключениях возвращаются клиентам при возникновении исключения в методе службы.</span><span class="sxs-lookup"><span data-stu-id="50ed8-115">If `true`, detailed exception messages are returned to clients when an exception is thrown in a service method.</span></span> <span data-ttu-id="50ed8-116">Значение по умолчанию — `false`.</span><span class="sxs-lookup"><span data-stu-id="50ed8-116">The default is `false`.</span></span> <span data-ttu-id="50ed8-117">Этому параметру `true` может привести к утечке конфиденциальной информации.</span><span class="sxs-lookup"><span data-stu-id="50ed8-117">Setting this to `true` can leak sensitive information.</span></span> |
| `CompressionProviders` | <span data-ttu-id="50ed8-118">Gzip</span><span class="sxs-lookup"><span data-stu-id="50ed8-118">gzip</span></span> | <span data-ttu-id="50ed8-119">Коллекция поставщиков сжатия, используемый для сжатия и распаковки сообщения.</span><span class="sxs-lookup"><span data-stu-id="50ed8-119">A collection of compression providers used to compress and decompress messages.</span></span> <span data-ttu-id="50ed8-120">Сжатие пользовательских поставщиков можно создать и добавить в коллекцию.</span><span class="sxs-lookup"><span data-stu-id="50ed8-120">Custom compression providers can be created and added to the collection.</span></span> <span data-ttu-id="50ed8-121">Значение по умолчанию настроен поставщик поддерживает **gzip** сжатия.</span><span class="sxs-lookup"><span data-stu-id="50ed8-121">The default configured provider supports **gzip** compression.</span></span> |
| `ResponseCompressionAlgorithm` | `null` | <span data-ttu-id="50ed8-122">Алгоритм сжатия, используемый для сжатия сообщений, отправленных сервером.</span><span class="sxs-lookup"><span data-stu-id="50ed8-122">The compression algorithm used to compress messages sent from the server.</span></span> <span data-ttu-id="50ed8-123">Алгоритм должен соответствовать поставщика сжатия в `CompressionProviders`.</span><span class="sxs-lookup"><span data-stu-id="50ed8-123">The algorithm must match a compression provider in `CompressionProviders`.</span></span> <span data-ttu-id="50ed8-124">Algorthm для сжатия ответа клиент должен указать, он поддерживает алгоритм, отправляя ему **grpc приемлемой кодировкой** заголовка.</span><span class="sxs-lookup"><span data-stu-id="50ed8-124">For the algorthm to compress a response the client must indicate it supports the algorithm by sending it in the **grpc-accept-encoding** header.</span></span> |
| `ResponseCompressionLevel` | `null` | <span data-ttu-id="50ed8-125">Уровень сжатия, используемый для сжатия сообщений, отправленных сервером.</span><span class="sxs-lookup"><span data-stu-id="50ed8-125">The compress level used to compress messages sent from the server.</span></span> |

<span data-ttu-id="50ed8-126">Можно настроить параметры для всех служб, предоставляя делегат параметры для `AddGrpc` вызов в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="50ed8-126">Options can be configured for all services by providing an options delegate to the `AddGrpc` call in `Startup.ConfigureServices`.</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

<span data-ttu-id="50ed8-127">Параметры для одной службы переопределяют глобальные параметры, указанные на `AddGrpc` и могут настраиваться с помощью `AddServiceOptions<TService>`:</span><span class="sxs-lookup"><span data-stu-id="50ed8-127">Options for a single service override the global options provided in `AddGrpc` and can be configured using `AddServiceOptions<TService>`:</span></span>

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="additional-resources"></a><span data-ttu-id="50ed8-128">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="50ed8-128">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
