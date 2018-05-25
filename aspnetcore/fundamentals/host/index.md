---
title: Размещение в ASP.NET Core
author: guardrex
description: Сведения о веб-узле ASP.NET Core и универсальном узле .NET, которые отвечают за запуск приложений и управление временем существования.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="3f2db-103">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="3f2db-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="3f2db-104">Приложения .NET настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="3f2db-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="3f2db-105">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="3f2db-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="3f2db-106">Для использования доступны два API узла:</span><span class="sxs-lookup"><span data-stu-id="3f2db-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="3f2db-107">[Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="3f2db-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="3f2db-108">[Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи).</span><span class="sxs-lookup"><span data-stu-id="3f2db-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="3f2db-109">В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="3f2db-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="3f2db-110">Со временем универсальный узел заменит веб-узел.</span><span class="sxs-lookup"><span data-stu-id="3f2db-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="3f2db-111">Сейчас разработчикам следует использовать [веб-узел](xref:fundamentals/host/web-host) на основе [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) для размещения приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="3f2db-111">At this time, developers should use the [Web Host](xref:fundamentals/host/web-host) based on [WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) for hosting ASP.NET Core apps.</span></span>
