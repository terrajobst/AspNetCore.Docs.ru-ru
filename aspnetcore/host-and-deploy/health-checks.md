---
title: Проверки работоспособности в ASP.NET Core
author: guardrex
description: Узнайте, как настроить проверки работоспособности для инфраструктуры ASP.NET Core, например приложений и баз данных.
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: host-and-deploy/health-checks
ms.openlocfilehash: e186a3cb484035199a8f355540c3e985db87ad98
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: ru-RU
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248579"
---
# <a name="health-checks-in-aspnet-core"></a><span data-ttu-id="26360-103">Проверки работоспособности в ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="26360-103">Health checks in ASP.NET Core</span></span>

<span data-ttu-id="26360-104">Авторы: [Люк Лэтем](https://github.com/guardrex) (Luke Latham) и [Гленн Кондрон](https://github.com/glennc) (Glenn Condron)</span><span class="sxs-lookup"><span data-stu-id="26360-104">By [Luke Latham](https://github.com/guardrex) and [Glenn Condron](https://github.com/glennc)</span></span>

<span data-ttu-id="26360-105">ASP.NET Core предоставляет ПО промежуточного слоя и библиотеки для создания отчетов о работоспособности компонентов инфраструктуры приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-105">ASP.NET Core offers Health Check Middleware and libraries for reporting the health of app infrastructure components.</span></span>

<span data-ttu-id="26360-106">Проверки работоспособности предоставляются приложением как конечные точки HTTP.</span><span class="sxs-lookup"><span data-stu-id="26360-106">Health checks are exposed by an app as HTTP endpoints.</span></span> <span data-ttu-id="26360-107">Конечные точки проверки работоспособности можно настроить для различных сценариев в реальном времени мониторинга:</span><span class="sxs-lookup"><span data-stu-id="26360-107">Health check endpoints can be configured for a variety of real-time monitoring scenarios:</span></span>

* <span data-ttu-id="26360-108">Проверки работоспособности можно использовать с оркестраторами контейнеров и подсистемами балансировки нагрузки, чтобы проверять состояние приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-108">Health probes can be used by container orchestrators and load balancers to check an app's status.</span></span> <span data-ttu-id="26360-109">Например, оркестратор контейнеров может реагировать на неуспешную проверку работоспособности, остановив последовательное развертывание или перезапустив контейнер.</span><span class="sxs-lookup"><span data-stu-id="26360-109">For example, a container orchestrator may respond to a failing health check by halting a rolling deployment or restarting a container.</span></span> <span data-ttu-id="26360-110">Балансировщик нагрузки может реагировать на неработоспособное приложение путем перенаправления трафика от неисправного экземпляра к работающему экземпляру.</span><span class="sxs-lookup"><span data-stu-id="26360-110">A load balancer might react to an unhealthy app by routing traffic away from the failing instance to a healthy instance.</span></span>
* <span data-ttu-id="26360-111">Использование памяти, диска и других ресурсов физического сервера можно отслеживать с точки зрения работоспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-111">Use of memory, disk, and other physical server resources can be monitored for healthy status.</span></span>
* <span data-ttu-id="26360-112">Проверки работоспособности позволяют проверять зависимости приложения, такие как базы данных и конечные точки внешних служб, чтобы убедиться в доступности и нормальной работе.</span><span class="sxs-lookup"><span data-stu-id="26360-112">Health checks can test an app's dependencies, such as databases and external service endpoints, to confirm availability and normal functioning.</span></span>

<span data-ttu-id="26360-113">[Просмотреть или скачать образец кода](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([как скачивать](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="26360-113">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/host-and-deploy/health-checks/samples) ([how to download](xref:index#how-to-download-a-sample))</span></span>

<span data-ttu-id="26360-114">Пример приложения включает примеры сценариев, описанных в этом разделе.</span><span class="sxs-lookup"><span data-stu-id="26360-114">The sample app includes examples of the scenarios described in this topic.</span></span> <span data-ttu-id="26360-115">Чтобы запустить пример приложения для заданного сценария, используйте команду [dotnet run](/dotnet/core/tools/dotnet-run) из папки проекта в командной строке.</span><span class="sxs-lookup"><span data-stu-id="26360-115">To run the sample app for a given scenario, use the [dotnet run](/dotnet/core/tools/dotnet-run) command from the project's folder in a command shell.</span></span> <span data-ttu-id="26360-116">См. файл *README.md* приложения и описания сценариев в этом разделе, чтобы получить сведения о том, как использовать пример приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-116">See the sample app's *README.md* file and the scenario descriptions in this topic for details on how to use the sample app.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="26360-117">Предварительные требования</span><span class="sxs-lookup"><span data-stu-id="26360-117">Prerequisites</span></span>

<span data-ttu-id="26360-118">Проверки работоспособности обычно используются в оркестраторе контейнеров или внешней службе мониторинга для проверки состояния приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-118">Health checks are usually used with an external monitoring service or container orchestrator to check the status of an app.</span></span> <span data-ttu-id="26360-119">Перед добавлением проверки работоспособности приложения выберите используемую систему мониторинга.</span><span class="sxs-lookup"><span data-stu-id="26360-119">Before adding health checks to an app, decide on which monitoring system to use.</span></span> <span data-ttu-id="26360-120">Система мониторинга определяет, какие виды проверки работоспособности создавать и как настроить их конечные точки.</span><span class="sxs-lookup"><span data-stu-id="26360-120">The monitoring system dictates what types of health checks to create and how to configure their endpoints.</span></span>

<span data-ttu-id="26360-121">Добавьте ссылку на [метапакет Microsoft.AspNetCore.App](xref:fundamentals/metapackage-app) или ссылку на пакет [Microsoft.AspNetCore.Diagnostics.HealthCheck](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks).</span><span class="sxs-lookup"><span data-stu-id="26360-121">Reference the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) or add a package reference to the [Microsoft.AspNetCore.Diagnostics.HealthChecks](https://www.nuget.org/packages/Microsoft.AspNetCore.Diagnostics.HealthChecks) package.</span></span>

<span data-ttu-id="26360-122">В примере приложения приведен код запуска для демонстрации проверки работоспособности для нескольких сценариев.</span><span class="sxs-lookup"><span data-stu-id="26360-122">The sample app provides startup code to demonstrate health checks for several scenarios.</span></span> <span data-ttu-id="26360-123">Сценарий [проверки базы данных](#database-probe) проверяет работоспособность подключения базы данных с помощью [BeatPulse](https://github.com/Xabaril/BeatPulse).</span><span class="sxs-lookup"><span data-stu-id="26360-123">The [database probe](#database-probe) scenario checks the health of a database connection using [BeatPulse](https://github.com/Xabaril/BeatPulse).</span></span> <span data-ttu-id="26360-124">Сценарий [проверки DbContext](#entity-framework-core-dbcontext-probe) проверяет базу данных с помощью EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="26360-124">The [DbContext probe](#entity-framework-core-dbcontext-probe) scenario checks a database using an EF Core `DbContext`.</span></span> <span data-ttu-id="26360-125">Чтобы изучить сценарии для баз данных, пример приложения выполняет следующие действия.</span><span class="sxs-lookup"><span data-stu-id="26360-125">To explore the database scenarios, the sample app:</span></span>

* <span data-ttu-id="26360-126">Создает базу данных и указывает ее строку подключения в файле приложения *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="26360-126">Creates a database and provides its connection string in the *appsettings.json* file.</span></span>
* <span data-ttu-id="26360-127">Содержит следующие ссылки на пакеты в своем файле проекта.</span><span class="sxs-lookup"><span data-stu-id="26360-127">Has the following package references in its project file:</span></span>
  * [<span data-ttu-id="26360-128">AspNetCore.HealthChecks.SqlServer</span><span class="sxs-lookup"><span data-stu-id="26360-128">AspNetCore.HealthChecks.SqlServer</span></span>](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/)
  * [<span data-ttu-id="26360-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span><span class="sxs-lookup"><span data-stu-id="26360-129">Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore</span></span>](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/)

> [!NOTE]
> <span data-ttu-id="26360-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) не поддерживается и не обслуживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="26360-130">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

<span data-ttu-id="26360-131">В другом сценарии проверки работоспособности демонстрируется способ фильтрации проверок работоспособности по порту управления.</span><span class="sxs-lookup"><span data-stu-id="26360-131">Another health check scenario demonstrates how to filter health checks to a management port.</span></span> <span data-ttu-id="26360-132">Пример приложения требует создать файл *Properties/launchSettings.json*, который включает в себя URL-адрес управления и порт управления.</span><span class="sxs-lookup"><span data-stu-id="26360-132">The sample app requires you to create a *Properties/launchSettings.json* file that includes the management URL and management port.</span></span> <span data-ttu-id="26360-133">Дополнительные сведения см. в разделе [Фильтр по портам](#filter-by-port).</span><span class="sxs-lookup"><span data-stu-id="26360-133">For more information, see the [Filter by port](#filter-by-port) section.</span></span>

## <a name="basic-health-probe"></a><span data-ttu-id="26360-134">Базовая проверка работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-134">Basic health probe</span></span>

<span data-ttu-id="26360-135">Для многих приложений достаточно конфигурации базовой проверки работоспособности, которая сообщает о доступности приложения для обработки запросов (*жизнеспособности*).</span><span class="sxs-lookup"><span data-stu-id="26360-135">For many apps, a basic health probe configuration that reports the app's availability to process requests (*liveness*) is sufficient to discover the status of the app.</span></span>

<span data-ttu-id="26360-136">Базовая конфигурация регистрирует службы проверки работоспособности и вызывает ПО промежуточного слоя для ответа о работоспособности на конечной точке по URL-адресу.</span><span class="sxs-lookup"><span data-stu-id="26360-136">The basic configuration registers health check services and calls the Health Check Middleware to respond at a URL endpoint with a health response.</span></span> <span data-ttu-id="26360-137">По умолчанию отдельные проверки работоспособности не регистрируются для тестирования какой-либо конкретной зависимости или подсистемы.</span><span class="sxs-lookup"><span data-stu-id="26360-137">By default, no specific health checks are registered to test any particular dependency or subsystem.</span></span> <span data-ttu-id="26360-138">Приложение считается исправным, если оно способно отвечать на запросы по URL-адресу конечной точки работоспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-138">The app is considered healthy if it's capable of responding at the health endpoint URL.</span></span> <span data-ttu-id="26360-139">Модуль записи ответа по умолчанию передает состояние (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) в виде обычного текста в ответе клиенту. Это может быть состояние [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) или [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="26360-139">The default response writer writes the status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) as a plaintext response back to the client, indicating either a [HealthStatus.Healthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus), [HealthStatus.Degraded](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) or [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) status.</span></span>

<span data-ttu-id="26360-140">Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26360-140">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="26360-141">Добавьте ПО промежуточного слоя в <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> в конвейере обработки запросов `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="26360-141">Add Health Check Middleware with <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the request processing pipeline of `Startup.Configure`.</span></span>

<span data-ttu-id="26360-142">В примере приложения конечная точка проверки работоспособности создается в `/health` (*BasicStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="26360-142">In the sample app, the health check endpoint is created at `/health` (*BasicStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/BasicStartup.cs?name=snippet1&highlight=5,10)]

<span data-ttu-id="26360-143">Чтобы запустить сценарий базовой конфигурации с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26360-143">To run the basic configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario basic
```

### <a name="docker-example"></a><span data-ttu-id="26360-144">Пример для Docker</span><span class="sxs-lookup"><span data-stu-id="26360-144">Docker example</span></span>

<span data-ttu-id="26360-145">[Docker](xref:host-and-deploy/docker/index) предлагает встроенную директиву `HEALTHCHECK`, которую можно вызвать для проверки состояния приложения, использующего базовую конфигурацию проверки работоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-145">[Docker](xref:host-and-deploy/docker/index) offers a built-in `HEALTHCHECK` directive that can be used to check the status of an app that uses the basic health check configuration:</span></span>

```
HEALTHCHECK CMD curl --fail http://localhost:5000/health || exit
```

## <a name="create-health-checks"></a><span data-ttu-id="26360-146">Создание проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-146">Create health checks</span></span>

<span data-ttu-id="26360-147">Проверки работоспособности создаются путем реализации интерфейса <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck>.</span><span class="sxs-lookup"><span data-stu-id="26360-147">Health checks are created by implementing the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface.</span></span> <span data-ttu-id="26360-148">Метод <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> возвращает `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>`, что означает состояние работоспособности `Healthy`, `Degraded` или `Unhealthy`.</span><span class="sxs-lookup"><span data-stu-id="26360-148">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck.CheckHealthAsync*> method returns a `Task<` <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> `>` that indicates the health as `Healthy`, `Degraded`, or `Unhealthy`.</span></span> <span data-ttu-id="26360-149">Результат записывается в ответ в виде обычного текста с кодом состояния, который можно настроить (конфигурация описана в разделе [Варианты проверки работоспособности](#health-check-options)).</span><span class="sxs-lookup"><span data-stu-id="26360-149">The result is written as a plaintext response with a configurable status code (configuration is described in the [Health check options](#health-check-options) section).</span></span> <span data-ttu-id="26360-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> также может возвращать необязательные пары "ключ-значение".</span><span class="sxs-lookup"><span data-stu-id="26360-150"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> can also return optional key-value pairs.</span></span>

### <a name="example-health-check"></a><span data-ttu-id="26360-151">Пример проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-151">Example health check</span></span>

<span data-ttu-id="26360-152">Следующий класс `ExampleHealthCheck` демонстрирует структуру проверки работоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-152">The following `ExampleHealthCheck` class demonstrates the layout of a health check:</span></span>

```csharp
public class ExampleHealthCheck : IHealthCheck
{
    public ExampleHealthCheck()
    {
        // Use dependency injection (DI) to supply any required services to the
        // health check.
    }

    public Task<HealthCheckResult> CheckHealthAsync(
        HealthCheckContext context, 
        CancellationToken cancellationToken = default(CancellationToken))
    {
        // Execute health check logic here. This example sets a dummy
        // variable to true.
        var healthCheckResultHealthy = true;

        if (healthCheckResultHealthy)
        {
            return Task.FromResult(
                HealthCheckResult.Healthy("The check indicates a healthy result."));
        }

        return Task.FromResult(
            HealthCheckResult.Unhealthy("The check indicates an unhealthy result."));
    }
}
```

### <a name="register-health-check-services"></a><span data-ttu-id="26360-153">Зарегистрируйте службы проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-153">Register health check services</span></span>

<span data-ttu-id="26360-154">Тип `ExampleHealthCheck` будет добавлен к службам проверки работоспособности через <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span><span class="sxs-lookup"><span data-stu-id="26360-154">The `ExampleHealthCheck` type is added to health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck<ExampleHealthCheck>("example_health_check");
}
```

<span data-ttu-id="26360-155">Перегрузка <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, показанная в следующем примере, устанавливает состояние сбоя (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) для отчета в ситуации, когда проверка работоспособности сообщает о сбое.</span><span class="sxs-lookup"><span data-stu-id="26360-155">The <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> overload shown in the following example sets the failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>) to report when the health check reports a failure.</span></span> <span data-ttu-id="26360-156">Если состояние сбоя имеет значение `null` (по умолчанию), сообщается [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus).</span><span class="sxs-lookup"><span data-stu-id="26360-156">If the failure status is set to `null` (default), [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported.</span></span> <span data-ttu-id="26360-157">Эта перегрузка — полезный сценарий для авторов библиотек, где состояние сбоя от библиотеки особо учитывается приложением при сбое проверки работоспособности, если реализация проверки работоспособности учитывает этот параметр.</span><span class="sxs-lookup"><span data-stu-id="26360-157">This overload is a useful scenario for library authors, where the failure status indicated by the library is enforced by the app when a health check failure occurs if the health check implementation honors the setting.</span></span>

<span data-ttu-id="26360-158">*Теги* можно использовать для фильтрации проверок работоспособности (описаны далее в разделе [Фильтрация проверок работоспособности](#filter-health-checks)).</span><span class="sxs-lookup"><span data-stu-id="26360-158">*Tags* can be used to filter health checks (described further in the [Filter health checks](#filter-health-checks) section).</span></span>

```csharp
services.AddHealthChecks()
    .AddCheck<ExampleHealthCheck>(
        "example_health_check", 
        failureStatus: HealthStatus.Degraded, 
        tags: new[] { "example" });
```

<span data-ttu-id="26360-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> также может выполнять лямбда-функции.</span><span class="sxs-lookup"><span data-stu-id="26360-159"><xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> can also execute a lambda function.</span></span> <span data-ttu-id="26360-160">В следующем примере имя проверки работоспособности указано как `Example`; проверка всегда возвращает работоспособное состояние:</span><span class="sxs-lookup"><span data-stu-id="26360-160">In the following example, the health check name is specified as `Example` and the check always returns a healthy state:</span></span>

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Example", () => 
            HealthCheckResult.Healthy("Example is OK!"), tags: new[] { "example" })
}
```

### <a name="use-health-checks-middleware"></a><span data-ttu-id="26360-161">Используйте ПО промежуточного слоя для проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-161">Use Health Checks Middleware</span></span>

<span data-ttu-id="26360-162">В `Startup.Configure` вызовите <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> в конвейере обработки с URL-адресом конечной точки или относительным путем:</span><span class="sxs-lookup"><span data-stu-id="26360-162">In `Startup.Configure`, call <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> in the processing pipeline with the endpoint URL or relative path:</span></span>

```csharp
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health");
}
```

<span data-ttu-id="26360-163">Если проверки работоспособности должны прослушивать определенный порт, используйте перегрузку <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, чтобы задать порт (описаны далее в разделе [Фильтр по портам](#filter-by-port)):</span><span class="sxs-lookup"><span data-stu-id="26360-163">If the health checks should listen on a specific port, use an overload of <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> to set the port (described further in the [Filter by port](#filter-by-port) section):</span></span>

```csharp
app.UseHealthChecks("/health", port: 8000);
```

<span data-ttu-id="26360-164">ПО промежуточного слоя — это *терминальное ПО промежуточного слоя* в конвейере обработки запросов приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-164">Health Checks Middleware is a *terminal middleware* in the app's request processing pipeline.</span></span> <span data-ttu-id="26360-165">Первая конечная точка проверки работоспособности, которая обнаружила точное соответствие URL-адресу запроса, выполняется и пропускает остаток конвейера ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="26360-165">The first health check endpoint encountered that's an exact match to the request URL executes and short-circuits the rest of the middleware pipeline.</span></span> <span data-ttu-id="26360-166">В случае пропуска ПО промежуточного слоя после совпавшей проверки работоспособности не выполняется.</span><span class="sxs-lookup"><span data-stu-id="26360-166">When short-circuiting occurs, no middleware following the matched health check executes.</span></span>

## <a name="health-check-options"></a><span data-ttu-id="26360-167">Варианты проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-167">Health check options</span></span>

<span data-ttu-id="26360-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> предоставляет возможность настройки поведения для проверки работоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-168"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions> provide an opportunity to customize health check behavior:</span></span>

* [<span data-ttu-id="26360-169">Фильтрация проверок работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-169">Filter health checks</span></span>](#filter-health-checks)
* [<span data-ttu-id="26360-170">Настройка кода состояния HTTP</span><span class="sxs-lookup"><span data-stu-id="26360-170">Customize the HTTP status code</span></span>](#customize-the-http-status-code)
* [<span data-ttu-id="26360-171">Подавление заголовков кэша</span><span class="sxs-lookup"><span data-stu-id="26360-171">Suppress cache headers</span></span>](#suppress-cache-headers)
* [<span data-ttu-id="26360-172">Настройка выходных данных</span><span class="sxs-lookup"><span data-stu-id="26360-172">Customize output</span></span>](#customize-output)

### <a name="filter-health-checks"></a><span data-ttu-id="26360-173">Фильтрация проверок работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-173">Filter health checks</span></span>

<span data-ttu-id="26360-174">По умолчанию ПО промежуточного слоя для проверки работоспособности выполняет все зарегистрированные проверки работоспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-174">By default, Health Check Middleware runs all registered health checks.</span></span> <span data-ttu-id="26360-175">Чтобы выполнить подмножество проверок работоспособности, укажите функцию, которая возвращает логическое значение через параметр <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate>.</span><span class="sxs-lookup"><span data-stu-id="26360-175">To run a subset of health checks, provide a function that returns a boolean to the <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate> option.</span></span> <span data-ttu-id="26360-176">В следующем примере проверка работоспособности `Bar` отфильтровывается по тегу (`bar_tag`) в условном операторе функции, где `true` возвращается только в том случае, если свойство проверки работоспособности <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> соответствует `foo_tag` или `baz_tag`:</span><span class="sxs-lookup"><span data-stu-id="26360-176">In the following example, the `Bar` health check is filtered out by its tag (`bar_tag`) in the function's conditional statement, where `true` is only returned if the health check's <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckRegistration.Tags> property matches `foo_tag` or `baz_tag`:</span></span>

```csharp
using System.Threading.Tasks;
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void ConfigureServices(IServiceCollection services)
{
    services.AddHealthChecks()
        .AddCheck("Foo", () => 
            HealthCheckResult.Healthy("Foo is OK!"), tags: new[] { "foo_tag" })
        .AddCheck("Bar", () => 
            HealthCheckResult.Unhealthy("Bar is unhealthy!"), tags: new[] { "bar_tag" })
        .AddCheck("Baz", () => 
            HealthCheckResult.Healthy("Baz is OK!"), tags: new[] { "baz_tag" });
}

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // Filter out the 'Bar' health check. Only Foo and Baz execute.
        Predicate = (check) => check.Tags.Contains("foo_tag") || 
            check.Tags.Contains("baz_tag")
    });
}
```

### <a name="customize-the-http-status-code"></a><span data-ttu-id="26360-177">Настройка кода состояния HTTP</span><span class="sxs-lookup"><span data-stu-id="26360-177">Customize the HTTP status code</span></span>

<span data-ttu-id="26360-178">Используйте <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> для настройки сопоставления состояний работоспособности и кодов состояния HTTP.</span><span class="sxs-lookup"><span data-stu-id="26360-178">Use <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResultStatusCodes> to customize the mapping of health status to HTTP status codes.</span></span> <span data-ttu-id="26360-179">Следующие назначения <xref:Microsoft.AspNetCore.Http.StatusCodes> являются значениями по умолчанию, используемыми ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="26360-179">The following <xref:Microsoft.AspNetCore.Http.StatusCodes> assignments are the default values used by the middleware.</span></span> <span data-ttu-id="26360-180">Измените значения кодов состояния в соответствии с требованиями.</span><span class="sxs-lookup"><span data-stu-id="26360-180">Change the status code values to meet your requirements.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The following StatusCodes are the default assignments for
        // the HealthStatus properties.
        ResultStatusCodes =
        {
            [HealthStatus.Healthy] = StatusCodes.Status200OK,
            [HealthStatus.Degraded] = StatusCodes.Status200OK,
            [HealthStatus.Unhealthy] = StatusCodes.Status503ServiceUnavailable
        }
    });
}
```

### <a name="suppress-cache-headers"></a><span data-ttu-id="26360-181">Подавление заголовков кэша</span><span class="sxs-lookup"><span data-stu-id="26360-181">Suppress cache headers</span></span>

<span data-ttu-id="26360-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> определяет, добавляет ли ПО промежуточного слоя HTTP-заголовки в ответ проверки, чтобы предотвратить кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="26360-182"><xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.AllowCachingResponses> controls whether the Health Check Middleware adds HTTP headers to a probe response to prevent response caching.</span></span> <span data-ttu-id="26360-183">Если значение равно `false` (по умолчанию), ПО промежуточного слоя задает или переопределяет заголовки `Cache-Control`, `Expires` и `Pragma`, чтобы предотвратить кэширование ответов.</span><span class="sxs-lookup"><span data-stu-id="26360-183">If the value is `false` (default), the middleware sets or overrides the `Cache-Control`, `Expires`, and `Pragma` headers to prevent response caching.</span></span> <span data-ttu-id="26360-184">Если значение равно `true`, ПО промежуточного слоя не изменяет заголовки кэша в ответе.</span><span class="sxs-lookup"><span data-stu-id="26360-184">If the value is `true`, the middleware doesn't modify the cache headers of the response.</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // The default value is false.
        AllowCachingResponses = false
    });
}
```

### <a name="customize-output"></a><span data-ttu-id="26360-185">Настройка выходных данных</span><span class="sxs-lookup"><span data-stu-id="26360-185">Customize output</span></span>

<span data-ttu-id="26360-186">Параметр <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> возвращает или задает делегат, используемый для записи ответа.</span><span class="sxs-lookup"><span data-stu-id="26360-186">The <xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.ResponseWriter> option gets or sets a delegate used to write the response.</span></span> <span data-ttu-id="26360-187">Делегат по умолчанию записывает минимальный ответ в формате открытого текста со строковым значением [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span><span class="sxs-lookup"><span data-stu-id="26360-187">The default delegate writes a minimal plaintext response with the string value of [HealthReport.Status](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthReport.Status).</span></span>

```csharp
using Microsoft.AspNetCore.Diagnostics.HealthChecks;
using Microsoft.Extensions.Diagnostics.HealthChecks;

public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    app.UseHealthChecks("/health", new HealthCheckOptions()
    {
        // WriteResponse is a delegate used to write the response.
        ResponseWriter = WriteResponse
    });
}

