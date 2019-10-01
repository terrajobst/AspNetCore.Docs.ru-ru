---
title: Фоновые задачи с размещенными службами в ASP.NET Core
author: guardrex
description: Узнайте, как реализовать фоновые задачи с размещенными службами в ASP.NET Core.
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 09/18/2019
uid: fundamentals/host/hosted-services
ms.openlocfilehash: 8df86b10d7ba853edb3265df0e02eabbf8a2c058
ms.sourcegitcommit: fa61d882be9d0c48bd681f2efcb97e05522051d0
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 09/23/2019
ms.locfileid: "71205715"
---
# <a name="background-tasks-with-hosted-services-in-aspnet-core"></a><span data-ttu-id="a97f9-103">Фоновые задачи с размещенными службами в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a97f9-103">Background tasks with hosted services in ASP.NET Core</span></span>

<span data-ttu-id="a97f9-104">Автор [Люк Латэм](https://github.com/guardrex) (Luke Latham)</span><span class="sxs-lookup"><span data-stu-id="a97f9-104">By [Luke Latham](https://github.com/guardrex)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a97f9-105">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="a97f9-105">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a97f9-106">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-106">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a97f9-107">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="a97f9-107">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a97f9-108">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="a97f9-108">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a97f9-109">Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="a97f9-109">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a97f9-110">Служба с заданной областью может использовать [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a97f9-110">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="a97f9-111">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="a97f9-111">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a97f9-112">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a97f9-112">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a97f9-113">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="a97f9-113">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="a97f9-114">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="a97f9-114">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="a97f9-115">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="a97f9-115">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="a97f9-116">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a97f9-116">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="a97f9-117">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a97f9-117">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a97f9-118">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a97f9-118">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="worker-service-template"></a><span data-ttu-id="a97f9-119">Шаблон службы рабочей роли</span><span class="sxs-lookup"><span data-stu-id="a97f9-119">Worker Service template</span></span>

<span data-ttu-id="a97f9-120">Шаблон службы рабочей роли ASP.NET Core может служить отправной точкой для написания длительно выполняющихся приложений служб.</span><span class="sxs-lookup"><span data-stu-id="a97f9-120">The ASP.NET Core Worker Service template provides a starting point for writing long running service apps.</span></span> <span data-ttu-id="a97f9-121">Чтобы использовать шаблон в качестве основы для приложения размещенных служб, выполните указанные ниже действия.</span><span class="sxs-lookup"><span data-stu-id="a97f9-121">To use the template as a basis for a hosted services app:</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="a97f9-122">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="a97f9-122">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="a97f9-123">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="a97f9-123">Create a new project.</span></span>
1. <span data-ttu-id="a97f9-124">Выберите **Новое веб-приложение ASP.NET Core**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-124">Select **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="a97f9-125">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-125">Select **Next**.</span></span>
1. <span data-ttu-id="a97f9-126">В поле **Имя проекта** укажите имя проекта или оставьте имя по умолчанию.</span><span class="sxs-lookup"><span data-stu-id="a97f9-126">Provide a project name in the **Project name** field or accept the default project name.</span></span> <span data-ttu-id="a97f9-127">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-127">Select **Create**.</span></span>
1. <span data-ttu-id="a97f9-128">В диалоговом окне **Создание веб-приложения ASP.NET Core** убедитесь в том, что выбраны платформы **.NET Core** и **ASP.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-128">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span>
1. <span data-ttu-id="a97f9-129">Выберите шаблон **Служба рабочей роли**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-129">Select the **Worker Service** template.</span></span> <span data-ttu-id="a97f9-130">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-130">Select **Create**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="a97f9-131">Visual Studio для Mac</span><span class="sxs-lookup"><span data-stu-id="a97f9-131">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="a97f9-132">Создайте новый проект.</span><span class="sxs-lookup"><span data-stu-id="a97f9-132">Create a new project.</span></span>
1. <span data-ttu-id="a97f9-133">В разделе **.NET Core** на боковой панели выберите **Приложение**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-133">Select **App** under **.NET Core** in the sidebar.</span></span>
1. <span data-ttu-id="a97f9-134">В разделе **ASP.NET Core** выберите **Рабочая роль**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-134">Select **Worker** under **ASP.NET Core**.</span></span> <span data-ttu-id="a97f9-135">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-135">Select **Next**.</span></span>
1. <span data-ttu-id="a97f9-136">Для параметра **Требуемая версия .NET Framework** выберите **.NET Core 3.0**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-136">Select **.NET Core 3.0** for the **Target Framework**.</span></span> <span data-ttu-id="a97f9-137">Выберите **Далее**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-137">Select **Next**.</span></span>
1. <span data-ttu-id="a97f9-138">Введите имя в поле **Имя проекта**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-138">Provide a name in the **Project Name** field.</span></span> <span data-ttu-id="a97f9-139">Выберите **Создать**.</span><span class="sxs-lookup"><span data-stu-id="a97f9-139">Select **Create**.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="a97f9-140">Интерфейс командной строки .NET Core</span><span class="sxs-lookup"><span data-stu-id="a97f9-140">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="a97f9-141">Используйте шаблон службы рабочей роли (`worker`) с командой [dotnet new](/dotnet/core/tools/dotnet-new) из командной оболочки.</span><span class="sxs-lookup"><span data-stu-id="a97f9-141">Use the Worker Service (`worker`) template with the [dotnet new](/dotnet/core/tools/dotnet-new) command from a command shell.</span></span> <span data-ttu-id="a97f9-142">В приведенном ниже примере создается приложение службы рабочей роли с именем `ContosoWorker`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-142">In the following example, a Worker Service app is created named `ContosoWorker`.</span></span> <span data-ttu-id="a97f9-143">Папка для приложения `ContosoWorker` создается автоматически при выполнении команды.</span><span class="sxs-lookup"><span data-stu-id="a97f9-143">A folder for the `ContosoWorker` app is created automatically when the command is executed.</span></span>

```dotnetcli
dotnet new worker -o ContosoWorker
```

---

## <a name="package"></a><span data-ttu-id="a97f9-144">Пакет</span><span class="sxs-lookup"><span data-stu-id="a97f9-144">Package</span></span>

<span data-ttu-id="a97f9-145">Ссылка на пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) добавляется неявно для приложений ASP.NET Core.</span><span class="sxs-lookup"><span data-stu-id="a97f9-145">A package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package is added implicitly for ASP.NET Core apps.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a97f9-146">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="a97f9-146">IHostedService interface</span></span>

