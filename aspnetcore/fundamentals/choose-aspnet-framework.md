---
title: Выбор между ASP.NET 4.x и ASP.NET Core
author: rick-anderson
description: Что такое ASP.NET Core и ASP.NET 4.x и как выбрать между ними.
ms.author: riande
ms.custom: seodec18
ms.date: 09/11/2018
uid: fundamentals/choose-between-aspnet-and-aspnetcore
ms.openlocfilehash: b75fbea330e48075c4a2789454e973d4c56ffa53
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121184"
---
# <a name="choose-between-aspnet-4x-and-aspnet-core"></a><span data-ttu-id="68a6b-103">Выбор между ASP.NET 4.x и ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68a6b-103">Choose between ASP.NET 4.x and ASP.NET Core</span></span>

<span data-ttu-id="68a6b-104">ASP.NET Core является переработанной версией ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="68a6b-104">ASP.NET Core is a redesign of ASP.NET 4.x.</span></span> <span data-ttu-id="68a6b-105">В этой статье перечислены различия между ними.</span><span class="sxs-lookup"><span data-stu-id="68a6b-105">This article lists the differences between them.</span></span>

## <a name="aspnet-core"></a><span data-ttu-id="68a6b-106">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68a6b-106">ASP.NET Core</span></span>

<span data-ttu-id="68a6b-107">ASP.NET Core — это кроссплатформенная среда с открытым кодом для создания современных облачных веб-приложений в Windows, macOS или Linux.</span><span class="sxs-lookup"><span data-stu-id="68a6b-107">ASP.NET Core is an open-source, cross-platform framework for building modern, cloud-based web apps on Windows, macOS, or Linux.</span></span>

[!INCLUDE[](~/includes/benefits.md)]

## <a name="aspnet-4x"></a><span data-ttu-id="68a6b-108">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="68a6b-108">ASP.NET 4.x</span></span>

<span data-ttu-id="68a6b-109">ASP.NET 4.x — это развитая платформа, предоставляющая необходимые службы для создания серверных веб-приложений корпоративного класса в Windows.</span><span class="sxs-lookup"><span data-stu-id="68a6b-109">ASP.NET 4.x is a mature framework that provides the services needed to build enterprise-grade, server-based web apps on Windows.</span></span>

## <a name="framework-selection"></a><span data-ttu-id="68a6b-110">Выбор платформы</span><span class="sxs-lookup"><span data-stu-id="68a6b-110">Framework selection</span></span>

<span data-ttu-id="68a6b-111">В следующей таблице сравниваются ASP.NET Core и ASP.NET 4.x.</span><span class="sxs-lookup"><span data-stu-id="68a6b-111">The following table compares ASP.NET Core to ASP.NET 4.x.</span></span>

