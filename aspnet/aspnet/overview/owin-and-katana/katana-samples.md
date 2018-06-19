---
uid: aspnet/overview/owin-and-katana/katana-samples
title: Образцы Katana | Документы Microsoft
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2014
ms.topic: article
ms.assetid: bec04f5d-2638-4417-b288-97c58c8d6379
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/katana-samples
msc.type: authoredcontent
ms.openlocfilehash: 815355c00c9c15cfefa5f98dc89da676743b0390
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 01/24/2018
ms.locfileid: "28034494"
---
<a name="katana-samples"></a><span data-ttu-id="26ebb-102">Образцы Katana</span><span class="sxs-lookup"><span data-stu-id="26ebb-102">Katana Samples</span></span>
====================
<span data-ttu-id="26ebb-103">по [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="26ebb-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="katana-samples"></a><span data-ttu-id="26ebb-104">Образцы Katana</span><span class="sxs-lookup"><span data-stu-id="26ebb-104">Katana Samples</span></span>

<span data-ttu-id="26ebb-105">**ASP.NET маршрутизирует образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="26ebb-105">**ASP.NET Routes Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/AspNetRoutes/ReadMe.txt)</span></span>  
<span data-ttu-id="26ebb-106">В некоторых приложениях требуется подключить компонентами OWIN в таблице маршрутов Asp.Net параллельно с компонентами не OWIN.</span><span class="sxs-lookup"><span data-stu-id="26ebb-106">In some applications you will want to hook up OWIN components in the Asp.Net route table side by side with non-OWIN components.</span></span> <span data-ttu-id="26ebb-107">В этом примере показано, как использовать методы расширения RouteCollection MapOwinPath и MapOwinRoute, предоставляемые Microsoft.Owin.Host.SystemWeb.</span><span class="sxs-lookup"><span data-stu-id="26ebb-107">This sample shows how to use the RouteCollection extension methods MapOwinPath and MapOwinRoute provided by Microsoft.Owin.Host.SystemWeb.</span></span>

<span data-ttu-id="26ebb-108">**Ветвление конвейеров образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="26ebb-108">**Branching Pipelines Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/BranchingPipelines/ReadMe.txt)</span></span>  
<span data-ttu-id="26ebb-109">Конвейера обработки запросов OWIN не обязательно должны быть линейными, они могут быть разветвлен для обработки запросов по-разному.</span><span class="sxs-lookup"><span data-stu-id="26ebb-109">OWIN request processing pipelines do not need to be linear, they can be branched to process requests in different ways.</span></span> <span data-ttu-id="26ebb-110">В этом примере показано, как для создания ветвления конвейера, на основе пути запроса или другие данные запроса, таких как заголовки.</span><span class="sxs-lookup"><span data-stu-id="26ebb-110">This sample shows how to construct a branching pipeline based on request paths or other request data such as headers.</span></span> <span data-ttu-id="26ebb-111">Следующие компоненты доступны в пакете nuget Microsoft.Owin.Mapping.</span><span class="sxs-lookup"><span data-stu-id="26ebb-111">These components are available in the Microsoft.Owin.Mapping nuget package.</span></span>

<span data-ttu-id="26ebb-112">**Образец пользовательского сервера** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span><span class="sxs-lookup"><span data-stu-id="26ebb-112">**Custom Server Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/CustomServer/MyCustomServer/CustomServer.cs) </span></span>  
<span data-ttu-id="26ebb-113">Показано, как использовать пользовательский сервер OWIN при размещении OWIN.</span><span class="sxs-lookup"><span data-stu-id="26ebb-113">Shows how to use a custom OWIN server when self-hosting OWIN.</span></span>

<span data-ttu-id="26ebb-114">**Внедренные образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="26ebb-114">**Embedded Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/Embedded/ReadMe.txt)</span></span>  
<span data-ttu-id="26ebb-115">Некоторые серверы OWIN может выполняться внутри собственного процесса (&quot;резидентной&quot;).</span><span class="sxs-lookup"><span data-stu-id="26ebb-115">Some OWIN servers can be run inside of your own process (&quot;self-hosted&quot;).</span></span> <span data-ttu-id="26ebb-116">В этом примере показано, как запуск приложения с помощью средств, предоставляемых пакетом nuget Microsoft.Owin.Hosting OWIN.</span><span class="sxs-lookup"><span data-stu-id="26ebb-116">This sample shows how to start an OWIN application using the tools provided by the Microsoft.Owin.Hosting nuget package.</span></span>

