---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/26/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: c1fbb5ae8ffc4ee506f42df6a4cbbe845b2b903d
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 10/15/2019
ms.locfileid: "72333657"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="19793-103">Фоновые задачи с размещенными службами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="19793-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="19793-104">Авторы: [Luke Latham](https://github.com/guardrex) (Люк Латэм) и [Jeow Li Huan](https://github.com/huan086) (Чжоу Ли Хуан)</span><span class="sxs-lookup"><span data-stu-id="19793-104">By [Luke Latham](https://github.com/guardrex) and [Jeow Li Huan](https://github.com/huan086)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="19793-105">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="19793-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="19793-106">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="19793-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="19793-107">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="19793-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="19793-108">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="19793-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="19793-109">Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="19793-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="19793-110">Служба с заданной областью может использовать [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="19793-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="19793-111">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="19793-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="19793-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19793-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="19793-113">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="19793-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="19793-114">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="19793-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="19793-115">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="19793-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="19793-116">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="19793-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="19793-117">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="19793-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="19793-118">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="19793-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="19793-119">Шаблон службы рабочей роли</span><span class="sxs-lookup"><span data-stu-id="19793-119">Worker Service template</span></span>

<span data-ttu-id="19793-120">Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб.</span><span class="sxs-lookup"><span data-stu-id="19793-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="19793-121">Чтобы использовать шаблон в качестве основы для приложения размещенных служб, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="19793-121">To use the template as a basis for a hosted services app:</span></span>

[!INCLUDE[](~/includes/worker-template-instructions.md)]

---

## <a name="package"></a><span data-ttu-id="19793-122">Пакет</span><span class="sxs-lookup"><span data-stu-id="19793-122">Package</span></span>

<span data-ttu-id="19793-123">Ссылка на пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) добавляется неявно для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="19793-123">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="19793-124">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="19793-124">IHostedService interface</span></span>

