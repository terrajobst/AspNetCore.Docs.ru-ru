---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 02/15/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: e5455e553cba817dce811391d4a909e501a20d9a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273785"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="27cad-103">Фоновые задачи с размещенными службами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="27cad-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="27cad-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="27cad-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="27cad-105">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="27cad-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="27cad-106">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="27cad-106">A hosted service is a class with background task logic that implements the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="27cad-107">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="27cad-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="27cad-108">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="27cad-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="27cad-109">Размещенная служба, которая активирует службу с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="27cad-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="27cad-110">Служба с заданной областью может использовать внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="27cad-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="27cad-111">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="27cad-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="27cad-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="27cad-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="27cad-113">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="27cad-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="27cad-114">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="27cad-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="27cad-115">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="27cad-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="27cad-116">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="27cad-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="27cad-117">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="27cad-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="27cad-118">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="27cad-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

::: moniker-end

## <a name="ihostedservice-interface"></a><span data-ttu-id="27cad-119">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="27cad-119">IHostedService interface</span></span>

<span data-ttu-id="27cad-120">Размещенные службы реализуют интерфейс [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice).</span><span class="sxs-lookup"><span data-stu-id="27cad-120">Hosted services implement the [IHostedService](/dotnet/api/microsoft.extensions.hosting.ihostedservice) interface.</span></span> <span data-ttu-id="27cad-121">Этот интерфейс определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="27cad-121">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="27cad-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) — вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted).</span><span class="sxs-lookup"><span data-stu-id="27cad-122">[StartAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.startasync) - Called after the server has started and [IApplicationLifetime.ApplicationStarted](/dotnet/api/microsoft.aspnetcore.hosting.iapplicationlifetime.applicationstarted) is triggered.</span></span> <span data-ttu-id="27cad-123">`StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="27cad-123">`StartAsync` contains the logic to start the background task.</span></span>

* <span data-ttu-id="27cad-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) — запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="27cad-124">[StopAsync(CancellationToken)](/dotnet/api/microsoft.extensions.hosting.ihostedservice.stopasync) - Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="27cad-125">`StopAsync` содержит логику для завершения фоновой задачи и удаления неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="27cad-125">`StopAsync` contains the logic to end the background task and dispose of any unmanaged resources.</span></span> <span data-ttu-id="27cad-126">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="27cad-126">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span>

<span data-ttu-id="27cad-127">Размещенная служба активируется при запуске приложения и корректно завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="27cad-127">The hosted service is activated once at app startup and gracefully shutdown at app shutdown.</span></span> <span data-ttu-id="27cad-128">Если реализуется [IDisposable](/dotnet/api/system.idisposable), ресурсы можно удалить при удалении контейнера службы.</span><span class="sxs-lookup"><span data-stu-id="27cad-128">When [IDisposable](/dotnet/api/system.idisposable) is implemented, resources can be disposed when the service container is disposed.</span></span> <span data-ttu-id="27cad-129">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="27cad-129">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="27cad-130">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="27cad-130">Timed background tasks</span></span>

<span data-ttu-id="27cad-131">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](/dotnet/api/system.threading.timer).</span><span class="sxs-lookup"><span data-stu-id="27cad-131">A timed background task makes use of the [System.Threading.Timer](/dotnet/api/system.threading.timer) class.</span></span> <span data-ttu-id="27cad-132">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="27cad-132">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="27cad-133">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="27cad-133">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="27cad-134">Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="27cad-134">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="27cad-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="27cad-135">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="27cad-136">Служба регистрируется в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="27cad-136">The service is registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="27cad-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span><span class="sxs-lookup"><span data-stu-id="27cad-137">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]</span></span>

::: moniker-end

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="27cad-138">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="27cad-138">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="27cad-139">Чтобы использовать службы с заданной областью в `IHostedService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="27cad-139">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="27cad-140">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="27cad-140">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="27cad-141">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="27cad-141">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="27cad-142">В следующем примере в службу вставляется [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger):</span><span class="sxs-lookup"><span data-stu-id="27cad-142">In the following example, [ILogger](/dotnet/api/microsoft.extensions.logging.ilogger) is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="27cad-143">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="27cad-143">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="27cad-144">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="27cad-144">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="27cad-145">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="27cad-145">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="27cad-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="27cad-146">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="27cad-147">Службы регистрируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="27cad-147">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="27cad-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span><span class="sxs-lookup"><span data-stu-id="27cad-148">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]</span></span>

::: moniker-end

## <a name="queued-background-tasks"></a><span data-ttu-id="27cad-149">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="27cad-149">Queued background tasks</span></span>

<span data-ttu-id="27cad-150">Очередь фоновых задач основывается на методе [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) в .NET 4.x ([предварительно запланирована встройка в ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="27cad-150">A background task queue is based on the .NET 4.x [QueueBackgroundWorkItem](/dotnet/api/system.web.hosting.hostingenvironment.queuebackgroundworkitem) ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="27cad-151">В `QueueHostedService` фоновые задачи (`workItem`) в очереди выводятся из очереди и выполняются:</span><span class="sxs-lookup"><span data-stu-id="27cad-151">In `QueueHostedService`, background tasks (`workItem`) in the queue are dequeued and executed:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=30-31,35)]

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="27cad-152">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="27cad-152">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="27cad-153">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="27cad-153">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

<span data-ttu-id="27cad-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="27cad-154">[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="27cad-155">Службы регистрируются в `Startup.ConfigureServices`:</span><span class="sxs-lookup"><span data-stu-id="27cad-155">The services are registered in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="27cad-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span><span class="sxs-lookup"><span data-stu-id="27cad-156">[!code-csharp[](hosted-services/samples-snapshot/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]</span></span>

::: moniker-end

<span data-ttu-id="27cad-157">В классе модели страницы индексов `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`:</span><span class="sxs-lookup"><span data-stu-id="27cad-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="27cad-158">Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="27cad-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="27cad-159">`QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="27cad-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="27cad-160">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="27cad-160">Additional resources</span></span>

* [<span data-ttu-id="27cad-161">Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService</span><span class="sxs-lookup"><span data-stu-id="27cad-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="27cad-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="27cad-162">System.Threading.Timer</span></span>](/dotnet/api/system.threading.timer)
