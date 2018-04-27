---
title: Введение в ASP.NET Core SignalR
author: rachelappel
description: Узнайте, как библиотека ASP.NET Core SignalR упрощает добавление функциональности в реальном времени веб-приложения.
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 03/07/2018
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: signalr/introduction
ms.openlocfilehash: fa9b10201b5dc0e67bcd6d1321a3737e2025fda4
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/18/2018
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="0752b-103">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="0752b-103">Introduction to ASP.NET Core SignalR</span></span>

<span data-ttu-id="0752b-104">Автор: [Рэйчел Аппель](https://twitter.com/rachelappel) (Rachel Appel)</span><span class="sxs-lookup"><span data-stu-id="0752b-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>


[!INCLUDE [2.1 preview notice](~/includes/2.1.md)]

## <a name="what-is-signalr"></a><span data-ttu-id="0752b-105">Что такое SignalR</span><span class="sxs-lookup"><span data-stu-id="0752b-105">What is SignalR?</span></span>

<span data-ttu-id="0752b-106">SignalR ASP.NET Core — это библиотека, упрощает создание новых функций реального времени для приложения.</span><span class="sxs-lookup"><span data-stu-id="0752b-106">ASP.NET Core SignalR is a library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="0752b-107">В режиме реального времени веб-функций позволяет коду серверные push-содержимое клиентам мгновенно.</span><span class="sxs-lookup"><span data-stu-id="0752b-107">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="0752b-108">Хорошими кандидатами для SignalR:</span><span class="sxs-lookup"><span data-stu-id="0752b-108">Good candidates for SignalR:</span></span>

* <span data-ttu-id="0752b-109">Приложения, которые требуют высокой частотой обновления с сервера.</span><span class="sxs-lookup"><span data-stu-id="0752b-109">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="0752b-110">Примерами являются игры, социальные сети, с правом голоса, аукционов, схемы и GPS приложений.</span><span class="sxs-lookup"><span data-stu-id="0752b-110">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="0752b-111">Панели мониторинга и мониторинга приложений.</span><span class="sxs-lookup"><span data-stu-id="0752b-111">Dashboards and monitoring apps.</span></span> <span data-ttu-id="0752b-112">Пример: информационные панели компании, мгновенного обновления продаж, или передаваться предупреждения.</span><span class="sxs-lookup"><span data-stu-id="0752b-112">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="0752b-113">Приложения для совместной работы.</span><span class="sxs-lookup"><span data-stu-id="0752b-113">Collaborative apps.</span></span> <span data-ttu-id="0752b-114">Доска приложений и группы программного обеспечения являются примерами приложений для совместной работы.</span><span class="sxs-lookup"><span data-stu-id="0752b-114">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="0752b-115">Приложения, требующие уведомления.</span><span class="sxs-lookup"><span data-stu-id="0752b-115">Apps that require notifications.</span></span> <span data-ttu-id="0752b-116">Уведомления о использовать социальных сетей, электронной почты, чат, игры, движения предупреждения и другие приложения.</span><span class="sxs-lookup"><span data-stu-id="0752b-116">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="0752b-117">SignalR предоставляет API-Интерфейс для создания клиента server [удаленных вызовов процедур (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="0752b-117">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="0752b-118">RPC вызывают функции JavaScript на клиентах из серверного кода .NET Core.</span><span class="sxs-lookup"><span data-stu-id="0752b-118">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="0752b-119">SignalR для ASP.NET Core:</span><span class="sxs-lookup"><span data-stu-id="0752b-119">SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="0752b-120">Автоматически обрабатывает управление соединениями.</span><span class="sxs-lookup"><span data-stu-id="0752b-120">Handles connection management automatically.</span></span>
* <span data-ttu-id="0752b-121">Включает широковещательная рассылка сообщений для всех подключенных клиентов одновременно.</span><span class="sxs-lookup"><span data-stu-id="0752b-121">Enables broadcasting messages to all connected clients simultaneously.</span></span> <span data-ttu-id="0752b-122">Например участников чата.</span><span class="sxs-lookup"><span data-stu-id="0752b-122">For example, a chat room.</span></span>
* <span data-ttu-id="0752b-123">Разрешает передачу сообщений для конкретных клиентов или групп клиентов.</span><span class="sxs-lookup"><span data-stu-id="0752b-123">Enables sending messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="0752b-124">Имеет открытый исходный код в [GitHub](https://github.com/aspnet/signalr).</span><span class="sxs-lookup"><span data-stu-id="0752b-124">Is open-sourced at [GitHub](https://github.com/aspnet/signalr).</span></span>
* <span data-ttu-id="0752b-125">Масштабируемость.</span><span class="sxs-lookup"><span data-stu-id="0752b-125">Scalable.</span></span>

<span data-ttu-id="0752b-126">Соединение между клиентом и сервером является постоянным, в отличие от HTTP-соединение.</span><span class="sxs-lookup"><span data-stu-id="0752b-126">The connection between the client and server is persistent, unlike an HTTP connection.</span></span>

## <a name="transports"></a><span data-ttu-id="0752b-127">Транспорты</span><span class="sxs-lookup"><span data-stu-id="0752b-127">Transports</span></span>

<span data-ttu-id="0752b-128">Краткие описания SignalR через ряд методов для построения в режиме реального времени веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="0752b-128">SignalR abstracts over a number of techniques for building real-time web applications.</span></span> <span data-ttu-id="0752b-129">[WebSockets](https://tools.ietf.org/html/rfc7118) является оптимальной транспортом, но другие методы, как события Server-Sent и Long опроса можно использовать при тех недоступны.</span><span class="sxs-lookup"><span data-stu-id="0752b-129">[WebSockets](https://tools.ietf.org/html/rfc7118) is the optimal transport, but other techniques like Server-Sent Events and Long Polling can be used when those aren't available.</span></span> <span data-ttu-id="0752b-130">SignalR автоматически обнаружит и инициализации транспорта на возможности, поддерживаемые на сервере и клиенте.</span><span class="sxs-lookup"><span data-stu-id="0752b-130">SignalR will automatically detect and initialize the appropriate transport based on features supported on the server and client.</span></span>

## <a name="hubs-and-endpoints"></a><span data-ttu-id="0752b-131">Концентраторы и конечных точек</span><span class="sxs-lookup"><span data-stu-id="0752b-131">Hubs and Endpoints</span></span>

<span data-ttu-id="0752b-132">SignalR использует концентраторы и конечные точки для взаимодействия между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="0752b-132">SignalR uses Hubs and Endpoints to communicate between clients and servers.</span></span> <span data-ttu-id="0752b-133">API концентраторов охватывает большинство сценариев.</span><span class="sxs-lookup"><span data-stu-id="0752b-133">The Hubs API covers the most scenarios.</span></span>

<span data-ttu-id="0752b-134">Концентратор является высокого уровня конвейера основаны на конечную точку API, который позволяет клиенту и серверу вызывать методы друг от друга.</span><span class="sxs-lookup"><span data-stu-id="0752b-134">A hub is a high-level pipeline built upon the Endpoint API that allows your client and server to call methods on each other.</span></span> <span data-ttu-id="0752b-135">SignalR обрабатывает доставку за пределами границ машины автоматически, позволив клиентам вызывать методы на сервере, как легко, как локальные методы и наоборот.</span><span class="sxs-lookup"><span data-stu-id="0752b-135">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server as easily as local methods, and vice versa.</span></span> <span data-ttu-id="0752b-136">Концентраторы разрешить передачи строго типизированных параметров в методы, что позволяет привязки модели.</span><span class="sxs-lookup"><span data-stu-id="0752b-136">Hubs allow passing strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="0752b-137">SignalR обеспечивает два встроенных концентратора протокола: протокол текст на основе JSON и двоичный протокол на основе [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="0752b-137">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="0752b-138">MessagePack обычно создает сообщения меньше, чем при использовании JSON.</span><span class="sxs-lookup"><span data-stu-id="0752b-138">MessagePack generally creates smaller messages than when using JSON.</span></span> <span data-ttu-id="0752b-139">Должен поддерживать старых браузерах [XHR уровня 2](https://caniuse.com/#feat=xhr2) для обеспечения поддержки протокола MessagePack.</span><span class="sxs-lookup"><span data-stu-id="0752b-139">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="0752b-140">Концентраторы вызывать код на стороне клиента, отправка сообщений с помощью активный транспорт.</span><span class="sxs-lookup"><span data-stu-id="0752b-140">Hubs call client-side code by sending messages using the active transport.</span></span> <span data-ttu-id="0752b-141">Сообщения содержат имя и параметры клиентского метода.</span><span class="sxs-lookup"><span data-stu-id="0752b-141">The messages contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="0752b-142">Объекты передаются как параметры метода десериализуются с помощью настроенных протокола.</span><span class="sxs-lookup"><span data-stu-id="0752b-142">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="0752b-143">Клиент пытается соответствовать имени метода в код на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="0752b-143">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="0752b-144">При появлении совпадения, метод клиента выполняется с использованием данных десериализованный параметра.</span><span class="sxs-lookup"><span data-stu-id="0752b-144">When a match happens, the client method runs using the deserialized parameter data.</span></span>

<span data-ttu-id="0752b-145">Конечные точки предоставляют необработанные API сокета, что дает им возможность чтения и записи клиента.</span><span class="sxs-lookup"><span data-stu-id="0752b-145">Endpoints provide a raw socket-like API, enabling them to read and write from the client.</span></span> <span data-ttu-id="0752b-146">Он работает разработчику для обработки группирования, широковещательной рассылки и другие функции.</span><span class="sxs-lookup"><span data-stu-id="0752b-146">It's up to the developer to handle grouping, broadcasting, and other functions.</span></span> <span data-ttu-id="0752b-147">API концентраторов построено на основе конечных точек слоя.</span><span class="sxs-lookup"><span data-stu-id="0752b-147">The Hubs API is built on top of the Endpoints layer.</span></span>

<span data-ttu-id="0752b-148">Следующей схеме показана связь между клиентами, концентраторов и конечных точек.</span><span class="sxs-lookup"><span data-stu-id="0752b-148">The following diagram shows the relationship between hubs, endpoints, and clients.</span></span>

![SignalR карты](introduction/_static/signalr-core-architecture.png)

## <a name="related-resources"></a><span data-ttu-id="0752b-150">Связанные ресурсы</span><span class="sxs-lookup"><span data-stu-id="0752b-150">Related resources</span></span>

[<span data-ttu-id="0752b-151">Начало работы с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="0752b-151">Get started with SignalR for ASP.NET Core</span></span>](xref:signalr/get-started)
