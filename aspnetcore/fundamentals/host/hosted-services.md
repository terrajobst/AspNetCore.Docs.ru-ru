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
ms.openlocfilehash: ace7fc8622864099b7c0e36e4a914de340d4d4e9
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 05/17/2018
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="d4081-103">Фоновые задачи с размещенными службами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d4081-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="d4081-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="d4081-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="d4081-105">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="d4081-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="d4081-106">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="d4081-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="d4081-107">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="d4081-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="d4081-108">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="d4081-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="d4081-109">Размещенная служба, которая активирует службу с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="d4081-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="d4081-110">Служба с заданной областью может использовать внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="d4081-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="d4081-111">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="d4081-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="d4081-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d4081-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="d4081-113">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="d4081-113">IHostedService interface</span></span>

<span data-ttu-id="d4081-114">Размещенные службы реализуют интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="d4081-114">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="d4081-115">Этот интерфейс определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="d4081-115">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="d4081-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted).</span><span class="sxs-lookup"><span data-stu-id="d4081-116">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="d4081-117">`StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="d4081-117">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="d4081-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) — запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="d4081-118">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="d4081-119">`StopAsync` содержит логику для завершения фоновой задачи и удаления неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="d4081-119">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="d4081-120">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="d4081-120">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="d4081-121">Размещенная служба является отдельным объектом, который активируется при запуске приложения и корректно завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="d4081-121">The hosted service is a singleton that's activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="d4081-122">Если реализуется [IDisposable](/dotnet/api/system.idisposable), ресурсы можно удалить при удалении контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="d4081-122">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="d4081-123">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="d4081-123">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="d4081-124">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="d4081-124">Timed background tasks</span></span>

<span data-ttu-id="d4081-125">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="d4081-125">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="d4081-126">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="d4081-126">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="d4081-127">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="d4081-127">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="d4081-128">Служба регистрируется в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4081-128">The service is registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="d4081-129">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="d4081-129">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="d4081-130">Чтобы использовать службы с заданной областью в `IHostedService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="d4081-130">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="d4081-131">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="d4081-131">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="d4081-132">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="d4081-132">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="d4081-133">В следующем примере в службу вставляется [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger):</span><span class="sxs-lookup"><span data-stu-id="d4081-133">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="d4081-134">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="d4081-134">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="d4081-135">Службы регистрируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4081-135">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="d4081-136">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="d4081-136">Queued background tasks</span></span>

<span data-ttu-id="d4081-137">Очередь фоновых задач основывается на методе [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) в .NET 4.x ([предварительно запланирована встройка в ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="d4081-137">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 2.2](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="d4081-138">В `QueueHostedService` фоновые задачи (`workItem`) в очереди выводятся из очереди и выполняются:</span><span class="sxs-lookup"><span data-stu-id="d4081-138">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

<span data-ttu-id="d4081-139">Службы регистрируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="d4081-139">The services are registered in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Startup.cs?name=snippet3)]

<span data-ttu-id="d4081-140">В классе модели страницы индексов `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`:</span><span class="sxs-lookup"><span data-stu-id="d4081-140">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="d4081-141">Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="d4081-141">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="d4081-142">`QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="d4081-142">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="d4081-143">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="d4081-143">Additional resources</span></span>

* [<span data-ttu-id="d4081-144">Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService</span><span class="sxs-lookup"><span data-stu-id="d4081-144">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="d4081-145">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="d4081-145">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
