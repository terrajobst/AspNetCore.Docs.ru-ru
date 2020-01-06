---
title: SignalR ASP.NET Core узла в фоновых службах
author: bradygaster
description: Узнайте, как отправить сообщения SignalR клиентам из классов .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 11/12/2019
no-loc:
- SignalR
uid: signalr/background-services
ms.openlocfilehash: 324592759af79d1229eb147fb4551e97c678ef64
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 12/25/2019
ms.locfileid: "75358682"
---
# <a name="host-aspnet-core-opno-locsignalr-in-background-services"></a><span data-ttu-id="8353c-103">SignalR ASP.NET Core узла в фоновых службах</span><span class="sxs-lookup"><span data-stu-id="8353c-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="8353c-104">По [Брейди Гастер](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="8353c-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="8353c-105">В этой статье приводятся рекомендации по следующим вопросам.</span><span class="sxs-lookup"><span data-stu-id="8353c-105">This article provides guidance for:</span></span>

* <span data-ttu-id="8353c-106">Размещение концентраторов SignalR с помощью фонового рабочего процесса, размещенного в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8353c-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="8353c-107">Отправка сообщений на подключенные клиенты из [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).NET Core.</span><span class="sxs-lookup"><span data-stu-id="8353c-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="8353c-108">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(Загрузка)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="8353c-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="enable-opno-locsignalr-in-startup"></a><span data-ttu-id="8353c-109">Включение SignalR при запуске</span><span class="sxs-lookup"><span data-stu-id="8353c-109">Enable SignalR in startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="8353c-110">Размещение ASP.NET Core SignalR концентраторов в контексте фонового рабочего процесса идентично размещению концентратора в ASP.NET Core веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="8353c-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="8353c-111">В методе `Startup.ConfigureServices` вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR.</span><span class="sxs-lookup"><span data-stu-id="8353c-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="8353c-112">В `Startup.Configure`метод `MapHub` вызывается в обратном вызове `UseEndpoints` для подключения конечных точек концентратора в конвейере запросов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8353c-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to connect the Hub endpoints in the ASP.NET Core request pipeline.</span></span>

```csharp
public class Startup
{
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddSignalR();
        services.AddHostedService<Worker>();
    }

    public void Configure(IApplicationBuilder app, IHostingEnvironment env)
    {
        if (env.IsDevelopment())
        {
            app.UseDeveloperExceptionPage();
        }

        app.UseRouting();
        app.UseEndpoints(endpoints =>
        {
            endpoints.MapHub<ClockHub>("/hubs/clock");
        });
    }
}
```

::: moniker-end

::: moniker range="<= aspnetcore-2.2"

