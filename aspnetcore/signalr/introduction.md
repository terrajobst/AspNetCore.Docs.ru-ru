---
title: Введение в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как библиотека ASP.NET Core SignalR упрощает добавление в приложения функций в режиме реального времени.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/27/2019
no-loc:
- SignalR
uid: signalr/introduction
ms.openlocfilehash: e84dd0d086cbfc80a80bc10baa33979da9b5d137
ms.sourcegitcommit: 3b6b0a54b20dc99b0c8c5978400c60adf431072f
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/03/2019
ms.locfileid: "74717238"
---
# <a name="introduction-to-aspnet-core-opno-locsignalr"></a><span data-ttu-id="49d40-103">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="49d40-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-opno-locsignalr"></a><span data-ttu-id="49d40-104">Что такое SignalR?</span><span class="sxs-lookup"><span data-stu-id="49d40-104">What is SignalR?</span></span>

<span data-ttu-id="49d40-105">ASP.NET Core SignalR — это библиотека с открытым исходным кодом, которая упрощает добавление веб-функций в режиме реального времени в приложения.</span><span class="sxs-lookup"><span data-stu-id="49d40-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="49d40-106">Веб-функции в режиме реального времени позволяют коду на стороне сервера мгновенно отправлять содержимое на клиенты.</span><span class="sxs-lookup"><span data-stu-id="49d40-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="49d40-107">Хорошие кандидаты для SignalR:</span><span class="sxs-lookup"><span data-stu-id="49d40-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="49d40-108">Приложения, требующие обновления с высокой частотой от сервера.</span><span class="sxs-lookup"><span data-stu-id="49d40-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="49d40-109">Примерами могут быть игры, социальные сети, голосование, аукцион, карты и приложения GPS.</span><span class="sxs-lookup"><span data-stu-id="49d40-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="49d40-110">Панели мониторинга и приложения мониторинга.</span><span class="sxs-lookup"><span data-stu-id="49d40-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="49d40-111">Примерами могут служить панели мониторинга Организации, обновления для мгновенных продаж или оповещения о командировках.</span><span class="sxs-lookup"><span data-stu-id="49d40-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="49d40-112">Приложения для совместной работы.</span><span class="sxs-lookup"><span data-stu-id="49d40-112">Collaborative apps.</span></span> <span data-ttu-id="49d40-113">Примерами приложений для совместной работы являются приложения доски и программа для собрания группы.</span><span class="sxs-lookup"><span data-stu-id="49d40-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="49d40-114">Приложения, для которых требуются уведомления.</span><span class="sxs-lookup"><span data-stu-id="49d40-114">Apps that require notifications.</span></span> <span data-ttu-id="49d40-115">Социальные сети, электронная почта, чат, игры, оповещения в командировках и многие другие приложения используют уведомления.</span><span class="sxs-lookup"><span data-stu-id="49d40-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