<span data-ttu-id="a97f9-147">Интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService> определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="a97f9-147">The <xref:Microsoft.Extensions.Hosting.IHostedService> interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a97f9-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-148">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="a97f9-149">*Первым* вызывается `StartAsync`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-149">`StartAsync` is called *before*:</span></span>

  * <span data-ttu-id="a97f9-150">Настраивается конвейер обработки запросов приложения (`Startup.Configure`).</span><span class="sxs-lookup"><span data-stu-id="a97f9-150">The app's request processing pipeline is configured (`Startup.Configure`).</span></span>
  * <span data-ttu-id="a97f9-151">Запускается сервер и активируется [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="a97f9-151">The server is started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span>

  <span data-ttu-id="a97f9-152">Поведение по умолчанию можно изменить таким образом, чтобы `StartAsync` размещенной службы выполнялся после настройки конвейера приложения и вызова `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-152">The default behavior can be changed so that the hosted service's `StartAsync` runs after the app's pipeline has been configured and `ApplicationStarted` is called.</span></span> <span data-ttu-id="a97f9-153">Чтобы изменить поведение по умолчанию, добавьте размещенную службу (`VideosWatcher` в следующем примере) после вызова `ConfigureWebHostDefaults`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-153">To change the default behavior, add the hosted service (`VideosWatcher` in the following example) after calling `ConfigureWebHostDefaults`:</span></span>

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

* <span data-ttu-id="a97f9-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="a97f9-154">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a97f9-155">`StopAsync` содержит логику для завершения фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-155">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="a97f9-156">Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a97f9-156">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="a97f9-157">Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным.</span><span class="sxs-lookup"><span data-stu-id="a97f9-157">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="a97f9-158">При запросе отмены происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="a97f9-158">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="a97f9-159">должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;</span><span class="sxs-lookup"><span data-stu-id="a97f9-159">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="a97f9-160">должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-160">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="a97f9-161">Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.</span><span class="sxs-lookup"><span data-stu-id="a97f9-161">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="a97f9-162">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="a97f9-162">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="a97f9-163">Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.</span><span class="sxs-lookup"><span data-stu-id="a97f9-163">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="a97f9-164">Чтобы увеличить время ожидания завершения работы по умолчанию (пять секунд), установите следующие значения:</span><span class="sxs-lookup"><span data-stu-id="a97f9-164">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="a97f9-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла.</span><span class="sxs-lookup"><span data-stu-id="a97f9-165"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="a97f9-166">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-166">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="a97f9-167">Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла.</span><span class="sxs-lookup"><span data-stu-id="a97f9-167">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="a97f9-168">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-168">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="a97f9-169">Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="a97f9-169">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="a97f9-170">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="a97f9-170">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="backgroundservice"></a><span data-ttu-id="a97f9-171">BackgroundService</span><span class="sxs-lookup"><span data-stu-id="a97f9-171">BackgroundService</span></span>

<span data-ttu-id="a97f9-172">`BackgroundService` — это базовый класс для реализации долго выполняющегося интерфейса <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-172">`BackgroundService` is a base class for implementing a long running <xref:Microsoft.Extensions.Hosting.IHostedService>.</span></span> <span data-ttu-id="a97f9-173">`BackgroundService` определяет два метода для фоновых операций:</span><span class="sxs-lookup"><span data-stu-id="a97f9-173">`BackgroundService` defines two methods for background operations:</span></span>

* <span data-ttu-id="a97f9-174">При запуске <xref:Microsoft.Extensions.Hosting.IHostedService> вызывается `ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-174">`ExecuteAsync(CancellationToken stoppingToken)` &ndash; `ExecuteAsync` Called when the <xref:Microsoft.Extensions.Hosting.IHostedService> starts.</span></span> <span data-ttu-id="a97f9-175">Реализация должна возвращать значение `Task`, представляющее время существования длительно выполняемых операций.</span><span class="sxs-lookup"><span data-stu-id="a97f9-175">The implementation should return a `Task` that represents the lifetime of the long running operations performed.</span></span> <span data-ttu-id="a97f9-176">`stoppingToken` активируется при вызове [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*).</span><span class="sxs-lookup"><span data-stu-id="a97f9-176">The `stoppingToken` Triggered when [IHostedService.StopAsync](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) is called.</span></span>
* <span data-ttu-id="a97f9-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` запускается, когда происходит нормальное завершение работы узла приложения.</span><span class="sxs-lookup"><span data-stu-id="a97f9-177">`StopAsync(CancellationToken stoppingToken)` &ndash; `StopAsync` is triggered when the application host is performing a graceful shutdown.</span></span> <span data-ttu-id="a97f9-178">`stoppingToken` указывает, что процесс завершения работы больше не должен быть корректным.</span><span class="sxs-lookup"><span data-stu-id="a97f9-178">The `stoppingToken` indicates that the shutdown process should no longer be graceful.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a97f9-179">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="a97f9-179">Timed background tasks</span></span>

<span data-ttu-id="a97f9-180">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="a97f9-180">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="a97f9-181">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-181">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a97f9-182">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-182">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=16-18,34,41)]

<span data-ttu-id="a97f9-183">Служба регистрируется в `IHostBuilder.ConfigureServices` (*Program.cs*) с использованием метода расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-183">The service is registered in `IHostBuilder.ConfigureServices` (*Program.cs*) with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a97f9-184">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="a97f9-184">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a97f9-185">Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `BackgroundService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="a97f9-185">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within a `BackgroundService`, create a scope.</span></span> <span data-ttu-id="a97f9-186">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="a97f9-186">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a97f9-187">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-187">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a97f9-188">В следующем примере:</span><span class="sxs-lookup"><span data-stu-id="a97f9-188">In the following example:</span></span>

* <span data-ttu-id="a97f9-189">Служба является асинхронной.</span><span class="sxs-lookup"><span data-stu-id="a97f9-189">The service is asynchronous.</span></span> <span data-ttu-id="a97f9-190">Метод `DoWork` возвращает значение `Task`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-190">The `DoWork` method returns a `Task`.</span></span> <span data-ttu-id="a97f9-191">В демонстрационных целях в методе `DoWork` ожидается задержка в десять секунд.</span><span class="sxs-lookup"><span data-stu-id="a97f9-191">For demonstration purposes, a delay of ten seconds is awaited in the `DoWork` method.</span></span>
* <span data-ttu-id="a97f9-192">В службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-192">An <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a97f9-193">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-193">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method.</span></span> <span data-ttu-id="a97f9-194">`DoWork` возвращает объект `Task`, ожидаемый в `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-194">`DoWork` returns a `Task`, which is awaited in `ExecuteAsync`:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=19,22-35)]

