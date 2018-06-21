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
ms.openlocfilehash: 7f8ccff7e3da93d6e617505ac93fafc3a82ed880
ms.sourcegitcommit: 63fb07fb3f71b32daf2c9466e132f2e7cc617163
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/10/2018
ms.locfileid: "35252013"
---
# <a name="host-in-aspnet-core"></a><span data-ttu-id="72932-103">Размещение в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="72932-103">Host in ASP.NET Core</span></span>

<span data-ttu-id="72932-104">Приложения .NET настраивают и запускают *узел*.</span><span class="sxs-lookup"><span data-stu-id="72932-104">.NET apps configure and launch a *host*.</span></span> <span data-ttu-id="72932-105">Узел отвечает за запуск приложения и управление временем существования.</span><span class="sxs-lookup"><span data-stu-id="72932-105">The host is responsible for app startup and lifetime management.</span></span> <span data-ttu-id="72932-106">Для использования доступны два API узла:</span><span class="sxs-lookup"><span data-stu-id="72932-106">Two host APIs are available for use:</span></span>

* <span data-ttu-id="72932-107">[Веб-узел](xref:fundamentals/host/web-host) &ndash; подходит для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="72932-107">[Web Host](xref:fundamentals/host/web-host) &ndash; Suitable for hosting web apps.</span></span>
* <span data-ttu-id="72932-108">[Универсальный узел](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 или более поздней версии) &ndash; подходит для размещения других приложений (например, приложений, которые выполняют фоновые задачи).</span><span class="sxs-lookup"><span data-stu-id="72932-108">[Generic Host](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 or later) &ndash; Suitable for hosting non-web apps (for example, apps that run background tasks).</span></span> <span data-ttu-id="72932-109">В следующем выпуске универсальный узел позволит размещать любые приложения, включая веб-приложения.</span><span class="sxs-lookup"><span data-stu-id="72932-109">In a future release, the Generic Host will be suitable for hosting any kind of app, including web apps.</span></span> <span data-ttu-id="72932-110">Со временем универсальный узел заменит веб-узел.</span><span class="sxs-lookup"><span data-stu-id="72932-110">The Generic Host will eventually replace the Web Host.</span></span>

<span data-ttu-id="72932-111">Для размещения *веб-приложений* ASP.NET Core разработчикам следует использовать веб-узел на основе [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span><span class="sxs-lookup"><span data-stu-id="72932-111">For hosting ASP.NET Core *web apps*, developers should use the Web Host based on [IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder).</span></span> <span data-ttu-id="72932-112">Если это *не веб-приложение*, для его размещения разработчикам следует использовать универсальный узел на основе [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span><span class="sxs-lookup"><span data-stu-id="72932-112">For hosting *non-web apps*, developers should use the Generic Host based on [HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder).</span></span>