<span data-ttu-id="26ebb-117">**Образец HelloWorld** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="26ebb-117">**HelloWorld Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorld/ReadMe.txt)</span></span>  
<span data-ttu-id="26ebb-118">OWIN — это сервер HTTP API абстракции, обеспечивающей переносимость приложения по различным серверам.</span><span class="sxs-lookup"><span data-stu-id="26ebb-118">OWIN is a HTTP server API abstraction that enables application portability across various servers.</span></span> <span data-ttu-id="26ebb-119">В этом примере показано, как написать приложение Hello World, с помощью некоторых **простыми оболочками** вокруг необработанные абстракции OWIN и запустите его на веб-сервере, например ASP.NET.</span><span class="sxs-lookup"><span data-stu-id="26ebb-119">This sample demonstrates how to write a Hello World application using some **simple wrappers** around the raw OWIN abstraction and run it on a web server like ASP.NET.</span></span>

<span data-ttu-id="26ebb-120">**Hello World-пример необработанных OWIN** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span><span class="sxs-lookup"><span data-stu-id="26ebb-120">**Hello World Raw OWIN Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/HelloWorldRawOwin/ReadMe.txt)</span></span>  
<span data-ttu-id="26ebb-121">В этом примере показано, как разрабатывать приложения Hello World с использованием **необработанные** абстракции OWIN и запустите его на веб-сервере как Asp.Net.</span><span class="sxs-lookup"><span data-stu-id="26ebb-121">This sample demonstrates how to write a Hello World application using the **raw** OWIN abstraction and run it on a web server like Asp.Net.</span></span>

<span data-ttu-id="26ebb-122">**Образец SignalR** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span><span class="sxs-lookup"><span data-stu-id="26ebb-122">**SignalR Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/SignalR/Program.cs)</span></span>  
<span data-ttu-id="26ebb-123">Показано, как для самостоятельного размещения SignalR с помощью OWIN и Katana.</span><span class="sxs-lookup"><span data-stu-id="26ebb-123">Shows how to self-host SignalR using OWIN / Katana.</span></span> <span data-ttu-id="26ebb-124">Дополнительные сведения о резидентным размещением SignalR см. в разделе [учебника: SignalR резидентной](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span><span class="sxs-lookup"><span data-stu-id="26ebb-124">For more info about self-hosting SignalR, see [Tutorial: SignalR Self-Host](../../../signalr/overview/deployment/tutorial-signalr-self-host.md).</span></span>

<span data-ttu-id="26ebb-125">**Статические файлы образца** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="26ebb-125">**Static Files Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/StaticFilesSample/Startup.cs) </span></span>  
<span data-ttu-id="26ebb-126">Показано, как для поддержки HTTP-запросов для статических файлов с помощью OWIN и Katana.</span><span class="sxs-lookup"><span data-stu-id="26ebb-126">Shows how to support HTTP requests for static files using OWIN / Katana.</span></span>

<span data-ttu-id="26ebb-127">**Веб-API** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span><span class="sxs-lookup"><span data-stu-id="26ebb-127">**Web API** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebApi/ReadMe.txt) </span></span>  
<span data-ttu-id="26ebb-128">В этом примере показано, как разместить OWIN в службах IIS и добавить веб-API в конвейер OWIN.</span><span class="sxs-lookup"><span data-stu-id="26ebb-128">This sample shows how to host OWIN in IIS and add Web API to the OWIN pipeline.</span></span>

<span data-ttu-id="26ebb-129">**Веб-сокета образец** | [исходный код](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span><span class="sxs-lookup"><span data-stu-id="26ebb-129">**Web Socket Sample** | [Source Code](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/Katana/WebSocketSample/WebSocketServer/Startup.cs) </span></span>  
<span data-ttu-id="26ebb-130">Показано, как обеспечить поддержку веб-сокеты в OWIN с помощью [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) класса.</span><span class="sxs-lookup"><span data-stu-id="26ebb-130">Shows how to support Web Sockets in OWIN by using the [System.Net.WebSockets.WebSocket](https://msdn.microsoft.com/library/system.net.websockets.websocket(v=vs.110).aspx) class.</span></span>