<span data-ttu-id="a97f9-195">Службы регистрируются в `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a97f9-195">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="a97f9-196">Размещенная служба регистрируется с использованием метода расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-196">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a97f9-197">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="a97f9-197">Queued background tasks</span></span>

<span data-ttu-id="a97f9-198">Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="a97f9-198">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a97f9-199">В следующем примере `QueueHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-199">In the following `QueueHostedService` example:</span></span>

* <span data-ttu-id="a97f9-200">Метод `BackgroundProcessing` возвращает объект `Task`, ожидаемый в `ExecuteAsync`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-200">The `BackgroundProcessing` method returns a `Task`, which is awaited in `ExecuteAsync`.</span></span>
* <span data-ttu-id="a97f9-201">Фоновые задачи в очереди выводятся из очереди и выполняются в `BackgroundProcessing`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-201">Background tasks in the queue are dequeued and executed in `BackgroundProcessing`.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=28,39-40,44)]

<span data-ttu-id="a97f9-202">Служба `MonitorLoop` обрабатывает задачи постановки в очередь для размещенной службы при выборе на устройстве ввода ключа `w`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-202">A `MonitorLoop` service handles enqueuing tasks for the hosted service whenever the `w` key is selected on an input device:</span></span>

* <span data-ttu-id="a97f9-203">В службу `MonitorLoop` внедряется `IBackgroundTaskQueue`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-203">The `IBackgroundTaskQueue` is injected into the `MonitorLoop` service.</span></span>
* <span data-ttu-id="a97f9-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="a97f9-204">`IBackgroundTaskQueue.QueueBackgroundWorkItem` is called to enqueue a work item.</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Services/MonitorLoop.cs?name=snippet_Monitor&highlight=7,33)]

