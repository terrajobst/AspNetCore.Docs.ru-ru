---
title: Выбор между ASP.NET 4.x и ASP.NET Core
author: rick-anderson
description: Что такое ASP.NET Core и ASP.NET 4.x и как выбрать между ними.
ms.author: riande
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: fa281a068dfdcd8a7c45dd5d0a0c3084c4bc8dbc
ms.sourcegitcommit: e8d80ff566bfe505b43389d7bc4551edb1c0c872
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/28/2018
ms.locfileid: "52549076"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="08829-103">Выбор между ASP.NET 4.x и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08829-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="08829-104">ASP.NET Core является переработанной версией ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="08829-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="08829-105">В этой статье перечислены различия между ними.</span><span class="sxs-lookup"><span data-stu-id="08829-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="08829-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08829-106">ASP.NET Core</span></span>

<span data-ttu-id="08829-107">ASP.NET Core — это кроссплатформенная среда с открытым кодом для создания современных облачных веб-приложений в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="08829-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="08829-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="08829-108">ASP.NET 4.x</span></span>

<span data-ttu-id="08829-109">ASP.NET 4.x — это развитая платформа, предоставляющая необходимые службы для создания серверных веб-приложений корпоративного класса в Windows.</span><span class="sxs-lookup"><span data-stu-id="08829-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="08829-110">Выбор платформы</span><span class="sxs-lookup"><span data-stu-id="08829-110">Framework selection</span></span>

<span data-ttu-id="08829-111">В следующей таблице сравниваются ASP.NET Core и ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="08829-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="08829-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08829-112">ASP.NET Core</span></span> | <span data-ttu-id="08829-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="08829-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="08829-114">Предназначена для Windows, macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="08829-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="08829-115">Предназначена для Windows</span><span class="sxs-lookup"><span data-stu-id="08829-115">Build for Windows</span></span>|
|<span data-ttu-id="08829-116">[Razor Pages](xref:razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="08829-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="08829-117">См. также [MVC](xref:mvc/overview), [веб-API](xref:tutorials/first-web-api), и [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="08829-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="08829-118">Использование [веб-форм](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [веб-API](/aspnet/web-api/), [веб-перехватчиков](/aspnet/webhooks/) или [веб-страниц](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="08829-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="08829-119">Несколько версий для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="08829-119">Multiple versions per machine</span></span>|<span data-ttu-id="08829-120">Одна версия для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="08829-120">One version per machine</span></span>|
|<span data-ttu-id="08829-121">Разработка в Visual Studio, [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) или [Visual Studio Code](https://code.visualstudio.com/) с использованием C# или F#</span><span class="sxs-lookup"><span data-stu-id="08829-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="08829-122">Разработка с Visual Studio с использованием C#, VB и F#</span><span class="sxs-lookup"><span data-stu-id="08829-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="08829-123">Более высокая производительность, чем в ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="08829-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="08829-124">Хорошая производительность</span><span class="sxs-lookup"><span data-stu-id="08829-124">Good performance</span></span>|
|[<span data-ttu-id="08829-125">Выбор среды выполнения .NET Framework или .NET Core</span><span class="sxs-lookup"><span data-stu-id="08829-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/articles/standard/choosing-core-framework-server)|<span data-ttu-id="08829-126">Использование среды выполнения .NET Framework</span><span class="sxs-lookup"><span data-stu-id="08829-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="08829-127">Дополнительные сведения о поддержке ASP.NET Core 2.x на платформе .NET Framework см. в разделе [ASP.NET Core для платформы .NET Framework](xref:index#target-framework).</span><span class="sxs-lookup"><span data-stu-id="08829-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="08829-128">Сценарии ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08829-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="08829-129">[Razor Pages](xref:razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="08829-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="08829-130">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="08829-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="08829-131">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="08829-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="08829-132">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="08829-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="08829-133">Развертывание приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="08829-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="08829-134">Сценарии ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="08829-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="08829-135">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="08829-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="08829-136">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="08829-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="08829-137">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="08829-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="08829-138">Создание веб-приложение ASP.NET 4.x в Azure</span><span class="sxs-lookup"><span data-stu-id="08829-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="08829-139">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="08829-139">Additional resources</span></span>

* [<span data-ttu-id="08829-140">Введение в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="08829-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="08829-141">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="08829-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> <span data-ttu-id="08829-142">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="08829-142">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="08829-143">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span><span class="sxs-lookup"><span data-stu-id="08829-143">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/rps16hd5).</span></span>