| <span data-ttu-id="68a6b-112">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68a6b-112">ASP.NET Core</span></span> | <span data-ttu-id="68a6b-113">ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="68a6b-113">ASP.NET 4.x</span></span> |
|---|---|
|<span data-ttu-id="68a6b-114">Предназначена для Windows, macOS или Linux</span><span class="sxs-lookup"><span data-stu-id="68a6b-114">Build for Windows, macOS, or Linux</span></span>|<span data-ttu-id="68a6b-115">Предназначена для Windows</span><span class="sxs-lookup"><span data-stu-id="68a6b-115">Build for Windows</span></span>|
|<span data-ttu-id="68a6b-116">[Razor Pages](xref:razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="68a6b-116">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span> <span data-ttu-id="68a6b-117">См. также [MVC](xref:mvc/overview), [веб-API](xref:tutorials/first-web-api), и [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="68a6b-117">See also [MVC](xref:mvc/overview), [Web API](xref:tutorials/first-web-api), and [SignalR](xref:signalr/introduction).</span></span>|<span data-ttu-id="68a6b-118">Использование [веб-форм](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [веб-API](/aspnet/web-api/), [веб-перехватчиков](/aspnet/webhooks/) или [веб-страниц](/aspnet/web-pages)</span><span class="sxs-lookup"><span data-stu-id="68a6b-118">Use [Web Forms](/aspnet/web-forms), [SignalR](/aspnet/signalr), [MVC](/aspnet/mvc), [Web API](/aspnet/web-api/), [WebHooks](/aspnet/webhooks/), or [Web Pages](/aspnet/web-pages)</span></span>|
|<span data-ttu-id="68a6b-119">Несколько версий для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="68a6b-119">Multiple versions per machine</span></span>|<span data-ttu-id="68a6b-120">Одна версия для одного компьютера</span><span class="sxs-lookup"><span data-stu-id="68a6b-120">One version per machine</span></span>|
|<span data-ttu-id="68a6b-121">Разработка в Visual Studio, [Visual Studio для Mac](https://www.visualstudio.com/vs/visual-studio-mac/) или [Visual Studio Code](https://code.visualstudio.com/) с использованием C# или F#</span><span class="sxs-lookup"><span data-stu-id="68a6b-121">Develop with Visual Studio, [Visual Studio for Mac](https://www.visualstudio.com/vs/visual-studio-mac/), or [Visual Studio Code](https://code.visualstudio.com/) using C# or F#</span></span>|<span data-ttu-id="68a6b-122">Разработка с Visual Studio с использованием C#, VB и F#</span><span class="sxs-lookup"><span data-stu-id="68a6b-122">Develop with Visual Studio using C#, VB, or F#</span></span>|
|<span data-ttu-id="68a6b-123">Более высокая производительность, чем в ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="68a6b-123">Higher performance than ASP.NET 4.x</span></span>|<span data-ttu-id="68a6b-124">Хорошая производительность</span><span class="sxs-lookup"><span data-stu-id="68a6b-124">Good performance</span></span>|
|[<span data-ttu-id="68a6b-125">Выбор среды выполнения .NET Framework или .NET Core</span><span class="sxs-lookup"><span data-stu-id="68a6b-125">Choose .NET Framework or .NET Core runtime</span></span>](/dotnet/standard/choosing-core-framework-server)|<span data-ttu-id="68a6b-126">Использование среды выполнения .NET Framework</span><span class="sxs-lookup"><span data-stu-id="68a6b-126">Use .NET Framework runtime</span></span>|

<span data-ttu-id="68a6b-127">Дополнительные сведения о поддержке ASP.NET Core 2.x на платформе .NET Framework см. в разделе [ASP.NET Core для платформы .NET Framework](xref:index#target-framework).</span><span class="sxs-lookup"><span data-stu-id="68a6b-127">See [ASP.NET Core targeting .NET Framework](xref:index#target-framework) for information on ASP.NET Core 2.x support on .NET Framework.</span></span>

## <a name="aspnet-core-scenarios"></a><span data-ttu-id="68a6b-128">Сценарии ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68a6b-128">ASP.NET Core scenarios</span></span>

* <span data-ttu-id="68a6b-129">[Razor Pages](xref:razor-pages/index) — рекомендуемый метод создания пользовательского веб-интерфейса в ASP.NET Core 2.x.</span><span class="sxs-lookup"><span data-stu-id="68a6b-129">[Razor Pages](xref:razor-pages/index) is the recommended approach to create a Web UI as of ASP.NET Core 2.x.</span></span>
* [<span data-ttu-id="68a6b-130">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="68a6b-130">Websites</span></span>](xref:tutorials/first-mvc-app/index)
* [<span data-ttu-id="68a6b-131">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="68a6b-131">APIs</span></span>](xref:tutorials/first-web-api)
* [<span data-ttu-id="68a6b-132">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="68a6b-132">Real-time</span></span>](xref:signalr/index)
* [<span data-ttu-id="68a6b-133">Развертывание приложения ASP.NET Core в Azure</span><span class="sxs-lookup"><span data-stu-id="68a6b-133">Deploy an ASP.NET Core app to Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet)

## <a name="aspnet-4x-scenarios"></a><span data-ttu-id="68a6b-134">Сценарии ASP.NET 4.x</span><span class="sxs-lookup"><span data-stu-id="68a6b-134">ASP.NET 4.x scenarios</span></span>

* [<span data-ttu-id="68a6b-135">Веб-сайты</span><span class="sxs-lookup"><span data-stu-id="68a6b-135">Websites</span></span>](/aspnet/mvc)
* [<span data-ttu-id="68a6b-136">API-интерфейсы</span><span class="sxs-lookup"><span data-stu-id="68a6b-136">APIs</span></span>](/aspnet/web-api)
* [<span data-ttu-id="68a6b-137">Режим реального времени</span><span class="sxs-lookup"><span data-stu-id="68a6b-137">Real-time</span></span>](/aspnet/signalr)
* [<span data-ttu-id="68a6b-138">Создание веб-приложение ASP.NET 4.x в Azure</span><span class="sxs-lookup"><span data-stu-id="68a6b-138">Create an ASP.NET 4.x web app in Azure</span></span>](/azure/app-service/app-service-web-get-started-dotnet-framework)

## <a name="additional-resources"></a><span data-ttu-id="68a6b-139">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="68a6b-139">Additional resources</span></span>

* [<span data-ttu-id="68a6b-140">Введение в ASP.NET</span><span class="sxs-lookup"><span data-stu-id="68a6b-140">Introduction to ASP.NET</span></span>](/aspnet/overview)
* [<span data-ttu-id="68a6b-141">Введение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="68a6b-141">Introduction to ASP.NET Core</span></span>](xref:index)
* <xref:host-and-deploy/azure-apps/index>

> [!NOTE]
> <span data-ttu-id="68a6b-142">Мы тестируем удобство использования новой предлагаемой структуры оглавления для ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="68a6b-142">We’re testing the usability of a proposed new structure for the ASP.NET Core table of contents.</span></span>  <span data-ttu-id="68a6b-143">Если у вас есть несколько минут, и вы хотите попробовать найти семь разных тем в существующей или планируемой структуре оглавления, [щелкните здесь, чтобы принять участие в исследовании](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span><span class="sxs-lookup"><span data-stu-id="68a6b-143">If you have a few minutes to try an exercise of finding 7 different topics in the current or proposed table of contents, please [click here to participate in the study](https://dpk4xbh5.optimalworkshop.com/treejack/aa11wn82).</span></span>
