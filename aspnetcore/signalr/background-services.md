---
title: Узел ASP.NET Core SignalR в фоновых служб
author: bradygaster
description: Узнайте, как для отправки сообщений SignalR клиентам из классов .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: b359bd7f6b0667aeb8d9c8f5eb450637b1347b19
ms.sourcegitcommit: e418cb9cddeb3de06fa0cb4fdb5529da03ff6d63
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/05/2019
ms.locfileid: "55739674"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="7d52d-103">Узел ASP.NET Core SignalR в фоновых служб</span><span class="sxs-lookup"><span data-stu-id="7d52d-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="7d52d-104">По [Brady Gaster](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="7d52d-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="7d52d-105">Эта статья содержит рекомендации для:</span><span class="sxs-lookup"><span data-stu-id="7d52d-105">This article provides guidance for:</span></span>

* <span data-ttu-id="7d52d-106">Размещение концентраторы SignalR с помощью фона рабочего процесса, размещенного с помощью ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d52d-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="7d52d-107">Отправка сообщений подключенных клиентов из в .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span><span class="sxs-lookup"><span data-stu-id="7d52d-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="7d52d-108">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(способ загрузки)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="7d52d-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="7d52d-109">Подключения SignalR во время запуска</span><span class="sxs-lookup"><span data-stu-id="7d52d-109">Wire up SignalR during startup</span></span>

<span data-ttu-id="7d52d-110">Размещение ASP.NET Core SignalR концентраторов в контексте фоновый рабочий процесс идентичен размещение концентратора в веб-приложение ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d52d-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="7d52d-111">В `Startup.ConfigureServices` метода, вызывая метод `services.AddSignalR` добавляет необходимые службы на уровень внедрения зависимостей ASP.NET Core (DI) для поддержки SignalR.</span><span class="sxs-lookup"><span data-stu-id="7d52d-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="7d52d-112">В `Startup.Configure`, `UseSignalR` вызывается метод пишем конечные концентратора в конвейере запросов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="7d52d-112">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

<span data-ttu-id="7d52d-113">В приведенном выше примере `ClockHub` класс реализует `Hub<T>` класс для создания строго типизированных центра.</span><span class="sxs-lookup"><span data-stu-id="7d52d-113">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="7d52d-114">`ClockHub` Был настроен в `Startup` класс отвечать на запросы в конечной точке `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="7d52d-114">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="7d52d-115">Дополнительные сведения о строго типизированных концентраторах см. в разделе [используют концентраторы в SignalR для ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="7d52d-115">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="7d52d-116">Эта функция не ограничивается [концентратора\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1) класса.</span><span class="sxs-lookup"><span data-stu-id="7d52d-116">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="7d52d-117">Любой класс, наследуемый от [концентратора](xref:Microsoft.AspNetCore.SignalR.Hub), такие как [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), также будет работать.</span><span class="sxs-lookup"><span data-stu-id="7d52d-117">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="7d52d-118">Интерфейс, через строго типизированные `ClockHub` является `IClock` интерфейс.</span><span class="sxs-lookup"><span data-stu-id="7d52d-118">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="7d52d-119">Вызов из фоновую службу концентратора SignalR</span><span class="sxs-lookup"><span data-stu-id="7d52d-119">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="7d52d-120">Во время запуска `Worker` класс, `BackgroundService`, построенная с помощью `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="7d52d-120">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="7d52d-121">Поскольку SignalR также уже будет устроено во время `Startup` этап, в которой каждый центр будет подключен к отдельной конечной точки в ASP.NET Core конвейер HTTP-запросов, представлены в каждом центре `IHubContext<T>` на сервере.</span><span class="sxs-lookup"><span data-stu-id="7d52d-121">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="7d52d-122">С помощью ASP.NET Core DI функции, другие классы, создается путем размещения слоя, например `BackgroundService` классы, классы контроллера MVC или модели страницы Razor, можно получить ссылки для концентраторов на стороне сервера, принимая экземпляров `IHubContext<ClockHub, IClock>` во время построения.</span><span class="sxs-lookup"><span data-stu-id="7d52d-122">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="7d52d-123">Как `ExecuteAsync` метод вызывается многократно в фоновой службы, текущую дату и время сервера отправляются к подключенным клиентам, использующим `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="7d52d-123">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="7d52d-124">Реагирование на события SignalR в работе фоновых служб</span><span class="sxs-lookup"><span data-stu-id="7d52d-124">React to SignalR events with background services</span></span>

<span data-ttu-id="7d52d-125">Как одностраничное приложение с помощью клиента JavaScript для SignalR или классическое приложение .NET можно сделать с помощью <xref:signalr/dotnet-client>, `BackgroundService` или `IHostedService` реализации может также использоваться для подключения к концентраторов SignalR и реагировать на события.</span><span class="sxs-lookup"><span data-stu-id="7d52d-125">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="7d52d-126">`ClockHubClient` Класс реализует оба `IClock` интерфейс и `IHostedService` интерфейс.</span><span class="sxs-lookup"><span data-stu-id="7d52d-126">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="7d52d-127">Таким образом, его можно регистрировать во время `Startup` в непрерывном режиме и реагировать на события концентратора с сервера.</span><span class="sxs-lookup"><span data-stu-id="7d52d-127">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span> 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="7d52d-128">Во время инициализации `ClockHubClient` создает экземпляр класса `HubConnection` и настраивающий `IClock.ShowTime` метода как обработчика для центра `ShowTime` событий.</span><span class="sxs-lookup"><span data-stu-id="7d52d-128">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="7d52d-129">В `IHostedService.StartAsync` реализации `HubConnection` запускается в асинхронном режиме.</span><span class="sxs-lookup"><span data-stu-id="7d52d-129">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="7d52d-130">Во время `IHostedService.StopAsync` метода `HubConnection` удаляется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="7d52d-130">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="7d52d-131">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="7d52d-131">Additional resources</span></span>

* [<span data-ttu-id="7d52d-132">Начало работы</span><span class="sxs-lookup"><span data-stu-id="7d52d-132">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="7d52d-133">Центры</span><span class="sxs-lookup"><span data-stu-id="7d52d-133">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="7d52d-134">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="7d52d-134">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="7d52d-135">Строго типизированные концентраторов</span><span class="sxs-lookup"><span data-stu-id="7d52d-135">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