private static Task WriteResponse(HttpContext httpContext, 
    HealthReport result)
{
    httpContext.Response.ContentType = "application/json";

    var json = new JObject(
        new JProperty("status", result.Status.ToString()),
        new JProperty("results", new JObject(result.Entries.Select(pair =>
            new JProperty(pair.Key, new JObject(
                new JProperty("status", pair.Value.Status.ToString()),
                new JProperty("description", pair.Value.Description),
                new JProperty("data", new JObject(pair.Value.Data.Select(
                    p => new JProperty(p.Key, p.Value))))))))));
    return httpContext.Response.WriteAsync(
        json.ToString(Formatting.Indented));
}
```

## <a name="database-probe"></a><span data-ttu-id="26360-188">Проверка базы данных</span><span class="sxs-lookup"><span data-stu-id="26360-188">Database probe</span></span>

<span data-ttu-id="26360-189">Проверка работоспособности может задать запрос для выполнения в качестве логического теста для указания того, отвечает ли база данных как обычно.</span><span class="sxs-lookup"><span data-stu-id="26360-189">A health check can specify a database query to run as a boolean test to indicate if the database is responding normally.</span></span>

<span data-ttu-id="26360-190">Пример приложения использует [BeatPulse](https://github.com/Xabaril/BeatPulse), библиотеку проверки работоспособности для приложений ASP.NET Core, чтобы проверить работоспособность базы данных SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26360-190">The sample app uses [BeatPulse](https://github.com/Xabaril/BeatPulse), a health check library for ASP.NET Core apps, to perform a health check on a SQL Server database.</span></span> <span data-ttu-id="26360-191">BeatPulse выполняет запрос `SELECT 1` к базе данных, чтобы подтвердить, что подключение к базе данных находится в работоспособном состоянии.</span><span class="sxs-lookup"><span data-stu-id="26360-191">BeatPulse executes a `SELECT 1` query against the database to confirm the connection to the database is healthy.</span></span>

> [!WARNING]
> <span data-ttu-id="26360-192">При проверке подключения к базе данных с помощью запроса выберите запрос, возвращающий результат быстро.</span><span class="sxs-lookup"><span data-stu-id="26360-192">When checking a database connection with a query, choose a query that returns quickly.</span></span> <span data-ttu-id="26360-193">Метод запроса связан с риском перегрузки базы данных и снижения ее производительности.</span><span class="sxs-lookup"><span data-stu-id="26360-193">The query approach runs the risk of overloading the database and degrading its performance.</span></span> <span data-ttu-id="26360-194">В большинстве случаев тестовый запрос не является обязательным.</span><span class="sxs-lookup"><span data-stu-id="26360-194">In most cases, running a test query isn't necessary.</span></span> <span data-ttu-id="26360-195">Достаточно успешного подключения к базе данных.</span><span class="sxs-lookup"><span data-stu-id="26360-195">Merely making a successful connection to the database is sufficient.</span></span> <span data-ttu-id="26360-196">Если вам понадобится выполнить запрос, выберите простой запрос SELECT, например `SELECT 1`.</span><span class="sxs-lookup"><span data-stu-id="26360-196">If you find it necessary to run a query, choose a simple SELECT query, such as `SELECT 1`.</span></span>

<span data-ttu-id="26360-197">Чтобы использовать библиотеку BeatPulse, включите ссылку на пакет [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span><span class="sxs-lookup"><span data-stu-id="26360-197">In order to use the BeatPulse library, include a package reference to [AspNetCore.HealthChecks.SqlServer](https://www.nuget.org/packages/AspNetCore.HealthChecks.SqlServer/).</span></span>

<span data-ttu-id="26360-198">Укажите допустимую строку подключения к базе данных в файле примера приложения *appsettings.json*.</span><span class="sxs-lookup"><span data-stu-id="26360-198">Supply a valid database connection string in the *appsettings.json* file of the sample app.</span></span> <span data-ttu-id="26360-199">Приложение использует базу данных SQL Server с именем `HealthCheckSample`:</span><span class="sxs-lookup"><span data-stu-id="26360-199">The app uses a SQL Server database named `HealthCheckSample`:</span></span>

[!code-json[](health-checks/samples/2.x/HealthChecksSample/appsettings.json?highlight=3)]

<span data-ttu-id="26360-200">Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26360-200">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="26360-201">Пример приложения вызывает метод BeatPulse `AddSqlServer` со строкой подключения базы данных (*DbHealthStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="26360-201">The sample app calls BeatPulse's `AddSqlServer` method with the database's connection string (*DbHealthStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="26360-202">Вызовите ПО промежуточного слоя для проверки работоспособности в конвейере обработки приложения в `Startup.Configure`:</span><span class="sxs-lookup"><span data-stu-id="26360-202">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="26360-203">Чтобы запустить сценарий проверки базы данных с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26360-203">To run the database probe scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario db
```

