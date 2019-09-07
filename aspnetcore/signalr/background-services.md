---
title: Ведущие ASP.NET Core SignalR в фоновых службах
author: bradygaster
description: Узнайте, как отправить сообщения клиентам SignalR из классов .NET Core BackgroundService.
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: 23a53f33a03ce3b76cf6846c3f214a6cad055209
ms.sourcegitcommit: f65d8765e4b7c894481db9b37aa6969abc625a48
ms.translationtype: MT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/06/2019
ms.locfileid: "70773938"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a><span data-ttu-id="da8ec-103">Ведущие ASP.NET Core SignalR в фоновых службах</span><span class="sxs-lookup"><span data-stu-id="da8ec-103">Host ASP.NET Core SignalR in background services</span></span>

<span data-ttu-id="da8ec-104">По [Брейди Гастер](https://twitter.com/bradygaster)</span><span class="sxs-lookup"><span data-stu-id="da8ec-104">By [Brady Gaster](https://twitter.com/bradygaster)</span></span>

<span data-ttu-id="da8ec-105">В этой статье приводятся рекомендации по следующим вопросам.</span><span class="sxs-lookup"><span data-stu-id="da8ec-105">This article provides guidance for:</span></span>

* <span data-ttu-id="da8ec-106">Размещение концентраторов SignalR с помощью фонового рабочего процесса, размещенного в ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da8ec-106">Hosting SignalR Hubs using a background worker process hosted with ASP.NET Core.</span></span>
* <span data-ttu-id="da8ec-107">Отправка сообщений на подключенные клиенты из [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).NET Core.</span><span class="sxs-lookup"><span data-stu-id="da8ec-107">Sending messages to connected clients from within a .NET Core [BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService).</span></span>

<span data-ttu-id="da8ec-108">[Просмотр или скачивание образца кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(как скачать)](xref:index#how-to-download-a-sample)</span><span class="sxs-lookup"><span data-stu-id="da8ec-108">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(how to download)](xref:index#how-to-download-a-sample)</span></span>

## <a name="wire-up-signalr-during-startup"></a><span data-ttu-id="da8ec-109">Подсоединить SignalR во время запуска</span><span class="sxs-lookup"><span data-stu-id="da8ec-109">Wire up SignalR during startup</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="da8ec-110">Размещение концентраторов SignalR ASP.NET Core в контексте фонового рабочего процесса идентично размещению центра в веб-приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da8ec-110">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="da8ec-111">В методе вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="da8ec-111">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="da8ec-112">В `Startup.Configure` методвызывается`MapHub` в обратномвызове,чтобыподключитьконечныеточкиконцентраторавконвейерезапросовASP.NETCore.`UseEndpoints`</span><span class="sxs-lookup"><span data-stu-id="da8ec-112">In `Startup.Configure`, the `MapHub` method is called in the `UseEndpoints` callback to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

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

<span data-ttu-id="da8ec-113">Размещение концентраторов SignalR ASP.NET Core в контексте фонового рабочего процесса идентично размещению центра в веб-приложении ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da8ec-113">Hosting ASP.NET Core SignalR Hubs in the context of a background worker process is identical to hosting a Hub in an ASP.NET Core web app.</span></span> <span data-ttu-id="da8ec-114">В методе вызов `services.AddSignalR` добавляет необходимые службы к слою ASP.NET Core внедрения зависимостей (DI) для поддержки SignalR. `Startup.ConfigureServices`</span><span class="sxs-lookup"><span data-stu-id="da8ec-114">In the `Startup.ConfigureServices` method, calling `services.AddSignalR` adds the required services to the ASP.NET Core Dependency Injection (DI) layer to support SignalR.</span></span> <span data-ttu-id="da8ec-115">В `Startup.Configure` `UseSignalR` метод вызывается, чтобы подключить конечные точки концентратора в конвейере запросов ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="da8ec-115">In `Startup.Configure`, the `UseSignalR` method is called to wire up the Hub endpoint(s) in the ASP.NET Core request pipeline.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

::: moniker-end

<span data-ttu-id="da8ec-116">В предыдущем примере `ClockHub` класс `Hub<T>` реализует класс для создания строго типизированного концентратора.</span><span class="sxs-lookup"><span data-stu-id="da8ec-116">In the preceding example, the `ClockHub` class implements the `Hub<T>` class to create a strongly typed Hub.</span></span> <span data-ttu-id="da8ec-117">Объект `ClockHub` настроен `/hubs/clock`в классе для реагирования на запросы в конечной точке. `Startup`</span><span class="sxs-lookup"><span data-stu-id="da8ec-117">The `ClockHub` has been configured in the `Startup` class to respond to requests at the endpoint `/hubs/clock`.</span></span>

<span data-ttu-id="da8ec-118">Дополнительные сведения о строго типизированных концентраторах см. в статье [Использование концентраторов в SignalR для ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span><span class="sxs-lookup"><span data-stu-id="da8ec-118">For more information on strongly typed Hubs, see [Use hubs in SignalR for ASP.NET Core](xref:signalr/hubs#strongly-typed-hubs).</span></span>

> [!NOTE]
> <span data-ttu-id="da8ec-119">Эта функция не ограничена классом [> концентратора\<](xref:Microsoft.AspNetCore.SignalR.Hub`1) .</span><span class="sxs-lookup"><span data-stu-id="da8ec-119">This functionality isn't limited to the [Hub\<T>](xref:Microsoft.AspNetCore.SignalR.Hub`1) class.</span></span> <span data-ttu-id="da8ec-120">Любой класс, наследующий от [концентратора](xref:Microsoft.AspNetCore.SignalR.Hub), например [динамичуб](xref:Microsoft.AspNetCore.SignalR.DynamicHub), также будет работать.</span><span class="sxs-lookup"><span data-stu-id="da8ec-120">Any class that inherits from [Hub](xref:Microsoft.AspNetCore.SignalR.Hub), such as [DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub), will also work.</span></span>

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

<span data-ttu-id="da8ec-121">Интерфейс, используемый строго типизированным `ClockHub` объектом, `IClock` является интерфейсом.</span><span class="sxs-lookup"><span data-stu-id="da8ec-121">The interface used by the strongly typed `ClockHub` is the `IClock` interface.</span></span>

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a><span data-ttu-id="da8ec-122">Вызов концентратора SignalR из фоновой службы</span><span class="sxs-lookup"><span data-stu-id="da8ec-122">Call a SignalR Hub from a background service</span></span>

<span data-ttu-id="da8ec-123">Во время запуска `Worker` класс `BackgroundService`() поддается с помощью `AddHostedService`.</span><span class="sxs-lookup"><span data-stu-id="da8ec-123">During startup, the `Worker` class, a `BackgroundService`, is wired up using `AddHostedService`.</span></span>

```csharp
services.AddHostedService<Worker>();
```

<span data-ttu-id="da8ec-124">Так как SignalR также `Startup` работает на этапе, в котором каждый концентратор подключен к отдельной конечной точке в конвейере HTTP-запросов ASP.NET Core, каждый концентратор `IHubContext<T>` представляется на сервере.</span><span class="sxs-lookup"><span data-stu-id="da8ec-124">Since SignalR is also wired up during the `Startup` phase, in which each Hub is attached to an individual endpoint in ASP.NET Core's HTTP request pipeline, each Hub is represented by an `IHubContext<T>` on the server.</span></span> <span data-ttu-id="da8ec-125">Используя функции внедрения ASP.NET Core, другие классы, созданные на уровне размещения, такие как `BackgroundService` классы, классы контроллеров MVC или модели страниц Razor, могут получать ссылки на серверные концентраторы, принимая `IHubContext<ClockHub, IClock>` экземпляры во время создания.</span><span class="sxs-lookup"><span data-stu-id="da8ec-125">Using ASP.NET Core's DI features, other classes instantiated by the hosting layer, like `BackgroundService` classes, MVC Controller classes, or Razor page models, can get references to server-side Hubs by accepting instances of `IHubContext<ClockHub, IClock>` during construction.</span></span>

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

<span data-ttu-id="da8ec-126">Так как `ClockHub`метод вызывается итеративно в фоновой службе, текущая дата и время сервера отправляются подключенным клиентам с помощью. `ExecuteAsync`</span><span class="sxs-lookup"><span data-stu-id="da8ec-126">As the `ExecuteAsync` method is called iteratively in the background service, the server's current date and time are sent to the connected clients using the `ClockHub`.</span></span>

## <a name="react-to-signalr-events-with-background-services"></a><span data-ttu-id="da8ec-127">Реагирование на события SignalR с помощью фоновых служб</span><span class="sxs-lookup"><span data-stu-id="da8ec-127">React to SignalR events with background services</span></span>

<span data-ttu-id="da8ec-128">Как и приложение с одним страничным приложением, использующее клиент JavaScript для SignalR или классическое приложение .NET, может использовать <xref:signalr/dotnet-client>с помощью `BackgroundService` , `IHostedService` а также для подключения к концентраторам SignalR и реагирования на события.</span><span class="sxs-lookup"><span data-stu-id="da8ec-128">Like a Single Page App using the JavaScript client for SignalR or a .NET desktop app can do using the using the <xref:signalr/dotnet-client>, a `BackgroundService` or `IHostedService` implementation can also be used to connect to SignalR Hubs and respond to events.</span></span>

<span data-ttu-id="da8ec-129">Класс реализует `IClock` интерфейс и`IHostedService` интерфейс. `ClockHubClient`</span><span class="sxs-lookup"><span data-stu-id="da8ec-129">The `ClockHubClient` class implements both the `IClock` interface and the `IHostedService` interface.</span></span> <span data-ttu-id="da8ec-130">Таким образом, она может быть подключена `Startup` во время непрерывного выполнения и реагировать на события концентратора с сервера.</span><span class="sxs-lookup"><span data-stu-id="da8ec-130">This way it can be wired up during `Startup` to run continuously and respond to Hub events from the server.</span></span>

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

<span data-ttu-id="da8ec-131">Во время инициализации объект `ClockHubClient` создает экземпляр `HubConnection` класса и подсоединяет `IClock.ShowTime` `ShowTime` метод к нему в качестве обработчика для события концентратора.</span><span class="sxs-lookup"><span data-stu-id="da8ec-131">During initialization, the `ClockHubClient` creates an instance of a `HubConnection` and wires up the `IClock.ShowTime` method as the handler for the Hub's `ShowTime` event.</span></span>

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

<span data-ttu-id="da8ec-132">`IHostedService.StartAsync` В реализации `HubConnection` запускается асинхронно.</span><span class="sxs-lookup"><span data-stu-id="da8ec-132">In the `IHostedService.StartAsync` implementation, the `HubConnection` is started asynchronously.</span></span>

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

<span data-ttu-id="da8ec-133">Во время выполнения `HubConnection`методаобъект уничтожается асинхронно. `IHostedService.StopAsync`</span><span class="sxs-lookup"><span data-stu-id="da8ec-133">During the `IHostedService.StopAsync` method, the `HubConnection` is disposed of asynchronously.</span></span>

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a><span data-ttu-id="da8ec-134">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="da8ec-134">Additional resources</span></span>

* [<span data-ttu-id="da8ec-135">Начало работы</span><span class="sxs-lookup"><span data-stu-id="da8ec-135">Get started</span></span>](xref:tutorials/signalr)
* [<span data-ttu-id="da8ec-136">Центры</span><span class="sxs-lookup"><span data-stu-id="da8ec-136">Hubs</span></span>](xref:signalr/hubs)
* [<span data-ttu-id="da8ec-137">Публикация в Azure</span><span class="sxs-lookup"><span data-stu-id="da8ec-137">Publish to Azure</span></span>](xref:signalr/publish-to-azure-web-app)
* [<span data-ttu-id="da8ec-138">Строго типизированные концентраторы</span><span class="sxs-lookup"><span data-stu-id="da8ec-138">Strongly typed Hubs</span></span>](xref:signalr/hubs#strongly-typed-hubs)
