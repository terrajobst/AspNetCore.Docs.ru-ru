---
title: Размещение и развертывание Blazor на стороне сервера
author: guardrex
description: Узнайте, как выполнять размещение и развертывание приложения Blazor на стороне сервера с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614756"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="2dade-103">Размещение и развертывание Blazor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="2dade-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="2dade-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="2dade-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="2dade-105">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="2dade-105">Host configuration values</span></span>

<span data-ttu-id="2dade-106">Серверные приложения, использующие [модель размещения на сервере](xref:blazor/hosting-models#server-side-hosting-model), могут принимать [значения конфигурации универсального узла](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="2dade-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="2dade-107">Развертывание</span><span class="sxs-lookup"><span data-stu-id="2dade-107">Deployment</span></span>

<span data-ttu-id="2dade-108">При использовании [модели размещения на сервере](xref:blazor/hosting-models#server-side-hosting-model) Blazor работает на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="2dade-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side-hosting-model), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="2dade-109">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="2dade-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="2dade-110">Приложение входит в состав приложения ASP.NET Core в публикуемых выходных данных; следовательно, два приложения развертываются вместе.</span><span class="sxs-lookup"><span data-stu-id="2dade-110">The app is included with the ASP.NET Core app in the published output, and the two apps are deployed together.</span></span> <span data-ttu-id="2dade-111">Веб-сервер, позволяющий размещать приложения ASP.NET Core, является обязательным.</span><span class="sxs-lookup"><span data-stu-id="2dade-111">A web server that's capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="2dade-112">Для развертывания на стороне сервера Visual Studio включает шаблон проекта **Razor Components** (шаблон `razorcomponents` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="2dade-112">For a server-side deployment, Visual Studio includes the **Razor Components** project template (`razorcomponents` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

<span data-ttu-id="2dade-113">Дополнительные сведения о размещении приложения ASP.NET Core и его развертывании см. в разделе <xref:host-and-deploy/index>.</span><span class="sxs-lookup"><span data-stu-id="2dade-113">For more information on ASP.NET Core app hosting and deployment, see <xref:host-and-deploy/index>.</span></span>

<span data-ttu-id="2dade-114">Подробнее о развертывании в службе приложений Azure см. в <xref:tutorials/publish-to-azure-webapp-using-vs>.</span><span class="sxs-lookup"><span data-stu-id="2dade-114">For information on deploying to Azure App Service, see <xref:tutorials/publish-to-azure-webapp-using-vs>.</span></span>
