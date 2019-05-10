---
title: Введение в ASP.NET Core SignalR
author: bradygaster
description: Узнайте, как библиотека ASP.NET Core SignalR упрощает добавление функциональности в режиме реального времени к приложениям.
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/25/2018
uid: signalr/introduction
ms.openlocfilehash: 673efafce60dfa46cb99f9537fda2bca42bf9822
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/27/2019
ms.locfileid: "64892251"
---
# <a name="introduction-to-aspnet-core-signalr"></a><span data-ttu-id="8aa4a-103">Введение в ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="8aa4a-103">Introduction to ASP.NET Core SignalR</span></span>

## <a name="what-is-signalr"></a><span data-ttu-id="8aa4a-104">Что такое SignalR</span><span class="sxs-lookup"><span data-stu-id="8aa4a-104">What is SignalR?</span></span>

<span data-ttu-id="8aa4a-105">ASP.NET Core SignalR — это библиотека открытым исходным кодом, которая упрощает создание новых функций в режиме реального времени к приложениям.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-105">ASP.NET Core SignalR is an open-source library that simplifies adding real-time web functionality to apps.</span></span> <span data-ttu-id="8aa4a-106">В режиме реального времени веб-функций позволяет коду на сервере содержимого Push-уведомлений на клиентах мгновенно.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-106">Real-time web functionality enables server-side code to push content to clients instantly.</span></span>

<span data-ttu-id="8aa4a-107">Хорошими кандидатами для использования SignalR:</span><span class="sxs-lookup"><span data-stu-id="8aa4a-107">Good candidates for SignalR:</span></span>

* <span data-ttu-id="8aa4a-108">Приложения, требующие высокой частотой обновления с сервера.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-108">Apps that require high frequency updates from the server.</span></span> <span data-ttu-id="8aa4a-109">Примерами являются игры, социальные сети, голосования, аукцион, карт и приложений GPS.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-109">Examples are gaming, social networks, voting, auction, maps, and GPS apps.</span></span>
* <span data-ttu-id="8aa4a-110">Панели мониторинга и мониторинга приложения.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-110">Dashboards and monitoring apps.</span></span> <span data-ttu-id="8aa4a-111">Примеры включают информационные панели компании, мгновенного обновления продаж, или командировки оповещения.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-111">Examples include company dashboards, instant sales updates, or travel alerts.</span></span>
* <span data-ttu-id="8aa4a-112">Совместной работы приложения.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-112">Collaborative apps.</span></span> <span data-ttu-id="8aa4a-113">Доска приложения и группы программного обеспечения являются примерами приложений совместной работы.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-113">Whiteboard apps and team meeting software are examples of collaborative apps.</span></span>
* <span data-ttu-id="8aa4a-114">Приложения, которым требуются уведомления.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-114">Apps that require notifications.</span></span> <span data-ttu-id="8aa4a-115">Уведомления о использовать социальных сетей, электронной почты, чат, игры, travel оповещения и многие другие приложения.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-115">Social networks, email, chat, games, travel alerts, and many other apps use notifications.</span></span>