SignalR<span data-ttu-id="49d40-116"> предоставляет API для создания [удаленных вызовов процедур (RPC) "](https://wikipedia.org/wiki/Remote_procedure_call)сервер-клиент".</span><span class="sxs-lookup"><span data-stu-id="49d40-116"> provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="49d40-117">Вызов RPC вызывает функции JavaScript на клиентах из кода .NET Core на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="49d40-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="49d40-118">Ниже приведены некоторые функции SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="49d40-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="49d40-119">Управляет автоматическим управлением соединениями.</span><span class="sxs-lookup"><span data-stu-id="49d40-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="49d40-120">Отправляет сообщения всем подключенным клиентам одновременно.</span><span class="sxs-lookup"><span data-stu-id="49d40-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="49d40-121">Например, комната чатов.</span><span class="sxs-lookup"><span data-stu-id="49d40-121">For example, a chat room.</span></span>
* <span data-ttu-id="49d40-122">Отправляет сообщения конкретным клиентам или группам клиентов.</span><span class="sxs-lookup"><span data-stu-id="49d40-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="49d40-123">Масштабируется для управления увеличением трафика.</span><span class="sxs-lookup"><span data-stu-id="49d40-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="49d40-124">Источник размещается в [репозиторииSignalR на сайте GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="49d40-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="49d40-125">Транспорты</span><span class="sxs-lookup"><span data-stu-id="49d40-125">Transports</span></span>

SignalR<span data-ttu-id="49d40-126"> поддерживает следующие методы обработки обмена данными в режиме реального времени (в порядке правильного резерва):</span><span class="sxs-lookup"><span data-stu-id="49d40-126"> supports the following techniques for handling real-time communication (in order of graceful fallback):</span></span>

* [<span data-ttu-id="49d40-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="49d40-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="49d40-128">События, отправленные сервером</span><span class="sxs-lookup"><span data-stu-id="49d40-128">Server-Sent Events</span></span>
* <span data-ttu-id="49d40-129">Длительный опрос</span><span class="sxs-lookup"><span data-stu-id="49d40-129">Long Polling</span></span>

SignalR<span data-ttu-id="49d40-130"> автоматически выбирает лучший транспортный метод, который находится в пределах возможностей сервера и клиента.</span><span class="sxs-lookup"><span data-stu-id="49d40-130"> automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="49d40-131">Концентраторы</span><span class="sxs-lookup"><span data-stu-id="49d40-131">Hubs</span></span>

SignalR<span data-ttu-id="49d40-132"> использует *концентраторы* для взаимодействия между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="49d40-132"> uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="49d40-133">Концентратор — это высокоуровневый конвейер, который позволяет клиенту и серверу вызывать методы друг друга.</span><span class="sxs-lookup"><span data-stu-id="49d40-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> SignalR<span data-ttu-id="49d40-134"> автоматически обрабатывает диспетчеризации между компьютерами, позволяя клиентам вызывать методы на сервере и наоборот.</span><span class="sxs-lookup"><span data-stu-id="49d40-134"> handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="49d40-135">Можно передать строго типизированные параметры методам, которые обеспечивают привязку модели.</span><span class="sxs-lookup"><span data-stu-id="49d40-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> SignalR<span data-ttu-id="49d40-136"> предоставляет два встроенных протокола концентратора: текстовый протокол на основе JSON и двоичный протокол на основе [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="49d40-136"> provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="49d40-137">MessagePack обычно создает меньшие сообщения по сравнению с JSON.</span><span class="sxs-lookup"><span data-stu-id="49d40-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="49d40-138">Более старые браузеры должны поддерживать [XHR уровня 2](https://caniuse.com/#feat=xhr2) , чтобы обеспечить поддержку протокола MessagePack.</span><span class="sxs-lookup"><span data-stu-id="49d40-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="49d40-139">Концентраторы вызывают код на стороне клиента, отправляя сообщения, содержащие имя и параметры клиентского метода.</span><span class="sxs-lookup"><span data-stu-id="49d40-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="49d40-140">Объекты, отправляемые как параметры метода, десериализованы с помощью настроенного протокола.</span><span class="sxs-lookup"><span data-stu-id="49d40-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="49d40-141">Клиент пытается сопоставить имя с методом в коде на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="49d40-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="49d40-142">Когда клиент находит совпадение, он вызывает метод и передает ему данные десериализованного параметра.</span><span class="sxs-lookup"><span data-stu-id="49d40-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="49d40-143">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="49d40-143">Additional resources</span></span>

* <span data-ttu-id="49d40-144">[Начало работы с SignalR для ASP.NET Core](xref:tutorials/signalr)</span><span class="sxs-lookup"><span data-stu-id="49d40-144">[Get started with SignalR for ASP.NET Core](xref:tutorials/signalr)</span></span>
* [<span data-ttu-id="49d40-145">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="49d40-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="49d40-146">Центры</span><span class="sxs-lookup"><span data-stu-id="49d40-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="49d40-147">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="49d40-147">JavaScript client</span></span>](xref:signalr/javascript-client)
