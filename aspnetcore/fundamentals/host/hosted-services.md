---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 11/14/2018
uid: fundamentals/host/hosted-services
ms.openlocfilehash: f8e13e13af22f1be4f14d5e59807c4dae3b78e84
ms.sourcegitcommit: 09bcda59a58019fdf47b2db5259fe87acf19dd38
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 11/15/2018
ms.locfileid: "51708495"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="a8bd9-103">Фоновые задачи с размещенными службами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a8bd9-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="a8bd9-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a8bd9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

<span data-ttu-id="a8bd9-105">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a8bd9-106">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a8bd9-107">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a8bd9-108">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a8bd9-109">Размещенная служба, которая активирует службу с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-109">Hosted service that activates a scoped service.</span></span> <span data-ttu-id="a8bd9-110">Служба с заданной областью может использовать внедрение зависимостей.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-110">The scoped service can use dependency injection.</span></span>
* <span data-ttu-id="a8bd9-111">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a8bd9-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a8bd9-112">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a8bd9-113">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="a8bd9-114">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-114">Web Host &ndash; The Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="a8bd9-115">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-115">The example code shown in this topic is from the Web Host version of the sample.</span></span> <span data-ttu-id="a8bd9-116">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="a8bd9-117">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-117">Generic Host &ndash; The Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a8bd9-118">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="a8bd9-119">Пакет</span><span class="sxs-lookup"><span data-stu-id="a8bd9-119">Package</span></span>

<span data-ttu-id="a8bd9-120">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-120">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a8bd9-121">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="a8bd9-121">IHostedService interface</span></span>

<span data-ttu-id="a8bd9-122">Размещенные службы реализуют интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-122">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a8bd9-123">Этот интерфейс определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-123">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a8bd9-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-124">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="a8bd9-125">При использовании [Web Host](xref:fundamentals/host/web-host) `StartAsync` вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-125">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="a8bd9-126">При использовании [универсального узла](xref:fundamentals/host/generic-host) `StartAsync` вызывается до активации `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-126">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="a8bd9-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-127">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a8bd9-128">`StopAsync` содержит логику для завершения фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-128">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="a8bd9-129">Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-129">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span> 

  <span data-ttu-id="a8bd9-130">Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-130">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="a8bd9-131">При запросе отмены происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-131">When cancellation is requested on the token:</span></span>
  
  * <span data-ttu-id="a8bd9-132">должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;</span><span class="sxs-lookup"><span data-stu-id="a8bd9-132">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="a8bd9-133">должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-133">Any methods called in `StopAsync` should return promptly.</span></span>
  
  <span data-ttu-id="a8bd9-134">Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-134">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="a8bd9-135">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-135">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="a8bd9-136">Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-136">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

<span data-ttu-id="a8bd9-137">Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-137">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="a8bd9-138">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-138">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a8bd9-139">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="a8bd9-139">Timed background tasks</span></span>

<span data-ttu-id="a8bd9-140">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="a8bd9-140">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="a8bd9-141">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-141">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a8bd9-142">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-142">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="a8bd9-143">Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-143">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a8bd9-144">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="a8bd9-144">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a8bd9-145">Чтобы использовать службы с заданной областью в `IHostedService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-145">To use scoped services within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="a8bd9-146">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-146">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a8bd9-147">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-147">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a8bd9-148">В следующем примере в службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-148">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a8bd9-149">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-149">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="a8bd9-150">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-150">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8bd9-151">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-151">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a8bd9-152">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="a8bd9-152">Queued background tasks</span></span>

<span data-ttu-id="a8bd9-153">Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="a8bd9-153">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core 3.0](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a8bd9-154">В `QueueHostedService` фоновые задачи в очереди из очереди выводятся из очереди и выполняются в качестве <xref:Microsoft.Extensions.Hosting.BackgroundService> — базового класса для реализации длительного выполнения `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-154">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="a8bd9-155">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-155">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a8bd9-156">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-156">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Startup.cs?name=snippet3)]

<span data-ttu-id="a8bd9-157">В классе модели страницы индексов `IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-157">In the Index page model class, the `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a8bd9-158">Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="a8bd9-158">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="a8bd9-159">`QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="a8bd9-159">`QueueBackgroundWorkItem` is called to enqueue the work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample-WebHost/Pages/Index.cshtml.cs?name=snippet2)]

## <a name="additional-resources"></a><span data-ttu-id="a8bd9-160">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a8bd9-160">Additional resources</span></span>

* [<span data-ttu-id="a8bd9-161">Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService</span><span class="sxs-lookup"><span data-stu-id="a8bd9-161">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* [<span data-ttu-id="a8bd9-162">System.Threading.Timer</span><span class="sxs-lookup"><span data-stu-id="a8bd9-162">System.Threading.Timer</span></span>](xref:System.Threading.Timer)