<span data-ttu-id="19793-125">Интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService> определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="19793-125">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="19793-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-126">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="19793-127">*Первым* вызывается `StartAsync`:</span><span class="sxs-lookup"><span data-stu-id="19793-127">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="19793-128">Настраивается конвейер обработки запросов приложения (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="19793-128">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="19793-129">Запускается сервер и активируется [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="19793-129">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="19793-130">Поведение по умолчанию можно изменить таким образом, чтобы `StartAsync` размещенной службы выполнялся после настройки конвейера приложения и вызова `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="19793-130">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="19793-131">Чтобы изменить поведение по умолчанию, добавьте размещенную службу (`VideosWatcher` в следующем примере) после вызова `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="19793-131">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

  ```csharp
  using Microsoft.AspNetCore.Hosting;
  using Microsoft.Extensions.DependencyInjection;
  using Microsoft.Extensions.Hosting;

  public class Program
  {
      public static void Main(string[] args)
      {
          CreateHostBuilder(args).Build().Run();
      }

      public static IHostBuilder CreateHostBuilder(string[] args) =>
          Host.CreateDefaultBuilder(args)
              .ConfigureWebHostDefaults(webBuilder =>
              {
                  webBuilder.UseStartup<Startup>();
              })
              .ConfigureServices(services =>
              {
                  services.AddHostedService<VideosWatcher>();
              });
  }
  ```

* <span data-ttu-id="19793-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="19793-132">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="19793-133">`StopAsync` содержит логику для завершения фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-133">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="19793-134">Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="19793-134">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="19793-135">Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным.</span><span class="sxs-lookup"><span data-stu-id="19793-135">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="19793-136">При запросе отмены происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="19793-136">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="19793-137">должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;</span><span class="sxs-lookup"><span data-stu-id="19793-137">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="19793-138">должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="19793-138">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="19793-139">Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.</span><span class="sxs-lookup"><span data-stu-id="19793-139">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="19793-140">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="19793-140">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="19793-141">Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.</span><span class="sxs-lookup"><span data-stu-id="19793-141">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="19793-142">Чтобы увеличить время ожидания завершения работы по умолчанию (пять секунд), установите следующие значения:</span><span class="sxs-lookup"><span data-stu-id="19793-142">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="19793-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла.</span><span class="sxs-lookup"><span data-stu-id="19793-143"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="19793-144">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="19793-144">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="19793-145">Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла.</span><span class="sxs-lookup"><span data-stu-id="19793-145">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="19793-146">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="19793-146">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="19793-147">Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="19793-147">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="19793-148">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="19793-148">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="19793-149">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="19793-149">BackgroundService</span></span>

<span data-ttu-id="19793-150">`BackgroundService` — это базовый класс для реализации долго выполняющегося интерфейса <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="19793-150">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="19793-151">`BackgroundService` предоставляет абстрактный метод `ExecuteAsync(CancellationToken stoppingToken)` для хранения логики службы.</span><span class="sxs-lookup"><span data-stu-id="19793-151">`BackgroundService` provides the `ExecuteAsync(CancellationToken stoppingToken)` abstract method to contain the service's logic.</span></span> <span data-ttu-id="19793-152">`stoppingToken` активируется при вызове [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*).</span><span class="sxs-lookup"><span data-stu-id="19793-152">The `stoppingToken` is triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span> <span data-ttu-id="19793-153">Реализация этого метода возвращает значение `Task`, представляющее все время существования фоновой службы.</span><span class="sxs-lookup"><span data-stu-id="19793-153">The implementation of this method returns a `Task` that represents the entire lifetime of the background service.</span></span>

<span data-ttu-id="19793-154">Кроме того, *в необязательном порядке* переопределяет методы, определенные в `IHostedService`, чтобы запустить код запуска и завершения работы службы:</span><span class="sxs-lookup"><span data-stu-id="19793-154">In addition, *optionally* override the methods defined on `IHostedService` to run startup and shutdown code for your service:</span></span>

* <span data-ttu-id="19793-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` вызывается, когда происходит нормальное завершение работы узла приложения.</span><span class="sxs-lookup"><span data-stu-id="19793-155">`StopAsync(CancellationToken cancellationToken)` &ndash; `StopAsync` is called when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="19793-156">`cancellationToken` возвращается, когда узел решает принудительно завершить работу службы.</span><span class="sxs-lookup"><span data-stu-id="19793-156">The `cancellationToken` is signalled when the host decides to forcibly terminate the service.</span></span> <span data-ttu-id="19793-157">Если этот метод переопределен, вы **должны** вызвать (и `await`) метод базового класса, чтобы обеспечить правильное завершение работы службы.</span><span class="sxs-lookup"><span data-stu-id="19793-157">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service shuts down properly.</span></span>
* <span data-ttu-id="19793-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` вызывается для запуска фоновой службы.</span><span class="sxs-lookup"><span data-stu-id="19793-158">`StartAsync(CancellationToken cancellationToken)` &ndash; `StartAsync` is called to start the background service.</span></span> <span data-ttu-id="19793-159">`cancellationToken` возвращается, если процесс запуска прерван.</span><span class="sxs-lookup"><span data-stu-id="19793-159">The `cancellationToken` is signalled if the startup process is interrupted.</span></span> <span data-ttu-id="19793-160">Реализация возвращает значение `Task`, представляющее процесс запуска службы.</span><span class="sxs-lookup"><span data-stu-id="19793-160">The implementation returns a `Task` that represents the startup process for the service.</span></span> <span data-ttu-id="19793-161">Никакие другие службы не запускаются до завершения этого `Task`.</span><span class="sxs-lookup"><span data-stu-id="19793-161">No further services are started until this `Task` completes.</span></span> <span data-ttu-id="19793-162">Если этот метод переопределен, вы **должны** вызвать (и `await`) метод базового класса, чтобы обеспечить правильный запуск службы.</span><span class="sxs-lookup"><span data-stu-id="19793-162">If this method is overridden, you **must** call (and `await`) the base class method to ensure the service starts properly.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="19793-163">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="19793-163">Timed background tasks</span></span>

<span data-ttu-id="19793-164">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="19793-164">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="19793-165">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-165">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="19793-166">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="19793-166">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="19793-167">Служба регистрируется в `IHostBuilder.ConfigureServices` (*Program.cs*) с использованием метода расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-167">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="19793-168">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="19793-168">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="19793-169">Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `BackgroundService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="19793-169">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="19793-170">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="19793-170">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="19793-171">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-171">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="19793-172">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="19793-172">In the following example:</span></span>

* <span data-ttu-id="19793-173">Служба является асинхронной.</span><span class="sxs-lookup"><span data-stu-id="19793-173">The service is asynchronous.</span></span> <span data-ttu-id="19793-174">Метод `DoWork` возвращает значение `Task`.</span><span class="sxs-lookup"><span data-stu-id="19793-174">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="19793-175">В демонстрационных целях в методе `DoWork` ожидается задержка в десять секунд.</span><span class="sxs-lookup"><span data-stu-id="19793-175">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="19793-176">В службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>.</span><span class="sxs-lookup"><span data-stu-id="19793-176">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="19793-177">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="19793-177">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="19793-178">`DoWork` возвращает объект `Task`, ожидаемый в `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="19793-178">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="19793-179">Службы регистрируются в `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="19793-179">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="19793-180">Размещенная служба регистрируется с использованием метода расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-180">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="19793-181">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="19793-181">Queued background tasks</span></span>

<span data-ttu-id="19793-182">Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="19793-182">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="19793-183">В следующем примере `QueueHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-183">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="19793-184">Метод `BackgroundProcessing` возвращает объект `Task`, ожидаемый в `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="19793-184">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="19793-185">Фоновые задачи в очереди выводятся из очереди и выполняются в `BackgroundProcessing`:</span><span class="sxs-lookup"><span data-stu-id="19793-185">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>
* <span data-ttu-id="19793-186">Рабочие элементы ожидают остановки службы через `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="19793-186">Work items are awaited before the service stops in `StopAsync`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28-29,33)]

<span data-ttu-id="19793-187">Служба `MonitorLoop` обрабатывает задачи постановки в очередь для размещенной службы при выборе на устройстве ввода ключа `w`:</span><span class="sxs-lookup"><span data-stu-id="19793-187">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="19793-188">В службу `MonitorLoop` внедряется `IBackgroundTaskQueue`.</span><span class="sxs-lookup"><span data-stu-id="19793-188">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="19793-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="19793-189">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>
* <span data-ttu-id="19793-190">Рабочий элемент имитирует долго выполняющуюся фоновую задачу:</span><span class="sxs-lookup"><span data-stu-id="19793-190">The work item simulates a long-running background task:</span></span>
  * <span data-ttu-id="19793-191">Выполняется три 5-секундных задержки (`Task.Delay`).</span><span class="sxs-lookup"><span data-stu-id="19793-191">Three 5-second delays are executed (`Task.Delay`).</span></span>
  * <span data-ttu-id="19793-192">Оператор `try-catch` перехватывается <xref:System.OperationCanceledException>, если задача отменена.</span><span class="sxs-lookup"><span data-stu-id="19793-192">A `try-catch` statement traps <xref:System.OperationCanceledException> if the task is cancelled.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="19793-193">Службы регистрируются в `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="19793-193">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="19793-194">Размещенная служба регистрируется с использованием метода расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-194">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="19793-195">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="19793-195">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="19793-196">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="19793-196">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="19793-197">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="19793-197">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="19793-198">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="19793-198">Background task that runs on a timer.</span></span>
* <span data-ttu-id="19793-199">Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="19793-199">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="19793-200">Служба с заданной областью может использовать [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="19793-200">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="19793-201">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="19793-201">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="19793-202">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="19793-202">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="19793-203">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="19793-203">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="19793-204">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="19793-204">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="19793-205">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="19793-205">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="19793-206">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="19793-206">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="19793-207">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="19793-207">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="19793-208">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="19793-208">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="19793-209">Пакет</span><span class="sxs-lookup"><span data-stu-id="19793-209">Package</span></span>

<span data-ttu-id="19793-210">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="19793-210">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="19793-211">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="19793-211">IHostedService interface</span></span>

<span data-ttu-id="19793-212">Размещенные службы реализуют интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="19793-212">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="19793-213">Этот интерфейс определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="19793-213">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="19793-214">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-214">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="19793-215">При использовании [Web Host](xref:fundamentals/host/web-host) `StartAsync` вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="19793-215">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="19793-216">При использовании [универсального узла](xref:fundamentals/host/generic-host) `StartAsync` вызывается до активации `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="19793-216">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="19793-217">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="19793-217">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="19793-218">`StopAsync` содержит логику для завершения фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-218">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="19793-219">Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="19793-219">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="19793-220">Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным.</span><span class="sxs-lookup"><span data-stu-id="19793-220">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="19793-221">При запросе отмены происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="19793-221">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="19793-222">должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;</span><span class="sxs-lookup"><span data-stu-id="19793-222">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="19793-223">должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="19793-223">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="19793-224">Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.</span><span class="sxs-lookup"><span data-stu-id="19793-224">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="19793-225">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="19793-225">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="19793-226">Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.</span><span class="sxs-lookup"><span data-stu-id="19793-226">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="19793-227">Чтобы увеличить время ожидания завершения работы по умолчанию (пять секунд), установите следующие значения:</span><span class="sxs-lookup"><span data-stu-id="19793-227">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="19793-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла.</span><span class="sxs-lookup"><span data-stu-id="19793-228"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="19793-229">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="19793-229">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="19793-230">Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла.</span><span class="sxs-lookup"><span data-stu-id="19793-230">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="19793-231">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="19793-231">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="19793-232">Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="19793-232">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="19793-233">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="19793-233">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="19793-234">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="19793-234">Timed background tasks</span></span>

<span data-ttu-id="19793-235">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="19793-235">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="19793-236">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-236">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="19793-237">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="19793-237">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="19793-238">Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-238">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="19793-239">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="19793-239">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="19793-240">Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `IHostedService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="19793-240">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="19793-241">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="19793-241">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="19793-242">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="19793-242">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="19793-243">В следующем примере в службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>:</span><span class="sxs-lookup"><span data-stu-id="19793-243">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="19793-244">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="19793-244">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="19793-245">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="19793-245">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="19793-246">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-246">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="19793-247">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="19793-247">Queued background tasks</span></span>

<span data-ttu-id="19793-248">Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET Framework 4.x ([предварительно запланировано встроить в ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="19793-248">A background task queue is based on the .NET Framework 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="19793-249">В `QueueHostedService` фоновые задачи в очереди из очереди выводятся из очереди и выполняются в качестве <xref:Microsoft.Extensions.Hosting.BackgroundService> — базового класса для реализации длительного выполнения `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-249">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="19793-250">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="19793-250">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="19793-251">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="19793-251">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="19793-252">В классе модели страницы индексов:</span><span class="sxs-lookup"><span data-stu-id="19793-252">In the Index page model class:</span></span>

* <span data-ttu-id="19793-253">`IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`.</span><span class="sxs-lookup"><span data-stu-id="19793-253">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="19793-254"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> вставляется и присваивается `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="19793-254">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="19793-255">Фабрика используется для создания экземпляров <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, который используется для создания служб с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="19793-255">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="19793-256">Для использования `AppDbContext` приложения создается область ([служба с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes)) для записи данных из базы данных в `IBackgroundTaskQueue` (отдельная служба).</span><span class="sxs-lookup"><span data-stu-id="19793-256">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="19793-257">Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="19793-257">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="19793-258">`QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="19793-258">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="19793-259">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="19793-259">Additional resources</span></span>

* [<span data-ttu-id="19793-260">Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService</span><span class="sxs-lookup"><span data-stu-id="19793-260">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
