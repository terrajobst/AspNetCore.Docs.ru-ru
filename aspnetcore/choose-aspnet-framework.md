---
title: Выбор между ASP.NET и ASP.NET Core
author: rick-anderson
description: Как выбрать между ASP.NET и ASP.NET Core.
manager: wpickett
ms.author: riande
ms.date: 03/14/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: e6ac9f54ef623895b81eea33d90791e5f0ad5120
ms.sourcegitcommit: 71b93b42cbce8a9b1a12c4d88391e75a4dfb6162
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 03/20/2018
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="c79ea-103">Выбор между ASP.NET и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79ea-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="c79ea-104">Независимо от того, какое веб-приложение вы создаете, ASP.NET предлагает вам решение: от корпоративных веб-приложений, предназначенных для Windows Server, до небольших микрослужб, предназначенных для контейнеров Linux, и все остальное.</span><span class="sxs-lookup"><span data-stu-id="c79ea-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="c79ea-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79ea-105">ASP.NET Core</span></span>

<span data-ttu-id="c79ea-106">ASP.NET Core — это кроссплатформенная среда с открытым кодом для создания современных облачных веб-приложений в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="c79ea-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="c79ea-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c79ea-107">ASP.NET</span></span>

<span data-ttu-id="c79ea-108">ASP.NET — это развитая платформа, предоставляющая все необходимые службы для создания серверных веб-приложений корпоративного класса в Windows.</span><span class="sxs-lookup"><span data-stu-id="c79ea-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-class, server-based web apps on Windows.</span></span>

## <a name="which-one-is-right-for-me"></a><span data-ttu-id="c79ea-109">Какой вариант подойдет мне лучше всего?</span><span class="sxs-lookup"><span data-stu-id="c79ea-109">Which one is right for me?</span></span>

| <span data-ttu-id="c79ea-110">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79ea-110">ASP.NET Core</span></span> | <span data-ttu-id="c79ea-111">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c79ea-111">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="c79ea-112">Предназначена для Windows, macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="c79ea-112">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="c79ea-113">Предназначена для Windows</span><span class="sxs-lookup"><span data-stu-id="c79ea-113">Build for Windows</span></span>|
|<span data-ttu-id="c79ea-114">[Razor Pages](xref:mvc/razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="c79ea-114">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="c79ea-115">См. также [MVC](xref:mvc/overview), [веб-API](xref:tutorials/first-web-api), и [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="c79ea-115">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="c79ea-116">Использование [веб-форм](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [веб-API](/aspnet/web-api/) или [веб-страниц](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="c79ea-116">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="c79ea-117">Несколько версий для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="c79ea-117">Multiple versions per machine</span></span>|<span data-ttu-id="c79ea-118">Одна версия для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="c79ea-118">One version per machine</span></span>|
|<span data-ttu-id="c79ea-119">Разработка в Visual Studio, [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) или [Visual Studio Code](https://code.visualstudio.com/) с использованием C# или F#</span><span class="sxs-lookup"><span data-stu-id="c79ea-119">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="c79ea-120">Разработка с Visual Studio с использованием C#, VB и F#</span><span class="sxs-lookup"><span data-stu-id="c79ea-120">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="c79ea-121">Более высокая производительность, чем в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c79ea-121">Higher performance than ASP.NET</span></span>|<span data-ttu-id="c79ea-122">Хорошая производительность</span><span class="sxs-lookup"><span data-stu-id="c79ea-122">Good performance</span></span>|
|[<span data-ttu-id="c79ea-123">Выбор среды выполнения .NET Framework или .NET Core</span><span class="sxs-lookup"><span data-stu-id="c79ea-123">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="c79ea-124">Использование среды выполнения .NET Framework</span><span class="sxs-lookup"><span data-stu-id="c79ea-124">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="c79ea-125">Сценарии ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79ea-125">ASP.NET Core scenarios</span></span>

<!-- update link to Razor Pages mvc movie series when done -->
* <span data-ttu-id="c79ea-126">[Razor Pages](xref:mvc/razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="c79ea-126">[Razor Pages](xref:mvc/razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="c79ea-127">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="c79ea-127">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="c79ea-128">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="c79ea-128">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="c79ea-129">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="c79ea-129">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="c79ea-130">Сценарии ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c79ea-130">ASP.NET scenarios</span></span>

* [<span data-ttu-id="c79ea-131">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="c79ea-131">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="c79ea-132">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="c79ea-132">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="c79ea-133">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="c79ea-133">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="c79ea-134">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="c79ea-134">Resources</span></span>

* [<span data-ttu-id="c79ea-135">Введение в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="c79ea-135">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="c79ea-136">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="c79ea-136">Introduction to ASP.NET Core</span></span>](xref:index)