> [!NOTE]
> <span data-ttu-id="26360-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) не поддерживается и не обслуживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="26360-204">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="entity-framework-core-dbcontext-probe"></a><span data-ttu-id="26360-205">Проверка DbContext в Entity Framework Core</span><span class="sxs-lookup"><span data-stu-id="26360-205">Entity Framework Core DbContext probe</span></span>

<span data-ttu-id="26360-206">Эта проверка `DbContext` подтверждает, что приложение может взаимодействовать с базой данных, настроенной для EF Core `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="26360-206">The `DbContext` check confirms that the app can communicate with the database configured for an EF Core `DbContext`.</span></span> <span data-ttu-id="26360-207">Проверка `DbContext` поддерживается в приложениях, которые:</span><span class="sxs-lookup"><span data-stu-id="26360-207">The `DbContext` check is supported in apps that:</span></span>

* <span data-ttu-id="26360-208">используют [Entity Framework (EF) Core](/ef/core/);</span><span class="sxs-lookup"><span data-stu-id="26360-208">Use [Entity Framework (EF) Core](/ef/core/).</span></span>
* <span data-ttu-id="26360-209">содержат ссылку на пакет [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span><span class="sxs-lookup"><span data-stu-id="26360-209">Include a package reference to [Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore](https://www.nuget.org/packages/Microsoft.Extensions.Diagnostics.HealthChecks.EntityFrameworkCore/).</span></span>

<span data-ttu-id="26360-210">`AddDbContextCheck<TContext>` регистрирует проверку работоспособности для `DbContext`.</span><span class="sxs-lookup"><span data-stu-id="26360-210">`AddDbContextCheck<TContext>` registers a health check for a `DbContext`.</span></span> <span data-ttu-id="26360-211">`DbContext` передается методу в качестве `TContext`.</span><span class="sxs-lookup"><span data-stu-id="26360-211">The `DbContext` is supplied as the `TContext` to the method.</span></span> <span data-ttu-id="26360-212">Доступна перегрузка, позволяющая настроить состояние ошибки, теги и запрос тестирования.</span><span class="sxs-lookup"><span data-stu-id="26360-212">An overload is available to configure the failure status, tags, and a custom test query.</span></span>

<span data-ttu-id="26360-213">По умолчанию:</span><span class="sxs-lookup"><span data-stu-id="26360-213">By default:</span></span>

* <span data-ttu-id="26360-214">`DbContextHealthCheck` вызывает метод EF Core `CanConnectAsync`.</span><span class="sxs-lookup"><span data-stu-id="26360-214">The `DbContextHealthCheck` calls EF Core's `CanConnectAsync` method.</span></span> <span data-ttu-id="26360-215">Вы можете указать, какая операция выполняется в том случае, когда проверка работоспособности производится с помощью перегрузок метода `AddDbContextCheck`.</span><span class="sxs-lookup"><span data-stu-id="26360-215">You can customize what operation is run when checking health using `AddDbContextCheck` method overloads.</span></span>
* <span data-ttu-id="26360-216">Имя проверки работоспособности — это имя типа `TContext`.</span><span class="sxs-lookup"><span data-stu-id="26360-216">The name of the health check is the name of the `TContext` type.</span></span>

<span data-ttu-id="26360-217">В примере приложения `AppDbContext` предоставляется для `AddDbContextCheck` и регистрируется в качестве службы в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26360-217">In the sample app, `AppDbContext` is provided to `AddDbContextCheck` and registered as a service in `Startup.ConfigureServices`.</span></span>

<span data-ttu-id="26360-218">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="26360-218">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="26360-219">В примере приложения `UseHealthChecks` добавляет ПО промежуточного слоя в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="26360-219">In the sample app, `UseHealthChecks` adds the Health Check Middleware in `Startup.Configure`.</span></span>

<span data-ttu-id="26360-220">*DbContextHealthStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="26360-220">*DbContextHealthStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/DbContextHealthStartup.cs?name=snippet_Configure)]

<span data-ttu-id="26360-221">Для запуска сценария проверки `DbContext` с помощью примера приложения убедитесь, что база данных, указанная в строке подключения, не существует в экземпляре SQL Server.</span><span class="sxs-lookup"><span data-stu-id="26360-221">To run the `DbContext` probe scenario using the sample app, confirm that the database specified by the the connection string doesn't exist in the SQL Server instance.</span></span> <span data-ttu-id="26360-222">Если база данных существует, удалите ее.</span><span class="sxs-lookup"><span data-stu-id="26360-222">If the database exists, delete it.</span></span>

<span data-ttu-id="26360-223">Выполните следующую команду в папке проекта в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26360-223">Execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario dbcontext
```