<span data-ttu-id="8353c-113">Размещение ASP.NET Core SignalR концентраторов в контексте фонового рабочего процесса идентично размещению концентратора в ASP.NET Core веб-приложении.</span><span class="sxs-lookup"><span data-stu-id="8353c-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="8353c-114">В методе `Startup.ConfigureServices` вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR.</span><span class="sxs-lookup"><span data-stu-id="8353c-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="8353c-115">В `Startup.Configure`вызывается метод `UseSignalR` для подключения конечных точек концентратора в конвейере запросов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="8353c-115">In `Startup.Configure`, the `UseSignalR` method is called to connect the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="8353c-116">В предыдущем примере класс `ClockHub` реализует класс `Hub<T>` для создания строго типизированного концентратора.</span><span class="sxs-lookup"><span data-stu-id="8353c-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="8353c-117">`ClockHub` был настроен в классе `Startup` для реагирования на запросы на конечной точке `/hubs/clock`.</span><span class="sxs-lookup"><span data-stu-id="8353c-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="8353c-118">Дополнительные сведения о строго типизированных концентраторах см. [в статье Использование концентраторов в SignalR для ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="8353c-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="8353c-119">Эта функциональность не ограничена классом [\<](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="8353c-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="8353c-120">Любой класс, наследующий от [концентратора](xref:Microsoft.AspNetCore.SignalR.Hub), например [динамичуб](xref:Microsoft.AspNetCore.SignalR.DynamicHub), также будет работать.</span><span class="sxs-lookup"><span data-stu-id="8353c-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="8353c-121">Интерфейс, используемый строго типизированным `ClockHub`, является интерфейсом `IClock`.</span><span class="sxs-lookup"><span data-stu-id="8353c-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-opno-locsignalr-hub-from-a-background-service"></a><span data-ttu-id="8353c-122">Вызов центра SignalR из фоновой службы</span><span class="sxs-lookup"><span data-stu-id="8353c-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="8353c-123">Во время запуска класс `Worker`, `BackgroundService`, включается с помощью `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="8353c-123">During startup, the `Worker` class, a `BackgroundService`, is enabled using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="8353c-124">Так как SignalR также включена на этапе `Startup`, в котором каждый концентратор подключен к отдельной конечной точке в конвейере HTTP-запросов ASP.NET Core, каждый концентратор представляется `IHubContext<T>` на сервере.</span><span class="sxs-lookup"><span data-stu-id="8353c-124">Since SignalR is also enabled up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="8353c-125">Используя функции DI ASP.NET Core, другие классы, созданные на уровне размещения, такие как классы `BackgroundService`, классы контроллеров MVC или модели страниц Razor, могут получать ссылки на серверные концентраторы, принимая экземпляры `IHubContext<ClockHub, IClock>` во время создания.</span><span class="sxs-lookup"><span data-stu-id="8353c-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="8353c-126">Так как метод `ExecuteAsync` вызывается итеративно в фоновой службе, текущая дата и время сервера отправляются подключенным клиентам с помощью `ClockHub`.</span><span class="sxs-lookup"><span data-stu-id="8353c-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-opno-locsignalr-events-with-background-services"></a><span data-ttu-id="8353c-127">Реагирование на события SignalR с помощью фоновых служб</span><span class="sxs-lookup"><span data-stu-id="8353c-127">React to SignalR events with background services</span></span>

<span data-ttu-id="8353c-128">Как и в одностраничном приложении, использующем клиент JavaScript для SignalR или классического приложения .NET, может выполнять с помощью <xref:signalr/dotnet-client>, `BackgroundService` или `IHostedService` можно также использовать для подключения к концентраторам SignalR и реагирования на события.</span><span class="sxs-lookup"><span data-stu-id="8353c-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="8353c-129">Класс `ClockHubClient` реализует интерфейс `IClock` и интерфейс `IHostedService`.</span><span class="sxs-lookup"><span data-stu-id="8353c-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="8353c-130">Таким образом, его можно включить во время `Startup` для непрерывного выполнения и реагирования на события концентратора с сервера.</span><span class="sxs-lookup"><span data-stu-id="8353c-130">This way it can be enabled during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="8353c-131">Во время инициализации `ClockHubClient` создает экземпляр `HubConnection` и включает метод `IClock.ShowTime` в качестве обработчика для события `ShowTime` концентратора.</span><span class="sxs-lookup"><span data-stu-id="8353c-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and enables the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="8353c-132">В реализации `IHostedService.StartAsync` `HubConnection` запускается асинхронно.</span><span class="sxs-lookup"><span data-stu-id="8353c-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="8353c-133">Во время выполнения метода `IHostedService.StopAsync` `HubConnection` удаляется асинхронно.</span><span class="sxs-lookup"><span data-stu-id="8353c-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="8353c-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="8353c-134">Additional resources</span></span>

* [<span data-ttu-id="8353c-135">Начало работы</span><span class="sxs-lookup"><span data-stu-id="8353c-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="8353c-136">Центры</span><span class="sxs-lookup"><span data-stu-id="8353c-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="8353c-137">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="8353c-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="8353c-138">Строго типизированные концентраторы</span><span class="sxs-lookup"><span data-stu-id="8353c-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
