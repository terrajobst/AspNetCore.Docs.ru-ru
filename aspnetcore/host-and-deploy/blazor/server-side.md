---
title: Размещение и развертывание приложений Blazor на стороне сервера с помощью ASP.NET Core
author: guardrex
description: Узнайте, как выполнять размещение и развертывание приложения Blazor на стороне сервера с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/11/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 8b332c2fb439e9832d604ed26f972b266eed2507
ms.sourcegitcommit: 9bb29f9ba6f0645ee8b9cabda07e3a5aa52cd659
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/26/2019
ms.locfileid: "67406124"
---
# <a name="host-and-deploy-blazor-server-side"></a><span data-ttu-id="f8546-103">Размещение и развертывание Blazor на стороне сервера</span><span class="sxs-lookup"><span data-stu-id="f8546-103">Host and deploy Blazor server-side</span></span>

<span data-ttu-id="f8546-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="f8546-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="f8546-105">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="f8546-105">Host configuration values</span></span>

<span data-ttu-id="f8546-106">Серверные приложения, использующие [модель размещения на сервере](xref:blazor/hosting-models#server-side), могут принимать [значения конфигурации универсального узла](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="f8546-106">Server-side apps that use the [server-side hosting model](xref:blazor/hosting-models#server-side) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="f8546-107">Развертывание</span><span class="sxs-lookup"><span data-stu-id="f8546-107">Deployment</span></span>

<span data-ttu-id="f8546-108">При использовании [модели размещения на сервере](xref:blazor/hosting-models#server-side) Blazor работает на сервере в приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8546-108">With the [server-side hosting model](xref:blazor/hosting-models#server-side), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="f8546-109">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="f8546-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="f8546-110">Необходим веб-сервер, позволяющий размещать приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="f8546-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="f8546-111">Visual Studio содержит шаблон проекта **Blazor (на стороне сервера)** (шаблон `blazorserverside` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="f8546-111">Visual Studio includes the **Blazor (server-side)** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="connection-scale-out"></a><span data-ttu-id="f8546-112">Горизонтальное масштабирование подключений</span><span class="sxs-lookup"><span data-stu-id="f8546-112">Connection scale out</span></span>

<span data-ttu-id="f8546-113">Приложения Blazor, работающие на стороне сервера, требуют наличия одного активного подключения SignalR для каждого пользователя.</span><span class="sxs-lookup"><span data-stu-id="f8546-113">Blazor server-side apps require one active SignalR connection for each user.</span></span> <span data-ttu-id="f8546-114">В Рабочем развертывании Blazor на стороне сервера необходимо реализовать решение для поддержки нужного приложению числа одновременных подключений.</span><span class="sxs-lookup"><span data-stu-id="f8546-114">A production Blazor server-side deployment requires a solution for supporting as many concurrent connections as required by the app.</span></span> <span data-ttu-id="f8546-115">[Служба Azure SignalR](/azure/azure-signalr/) управляет масштабированием подключений и является рекомендуемым решением для масштабирования приложений Blazor, работающих на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="f8546-115">The [Azure SignalR Service](/azure/azure-signalr/) handles the scaling of connections and is recommended as a scaling solution for Blazor server-side apps.</span></span> <span data-ttu-id="f8546-116">Дополнительные сведения можно найти по адресу: <xref:signalr/publish-to-azure-web-app>.</span><span class="sxs-lookup"><span data-stu-id="f8546-116">For more information, see <xref:signalr/publish-to-azure-web-app>.</span></span>

## <a name="signalr-configuration"></a><span data-ttu-id="f8546-117">Конфигурация SignalR</span><span class="sxs-lookup"><span data-stu-id="f8546-117">SignalR configuration</span></span>

<span data-ttu-id="f8546-118">Служба SignalR настраивается ASP.NET Core для самых распространенных сценариев приложений Blazor на стороне сервера.</span><span class="sxs-lookup"><span data-stu-id="f8546-118">SignalR is configured by ASP.NET Core for the most common Blazor server-side scenarios.</span></span> <span data-ttu-id="f8546-119">Подробные сведения о пользовательских и расширенных сценариях можно найти в статьях о SignalR в разделе [Дополнительные ресурсы](#additional-resources).</span><span class="sxs-lookup"><span data-stu-id="f8546-119">For custom and advanced scenarios, consult the SignalR articles in the [Additional resources](#additional-resources) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f8546-120">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="f8546-120">Additional resources</span></span>

* <xref:signalr/introduction>
* [<span data-ttu-id="f8546-121">Документация по службе Azure SignalR</span><span class="sxs-lookup"><span data-stu-id="f8546-121">Azure SignalR Service Documentation</span></span>](/azure/azure-signalr/)
* [<span data-ttu-id="f8546-122">Краткое руководство. Создание чата с помощью службы SignalR</span><span class="sxs-lookup"><span data-stu-id="f8546-122">Quickstart: Create a chat room by using SignalR Service</span></span>](/azure/azure-signalr/signalr-quickstart-dotnet-core)
* <xref:host-and-deploy/index>
* <xref:tutorials/publish-to-azure-webapp-using-vs>
* [<span data-ttu-id="f8546-123">Развертывание предварительной версии ASP.NET Core в службе приложений Azure</span><span class="sxs-lookup"><span data-stu-id="f8546-123">Deploy ASP.NET Core preview release to Azure App Service</span></span>](xref:host-and-deploy/azure-apps/index#deploy-aspnet-core-preview-release-to-azure-app-service)