<span data-ttu-id="26360-224">После запуска приложения проверьте состояние работоспособности, выполнив запрос к конечной точке `/health` в браузере.</span><span class="sxs-lookup"><span data-stu-id="26360-224">After the app is running, check the health status by making a request to the `/health` endpoint in a browser.</span></span> <span data-ttu-id="26360-225">Базы данных и `AppDbContext` не существуют, поэтому приложение дает следующий ответ:</span><span class="sxs-lookup"><span data-stu-id="26360-225">The database and `AppDbContext` don't exist, so app provides the following response:</span></span>

```
Unhealthy
```

<span data-ttu-id="26360-226">Заставьте пример приложения создать базу данных.</span><span class="sxs-lookup"><span data-stu-id="26360-226">Trigger the sample app to create the database.</span></span> <span data-ttu-id="26360-227">Сделайте запрос к `/createdatabase`.</span><span class="sxs-lookup"><span data-stu-id="26360-227">Make a request to `/createdatabase`.</span></span> <span data-ttu-id="26360-228">Приложение выдает ответ:</span><span class="sxs-lookup"><span data-stu-id="26360-228">The app responds:</span></span>

```
Creating the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="26360-229">Сделайте запрос к конечной точке `/health`.</span><span class="sxs-lookup"><span data-stu-id="26360-229">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="26360-230">База данных и контекст существуют, поэтому приложение отвечает:</span><span class="sxs-lookup"><span data-stu-id="26360-230">The database and context exist, so app responds:</span></span>

```
Healthy
```

<span data-ttu-id="26360-231">Заставьте пример приложения удалить базу данных.</span><span class="sxs-lookup"><span data-stu-id="26360-231">Trigger the sample app to delete the database.</span></span> <span data-ttu-id="26360-232">Сделайте запрос к `/deletedatabase`.</span><span class="sxs-lookup"><span data-stu-id="26360-232">Make a request to `/deletedatabase`.</span></span> <span data-ttu-id="26360-233">Приложение выдает ответ:</span><span class="sxs-lookup"><span data-stu-id="26360-233">The app responds:</span></span>

```
Deleting the database...
Done!
Navigate to /health to see the health status.
```

<span data-ttu-id="26360-234">Сделайте запрос к конечной точке `/health`.</span><span class="sxs-lookup"><span data-stu-id="26360-234">Make a request to the `/health` endpoint.</span></span> <span data-ttu-id="26360-235">Приложение предоставляет ответ о неработоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-235">The app provides an unhealthy response:</span></span>

```
Unhealthy
```

## <a name="separate-readiness-and-liveness-probes"></a><span data-ttu-id="26360-236">Разделение проверок готовности и активности</span><span class="sxs-lookup"><span data-stu-id="26360-236">Separate readiness and liveness probes</span></span>

<span data-ttu-id="26360-237">В некоторых сценариях размещения используются пары проверок работоспособности, отличающие два состояния приложения:</span><span class="sxs-lookup"><span data-stu-id="26360-237">In some hosting scenarios, a pair of health checks are used that distinguish two app states:</span></span>

* <span data-ttu-id="26360-238">Приложение работает, но еще не готово к получению запросов.</span><span class="sxs-lookup"><span data-stu-id="26360-238">The app is functioning but not yet ready to receive requests.</span></span> <span data-ttu-id="26360-239">Это состояние *готовности* приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-239">This state is the app's *readiness*.</span></span>
* <span data-ttu-id="26360-240">Приложение работает и отвечает на запросы.</span><span class="sxs-lookup"><span data-stu-id="26360-240">The app is functioning and responding to requests.</span></span> <span data-ttu-id="26360-241">Это состояние *жизнеспособности* приложения.</span><span class="sxs-lookup"><span data-stu-id="26360-241">This state is the app's *liveness*.</span></span>

<span data-ttu-id="26360-242">Проверка готовности обычно выполняет ряд проверок, чтобы определить, все ли подсистемы и ресурсы приложения доступны; она более обширна и требует много времени.</span><span class="sxs-lookup"><span data-stu-id="26360-242">The readiness check usually performs a more extensive and time-consuming set of checks to determine if all of the app's subsystems and resources are available.</span></span> <span data-ttu-id="26360-243">Проверка жизнеспособности лишь быстро определяет, доступно ли приложение для обработки запросов.</span><span class="sxs-lookup"><span data-stu-id="26360-243">A liveness check merely performs a quick check to determine if the app is available to process requests.</span></span> <span data-ttu-id="26360-244">По прохождении проверки готовности приложения нет необходимости создавать чрезмерную нагрузку на приложение с дорогостоящим набором проверок готовности&mdash;дальнейшие проверки требуют лишь проверки жизнеспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-244">After the app passes its readiness check, there's no need to burden the app further with the expensive set of readiness checks&mdash;further checks only require checking for liveness.</span></span>

<span data-ttu-id="26360-245">Пример приложения содержит проверку работоспособности, которая сообщает о завершении длительной задачи запуска [размещенной службы](xref:fundamentals/host/hosted-services).</span><span class="sxs-lookup"><span data-stu-id="26360-245">The sample app contains a health check to report the completion of long-running startup task in a [Hosted Service](xref:fundamentals/host/hosted-services).</span></span> <span data-ttu-id="26360-246">`StartupHostedServiceHealthCheck` предоставляет свойство, `StartupTaskCompleted`, которое размещенная служба может задать как `true` после завершения длительной задачи (*StartupHostedServiceHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="26360-246">The `StartupHostedServiceHealthCheck` exposes a property, `StartupTaskCompleted`, that the hosted service can set to `true` when its long-running task is finished (*StartupHostedServiceHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/StartupHostedServiceHealthCheck.cs?name=snippet1&highlight=5)]

<span data-ttu-id="26360-247">Длительная фоновая задача запущена [размещенной службой](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span><span class="sxs-lookup"><span data-stu-id="26360-247">The long-running background task is started by a [Hosted Service](xref:fundamentals/host/hosted-services) (*Services/StartupHostedService*).</span></span> <span data-ttu-id="26360-248">По завершении задачи `StartupHostedServiceHealthCheck.StartupTaskCompleted` получает значение `true`:</span><span class="sxs-lookup"><span data-stu-id="26360-248">At the conclusion of the task, `StartupHostedServiceHealthCheck.StartupTaskCompleted` is set to `true`:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/Services/StartupHostedService.cs?name=snippet1&highlight=18-20)]

<span data-ttu-id="26360-249">Проверка работоспособности регистрируется через <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> в `Startup.ConfigureServices` вместе с размещенной службой.</span><span class="sxs-lookup"><span data-stu-id="26360-249">The health check is registered with <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*> in `Startup.ConfigureServices` along with the hosted service.</span></span> <span data-ttu-id="26360-250">Поскольку размещенной службе необходимо задать свойство проверки работоспособности, проверка работоспособности также регистрируется в контейнере службы (*LivenessProbeStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="26360-250">Because the hosted service must set the property on the health check, the health check is also registered in the service container (*LivenessProbeStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices)]

<span data-ttu-id="26360-251">Вызовите ПО промежуточного слоя для проверки работоспособности в конвейере обработки приложения в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="26360-251">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="26360-252">В примере приложения создаются конечные точки проверки работоспособности в `/health/ready` для проверки готовности и в `/health/live` для проверки жизнеспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-252">In the sample app, the health check endpoints are created at `/health/ready` for the readiness check and `/health/live` for the liveness check.</span></span> <span data-ttu-id="26360-253">Проверка готовности фильтрует проверки работоспособности по тегу `ready`.</span><span class="sxs-lookup"><span data-stu-id="26360-253">The readiness check filters health checks to the health check with the `ready` tag.</span></span> <span data-ttu-id="26360-254">Проверка жизнеспособности отфильтровывает `StartupHostedServiceHealthCheck`, возвращая `false` в [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (дополнительные сведения см. в разделе [Фильтрация проверок работоспособности](#filter-health-checks)):</span><span class="sxs-lookup"><span data-stu-id="26360-254">The liveness check filters out the `StartupHostedServiceHealthCheck` by returning `false` in the [HealthCheckOptions.Predicate](xref:Microsoft.AspNetCore.Diagnostics.HealthChecks.HealthCheckOptions.Predicate) (for more information, see [Filter health checks](#filter-health-checks)):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_Configure)]

<span data-ttu-id="26360-255">Чтобы запустить сценарий конфигурации проверок готовности и жизнеспособности с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26360-255">To run the readiness/liveness configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario liveness
```

