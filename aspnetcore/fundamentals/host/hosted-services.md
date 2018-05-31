---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/hosted-services
ms.openlocfilehash: cc39d125b639719599eca68d627fda014fb107e0
ms.sourcegitcommit: 466300d32f8c33e64ee1b419a2cbffe702863cdf
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/27/2018
ms.locfileid: "34555304"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="585e3-103">Фоновые задачи с размещенными службами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="585e3-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="585e3-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="585e3-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="585e3-105">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="585e3-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="585e3-106">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="585e3-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="585e3-107">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="585e3-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="585e3-108">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="585e3-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="585e3-109">Размещенная служба, которая активирует службу с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="585e3-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="585e3-110">Служба с заданной областью может использовать внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="585e3-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="585e3-111">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="585e3-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="585e3-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="585e3-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="585e3-113">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="585e3-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="585e3-114">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="585e3-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="585e3-115">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="585e3-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="585e3-116">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="585e3-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="585e3-117">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="585e3-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="585e3-118">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="585e3-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="585e3-119">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="585e3-119">IHostedService interface</span></span>

<span data-ttu-id="585e3-120">Размещенные службы реализуют интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="585e3-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="585e3-121">Этот интерфейс определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="585e3-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="585e3-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted).</span><span class="sxs-lookup"><span data-stu-id="585e3-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="585e3-123">`StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="585e3-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="585e3-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) — запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="585e3-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="585e3-125">`StopAsync` содержит логику для завершения фоновой задачи и удаления неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="585e3-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="585e3-126">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="585e3-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="585e3-127">Размещенная служба является отдельным объектом, который активируется при запуске приложения и корректно завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="585e3-127">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="585e3-128">Если реализуется [IDisposable](/dotnet/api/system.idisposable), ресурсы можно удалить при удалении контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="585e3-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="585e3-129">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="585e3-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="585e3-130">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="585e3-130">Timed background tasks</span></span>

<span data-ttu-id="585e3-131">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="585e3-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="585e3-132">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="585e3-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="585e3-133">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="585e3-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="585e3-134">Служба регистрируется в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="585e3-134">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="585e3-135">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="585e3-135">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="585e3-136">Чтобы использовать службы с заданной областью в `IHostedService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="585e3-136">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="585e3-137">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="585e3-137">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="585e3-138">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="585e3-138">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="585e3-139">В следующем примере в службу вставляется [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger):</span><span class="sxs-lookup"><span data-stu-id="585e3-139">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="585e3-140">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="585e3-140">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="585e3-141">Службы регистрируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="585e3-141">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="585e3-142">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="585e3-142">Queued background tasks</span></span>

<span data-ttu-id="585e3-143">Очередь фоновых задач основывается на методе [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) в .NET 4.x ([предварительно запланирована встройка в ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="585e3-143">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="585e3-144">В `QueueHostedService` фоновые задачи (`workItem`) в очереди выводятся из очереди и выполняются:</span><span class="sxs-lookup"><span data-stu-id="585e3-144">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="585e3-145">Службы регистрируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="585e3-145">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="585e3-146">В классе модели страницы индексов `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`:</span><span class="sxs-lookup"><span data-stu-id="585e3-146">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="585e3-147">Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="585e3-147">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="585e3-148">`QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="585e3-148">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="585e3-149">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="585e3-149">Additional resources</span></span>

* [<span data-ttu-id="585e3-150">Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService</span><span class="sxs-lookup"><span data-stu-id="585e3-150">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="585e3-151">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="585e3-151">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
