---
title: Размещение и развертывание сервера Blazor с помощью ASP.NET Core
author: guardrex
description: Сведения о размещении и развертывании серверного приложения Blazor с помощью ASP.NET Core.
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/05/2019
uid: host-and-deploy/blazor/server
ms.openlocfilehash: 693d7ff67bad3a0c5bd050b795833763056ed511
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72378814"
---
# <a name="host-and-deploy-blazor-server"></a><span data-ttu-id="96483-103">Размещение и развертывание сервера Blazor</span><span class="sxs-lookup"><span data-stu-id="96483-103">Host and deploy Blazor Server</span></span>

<span data-ttu-id="96483-104">Авторы: [Люк Лэтем](https://github.com/guardrex), [Рэйнер Стропек](https://www.timecockpit.com) и [Дэниэл Рот](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="96483-104">By [Luke Latham](https://github.com/guardrex), [Rainer Stropek](https://www.timecockpit.com), and [Daniel Roth](https://github.com/danroth27)</span></span>

## <a name="host-configuration-values"></a><span data-ttu-id="96483-105">Значения конфигурации узла</span><span class="sxs-lookup"><span data-stu-id="96483-105">Host configuration values</span></span>

<span data-ttu-id="96483-106">[Серверные приложения Blazor](xref:blazor/hosting-models#blazor-server) могут принимать [значения конфигурации универсального узла](xref:fundamentals/host/generic-host#host-configuration).</span><span class="sxs-lookup"><span data-stu-id="96483-106">[Blazor Server apps](xref:blazor/hosting-models#blazor-server) can accept [Generic Host configuration values](xref:fundamentals/host/generic-host#host-configuration).</span></span>

## <a name="deployment"></a><span data-ttu-id="96483-107">Развертывание</span><span class="sxs-lookup"><span data-stu-id="96483-107">Deployment</span></span>

<span data-ttu-id="96483-108">Если используется [модель размещения на сервере Blazor](xref:blazor/hosting-models#blazor-server), Blazor выполняется на сервере из приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96483-108">Using the [Blazor Server hosting model](xref:blazor/hosting-models#blazor-server), Blazor is executed on the server from within an ASP.NET Core app.</span></span> <span data-ttu-id="96483-109">Обновление элементов пользовательского интерфейса, обработка событий и вызовы JavaScript обрабатываются через подключение [SignalR](xref:signalr/introduction).</span><span class="sxs-lookup"><span data-stu-id="96483-109">UI updates, event handling, and JavaScript calls are handled over a [SignalR](xref:signalr/introduction) connection.</span></span>

<span data-ttu-id="96483-110">Необходим веб-сервер, позволяющий размещать приложения ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="96483-110">A web server capable of hosting an ASP.NET Core app is required.</span></span> <span data-ttu-id="96483-111">Visual Studio содержит шаблон проекта **Серверное приложение Blazor** (шаблон `blazorserverside` при использовании команды [dotnet new](/dotnet/core/tools/dotnet-new)).</span><span class="sxs-lookup"><span data-stu-id="96483-111">Visual Studio includes the **Blazor Server App** project template (`blazorserverside` template when using the [dotnet new](/dotnet/core/tools/dotnet-new) command).</span></span>

## <a name="scalability"></a><span data-ttu-id="96483-112">Масштабируемость</span><span class="sxs-lookup"><span data-stu-id="96483-112">Scalability</span></span>

<span data-ttu-id="96483-113">Планируйте развертывание так, чтобы доступная инфраструктура максимально эффективно использовалась серверным приложением Blazor.</span><span class="sxs-lookup"><span data-stu-id="96483-113">Plan a deployment to make the best use of the available infrastructure for a Blazor Server app.</span></span> <span data-ttu-id="96483-114">Вопросы, связанные с масштабируемостью серверных приложений Blazor, рассматриваются в следующих статьях:</span><span class="sxs-lookup"><span data-stu-id="96483-114">See the following resources to address Blazor Server app scalability:</span></span>

* [<span data-ttu-id="96483-115">Основные сведения о серверных приложениях Blazor.</span><span class="sxs-lookup"><span data-stu-id="96483-115">Fundamentals of Blazor Server apps</span></span>](xref:blazor/hosting-models#blazor-server)
* <xref:security/blazor/server>

### <a name="deployment-server"></a><span data-ttu-id="96483-116">Сервер развертывания</span><span class="sxs-lookup"><span data-stu-id="96483-116">Deployment server</span></span>

<span data-ttu-id="96483-117">В контексте масштабируемости отдельного сервера (вертикальное масштабирование) доступная приложению память с большой вероятностью будет первым ресурсом, расходуемым приложением при изменении потребностей пользователей.</span><span class="sxs-lookup"><span data-stu-id="96483-117">When considering the scalability of a single server (scale up), the memory available to an app is likely the first resource that the app will exhaust as user demands increase.</span></span> <span data-ttu-id="96483-118">Объем доступной памяти на сервере влияет на:</span><span class="sxs-lookup"><span data-stu-id="96483-118">The available memory on the server affects the:</span></span>

* <span data-ttu-id="96483-119">число активных каналов, которые может поддерживать сервер;</span><span class="sxs-lookup"><span data-stu-id="96483-119">Number of active circuits that a server can support.</span></span>
* <span data-ttu-id="96483-120">задержку пользовательского интерфейса в клиенте.</span><span class="sxs-lookup"><span data-stu-id="96483-120">UI latency on the client.</span></span>

<span data-ttu-id="96483-121">Рекомендации по созданию безопасных и масштабируемых серверных приложений Blazor см. здесь: <xref:security/blazor/server>.</span><span class="sxs-lookup"><span data-stu-id="96483-121">For guidance on building secure and scalable Blazor server apps, see <xref:security/blazor/server>.</span></span>

<span data-ttu-id="96483-122">Каждый канал использует около 250 КБ памяти для минималистичного приложения в стиле *Hello World*.</span><span class="sxs-lookup"><span data-stu-id="96483-122">Each circuit uses approximately 250 KB of memory for a minimal *Hello World*-style app.</span></span> <span data-ttu-id="96483-123">Размер канала зависит от кода приложения и требований к обслуживанию состояний для каждого компонента.</span><span class="sxs-lookup"><span data-stu-id="96483-123">The size of a circuit depends on the app's code and the state maintenance requirements associated with each component.</span></span> <span data-ttu-id="96483-124">Мы рекомендуем определять требования к ресурсам во время разработки приложения и инфраструктуры, но при планировании цели развертывания можно использовать следующие базовые показатели: если вы предполагаете, что приложение будет одновременно поддерживать 5000 пользователей, выделите для приложения по меньшей мере 1,3 ГБ серверной памяти (или около 273 КБ на пользователя).</span><span class="sxs-lookup"><span data-stu-id="96483-124">We recommend that you measure resource demands during development for your app and infrastructure, but the following baseline can be a starting point in planning your deployment target: If you expect your app to support 5,000 concurrent users, consider budgeting at least 1.3 GB of server memory to the app (or ~273 KB per user).</span></span>

### <a name="signalr-configuration"></a><span data-ttu-id="96483-125">Конфигурация SignalR</span><span class="sxs-lookup"><span data-stu-id="96483-125">SignalR configuration</span></span>

<span data-ttu-id="96483-126">Серверные приложения Blazor используют службу SignalR в ASP.NET Core для обмена данными с браузером.</span><span class="sxs-lookup"><span data-stu-id="96483-126">Blazor Server apps use ASP.NET Core SignalR to communicate with the browser.</span></span> <span data-ttu-id="96483-127">[Условия размещения и масштабирования SignalR](xref:signalr/publish-to-azure-web-app) применяются и к серверным приложениям Blazor.</span><span class="sxs-lookup"><span data-stu-id="96483-127">[SignalR's hosting and scaling conditions](xref:signalr/publish-to-azure-web-app) apply to Blazor Server apps.</span></span>

<span data-ttu-id="96483-128">Blazor лучше всего работает с WebSocket в качестве транспортного механизма SignalR благодаря низкой задержке, надежности и [безопасности](xref:signalr/security).</span><span class="sxs-lookup"><span data-stu-id="96483-128">Blazor works best when using WebSockets as the SignalR transport due to lower latency, reliability, and [security](xref:signalr/security).</span></span> <span data-ttu-id="96483-129">Продолжительный опрос используется службой SignalR, если подключения WebSocket недоступны или если приложение явно настроено на применение продолжительного опроса.</span><span class="sxs-lookup"><span data-stu-id="96483-129">Long Polling is used by SignalR when WebSockets isn't available or when the app is explicitly configured to use Long Polling.</span></span> <span data-ttu-id="96483-130">При развертывании Службы приложений Azure настройте приложение на использование WebSocket в параметрах службы на портале Azure.</span><span class="sxs-lookup"><span data-stu-id="96483-130">When deploying to Azure App Service, configure the app to use WebSockets in the Azure portal settings for the service.</span></span> <span data-ttu-id="96483-131">Подробные сведения о настройке приложения для Службы приложений Azure см. в статье [Publish an ASP.NET Core SignalR app to Azure App Service](xref:signalr/publish-to-azure-web-app) (Публикация приложения SignalR для ASP.NET Core в Службе приложений Azure).</span><span class="sxs-lookup"><span data-stu-id="96483-131">For details on configuring the app for Azure App Service, see the [SignalR publishing guidelines](xref:signalr/publish-to-azure-web-app).</span></span>

<span data-ttu-id="96483-132">Мы рекомендуем использовать для серверных приложений Blazor [службу Azure SignalR](/azure/azure-signalr).</span><span class="sxs-lookup"><span data-stu-id="96483-132">We recommend using the [Azure SignalR Service](/azure/azure-signalr) for Blazor Server apps.</span></span> <span data-ttu-id="96483-133">Она позволяет вертикально масштабировать серверные приложения Blazor для одновременного использования большого числа подключений SignalR.</span><span class="sxs-lookup"><span data-stu-id="96483-133">The service allows for scaling up a Blazor Server app to a large number of concurrent SignalR connections.</span></span> <span data-ttu-id="96483-134">Кроме того, глобальный охват службы SignalR и ее высокопроизводительные центры обработки данных обеспечивают низкую задержку благодаря доступности в разных регионах.</span><span class="sxs-lookup"><span data-stu-id="96483-134">In addition, the SignalR service's global reach and high-performance data centers significantly aid in reducing latency due to geography.</span></span> <span data-ttu-id="96483-135">Чтобы настроить приложение (и при необходимости подготовить к работе) для службы Azure SignalR, сделайте следующее:</span><span class="sxs-lookup"><span data-stu-id="96483-135">To configure an app (and optionally provision) the Azure SignalR Service:</span></span>

* <span data-ttu-id="96483-136">Создайте профиль публикации приложений Azure Apps в Visual Studio для серверного приложения Blazor.</span><span class="sxs-lookup"><span data-stu-id="96483-136">Create an Azure Apps publish profile in Visual Studio for the Blazor Server app.</span></span>
* <span data-ttu-id="96483-137">Добавьте в профиль зависимость **службы Azure SignalR**.</span><span class="sxs-lookup"><span data-stu-id="96483-137">Add the **Azure SignalR Service** dependency to the profile.</span></span> <span data-ttu-id="96483-138">Если подписка Azure не имеет уже существующего экземпляра службы Azure SignalR для назначения приложению, выберите **Создать новый экземпляр службы Azure SignalR** для предоставления нового экземпляра службы.</span><span class="sxs-lookup"><span data-stu-id="96483-138">If the Azure subscription doesn't have a pre-existing Azure SignalR Service instance to assign to the app, select **Create a new Azure SignalR Service instance** to provision a new service instance.</span></span>
* <span data-ttu-id="96483-139">Публикация приложения в Azure.</span><span class="sxs-lookup"><span data-stu-id="96483-139">Publish the app to Azure.</span></span>

### <a name="measure-network-latency"></a><span data-ttu-id="96483-140">Измерение задержки сети</span><span class="sxs-lookup"><span data-stu-id="96483-140">Measure network latency</span></span>

<span data-ttu-id="96483-141">Для оценки задержки сети можно использовать [средства взаимодействия JavaScript](xref:blazor/javascript-interop), как показано в следующем примере:</span><span class="sxs-lookup"><span data-stu-id="96483-141">[JS interop](xref:blazor/javascript-interop) can be used to measure network latency, as the following example demonstrates:</span></span>

```cshtml
@inject IJSRuntime JS

@if (latency is null)
{
    <span>Calculating...</span>
}
else
{
    <span>@(latency.Value.TotalMilliseconds)ms</span>
}

@code
{
    private DateTime startTime;
    private TimeSpan? latency;

    protected override async Task OnInitializedAsync()
    {
        startTime = DateTime.UtcNow;
        var _ = await JS.InvokeAsync<string>("toString");
        latency = DateTime.UtcNow - startTime;
    }
}
```

<span data-ttu-id="96483-142">Для удобной работы с пользовательским интерфейсом мы рекомендуем обеспечить стабильную задержку не более 250 мс.</span><span class="sxs-lookup"><span data-stu-id="96483-142">For a reasonable UI experience, we recommend a sustained UI latency of 250ms or less.</span></span>