<span data-ttu-id="26360-256">В браузере посетите `/health/ready` несколько раз, пока не пройдет 15 секунд.</span><span class="sxs-lookup"><span data-stu-id="26360-256">In a browser, visit `/health/ready` several times until 15 seconds have passed.</span></span> <span data-ttu-id="26360-257">Проверка работоспособности будет передавать состояние *Unhealthy* первые 15 секунд.</span><span class="sxs-lookup"><span data-stu-id="26360-257">The health check reports *Unhealthy* for the first 15 seconds.</span></span> <span data-ttu-id="26360-258">По истечении 15 секунд конечная точка передает состояние *Healthy*, что означает завершение длительной задачи размещенной службой.</span><span class="sxs-lookup"><span data-stu-id="26360-258">After 15 seconds, the endpoint reports *Healthy*, which reflects the completion of the long-running task by the hosted service.</span></span>

<span data-ttu-id="26360-259">В этом примере также создается издатель проверки работоспособности (реализация <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>), который выполнит первую проверку готовности с задержкой в две секунды.</span><span class="sxs-lookup"><span data-stu-id="26360-259">This example also creates a Health Check Publisher (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation) that runs the first readiness check with a two second delay.</span></span> <span data-ttu-id="26360-260">Дополнительные сведения см. в разделе [об издателе проверки работоспособности](#health-check-publisher).</span><span class="sxs-lookup"><span data-stu-id="26360-260">For more information, see the [Health Check Publisher](#health-check-publisher) section.</span></span>

### <a name="kubernetes-example"></a><span data-ttu-id="26360-261">Пример для Kubernetes</span><span class="sxs-lookup"><span data-stu-id="26360-261">Kubernetes example</span></span>

<span data-ttu-id="26360-262">Использование отдельных проверок готовности и жизнеспособности полезно в таких средах, как [Kubernetes](https://kubernetes.io/).</span><span class="sxs-lookup"><span data-stu-id="26360-262">Using separate readiness and liveness checks is useful in an environment such as [Kubernetes](https://kubernetes.io/).</span></span> <span data-ttu-id="26360-263">В Kubernetes приложениям может потребоваться долго выполнять задачи во время запуска, прежде чем начать принимать запросы; например, проверить доступность основной базы данных.</span><span class="sxs-lookup"><span data-stu-id="26360-263">In Kubernetes, an app might be required to perform time-consuming startup work before accepting requests, such as a test of the underlying database availability.</span></span> <span data-ttu-id="26360-264">Использование отдельных проверок позволяет оркестратору различать, когда приложение работает, но еще не готово, а когда приложение не удалось запустить.</span><span class="sxs-lookup"><span data-stu-id="26360-264">Using separate checks allows the orchestrator to distinguish whether the app is functioning but not yet ready or if the app has failed to start.</span></span> <span data-ttu-id="26360-265">Дополнительные сведения о проверках готовности и жизнеспособности в Kubernetes см. в разделе [Настройка проверок готовности и жизнеспособности](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) в документации по Kubernetes.</span><span class="sxs-lookup"><span data-stu-id="26360-265">For more information on readiness and liveness probes in Kubernetes, see [Configure Liveness and Readiness Probes](https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-probes/) in the Kubernetes documentation.</span></span>

<span data-ttu-id="26360-266">В следующем примере показана конфигурация проверки готовности для Kubernetes:</span><span class="sxs-lookup"><span data-stu-id="26360-266">The following example demonstrates a Kubernetes readiness probe configuration:</span></span>

```
spec:
  template:
  spec:
    readinessProbe:
      # an http probe
      httpGet:
        path: /health/ready
        port: 80
      # length of time to wait for a pod to initialize
      # after pod startup, before applying health checking
      initialDelaySeconds: 30
      timeoutSeconds: 1
    ports:
      - containerPort: 80
```

## <a name="metric-based-probe-with-a-custom-response-writer"></a><span data-ttu-id="26360-267">Проба на основе метрики с настраиваемым модулем записи ответов</span><span class="sxs-lookup"><span data-stu-id="26360-267">Metric-based probe with a custom response writer</span></span>

<span data-ttu-id="26360-268">Этот пример демонстрирует проверку работоспособности памяти с настраиваемым модулем записи ответов.</span><span class="sxs-lookup"><span data-stu-id="26360-268">The sample app demonstrates a memory health check with a custom response writer.</span></span>

<span data-ttu-id="26360-269">Если приложение использует больше заданного порогового значения памяти (1 ГБ в примере приложения), `MemoryHealthCheck` сообщает о состоянии пониженной функциональности.</span><span class="sxs-lookup"><span data-stu-id="26360-269">`MemoryHealthCheck` reports a degraded status if the app uses more than a given threshold of memory (1 GB in the sample app).</span></span> <span data-ttu-id="26360-270"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> включает сведения от сборщика мусора для приложения (*MemoryHealthCheck.cs*):</span><span class="sxs-lookup"><span data-stu-id="26360-270">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckResult> includes Garbage Collector (GC) information for the app (*MemoryHealthCheck.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/MemoryHealthCheck.cs?name=snippet1)]

<span data-ttu-id="26360-271">Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26360-271">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="26360-272">Вместо того чтобы включать проверку работоспособности, передав ее в <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, `MemoryHealthCheck` регистрируется в качестве службы.</span><span class="sxs-lookup"><span data-stu-id="26360-272">Instead of enabling the health check by passing it to <xref:Microsoft.Extensions.DependencyInjection.HealthChecksBuilderAddCheckExtensions.AddCheck*>, the `MemoryHealthCheck` is registered as a service.</span></span> <span data-ttu-id="26360-273">Все зарегистрированные службы <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> доступны в службах проверки работоспособности и ПО промежуточного слоя.</span><span class="sxs-lookup"><span data-stu-id="26360-273">All <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> registered services are available to the health check services and middleware.</span></span> <span data-ttu-id="26360-274">Мы рекомендуем регистрировать службы проверки работоспособности в качестве одноэлементных служб.</span><span class="sxs-lookup"><span data-stu-id="26360-274">We recommend registering health check services as Singleton services.</span></span>

<span data-ttu-id="26360-275">*CustomWriterStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="26360-275">*CustomWriterStartup.cs*:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_ConfigureServices&highlight=4)]

