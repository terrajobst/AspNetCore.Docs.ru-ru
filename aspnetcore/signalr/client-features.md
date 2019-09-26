---
title: Функции клиентов SignalR
author: bradygaster
description: Узнайте, какие функции поддерживаются различными клиентами SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 2d6759a5484c37aee6db3d22b3127414231605ae
ms.sourcegitcommit: 14b25156e34c82ed0495b4aff5776ac5b1950b5e
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/26/2019
ms.locfileid: "71301192"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="1ffe1-103">Функции клиента SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ffe1-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="1ffe1-104">Распространение компонентов</span><span class="sxs-lookup"><span data-stu-id="1ffe1-104">Feature distribution</span></span>

<span data-ttu-id="1ffe1-105">В таблице ниже приведены функции и поддержка для клиентов, которые предлагают поддержку в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="1ffe1-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="1ffe1-106">Функция</span><span class="sxs-lookup"><span data-stu-id="1ffe1-106">Feature</span></span> | <span data-ttu-id="1ffe1-107">.NET</span><span class="sxs-lookup"><span data-stu-id="1ffe1-107">.NET</span></span> | <span data-ttu-id="1ffe1-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="1ffe1-108">JavaScript</span></span> | <span data-ttu-id="1ffe1-109">Java</span><span class="sxs-lookup"><span data-stu-id="1ffe1-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| <span data-ttu-id="1ffe1-110">Поддержка службы SignalR Azure</span><span class="sxs-lookup"><span data-stu-id="1ffe1-110">Azure SignalR Service Support</span></span> |<span data-ttu-id="1ffe1-111">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-111">✔</span></span>|<span data-ttu-id="1ffe1-112">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-112">✔</span></span>|<span data-ttu-id="1ffe1-113">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-113">✔</span></span>|
| [<span data-ttu-id="1ffe1-114">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="1ffe1-114">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="1ffe1-115">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-115">✔</span></span>|<span data-ttu-id="1ffe1-116">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-116">✔</span></span>|<span data-ttu-id="1ffe1-117">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-117">✔</span></span>|
| [<span data-ttu-id="1ffe1-118">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="1ffe1-118">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="1ffe1-119">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-119">✔</span></span>|<span data-ttu-id="1ffe1-120">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-120">✔</span></span>|<span data-ttu-id="1ffe1-121">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-121">✔</span></span>|
| <span data-ttu-id="1ffe1-122">Автоматическое повторное подключение ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="1ffe1-122">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="1ffe1-123">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-123">✔</span></span>|<span data-ttu-id="1ffe1-124">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-124">✔</span></span>| |
| <span data-ttu-id="1ffe1-125">Транспорт WebSockets</span><span class="sxs-lookup"><span data-stu-id="1ffe1-125">WebSockets Transport</span></span> |<span data-ttu-id="1ffe1-126">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-126">✔</span></span>|<span data-ttu-id="1ffe1-127">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-127">✔</span></span>|<span data-ttu-id="1ffe1-128">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-128">✔</span></span>|
| <span data-ttu-id="1ffe1-129">Транспорт событий, отправленных сервером</span><span class="sxs-lookup"><span data-stu-id="1ffe1-129">Server-Sent Events Transport</span></span> |<span data-ttu-id="1ffe1-130">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-130">✔</span></span>|<span data-ttu-id="1ffe1-131">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-131">✔</span></span>| |
| <span data-ttu-id="1ffe1-132">Длинный опрашивающий транспорт</span><span class="sxs-lookup"><span data-stu-id="1ffe1-132">Long Polling Transport</span></span> |<span data-ttu-id="1ffe1-133">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-133">✔</span></span>|<span data-ttu-id="1ffe1-134">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-134">✔</span></span>|<span data-ttu-id="1ffe1-135">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-135">✔</span></span>|
| <span data-ttu-id="1ffe1-136">Протокол концентратора JSON</span><span class="sxs-lookup"><span data-stu-id="1ffe1-136">JSON Hub Protocol</span></span> |<span data-ttu-id="1ffe1-137">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-137">✔</span></span>|<span data-ttu-id="1ffe1-138">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-138">✔</span></span>|<span data-ttu-id="1ffe1-139">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-139">✔</span></span>|
| <span data-ttu-id="1ffe1-140">Протокол MessagePack для концентратора</span><span class="sxs-lookup"><span data-stu-id="1ffe1-140">MessagePack Hub Protocol</span></span> |<span data-ttu-id="1ffe1-141">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-141">✔</span></span>|<span data-ttu-id="1ffe1-142">✔</span><span class="sxs-lookup"><span data-stu-id="1ffe1-142">✔</span></span>| |

<span data-ttu-id="1ffe1-143">Поддержка автоматического повторного подключения в клиенте Java осуществляется с помощью средства [записи вопросов](https://github.com/aspnet/AspNetCore/issues/8711).</span><span class="sxs-lookup"><span data-stu-id="1ffe1-143">Support for automatic reconnect in the Java client is tracked in [our issue tracker](https://github.com/aspnet/AspNetCore/issues/8711).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="1ffe1-144">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="1ffe1-144">Additional resources</span></span>

* [<span data-ttu-id="1ffe1-145">Приступая к работе с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="1ffe1-145">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="1ffe1-146">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="1ffe1-146">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="1ffe1-147">Центры</span><span class="sxs-lookup"><span data-stu-id="1ffe1-147">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="1ffe1-148">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="1ffe1-148">JavaScript client</span></span>](xref:signalr/javascript-client)