<span data-ttu-id="a97f9-205">Службы регистрируются в `IHostBuilder.ConfigureServices` (*Program.cs*).</span><span class="sxs-lookup"><span data-stu-id="a97f9-205">The services are registered in `IHostBuilder.ConfigureServices` (*Program.cs*).</span></span> <span data-ttu-id="a97f9-206">Размещенная служба регистрируется с использованием метода расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-206">The hosted service is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/3.x/BackgroundTasksSample/Program.cs?name=snippet3)]

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a97f9-207">В ASP.NET Core фоновые задачи реализуются как *размещенные службы*.</span><span class="sxs-lookup"><span data-stu-id="a97f9-207">In ASP.NET Core, background tasks can be implemented as *hosted services*.</span></span> <span data-ttu-id="a97f9-208">Размещенная служба — это класс с логикой фоновой задачи, реализующий интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-208">A hosted service is a class with background task logic that implements the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a97f9-209">Этот раздел содержит три примера размещенных служб:</span><span class="sxs-lookup"><span data-stu-id="a97f9-209">This topic provides three hosted service examples:</span></span>

* <span data-ttu-id="a97f9-210">Фоновая задача, которая выполняется по таймеру.</span><span class="sxs-lookup"><span data-stu-id="a97f9-210">Background task that runs on a timer.</span></span>
* <span data-ttu-id="a97f9-211">Размещенная служба, которая активирует [службу с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes).</span><span class="sxs-lookup"><span data-stu-id="a97f9-211">Hosted service that activates a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes).</span></span> <span data-ttu-id="a97f9-212">Служба с заданной областью может использовать [внедрение зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="a97f9-212">The scoped service can use [dependency injection (DI)](xref:fundamentals/dependency-injection)</span></span>
* <span data-ttu-id="a97f9-213">Очередь фоновых задач, которые выполняются последовательно.</span><span class="sxs-lookup"><span data-stu-id="a97f9-213">Queued background tasks that run sequentially.</span></span>

<span data-ttu-id="a97f9-214">[Просмотреть или скачать образец кода](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="a97f9-214">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/fundamentals/host/hosted-services/samples/) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="a97f9-215">Пример приложения предоставляется в двух версиях:</span><span class="sxs-lookup"><span data-stu-id="a97f9-215">The sample app is provided in two versions:</span></span>

* <span data-ttu-id="a97f9-216">Веб-узел &ndash; удобно использовать для размещения веб-приложений.</span><span class="sxs-lookup"><span data-stu-id="a97f9-216">Web Host &ndash; Web Host is useful for hosting web apps.</span></span> <span data-ttu-id="a97f9-217">Пример кода, приведенный в этой статье, относится к этой версии примера приложения.</span><span class="sxs-lookup"><span data-stu-id="a97f9-217">The example code shown in this topic is from Web Host version of the sample.</span></span> <span data-ttu-id="a97f9-218">Дополнительные сведения см. в статье о [веб-узле](xref:fundamentals/host/web-host).</span><span class="sxs-lookup"><span data-stu-id="a97f9-218">For more information, see the [Web Host](xref:fundamentals/host/web-host) topic.</span></span>
* <span data-ttu-id="a97f9-219">Универсальный узел &ndash; — новая возможность в ASP.NET Core 2.1.</span><span class="sxs-lookup"><span data-stu-id="a97f9-219">Generic Host &ndash; Generic Host is new in ASP.NET Core 2.1.</span></span> <span data-ttu-id="a97f9-220">Дополнительные сведения см. в статье об [универсальном узле](xref:fundamentals/host/generic-host).</span><span class="sxs-lookup"><span data-stu-id="a97f9-220">For more information, see the [Generic Host](xref:fundamentals/host/generic-host) topic.</span></span>

## <a name="package"></a><span data-ttu-id="a97f9-221">Пакет</span><span class="sxs-lookup"><span data-stu-id="a97f9-221">Package</span></span>

<span data-ttu-id="a97f9-222">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или добавьте ссылку на пакет в пакет [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting).</span><span class="sxs-lookup"><span data-stu-id="a97f9-222">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.Extensions.Hosting](https://www.nuget.org/packages/Microsoft.Extensions.Hosting) package.</span></span>

## <a name="ihostedservice-interface"></a><span data-ttu-id="a97f9-223">Интерфейс IHostedService</span><span class="sxs-lookup"><span data-stu-id="a97f9-223">IHostedService interface</span></span>

<span data-ttu-id="a97f9-224">Размещенные службы реализуют интерфейс <xref:Microsoft.Extensions.Hosting.IHostedService>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-224">Hosted services implement the <xref:Microsoft.Extensions.Hosting.IHostedService> interface.</span></span> <span data-ttu-id="a97f9-225">Этот интерфейс определяет два метода для объектов, которые управляются узлом:</span><span class="sxs-lookup"><span data-stu-id="a97f9-225">The interface defines two methods for objects that are managed by the host:</span></span>

* <span data-ttu-id="a97f9-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` содержит логику для запуска фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-226">[StartAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StartAsync*) &ndash; `StartAsync` contains the logic to start the background task.</span></span> <span data-ttu-id="a97f9-227">При использовании [Web Host](xref:fundamentals/host/web-host) `StartAsync` вызывается после запуска сервера и активации [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*).</span><span class="sxs-lookup"><span data-stu-id="a97f9-227">When using the [Web Host](xref:fundamentals/host/web-host), `StartAsync` is called after the server has started and [IApplicationLifetime.ApplicationStarted](xref:Microsoft.AspNetCore.Hosting.IApplicationLifetime.ApplicationStarted*) is triggered.</span></span> <span data-ttu-id="a97f9-228">При использовании [универсального узла](xref:fundamentals/host/generic-host) `StartAsync` вызывается до активации `ApplicationStarted`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-228">When using the [Generic Host](xref:fundamentals/host/generic-host), `StartAsync` is called before `ApplicationStarted` is triggered.</span></span>

* <span data-ttu-id="a97f9-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; запускается, когда происходит нормальное завершение работы узла.</span><span class="sxs-lookup"><span data-stu-id="a97f9-229">[StopAsync(CancellationToken)](xref:Microsoft.Extensions.Hosting.IHostedService.StopAsync*) &ndash; Triggered when the host is performing a graceful shutdown.</span></span> <span data-ttu-id="a97f9-230">`StopAsync` содержит логику для завершения фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-230">`StopAsync` contains the logic to end the background task.</span></span> <span data-ttu-id="a97f9-231">Реализуйте <xref:System.IDisposable> и [методы завершения (деструкторы)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) для освобождения неуправляемых ресурсов.</span><span class="sxs-lookup"><span data-stu-id="a97f9-231">Implement <xref:System.IDisposable> and [finalizers (destructors)](/dotnet/csharp/programming-guide/classes-and-structs/destructors) to dispose of any unmanaged resources.</span></span>

  <span data-ttu-id="a97f9-232">Токен отмены использует заданное по умолчанию 5-секундное время ожидания, указывающее, что процесс завершения работы больше не должен быть нормальным.</span><span class="sxs-lookup"><span data-stu-id="a97f9-232">The cancellation token has a default five second timeout to indicate that the shutdown process should no longer be graceful.</span></span> <span data-ttu-id="a97f9-233">При запросе отмены происходит следующее:</span><span class="sxs-lookup"><span data-stu-id="a97f9-233">When cancellation is requested on the token:</span></span>

  * <span data-ttu-id="a97f9-234">должны быть прерваны все оставшиеся фоновые операции, выполняемые приложением;</span><span class="sxs-lookup"><span data-stu-id="a97f9-234">Any remaining background operations that the app is performing should be aborted.</span></span>
  * <span data-ttu-id="a97f9-235">должны быть незамедлительно возвращены все методы, вызываемые в `StopAsync`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-235">Any methods called in `StopAsync` should return promptly.</span></span>

  <span data-ttu-id="a97f9-236">Однако после запроса отмены выполнение задач не прекращается &mdash; вызывающий объект ожидает завершения всех задач.</span><span class="sxs-lookup"><span data-stu-id="a97f9-236">However, tasks aren't abandoned after cancellation is requested&mdash;the caller awaits all tasks to complete.</span></span>

  <span data-ttu-id="a97f9-237">Если приложение завершает работу неожиданно (например, при сбое процесса приложения), `StopAsync` может не вызываться.</span><span class="sxs-lookup"><span data-stu-id="a97f9-237">If the app shuts down unexpectedly (for example, the app's process fails), `StopAsync` might not be called.</span></span> <span data-ttu-id="a97f9-238">Поэтому вызов методов или выполнение операций в `StopAsync` может быть невозможным.</span><span class="sxs-lookup"><span data-stu-id="a97f9-238">Therefore, any methods called or operations conducted in `StopAsync` might not occur.</span></span>

  <span data-ttu-id="a97f9-239">Чтобы увеличить время ожидания завершения работы по умолчанию (пять секунд), установите следующие значения:</span><span class="sxs-lookup"><span data-stu-id="a97f9-239">To extend the default five second shutdown timeout, set:</span></span>

  * <span data-ttu-id="a97f9-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> при использовании универсального узла.</span><span class="sxs-lookup"><span data-stu-id="a97f9-240"><xref:Microsoft.Extensions.Hosting.HostOptions.ShutdownTimeout*> when using Generic Host.</span></span> <span data-ttu-id="a97f9-241">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/generic-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-241">For more information, see <xref:fundamentals/host/generic-host#shutdown-timeout>.</span></span>
  * <span data-ttu-id="a97f9-242">Параметр конфигурации узла для времени ожидания завершения работы при использовании веб-узла.</span><span class="sxs-lookup"><span data-stu-id="a97f9-242">Shutdown timeout host configuration setting when using Web Host.</span></span> <span data-ttu-id="a97f9-243">Дополнительные сведения можно найти по адресу: <xref:fundamentals/host/web-host#shutdown-timeout>.</span><span class="sxs-lookup"><span data-stu-id="a97f9-243">For more information, see <xref:fundamentals/host/web-host#shutdown-timeout>.</span></span>

<span data-ttu-id="a97f9-244">Размещенная служба активируется при запуске приложения и нормально завершает работу при завершении работы приложения.</span><span class="sxs-lookup"><span data-stu-id="a97f9-244">The hosted service is activated once at app startup and gracefully shut down at app shutdown.</span></span> <span data-ttu-id="a97f9-245">Если во время выполнения задачи в фоновом режиме возникает ошибка, необходимо вызвать `Dispose`, даже если `StopAsync` не вызывается.</span><span class="sxs-lookup"><span data-stu-id="a97f9-245">If an error is thrown during background task execution, `Dispose` should be called even if `StopAsync` isn't called.</span></span>

## <a name="timed-background-tasks"></a><span data-ttu-id="a97f9-246">Фоновые задачи с заданным временем</span><span class="sxs-lookup"><span data-stu-id="a97f9-246">Timed background tasks</span></span>

<span data-ttu-id="a97f9-247">Для фоновых задач с заданным временем используется класс [System.Threading.Timer](xref:System.Threading.Timer).</span><span class="sxs-lookup"><span data-stu-id="a97f9-247">A timed background task makes use of the [System.Threading.Timer](xref:System.Threading.Timer) class.</span></span> <span data-ttu-id="a97f9-248">Таймер запускает метод `DoWork` задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-248">The timer triggers the task's `DoWork` method.</span></span> <span data-ttu-id="a97f9-249">Таймер отключается методом `StopAsync` и удаляется при удалении контейнера службы методом `Dispose`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-249">The timer is disabled on `StopAsync` and disposed when the service container is disposed on `Dispose`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/TimedHostedService.cs?name=snippet1&highlight=15-16,30,37)]