<span data-ttu-id="26360-276">Вызовите ПО промежуточного слоя для проверки работоспособности в конвейере обработки приложения в `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="26360-276">Call Health Check Middleware in the app processing pipeline in `Startup.Configure`.</span></span> <span data-ttu-id="26360-277">Делегат `WriteResponse` передается в свойство `ResponseWriter` для вывода настраиваемого ответа JSON при выполнении проверки работоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-277">A `WriteResponse` delegate is provided to the `ResponseWriter` property to output a custom JSON response when the health check executes:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_Configure&highlight=6)]

<span data-ttu-id="26360-278">Метод `WriteResponse` форматирует `CompositeHealthCheckResult` в объект JSON и выдает выходные данные JSON в качестве ответа проверки работоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-278">The `WriteResponse` method formats the `CompositeHealthCheckResult` into a JSON object and yields JSON output for the health check response:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/CustomWriterStartup.cs?name=snippet_WriteResponse)]

<span data-ttu-id="26360-279">Чтобы запустить проверку метрики с настраиваемым модулем записи ответа с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26360-279">To run the metric-based probe with custom response writer output using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario writer
```

> [!NOTE]
> <span data-ttu-id="26360-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) включает сценарии для проверки работоспособности на основе метрик, включая проверки жизнеспособности для дискового хранилища и максимальных значений.</span><span class="sxs-lookup"><span data-stu-id="26360-280">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes metric-based health check scenarios, including disk storage and maximum value liveness checks.</span></span>
>
> <span data-ttu-id="26360-281">[BeatPulse](https://github.com/Xabaril/BeatPulse) не поддерживается и не обслуживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="26360-281">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>

## <a name="filter-by-port"></a><span data-ttu-id="26360-282">Фильтр по портам</span><span class="sxs-lookup"><span data-stu-id="26360-282">Filter by port</span></span>

<span data-ttu-id="26360-283">Вызов <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> с портом ограничивает запросы на проверку работоспособности указанным портом.</span><span class="sxs-lookup"><span data-stu-id="26360-283">Calling <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> with a port restricts health check requests to the port specified.</span></span> <span data-ttu-id="26360-284">Обычно это используется в контейнерной среде с целью предоставления порта для мониторинга служб.</span><span class="sxs-lookup"><span data-stu-id="26360-284">This is typically used in a container environment to expose a port for monitoring services.</span></span>

<span data-ttu-id="26360-285">Пример приложения настраивает порт с помощью [поставщика конфигурации переменных среды](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span><span class="sxs-lookup"><span data-stu-id="26360-285">The sample app configures the port using the [Environment Variable Configuration Provider](xref:fundamentals/configuration/index#environment-variables-configuration-provider).</span></span> <span data-ttu-id="26360-286">Порт устанавливается в файле *launchSettings.json* и передается поставщику конфигурации с помощью переменной среды.</span><span class="sxs-lookup"><span data-stu-id="26360-286">The port is set in the *launchSettings.json* file and passed to the configuration provider via an environment variable.</span></span> <span data-ttu-id="26360-287">Необходимо также настроить на сервере прослушивание запросов через порт управления.</span><span class="sxs-lookup"><span data-stu-id="26360-287">You must also configure the server to listen to requests on the management port.</span></span>

<span data-ttu-id="26360-288">Чтобы использовать пример приложения для демонстрации настройки портов управления, создайте файл *launchSettings.json* в папке *Properties*.</span><span class="sxs-lookup"><span data-stu-id="26360-288">To use the sample app to demonstrate management port configuration, create the *launchSettings.json* file in a *Properties* folder.</span></span>

<span data-ttu-id="26360-289">Следующий файл *launchSettings.json* не включен в файлы проекта примера приложения и должен быть создан вручную.</span><span class="sxs-lookup"><span data-stu-id="26360-289">The following *launchSettings.json* file isn't included in the sample app's project files and must be created manually.</span></span>

<span data-ttu-id="26360-290">*Properties/launchSettings.json*:</span><span class="sxs-lookup"><span data-stu-id="26360-290">*Properties/launchSettings.json*:</span></span>

```json
{
  "profiles": {
    "SampleApp": {
      "commandName": "Project",
      "commandLineArgs": "",
      "launchBrowser": true,
      "environmentVariables": {
        "ASPNETCORE_ENVIRONMENT": "Development",
        "ASPNETCORE_URLS": "http://localhost:5000/;http://localhost:5001/",
        "ASPNETCORE_MANAGEMENTPORT": "5001"
      },
      "applicationUrl": "http://localhost:5000/"
    }
  }
}
```

<span data-ttu-id="26360-291">Зарегистрируйте службы проверки работоспособности при помощи <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> в `Startup.ConfigureServices`.</span><span class="sxs-lookup"><span data-stu-id="26360-291">Register health check services with <xref:Microsoft.Extensions.DependencyInjection.HealthCheckServiceCollectionExtensions.AddHealthChecks*> in `Startup.ConfigureServices`.</span></span> <span data-ttu-id="26360-292">Вызов <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> задает порт управления (*ManagementPortStartup.cs*):</span><span class="sxs-lookup"><span data-stu-id="26360-292">The call to <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> specifies the management port (*ManagementPortStartup.cs*):</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ManagementPortStartup.cs?name=snippet1&highlight=12,18)]

> [!NOTE]
> <span data-ttu-id="26360-293">Можно избежать создания файла *launchSettings.json* в примере приложения, явно указав URL-адрес и порт управления в коде.</span><span class="sxs-lookup"><span data-stu-id="26360-293">You can avoid creating the *launchSettings.json* file in the sample app by setting the URLs and management port explicitly in code.</span></span> <span data-ttu-id="26360-294">В *Program.cs*, где создается <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder>, добавьте вызов <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> и укажите обычную конечную точку ответа приложения и порт конечной точки управления.</span><span class="sxs-lookup"><span data-stu-id="26360-294">In *Program.cs* where the <xref:Microsoft.AspNetCore.Hosting.WebHostBuilder> is created, add a call to <xref:Microsoft.AspNetCore.Hosting.HostingAbstractionsWebHostBuilderExtensions.UseUrls*> and provide the app's normal response endpoint and the management port endpoint.</span></span> <span data-ttu-id="26360-295">В *ManagementPortStartup.cs*, где вызывается <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*>, явно укажите порт управления.</span><span class="sxs-lookup"><span data-stu-id="26360-295">In *ManagementPortStartup.cs* where <xref:Microsoft.AspNetCore.Builder.HealthCheckApplicationBuilderExtensions.UseHealthChecks*> is called, specify the management port explicitly.</span></span>
>
> <span data-ttu-id="26360-296">*Program.cs*:</span><span class="sxs-lookup"><span data-stu-id="26360-296">*Program.cs*:</span></span>
>
> ```csharp
> return new WebHostBuilder()
>     .UseConfiguration(config)
>     .UseUrls("http://localhost:5000/;http://localhost:5001/")
>     .ConfigureLogging(builder =>
>     {
>         builder.SetMinimumLevel(LogLevel.Trace);
>         builder.AddConfiguration(config);
>         builder.AddConsole();
>     })
>     .UseKestrel()
>     .UseStartup(startupType)
>     .Build();
> ```
>
> <span data-ttu-id="26360-297">*ManagementPortStartup.cs*:</span><span class="sxs-lookup"><span data-stu-id="26360-297">*ManagementPortStartup.cs*:</span></span>
>
> ```csharp
> app.UseHealthChecks("/health", port: 5001);
> ```

<span data-ttu-id="26360-298">Чтобы запустить сценарий с портом управления с помощью примера приложения, выполните следующую команду из папки проекта в командной строке:</span><span class="sxs-lookup"><span data-stu-id="26360-298">To run the management port configuration scenario using the sample app, execute the following command from the project's folder in a command shell:</span></span>

```console
dotnet run --scenario port
```

## <a name="distribute-a-health-check-library"></a><span data-ttu-id="26360-299">Распространение библиотеки проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-299">Distribute a health check library</span></span>

<span data-ttu-id="26360-300">Чтобы распространять проверки работоспособности в качестве библиотеки, выполните следующие действия.</span><span class="sxs-lookup"><span data-stu-id="26360-300">To distribute a health check as a library:</span></span>

1. <span data-ttu-id="26360-301">Напишите проверку работоспособности, которая реализует интерфейс <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> как автономный класс.</span><span class="sxs-lookup"><span data-stu-id="26360-301">Write a health check that implements the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheck> interface as a standalone class.</span></span> <span data-ttu-id="26360-302">Класс может полагаться на [внедрение зависимостей (DI)](xref:fundamentals/dependency-injection), активацию типов и [именованные параметры](xref:fundamentals/configuration/options) для доступа к данным конфигурации.</span><span class="sxs-lookup"><span data-stu-id="26360-302">The class can rely on [dependency injection (DI)](xref:fundamentals/dependency-injection), type activation, and [named options](xref:fundamentals/configuration/options) to access configuration data.</span></span>

   ```csharp
   using System;
   using System.Threading;
   using System.Threading.Tasks;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   namespace SampleApp
   {
       public class ExampleHealthCheck : IHealthCheck
       {
           private readonly string _data1;
           private readonly int? _data2;

           public ExampleHealthCheck(string data1, int? data2)
           {
               _data1 = data1 ?? throw new ArgumentNullException(nameof(data1));
               _data2 = data2 ?? throw new ArgumentNullException(nameof(data2));
           }

           public async Task<HealthCheckResult> CheckHealthAsync(
               HealthCheckContext context, CancellationToken cancellationToken)
           {
               try
               {
                   // Health check logic
                   //
                   // data1 and data2 are used in the method to
                   // run the probe's health check logic.

                   // Assume that it's possible for this health check
                   // to throw an AccessViolationException.

                   return HealthCheckResult.Healthy();
               }
               catch (AccessViolationException ex)
               {
                   return new HealthCheckResult(
                       context.Registration.FailureStatus,
                       description: "An access violation occurred during the check.",
                       exception: ex,
                       data: null);
               }
           }
       }
   }
   ```

1. <span data-ttu-id="26360-303">Напишите метод расширения с параметрами, который приложение вызывает в своем методе `Startup.Configure`.</span><span class="sxs-lookup"><span data-stu-id="26360-303">Write an extension method with parameters that the consuming app calls in its `Startup.Configure` method.</span></span> <span data-ttu-id="26360-304">В следующем примере предполагается следующая сигнатура метода проверки работоспособности:</span><span class="sxs-lookup"><span data-stu-id="26360-304">In the following example, assume the following health check method signature:</span></span>

   ```csharp
   ExampleHealthCheck(string, string, int )
   ```

   <span data-ttu-id="26360-305">Эта сигнатура указывает, что `ExampleHealthCheck` требуются дополнительные данные для обработки логики проверки работоспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-305">The preceding signature indicates that the `ExampleHealthCheck` requires additional data to process the health check probe logic.</span></span> <span data-ttu-id="26360-306">Данные передаются делегату, используемому для создания экземпляра проверки работоспособности, когда проверка работоспособности регистрируется с помощью метода расширения.</span><span class="sxs-lookup"><span data-stu-id="26360-306">The data is provided to the delegate used to create the health check instance when the health check is registered with an extension method.</span></span> <span data-ttu-id="26360-307">В следующем примере вызывающая сторона задает:</span><span class="sxs-lookup"><span data-stu-id="26360-307">In the following example, the caller specifies optional:</span></span>

   * <span data-ttu-id="26360-308">Необязательное имя проверки работоспособности (`name`).</span><span class="sxs-lookup"><span data-stu-id="26360-308">health check name (`name`).</span></span> <span data-ttu-id="26360-309">Если `null`, используется `example_health_check`.</span><span class="sxs-lookup"><span data-stu-id="26360-309">If `null`, `example_health_check` is used.</span></span>
   * <span data-ttu-id="26360-310">Строковая точка данных для проверки работоспособности (`data1`).</span><span class="sxs-lookup"><span data-stu-id="26360-310">string data point for the health check (`data1`).</span></span>
   * <span data-ttu-id="26360-311">Целочисленная точка данных для проверки работоспособности (`data2`).</span><span class="sxs-lookup"><span data-stu-id="26360-311">integer data point for the health check (`data2`).</span></span> <span data-ttu-id="26360-312">Если `null`, используется `1`.</span><span class="sxs-lookup"><span data-stu-id="26360-312">If `null`, `1` is used.</span></span>
   * <span data-ttu-id="26360-313">состояние сбоя (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span><span class="sxs-lookup"><span data-stu-id="26360-313">failure status (<xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus>).</span></span> <span data-ttu-id="26360-314">Значение по умолчанию — `null`.</span><span class="sxs-lookup"><span data-stu-id="26360-314">The default is `null`.</span></span> <span data-ttu-id="26360-315">В случае `null` возвращается [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) как состояние сбоя.</span><span class="sxs-lookup"><span data-stu-id="26360-315">If `null`, [HealthStatus.Unhealthy](xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthStatus) is reported for a failure status.</span></span>
   * <span data-ttu-id="26360-316">Теги (`IEnumerable<string>`).</span><span class="sxs-lookup"><span data-stu-id="26360-316">tags (`IEnumerable<string>`).</span></span>

   ```csharp
   using System.Collections.Generic;
   using Microsoft.Extensions.Diagnostics.HealthChecks;

   public static class ExampleHealthCheckBuilderExtensions
   {
       const string NAME = "example_health_check";

       public static IHealthChecksBuilder AddExampleHealthCheck(
           this IHealthChecksBuilder builder, 
           string name = default, 
           string data1, 
           int data2 = 1, 
           HealthStatus? failureStatus = default, 
           IEnumerable<string> tags = default)
       {
           return builder.Add(new HealthCheckRegistration(
               name ?? NAME,
               sp => new ExampleHealthCheck(data1, data2),
               failureStatus,
               tags));
       }
   }
   ```

## <a name="health-check-publisher"></a><span data-ttu-id="26360-317">Издатель проверки работоспособности</span><span class="sxs-lookup"><span data-stu-id="26360-317">Health Check Publisher</span></span>

<span data-ttu-id="26360-318">Когда <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> добавляется в контейнер службы, система проверки работоспособности периодически выполняет ваши проверки работоспособности и вызывает `PublishAsync` с полученным результатом.</span><span class="sxs-lookup"><span data-stu-id="26360-318">When an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> is added to the service container, the health check system periodically executes your health checks and calls `PublishAsync` with the result.</span></span> <span data-ttu-id="26360-319">Это полезно в ситуации принудительной передачи данных мониторинга работоспособности, когда ожидается, что каждый процесс периодически вызывает систему мониторинга для определения работоспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-319">This is useful in a push-based health monitoring system scenario that expects each process to call the monitoring system periodically in order to determine health.</span></span>

<span data-ttu-id="26360-320">Интерфейс <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> содержит один метод:</span><span class="sxs-lookup"><span data-stu-id="26360-320">The <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> interface has a single method:</span></span>

```csharp
Task PublishAsync(HealthReport report, CancellationToken cancellationToken);
```

<span data-ttu-id="26360-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> позволяет задать следующие параметры:</span><span class="sxs-lookup"><span data-stu-id="26360-321"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions> allow you to set:</span></span>

* <span data-ttu-id="26360-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> — начальная задержка, которая применяется после запуска приложения, но перед выполнением экземпляров <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="26360-322"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay> &ndash; The initial delay applied after the app starts before executing <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="26360-323">Задержка применяется при запуске один раз и не распространяется на все последующие итерации.</span><span class="sxs-lookup"><span data-stu-id="26360-323">The delay is applied once at startup and doesn't apply to subsequent iterations.</span></span> <span data-ttu-id="26360-324">Значение по умолчанию — пять секунд.</span><span class="sxs-lookup"><span data-stu-id="26360-324">The default value is five seconds.</span></span>
* <span data-ttu-id="26360-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> — период выполнения <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="26360-325"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> &ndash; The period of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> execution.</span></span> <span data-ttu-id="26360-326">Значение по умолчанию - 30 секунды.</span><span class="sxs-lookup"><span data-stu-id="26360-326">The default value is 30 seconds.</span></span>
* <span data-ttu-id="26360-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> — если <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> имеет значение `null` (по умолчанию), служба издателя проверки работоспособности выполняет все зарегистрированные проверки работоспособности.</span><span class="sxs-lookup"><span data-stu-id="26360-327"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> &ndash; If <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Predicate> is `null` (default), the health check publisher service runs all registered health checks.</span></span> <span data-ttu-id="26360-328">Чтобы выполнить ряд проверок работоспособности, укажите функцию, которая отфильтровывает нужный набор.</span><span class="sxs-lookup"><span data-stu-id="26360-328">To run a subset of health checks, provide a function that filters the set of checks.</span></span> <span data-ttu-id="26360-329">Предикат вычисляется в каждом периоде.</span><span class="sxs-lookup"><span data-stu-id="26360-329">The predicate is evaluated each period.</span></span>
* <span data-ttu-id="26360-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> — время ожидания до выполнения проверок работоспособности для всех экземпляров <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="26360-330"><xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Timeout> &ndash; The timeout for executing the health checks for all <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instances.</span></span> <span data-ttu-id="26360-331">Используйте <xref:System.Threading.Timeout.InfiniteTimeSpan>, чтобы выполнить проверки без задержки.</span><span class="sxs-lookup"><span data-stu-id="26360-331">Use <xref:System.Threading.Timeout.InfiniteTimeSpan> to execute without a timeout.</span></span> <span data-ttu-id="26360-332">Значение по умолчанию - 30 секунды.</span><span class="sxs-lookup"><span data-stu-id="26360-332">The default value is 30 seconds.</span></span>

::: moniker range="= aspnetcore-2.2"

> [!WARNING]
> <span data-ttu-id="26360-333">В выпуске ASP.NET Core 2.2 значение <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> не поддерживается в реализации <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>. Здесь задается значение <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span><span class="sxs-lookup"><span data-stu-id="26360-333">In the ASP.NET Core 2.2 release, setting <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Period> isn't honored by the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation; it sets the value of <xref:Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherOptions.Delay>.</span></span> <span data-ttu-id="26360-334">Эта проблема будет устранена в выпуске ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="26360-334">This issue will be fixed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="26360-335">Дополнительные сведения см. в описании проблемы [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041) (HealthCheckPublisherOptions.Period задает значение свойству .Delay).</span><span class="sxs-lookup"><span data-stu-id="26360-335">For more information, see [HealthCheckPublisherOptions.Period sets the value of .Delay](https://github.com/aspnet/Extensions/issues/1041).</span></span>

::: moniker-end

<span data-ttu-id="26360-336">В примере приложения `ReadinessPublisher` является реализацией <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>.</span><span class="sxs-lookup"><span data-stu-id="26360-336">In the sample app, `ReadinessPublisher` is an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation.</span></span> <span data-ttu-id="26360-337">Состояния проверки работоспособности записывается в `Entries` и в журнал для каждой проверки:</span><span class="sxs-lookup"><span data-stu-id="26360-337">The health check status is recorded in `Entries` and logged for each check:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/ReadinessPublisher.cs?name=snippet_ReadinessPublisher&highlight=20,22-23)]

