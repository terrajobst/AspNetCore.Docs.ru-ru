---
title: Выбор между ASP.NET и ASP.NET Core
author: rick-anderson
description: Как выбрать между ASP.NET и ASP.NET Core.
ms.author: riande
ms.date: 05/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: 6d759c0bc5e5c7d32d6c14786db6ba9fe7a2f1e8
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36297233"
---
# <a name="choose-between-aspnet-and-aspnet-core"></a><span data-ttu-id="8b0d4-103">Выбор между ASP.NET и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b0d4-103">Choose between ASP.NET and ASP.NET Core</span></span>

<span data-ttu-id="8b0d4-104">Независимо от того, какое веб-приложение вы создаете, ASP.NET предлагает вам решение: от корпоративных веб-приложений, предназначенных для Windows Server, до небольших микрослужб, предназначенных для контейнеров Linux, и все остальное.</span><span class="sxs-lookup"><span data-stu-id="8b0d4-104">No matter the web app you're creating, ASP.NET has a solution for you: from enterprise web apps targeting Windows Server, to small microservices targeting Linux containers, and everything in between.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="8b0d4-105">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b0d4-105">ASP.NET Core</span></span>

<span data-ttu-id="8b0d4-106">ASP.NET Core — это кроссплатформенная среда с открытым кодом для создания современных облачных веб-приложений в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="8b0d4-106">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

## <a name="aspnet"></a><span data-ttu-id="8b0d4-107">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b0d4-107">ASP.NET</span></span>

<span data-ttu-id="8b0d4-108">ASP.NET — это развитая платформа, предоставляющая все необходимые службы для создания серверных веб-приложений корпоративного класса в Windows.</span><span class="sxs-lookup"><span data-stu-id="8b0d4-108">ASP.NET is a mature framework that provides all the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="8b0d4-109">Выбор платформы</span><span class="sxs-lookup"><span data-stu-id="8b0d4-109">Framework selection</span></span>

<span data-ttu-id="8b0d4-110">Просмотрите следующую таблицу и выберите подходящую платформу.</span><span class="sxs-lookup"><span data-stu-id="8b0d4-110">Review the table below to determine which framework is most appropriate for your needs.</span></span>

| <span data-ttu-id="8b0d4-111">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b0d4-111">ASP.NET Core</span></span> | <span data-ttu-id="8b0d4-112">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b0d4-112">ASP.NET</span></span> |
|---|---|
|<span data-ttu-id="8b0d4-113">Предназначена для Windows, macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="8b0d4-113">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="8b0d4-114">Предназначена для Windows</span><span class="sxs-lookup"><span data-stu-id="8b0d4-114">Build for Windows</span></span>|
|<span data-ttu-id="8b0d4-115">[Razor Pages](xref:razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="8b0d4-115">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="8b0d4-116">См. также [MVC](xref:mvc/overview), [веб-API](xref:tutorials/first-web-api), и [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="8b0d4-116">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="8b0d4-117">Использование [веб-форм](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [веб-API](/aspnet/web-api/), [веб-перехватчиков](/aspnet/webhooks/) или [веб-страниц](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="8b0d4-117">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="8b0d4-118">Несколько версий для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="8b0d4-118">Multiple versions per machine</span></span>|<span data-ttu-id="8b0d4-119">Одна версия для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="8b0d4-119">One version per machine</span></span>|
|<span data-ttu-id="8b0d4-120">Разработка в Visual Studio, [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) или [Visual Studio Code](https://code.visualstudio.com/) с использованием C# или F#</span><span class="sxs-lookup"><span data-stu-id="8b0d4-120">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="8b0d4-121">Разработка с Visual Studio с использованием C#, VB и F#</span><span class="sxs-lookup"><span data-stu-id="8b0d4-121">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="8b0d4-122">Более высокая производительность, чем в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b0d4-122">Higher performance than ASP.NET</span></span>|<span data-ttu-id="8b0d4-123">Хорошая производительность</span><span class="sxs-lookup"><span data-stu-id="8b0d4-123">Good performance</span></span>|
|[<span data-ttu-id="8b0d4-124">Выбор среды выполнения .NET Framework или .NET Core</span><span class="sxs-lookup"><span data-stu-id="8b0d4-124">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="8b0d4-125">Использование среды выполнения .NET Framework</span><span class="sxs-lookup"><span data-stu-id="8b0d4-125">Use .NET Framework runtime</span></span>|

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="8b0d4-126">Сценарии ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b0d4-126">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="8b0d4-127">[Razor Pages](xref:razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="8b0d4-127">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="8b0d4-128">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="8b0d4-128">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="8b0d4-129">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="8b0d4-129">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="8b0d4-130">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="8b0d4-130">Real-time</span></span>](xref:signalr/index)

## <a name="aspnet-scenarios"></a><span data-ttu-id="8b0d4-131">Сценарии ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b0d4-131">ASP.NET scenarios</span></span>

* [<span data-ttu-id="8b0d4-132">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="8b0d4-132">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="8b0d4-133">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="8b0d4-133">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="8b0d4-134">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="8b0d4-134">Real-time</span></span>](/aspnet/signalr)

## <a name="resources"></a><span data-ttu-id="8b0d4-135">Ресурсы</span><span class="sxs-lookup"><span data-stu-id="8b0d4-135">Resources</span></span>

* [<span data-ttu-id="8b0d4-136">Введение в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="8b0d4-136">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="8b0d4-137">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8b0d4-137">Introduction to ASP.NET Core</span></span>](xref:index)