<span data-ttu-id="a97f9-250">Служба зарегистрирована в `Startup.ConfigureServices` с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-250">The service is registered in `Startup.ConfigureServices` with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet1)]

## <a name="consuming-a-scoped-service-in-a-background-task"></a><span data-ttu-id="a97f9-251">Использование службы с заданной областью в фоновой задаче</span><span class="sxs-lookup"><span data-stu-id="a97f9-251">Consuming a scoped service in a background task</span></span>

<span data-ttu-id="a97f9-252">Чтобы использовать [службы с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes) в `IHostedService`, создайте область.</span><span class="sxs-lookup"><span data-stu-id="a97f9-252">To use [scoped services](xref:fundamentals/dependency-injection#service-lifetimes) within an `IHostedService`, create a scope.</span></span> <span data-ttu-id="a97f9-253">Для размещенной службы по умолчанию не создается область.</span><span class="sxs-lookup"><span data-stu-id="a97f9-253">No scope is created for a hosted service by default.</span></span>

<span data-ttu-id="a97f9-254">Служба фоновой задачи с заданной областью содержит логику фоновой задачи.</span><span class="sxs-lookup"><span data-stu-id="a97f9-254">The scoped background task service contains the background task's logic.</span></span> <span data-ttu-id="a97f9-255">В следующем примере в службу вставляется <xref:Microsoft.Extensions.Logging.ILogger>:</span><span class="sxs-lookup"><span data-stu-id="a97f9-255">In the following example, an <xref:Microsoft.Extensions.Logging.ILogger> is injected into the service:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ScopedProcessingService.cs?name=snippet1)]

<span data-ttu-id="a97f9-256">Размещенная служба создает область для разрешения службы фоновой задачи с заданной областью, чтобы вызвать ее метод `DoWork`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-256">The hosted service creates a scope to resolve the scoped background task service to call its `DoWork` method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/ConsumeScopedServiceHostedService.cs?name=snippet1&highlight=29-36)]

<span data-ttu-id="a97f9-257">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-257">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a97f9-258">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-258">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet2)]

## <a name="queued-background-tasks"></a><span data-ttu-id="a97f9-259">Фоновые задачи в очереди</span><span class="sxs-lookup"><span data-stu-id="a97f9-259">Queued background tasks</span></span>

<span data-ttu-id="a97f9-260">Очередь фоновых задач основывается на методе <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> в .NET 4.x ([предварительно запланировано встроить в ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span><span class="sxs-lookup"><span data-stu-id="a97f9-260">A background task queue is based on the .NET 4.x <xref:System.Web.Hosting.HostingEnvironment.QueueBackgroundWorkItem*> ([tentatively scheduled to be built-in for ASP.NET Core](https://github.com/aspnet/Hosting/issues/1280)):</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/BackgroundTaskQueue.cs?name=snippet1)]

<span data-ttu-id="a97f9-261">В `QueueHostedService` фоновые задачи в очереди из очереди выводятся из очереди и выполняются в качестве <xref:Microsoft.Extensions.Hosting.BackgroundService> — базового класса для реализации длительного выполнения `IHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-261">In `QueueHostedService`, background tasks in the queue are dequeued and executed as a <xref:Microsoft.Extensions.Hosting.BackgroundService>, which is a base class for implementing a long running `IHostedService`:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Services/QueuedHostedService.cs?name=snippet1&highlight=21,25)]

<span data-ttu-id="a97f9-262">Службы регистрируются в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-262">The services are registered in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="a97f9-263">Реализация `IHostedService` зарегистрирована с методом расширения `AddHostedService`:</span><span class="sxs-lookup"><span data-stu-id="a97f9-263">The `IHostedService` implementation is registered with the `AddHostedService` extension method:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Startup.cs?name=snippet3)]

<span data-ttu-id="a97f9-264">В классе модели страницы индексов:</span><span class="sxs-lookup"><span data-stu-id="a97f9-264">In the Index page model class:</span></span>

* <span data-ttu-id="a97f9-265">`IBackgroundTaskQueue` вставляется в конструктор и присваивается `Queue`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-265">The `IBackgroundTaskQueue` is injected into the constructor and assigned to `Queue`.</span></span>
* <span data-ttu-id="a97f9-266"><xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> вставляется и присваивается `_serviceScopeFactory`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-266">An <xref:Microsoft.Extensions.DependencyInjection.IServiceScopeFactory> is injected and assigned to `_serviceScopeFactory`.</span></span> <span data-ttu-id="a97f9-267">Фабрика используется для создания экземпляров <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, который используется для создания служб с заданной областью.</span><span class="sxs-lookup"><span data-stu-id="a97f9-267">The factory is used to create instances of <xref:Microsoft.Extensions.DependencyInjection.IServiceScope>, which is used to create services within a scope.</span></span> <span data-ttu-id="a97f9-268">Для использования `AppDbContext` приложения создается область ([служба с заданной областью](xref:fundamentals/dependency-injection#service-lifetimes)) для записи данных из базы данных в `IBackgroundTaskQueue` (отдельная служба).</span><span class="sxs-lookup"><span data-stu-id="a97f9-268">A scope is created in order to use the app's `AppDbContext` (a [scoped service](xref:fundamentals/dependency-injection#service-lifetimes)) to write database records in the `IBackgroundTaskQueue` (a singleton service).</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet1)]

<span data-ttu-id="a97f9-269">Если нажать кнопку **Добавить задачу** на странице индексов, выполняется метод `OnPostAddTask`.</span><span class="sxs-lookup"><span data-stu-id="a97f9-269">When the **Add Task** button is selected on the Index page, the `OnPostAddTask` method is executed.</span></span> <span data-ttu-id="a97f9-270">`QueueBackgroundWorkItem` вызывается для постановки рабочего элемента в очередь:</span><span class="sxs-lookup"><span data-stu-id="a97f9-270">`QueueBackgroundWorkItem` is called to enqueue a work item:</span></span>

[!code-csharp[](hosted-services/samples/2.x/BackgroundTasksSample/Pages/Index.cshtml.cs?name=snippet2)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="a97f9-271">Дополнительные ресурсы</span><span class="sxs-lookup"><span data-stu-id="a97f9-271">Additional resources</span></span>

* [<span data-ttu-id="a97f9-272">Реализация фоновых задач в микрослужбах с помощью IHostedService и класса BackgroundService</span><span class="sxs-lookup"><span data-stu-id="a97f9-272">Implement background tasks in microservices with IHostedService and the BackgroundService class</span></span>](/dotnet/standard/microservices-architecture/multi-container-microservice-net-applications/background-tasks-with-ihostedservice)
* <xref:System.Threading.Timer>