<span data-ttu-id="26360-338">В примере приложения `LivenessProbeStartup` проверка готовности `StartupHostedService` использует задержку запуска в две секунды и выполняет проверку каждые 30 секунд.</span><span class="sxs-lookup"><span data-stu-id="26360-338">In the sample app's `LivenessProbeStartup` example, the `StartupHostedService` readiness check has a two second startup delay and runs the check every 30 seconds.</span></span> <span data-ttu-id="26360-339">Чтобы активировать реализацию <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher>, этот пример регистрирует `ReadinessPublisher` как отдельную службу в контейнере [внедрения зависимостей](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="26360-339">To activate the <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> implementation, the sample registers `ReadinessPublisher` as a singleton service in the [dependency injection (DI)](xref:fundamentals/dependency-injection) container:</span></span>

[!code-csharp[](health-checks/samples/2.x/HealthChecksSample/LivenessProbeStartup.cs?name=snippet_ConfigureServices&highlight=12-17,28)]

::: moniker range="= aspnetcore-2.2"

> [!NOTE]
> <span data-ttu-id="26360-340">Следующий вариант позволяет добавлять экземпляр <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> в контейнер службы, когда один или несколько других размещенных служб уже добавлены в приложение.</span><span class="sxs-lookup"><span data-stu-id="26360-340">The following workaround permits adding an <xref:Microsoft.Extensions.Diagnostics.HealthChecks.IHealthCheckPublisher> instance to the service container when one or more other hosted services have already been added to the app.</span></span> <span data-ttu-id="26360-341">Этот способ не нужно использовать в выпуске ASP.NET Core 3.0.</span><span class="sxs-lookup"><span data-stu-id="26360-341">This workaround won't be required with the release of ASP.NET Core 3.0.</span></span> <span data-ttu-id="26360-342">Дополнительные сведения см. в разделе https://github.com/aspnet/Extensions/issues/639.</span><span class="sxs-lookup"><span data-stu-id="26360-342">For more information, see: https://github.com/aspnet/Extensions/issues/639.</span></span>
>
> ```csharp
> private const string HealthCheckServiceAssembly = 
>     "Microsoft.Extensions.Diagnostics.HealthChecks.HealthCheckPublisherHostedService";
>
> services.TryAddEnumerable(
>     ServiceDescriptor.Singleton(typeof(IHostedService), 
>         typeof(HealthCheckPublisherOptions).Assembly
>             .GetType(HealthCheckServiceAssembly)));
> ```

::: moniker-end

> [!NOTE]
> <span data-ttu-id="26360-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) включает издатели для нескольких систем, в том числе [Application Insights](/azure/application-insights/app-insights-overview).</span><span class="sxs-lookup"><span data-stu-id="26360-343">[BeatPulse](https://github.com/Xabaril/BeatPulse) includes publishers for several systems, including [Application Insights](/azure/application-insights/app-insights-overview).</span></span>
>
> <span data-ttu-id="26360-344">[BeatPulse](https://github.com/Xabaril/BeatPulse) не поддерживается и не обслуживается корпорацией Майкрософт.</span><span class="sxs-lookup"><span data-stu-id="26360-344">[BeatPulse](https://github.com/Xabaril/BeatPulse) isn't maintained or supported by Microsoft.</span></span>