<span data-ttu-id="8aa4a-116">SignalR предоставляет API для создания клиентом и сервером [удаленные вызовы процедур (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span><span class="sxs-lookup"><span data-stu-id="8aa4a-116">SignalR provides an API for creating server-to-client [remote procedure calls (RPC)](https://wikipedia.org/wiki/Remote_procedure_call).</span></span> <span data-ttu-id="8aa4a-117">RPC вызывают функции JavaScript на клиентах из кода .NET Core на сервере.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-117">The RPCs call JavaScript functions on clients from server-side .NET Core code.</span></span>

<span data-ttu-id="8aa4a-118">Ниже приведены некоторые функции SignalR для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-118">Here are some features of SignalR for ASP.NET Core:</span></span>

* <span data-ttu-id="8aa4a-119">Автоматически обрабатывает управление подключениями.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-119">Handles connection management automatically.</span></span>
* <span data-ttu-id="8aa4a-120">Отправляет сообщения на всех подключенных клиентов одновременно.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-120">Sends messages to all connected clients simultaneously.</span></span> <span data-ttu-id="8aa4a-121">Например в чат.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-121">For example, a chat room.</span></span>
* <span data-ttu-id="8aa4a-122">Отправляет сообщения в определенные клиенты или групп клиентов.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-122">Sends messages to specific clients or groups of clients.</span></span>
* <span data-ttu-id="8aa4a-123">Масштабирование для обработки увеличение трафика.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-123">Scales to handle increasing traffic.</span></span>

<span data-ttu-id="8aa4a-124">Источник размещен в [GitHub-репозитории SignalR](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span><span class="sxs-lookup"><span data-stu-id="8aa4a-124">The source is hosted in a [SignalR repository on GitHub](https://github.com/aspnet/AspNetCore/tree/master/src/SignalR).</span></span>

## <a name="transports"></a><span data-ttu-id="8aa4a-125">Транспорты</span><span class="sxs-lookup"><span data-stu-id="8aa4a-125">Transports</span></span>

<span data-ttu-id="8aa4a-126">SignalR поддерживает несколько методов для обработки в режиме реального времени:</span><span class="sxs-lookup"><span data-stu-id="8aa4a-126">SignalR supports several techniques for handling real-time communications:</span></span>

* [<span data-ttu-id="8aa4a-127">WebSockets</span><span class="sxs-lookup"><span data-stu-id="8aa4a-127">WebSockets</span></span>](https://tools.ietf.org/html/rfc7118)
* <span data-ttu-id="8aa4a-128">Сервер отправил события</span><span class="sxs-lookup"><span data-stu-id="8aa4a-128">Server-Sent Events</span></span>
* <span data-ttu-id="8aa4a-129">Долгие опросы</span><span class="sxs-lookup"><span data-stu-id="8aa4a-129">Long Polling</span></span>

<span data-ttu-id="8aa4a-130">SignalR автоматически выбирает лучший метод транспорта, в рамках возможностей клиента и сервера.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-130">SignalR automatically chooses the best transport method that is within the capabilities of the server and client.</span></span>

## <a name="hubs"></a><span data-ttu-id="8aa4a-131">Концентраторы</span><span class="sxs-lookup"><span data-stu-id="8aa4a-131">Hubs</span></span>

<span data-ttu-id="8aa4a-132">SignalR использует *концентраторов* для обмена данными между клиентами и серверами.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-132">SignalR uses *hubs* to communicate between clients and servers.</span></span>

<span data-ttu-id="8aa4a-133">Концентратор — общий конвейер, который позволяет клиенту и серверу вызывать методы на друг с другом.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-133">A hub is a high-level pipeline that allows a client and server to call methods on each other.</span></span> <span data-ttu-id="8aa4a-134">SignalR обрабатывает доставку разных компьютерах автоматически, что позволяет клиентам вызывать методы на сервере и наоборот.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-134">SignalR handles the dispatching across machine boundaries automatically, allowing clients to call methods on the server and vice versa.</span></span> <span data-ttu-id="8aa4a-135">Можно передать со строгой типизацией параметров методов, что позволяет привязки модели.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-135">You can pass strongly-typed parameters to methods, which enables model binding.</span></span> <span data-ttu-id="8aa4a-136">SignalR обеспечивает два встроенных концентратор протокола: протокол текста на основе JSON и двоичный протокол, основанный на [MessagePack](https://msgpack.org/).</span><span class="sxs-lookup"><span data-stu-id="8aa4a-136">SignalR provides two built-in hub protocols: a text protocol based on JSON and a binary protocol based on [MessagePack](https://msgpack.org/).</span></span>  <span data-ttu-id="8aa4a-137">Как правило, MessagePack создает относительно мелкие сообщения, по сравнению с JSON.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-137">MessagePack generally creates smaller messages compared to JSON.</span></span> <span data-ttu-id="8aa4a-138">Более старые браузеры должны поддерживать [XHR уровня 2](https://caniuse.com/#feat=xhr2) для обеспечения поддержки протокола MessagePack.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-138">Older browsers must support [XHR level 2](https://caniuse.com/#feat=xhr2) to provide MessagePack protocol support.</span></span>

<span data-ttu-id="8aa4a-139">Концентраторы вызывать код на стороне клиента, отправляя сообщения, которые содержат имя и параметры метода со стороны клиента.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-139">Hubs call client-side code by sending messages that contain the name and parameters of the client-side method.</span></span> <span data-ttu-id="8aa4a-140">Объекты, которые передаются как параметры метода десериализуются с помощью настроенный протокол.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-140">Objects sent as method parameters are deserialized using the configured protocol.</span></span> <span data-ttu-id="8aa4a-141">Клиент пытается соответствовать имени метода в коде на стороне клиента.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-141">The client tries to match the name to a method in the client-side code.</span></span> <span data-ttu-id="8aa4a-142">Когда клиент находит совпадение, он вызывает метод и передает ему параметр десериализованные данные.</span><span class="sxs-lookup"><span data-stu-id="8aa4a-142">When the client finds a match, it calls the method and passes to it the deserialized parameter data.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8aa4a-143">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8aa4a-143">Additional resources</span></span>

* [<span data-ttu-id="8aa4a-144">Начало работы с SignalR для ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8aa4a-144">Get started with SignalR for ASP.NET Core</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="8aa4a-145">Поддерживаемые платформы</span><span class="sxs-lookup"><span data-stu-id="8aa4a-145">Supported Platforms</span></span>](xref:signalr/supported-platforms)
* [<span data-ttu-id="8aa4a-146">Центры</span><span class="sxs-lookup"><span data-stu-id="8aa4a-146">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8aa4a-147">Клиент JavaScript</span><span class="sxs-lookup"><span data-stu-id="8aa4a-147">JavaScript client</span></span>](xref:signalr/javascript-client)
