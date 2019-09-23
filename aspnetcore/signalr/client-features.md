---
title: Функции клиента SignalR
author: bradygaster
description: Узнайте, какие функции поддерживаются различными клиентами SignalR ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: bradyg
ms.custom: mvc
ms.date: 09/18/2019
uid: signalr/client-features
ms.openlocfilehash: 55086673e0c9f9b73f07730ea25c3fa322f7fd98
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187392"
---
# <a name="aspnet-core-signalr-client-features"></a><span data-ttu-id="55751-103">Функции клиента SignalR ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55751-103">ASP.NET Core SignalR client features</span></span>

## <a name="feature-distribution"></a><span data-ttu-id="55751-104">Распространение компонентов</span><span class="sxs-lookup"><span data-stu-id="55751-104">Feature distribution</span></span>

<span data-ttu-id="55751-105">В таблице ниже приведены функции и поддержка для клиентов, которые предлагают поддержку в режиме реального времени.</span><span class="sxs-lookup"><span data-stu-id="55751-105">The table below shows the features and support for the clients that offer real-time support.</span></span>

| <span data-ttu-id="55751-106">Функция</span><span class="sxs-lookup"><span data-stu-id="55751-106">Feature</span></span> | <span data-ttu-id="55751-107">.NET Core</span><span class="sxs-lookup"><span data-stu-id="55751-107">.NET Core</span></span> | <span data-ttu-id="55751-108">JavaScript</span><span class="sxs-lookup"><span data-stu-id="55751-108">JavaScript</span></span> | <span data-ttu-id="55751-109">Java</span><span class="sxs-lookup"><span data-stu-id="55751-109">Java</span></span> |
| ---- | :-: | :-: | :-: |
| [<span data-ttu-id="55751-110">Потоковая передача из сервера в клиент</span><span class="sxs-lookup"><span data-stu-id="55751-110">Server-to-client Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="55751-111">✔</span><span class="sxs-lookup"><span data-stu-id="55751-111">✔</span></span>|<span data-ttu-id="55751-112">✔</span><span class="sxs-lookup"><span data-stu-id="55751-112">✔</span></span>|<span data-ttu-id="55751-113">✔</span><span class="sxs-lookup"><span data-stu-id="55751-113">✔</span></span>|
| [<span data-ttu-id="55751-114">Потоковая передача клиента в сервер</span><span class="sxs-lookup"><span data-stu-id="55751-114">Client-to-server Streaming</span></span>](xref:signalr/streaming)          |<span data-ttu-id="55751-115">✔</span><span class="sxs-lookup"><span data-stu-id="55751-115">✔</span></span>|<span data-ttu-id="55751-116">✔</span><span class="sxs-lookup"><span data-stu-id="55751-116">✔</span></span>|<span data-ttu-id="55751-117">✔</span><span class="sxs-lookup"><span data-stu-id="55751-117">✔</span></span>|
| <span data-ttu-id="55751-118">Автоматическое повторное подключение ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span><span class="sxs-lookup"><span data-stu-id="55751-118">Automatic Reconnection ([.NET](/aspnet/core/signalr/dotnet-client?view=aspnetcore-3.0&tabs=visual-studio#handle-lost-connection), [JavaScript](/aspnet/core/signalr/javascript-client?view=aspnetcore-3.0#reconnect-clients))</span></span>          |<span data-ttu-id="55751-119">✔</span><span class="sxs-lookup"><span data-stu-id="55751-119">✔</span></span>|<span data-ttu-id="55751-120">✔</span><span class="sxs-lookup"><span data-stu-id="55751-120">✔</span></span>| |

## <a name="additional-resources"></a><span data-ttu-id="55751-121">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="55751-121">Additional resources</span></span>

* [<span data-ttu-id="55751-122">Приступая к работе с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="55751-122">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="55751-123">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="55751-123">Supported platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="55751-124">Центры</span><span class="sxs-lookup"><span data-stu-id="55751-124">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="55751-125">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="55751-125">JavaScript client</span></span>](xref:signalr/javascript-client)
